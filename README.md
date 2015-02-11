## Glitter - Native Paint Demos

Demos for the native implementation of the Glitter Paint APIs in Chromium.

The following patches are required for them to run:

* https://codereview.chromium.org/835353005/
* https://codereview.chromium.org/849183003/
* https://codereview.chromium.org/866223003/
    * Optional: required for patterns, gradients, images, text)

They don't make use of any pipeline features (most notably, the animation is
done in a very hacky way) and exist just to demonstrate the proof-of-concept
native implementation.

In the native implementation, you may call .registerCustomPaint(function)
on any element with a background. The function takes in a "canvas"/"graphics
context" as argument: eg

```element.registerCustomPaint(function(ctx) {console.log(ctx.width);})```

that is essentially a "write-only" version of a HTML canvas element's context.
Core features of the "write-only canvas" include paths, stroke/fill/clip,
state save/restore and transforms. The notable feature excluded is getting
image data out of the canvas, which is excluded for security/performance
concerns.

There are some minor differences in function names/etc between this
and the non-native Glitter implementation, as the native implementation mirrors
the HTML canvas element API as closely as possible. Additionally, the native
implementation currently only allows drawing on background (no onContent).

Both button-demo and text-demo manually animate the "shapes" being painted
by updating, redrawing, sleeping for frame length, and repeating, as for now,
the sizes, positions, etc of the shapes (ripples for button, rectangles for
text) need to be global so that they can be accessed within the custom paint
function. Ideally this can be replaced with custom properties in the future.
