Google Tag Manager Cookbook
===========================

Event Tracking
--------------

There are 2 methods for event tracking in Google Tag Manager.

1. Auto-Event Tracking
2. Using jQuery

There are some known limitations with auto-event tracking and they include the following

- Other non GTM event code blocking auto-event tracking
- No feature for non onclick events. eg. selected value from dropdown
- No ability to detect errors including if an element has been removed due to website changes. (solved with option 2 using jQuery)
- If you have elements within elements that can be clicked on GTM will use the child element which may result in the need for multiple rules to select all the child elements. eg see code below.

```html
<li>
  <a href="/family-center/family-filter?category=cards">
    <h3>Cards</h3>
    <div class="img-bg">
    </div>
    <div class="img-holder">
      <img src="/~/media/creative-center/images/category-images/family-center/cards.ashx" alt="Cards" width="271" height="229">
    </div>
  </a>
</li>
```

For these reasons I recommend option 2 using jQuery.

Option 1: Auto-Event Tracking
---------------------------

Option 2: Event Tracking Using jQuery
---------------------------

### Event Tracking with built in test

The following code is an example of how you can bind an event to an element but it also includes a test first to see if that element exists on the page. This is a very useful way to easily detect if any of your events stop working due to website changes you may be unaware of.

```html
<script>
var elementSelector = '#btnStoreSearc';

if( $(elementSelector).length === 0 ){

  console.log( 'missing' );
  
  dataLayer.push({
    'event' : 'event tracking error',
    'eventCategory' : 'event tracking error',
    'eventLabel' : elementSelector,
    'eventAction' : 'onclick element missing',
    'eventNonInteraction' : true,
  })

} else {

  $(elementSelector).on('click',function(){
  
    console.log( 'found' );
    console.log( $( this ) );
  
    dataLayer.push({
      'event' : 'event tracking',
      'eventCategory' : 'event category',
      'eventLabel' : 'event label',
      'eventAction' : 'event action',
      'eventNonInteraction' : true,
    })
  
  })

}
</script>
```

### Event Tracking delay default behaviour

You may need to delay the behaviour of the event such as form submit so there is enough time for the tag to execute.

In the example below you can prevent the form from submitting when the button is clicked for 1 second.

```html
<script>
$( '#cccontent_0_pagecontent_0_PersonaliseButton' ).on( "click", function( event ) {
  
  // prevent the form from submitting
  event.preventDefault();
  
  dataLayer.push({
      'event' : 'event tracking',
      'eventCategory' : 'event category',
      'eventLabel' : 'event label',
      'eventAction' : 'event action',
      'eventNonInteraction' : true,
    })
  
  // submit the form after 1 second (1000 milliseconds)
  setTimeout(function(){
  $('form').submit();
  },1000)
});
</script>
```

### Event tracking and capturing child element information

```html
<script>
$('.categories-container li').on('click', function(event) {

event.preventDefault();

console.log('click');
console.dir( $(this)[0] );
console.log( $('h3',this)[0] );
console.log( $('h3',this)[0].innerText );
console.dir( event.target );
console.log( event.type );

})
</script>
```
