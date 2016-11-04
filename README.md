# .impress()
A light-weight jQuery plugin to standardize on click and on touchstart events

## Function
Extend jQuery

```javascript
$.fn.impress = function (callback) {
    
    // Track touches
    var trackTouch = {};
    
    this.on('touchstart', function (e) {
        
        // Track touchstart
        trackTouch.x = e.touches[0].pageX;
        trackTouch.y = e.touches[0].pageY;
        
    }).on('touchend', function (e) {
        
        e.preventDefault();
        
        // Determine if this is a static touch or a scroll attempt
        var scrollX = Math.abs(trackTouch.x - e.changedTouches[0].pageX),
            scrollY = Math.abs(trackTouch.y - e.changedTouches[0].pageY);
        
        // Fire if touch event is not a scroll
        if ( (scrollX < 10) && (scrollY < 10) ) {
            callback.call(this, e);
        }
        
    }).on('click', function (e) {
        
        e.preventDefault();
        
        // Fire if no touch event was deteched
        if (typeof trackTouch.x === 'undefined') {
            callback.call(this, e);    
        }
    });
    
    // Continue chain method
    return this;
    
};
```

## Use
Functionally, the impress() function operates the same as the click() function.

```javascript
$('#div').impress(function () {
  // Do something
});
```
