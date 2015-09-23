# FTScroller

FTScroller is a library for adding momentum scrolling to web content on devices with a touch interface, compatible across most modern platforms including desktop browsers.  Although recently support for `overflow: scroll` (or touch equivalents) has increased, this is often still not implemented in a cross-platform or backwards-compatible way, and with no support for features like snapping.

FTScroller is developed by [FT Labs](http://labs.ft.com), part of the Financial Times.  It is inspired by [Touchscroll](https://github.com/davidaurelio/TouchScroll) and [Zynga Scroller](https://github.com/zynga/scroller), but is a complete rewrite.  It is extensively used in the [FT Web App](http://app.ft.com), and was developed to achieve better performance and compatibility, including mouse and touch input.


## Usage

Include ftscroller.js in your JavaScript bundle or add it to your HTML page like this:

    <script type='text/javascript' src='/path/to/ftscroller.js'></script>

The script must be loaded prior to instantiating a scroller on any element of the page.

To create a scroller, with a few minimal options:

```js
var containerElement, scroller;

containerElement = document.getElementById('scrollcontainer');

scroller = new FTScroller(containerElement, {
	scrollbars: false,
	scrollingX: false
});
```

## Examples

FTScroller is designed to accommodate a range of use cases.  Here are some examples - feel free to copy the code and use as the basis for your own projects.

* [Vertical continuous list](http://ftlabs.github.com/ftscroller/examples/verticalcontinuous.html)
* [Horizontal paged](http://ftlabs.github.com/ftscroller/examples/horizontalpaged.html) (and a [strict version](http://ftlabs.github.com/ftscroller/examples/horizontalpaged-strict.html), constrained to scrolling one page at a time)
* [Multiple vertical scrollers in a horizontally paged scroller](http://ftlabs.github.com/ftscroller/examples/galleryscrollers.html)
* [Whole page scroller](http://ftlabs.github.com/ftscroller/examples/wholepage.html)


## Options

Options must be specified at create-time by passing a JSON object as the second argument to the `FTScroller` constructor.

* `alwaysScroll` Whether to always enable scrolling, even if the content of the scroller is not large enough to spill out of the container.  This makes the scroller behave more like an element set to "overflow: scroll", with bouncing always occurring if enabled _(boolean, default false)_
* `baseAlignments` Determines where to anchor the content when the scroller is set up, specified individually for each axis using a JSON object with the keys `x` and `y`. Valid alignments for each axis are -1 (top or left), 0 (center), and 1 (bottom or right).  For example, the default baseAlignments of `{x:-1,y:-1}` will display the scroller initially scrolled to the top left of its range.  This also affects the alignment of content where the content is smaller than the scroller.
* `bouncing` Allow scroll bouncing and elasticity near the ends of the range and at snap points _(boolean, default true)_
* `contentWidth` Define the scrollable width; if not defined, this will match the content width _(numeric, default undefined)_
* `contentHeight` Define the scrollable height; if not defined, this will match the content height _(numeric, default undefined)_
* `disabledInputMethods` Define any input methods to disable; on some multi-input devices custom behaviour may be desired for some scrollers.  No inputs methods are disabled by default. _(object, default { mouse: false, touch: false, scroll: false, pointer: false, focus: false })_
* `enableRequestAnimationFrameSupport` FTScroller will use requestAnimationFrame on platforms which support it, which is highly recommended; however this can result in the animation being a further half-frame behind the input method, increasing perceived lag slightly.  To disable this, set this property to false. _(boolean, default true)_
* `flinging` Allow a fast scroll to continue with momentum when released _(boolean, default true)_
* `hwAccelerationClass` FTScroller uses normal translate properties rather than translate3d to position content when scrolling, and triggers hardware acceleration by adding CSS properties (specifically backface-visibility) to this class on platforms that support it.  Adjusting this class allows for negotiating complex CSS inheritance to override the default behaviour of FTScroller if you want to change or disable backing layers/3D acceleration. _(string, default an internal class which triggers backing layers)_
* `maxFlingDuration` Set the maximum time (ms) that a fling can take to complete once the input has ended _(numeric, default 1000ms)_
* `scrollbars` Whether to display iOS-style scrollbars (which you can style yourself using `.ftscroller_scrollbar` and `.ftscroller_scrollbarx`/`.ftscroller_scrollbary`) while the content is animating _(boolean, default true)_
* `scrollBoundary` The initial movement required to trigger a full scroll, in pixels; this is the point at which the scroll is exclusive to this particular FTScroller instance and flings become active _(integer, default 1)_
* `scrollingClassName` The classname to add to the scroller container when it is being actively scrolled.  This is disabled by default as it can cause a CSS relayout if enabled, but allows custom styling in response to scrolls _(string, default not set)_
* `scrollResponseBoundary` The initial movement required to trigger a visual scroll update, in pixels _(integer, default 1)_
* `scrollingX` Enable scrolling on the X axis if content is available _(boolean, default true)_
* `scrollingY` Enable scrolling on the Y axis if content is available _(boolean, default true)_
* `singlePageScrolls` _(was `paginatedSnap`)_ If snapping is enabled, restricts each scroll movement to one 'page' span.  That is, if set to true, it will not be possible to move more than one page in a single movement. _(boolean, default false)_
* `snapping` Enable snapping of content to defined 'pages' or segments _(boolean, default false)_
* `snapSizeX` Define the horizontal interval content should snap to, in pixels.  If this is not set, snapping will be based on pages corresponding to the container size. _(numeric, default undefined)_
* `snapSizeY` Define the vertical interval content should snap to, in pixels.  If this is not set, snapping will be based on pages corresponding to the container size. _(numeric, default undefined)_
* `updateOnChanges` Automatically detect changes to the content of the scrollable element and update the scroller dimensions whenever the content changes. This is set to false automatically if `contentWidth` and `contentHeight` are specified _(boolean, default true)_
* `updateOnWindowResize` Automatically catch changes to the window size and update the dimensions of the scroller.  It's advisable to set this to true if the scroller has a flexible width or height based on the viewport size. _(boolean, default false)_
* `windowScrollingActiveFlag` Whether to use a global property of the window object to control whether to allow scrolling to start or not.  If the specified window property is set to a truthy value, the scroller will not react to input.  If the property is not truthy, the scroller will set it to itself and will scroll.  Where multiple scrollers exist on the same page, this ensures that only one can be used at a time, which is particularly useful for nested scrollers (see [Multiple vertical scrollers in a horizontally paged scroller](examples/galleryscrollers.html)).  Note that FTScroller automatically allows only one scroller instance to be scrolled at once; use this flag to coordinate input with other parts of your code. _(string, default not set)_
* `flingBezier` The bezier curve to use for momentum-like flings. _(CubicBezier, default CubicBezier(0.103, 0.389, 0.307, 0.966))_
* `bounceDecelerationBezier` The bezier curve to use for deceleration when a fling hits the bounds. _(CubicBezier, default CubicBezier(0, 0.5, 0.5, 1))_
* `bounceBezier` The bezier curve to use for bouncebacks when the scroll exceeds the bounds. _(CubicBezier, default CubicBezier(0.7, 0, 0.9, 0.6))_
* `invertScrollWheel` If the scroller is constrained to an x axis, convert y scroll to allow single-axis scroll wheels to scroll constrained content. _(boolean, default true)_

## Public interface

Once the scroller has been applied to an element, the return value from the constructor is an object that offers a number of public properties, methods and events.

### Properties

* `scrollHeight` Gets the scrollable height of the contained content. **Read only**
* `scrollLeft` Gets or sets the current left scroll offset of the scroller in pixels.  When set, will cause the scroller to jump to that position (without animating)
* `scrollTop` Gets or sets the current top scroll offset of the scroller in pixels.  When set, will cause the scroller to jump to that position (without animating)
* `scrollWidth` Gets the scrollable width of the contained content. **Read only**
* `segmentCount` When snapping is enabled, returns the number of snap segments (which in many use cases can be considered 'pages'). **Read only**.
* `currentSegment` Returns the index of the current segment, starting from 0.  Updated when the scroller comes to rest on a new segment.  Applies only when `snapping` is set to true. **Read only**.
* `contentContainerNode` Returns the DOM node that contains the scroller contents, for use if you want to amend the contents. **Read only**.

### Methods

* `addEventListener(eventname, callback)` Attaches a specified function to an FTScroller custom event.  Available events are listed in the events section below.  The function will be called by FTScroller when the event occurs.
* `destroy(removeElements)` Unbinds all event listeners to prevent circular references preventing items from being deallocated, and clean up references to dom elements.  Does not remove scroller markup from the DOM unless the optional `removeElements` argument is set to a truthy value.
* `removeEventListener(eventname, callback)` Removes a previously bound function from an event.  The function specified in the mathod must be the same object reference passed in the `addEventListener` call (not a redefinition of the same function code)
* `scrollBy(horizontal, vertical[, duration])` Similar to `scrollTo` but allows scrolling relative to the current position rather than to an absolute offset.
* `scrollTo(left, top[, duration])` Scroll to a specified left and top offet over a specified duration.  If duration is zero, no animation will occur.  If duration is `true` FTScroller will itself choose a duration based on the distance to be scrolled.  The left and top inputs will be constrained by the size of the content and the snap points.  If false is supplied for either left or top, that axis will not be scrolled and will retain its current offset.
* `setSnapSize(width, height)` Configures the snapping boundaries within the scrolling element if snapping is active.  If this is never called, segment size defaults to the width and height of the scroller, ie. page-at-a-time.
* `updateDimensions(width, height[, nosnap])` Sets the dimensions of the scrollable content.  If snapping is enabled, and you wish to disable updates of the snapping grid and prevent the current position from being updated, set `nosnap` to true; it defaults to false if not supplied.
* `setDisabledInputMethods(disabledInputMethods)` Set the input methods to disable. No inputs methods are disabled by default. `(object, default { mouse: false, touch: false, scroll: false, pointer: false, focus: false })`

### Prototype methods

* `getPrependedHTML([excludeXAxis, excludeYAxis, hwAccelerationClass])` - Provides half of the HTML that is used to construct the scroller DOM, for use to save a DOM manipulation on Scroller initialisation (see Tips and tricks below).  Optionally the x and y axes can be excluded, or a custom layer backing triggering class can be supplied (see the `hwAccelerationClass` option for the constructor).
* `getAppendedHTML([excludeXAxis, excludeYAxis, hwAccelerationClass, scrollbars])` - Provides the second half of the HTML that is used to construct the scroller DOM, for use to save a DOM manipulation on Scroller initialisation (see Tips and tricks below).  Optionally the x and y axes can be excluded, or a custom layer backing triggering class can be supplied (see the `hwAccelerationClass` option for the constructor).  Pass a truthy value in for the `scrollbars` parameter if you are enabling scrolling.  _Any parameters should match those passed in to `getPrependedHTML`._


### Events

Events can be bound with the `addEventListener` method.  Events are fired syncronously.  Regardless of the event, the listener function will receive a single argument.

* `scroll` Fired whenever the scroll position changes, continuously if an input type is modifying the position.  Passes an object containing `scrollLeft` and `scrollTop` properties matching the new scroll position, eg `{scrollLeft:10, scrollTop:45}`
* `scrollstart` Fired when a scroll movement starts.  Passes an object with the same characteristics as `scroll`.
* `scrollend`  Fired when a scroll movement ends.  Passes an object with the same characteristics as `scroll`.
* `segmentdidchange` Fires on completion of a scroll movement, if the scroller is on a different segment to the one it was on at the start of the movement.  Passes an object with `segmentX` and `segmentY` properties.
* `segmentwillchange` Fires as soon as the scroll position crosses a segment boundary, during a scroll movement.  Passes an object with `segmentX` and `segmentY` properties.
* `reachedstart` Fires when the scroll position reaches the top or left of a scroller.  Passes an object with an `axis` property indicating whether the `x` or `y` axis reached its start position.
* `reachedend` Fires when the scroll position reaches the bottom or right of a scroller.  Passes an object with an `axis` property indicating whether the `x` or `y` axis reached its end position.

## Compatibility

FTScroller supports input via mouse or touch on the following browsers:

* **Apple Mobile Safari** (iOS 3.2+ confirmed, best after 4.0+)
* **Google Android Browser** (Android 2.2+ confirmed)
* **Microsoft Internet Explorer 10** (Windows 8 RTM confirmed)
* **RIM BlackBerry PlayBook** (2.0 confirmed, 1.0 should be fine)
* **Most modern desktop browsers** (IE 9+, FF 4+, Safari 3+, Chrome 1+, Opera 11.5+ - ECMAScript 5 getters and setters are currently used)

## Tips and tricks

* If you are putting together the DOM in JavaScript using HTML or a template, it's advantageous to use the prototype methods for `getPrependedHTML` and `getAppendedHTML` to add the FTScroller HTML at the same time.  If the elements are added to the very start and very end of the content within the element passed in to the constructor (ignoring whitespace), the Scroller instantiation will detect this and skip the DOM manipulation step, leading to faster instantiation without an additional layout.
* If scrolling is going to only occur along one axis, setting `scrollingX` or `scrollingY` to `false` as part of the construction options skips creating of the corresponding DOM element, saving memory (particularly when using hardware acceleration).
* Depending on your CSS, you may find that you can't scroll quite to the bottom of your content.  This is typically a **[collapsing margin](http://www.complexspiral.com/publications/uncollapsing-margins/)** issue — the contents are measured, then wrapped in additional elements, but the margins spilling _out_ of the contents can't be measured.  If you see this, an easy fix is typically to put `overflow: hidden` either your scrollable element or its contents; that will act as a boundary for margins so everything can be measured, and typically has no effect on appearance when applied to the content as the scrollable content won't have dimensional limits.



## Credits and collaboration

The lead developer of FTScroller is Rowan Beentje at FT Labs.  All open source code released by FT Labs is licenced under the MIT licence.  We welcome comments, feedback and suggestions.  Please feel free to raise an issue or pull request.  Enjoy.
