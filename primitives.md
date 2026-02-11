**CMSI 3710** Computer Graphics, Spring 2026

# Assignment 0225
Before we plunge into our own 3D library, we have you spend some time on the primitive side of graphics in order to gain an appreciation for what it takes to implement these lower-level graphics operations.

## Background Reading
The entire [Pixar in a Box](https://www.khanacademy.org/computing/pixar) collection from Khan Academy is awesome, but in particular, you will want to look at these:
* [Color Science](https://www.khanacademy.org/computing/pixar/color) as a whole provides some key background information on color
* [RGB color model](https://www.khanacademy.org/computing/pixar/color/color-101/v/color-2) within that sequence, and its accompanying practice exercise, gives you the most direct information relating to this assignment

In the Shirley/Marscher textbook ([available on Brightspace](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870087/View)), you can get deeper treatment of this material and some future course content with _Chapter 3: Raster Images_.

To sharpen your “color intuition,” try out the [What the Hex](http://yizzle.com/whatthehex/) game.

And finally, if you want to learn more about the 2D drawing capabilities of the `canvas` element, try out this [tutorial from the Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial). There are also many other resources on `canvas` that can be found on the web.

## Primitives
The _primitives_ sample code shown in class is part of the starter code in this repository. It will be best to fully collaborate on this one as a group—except for the individualized items, do _all_ of your code-writing together, in person or over a screenshare. Use commits, pulls, and pushes to keep everyone in sync.

For this portion of the assignment, implement the following primitives-level operations. They may seem to vary in difficulty but they follow the same general pattern—which is part of the takeaway here 😁 Once you “crack” it, the actual code to write will consist of only a few lines apiece. Remember, when actually painting a color, _only_ use the given `setPixel` function to simulate what happens at the raw graphics primitive level.

For these exercises, don’t worry about the usual primitive concern of minimizing operations and restricting calculations to just integers. Compute as needed, and with floating point arithmetic.

Make sure that the _Primitives_ section of the demo application visually and effectively demonstrates your enhanced implementations.

### Dye Hard: 🌈 Radial Sunrise/Sunset Gradient 🌅
_Implement a two-color radial gradient for rectangles:_ Modify the `fillRectWithHalfRadialGradient` function in the _primitives_ module so that it takes two colors `c1` and `c2` and uses those colors to fill a rectangle with a semicircular _radial gradient_, with the center of the semicircle being the bottom-center of the rectangle. Its diameter should be the _smaller_ of the rectangle’s two dimensions. For example, the radial gradient for a 100×40 rectangle will have a diameter of 40 pixels; the radial gradient for a 30×98 rectangle will have a diameter of 30 pixels.

The result will resemble a sunrise or sunset. The bottom-center of the rectangle gets `c1`, grading outward such that any pixel that is on or beyond the semicircle gets `c2`. With the diameter specified as the smaller of the rectangle’s two dimensions, wide rectangles will have their semicircle reach the midpoint while tall rectangles will have the semicircle span the entire rectangle base. Refer to the illustrations drawn in class for visuals.

_Hint:_ The overall structure of the fill loop does not have to change. It’s only the color calculation that needs modification.

_Protip:_ Start with a solid-color rectangle first (drawn only with `setPixel` of course), then revise the code to choose the right colors after that.

_Demo:_ Make sure to provide rectangles of varying aspect ratios in order to show that your semicircle computation is generalized. Comment out the line-drawing section of the demo so that you have more vertical space.

### Dye Harder: 🟣 Two-Color Circle Fill 🟠
_Implement a linear two-color gradient for circles:_ Modify the circle functions and their `plotCirclePoints` helper in the _primitives_ module so that they accept _two_ colors and such that the circles get painted with a linear horizontal or vertical gradient (your choice) from the first color at the circles’s leftmost _x_- or uppermost _y_-coordinate to the second color at the circle’s right _x_- or bottom _y_-coordinate, respectively. Again, your group can decide whether to go horizontal or vertical.

_Hint:_ Figure out how to make `plotCirclePoints` fill the circle with a single color first. Note that the first couple of variations won’t look solid due to the way the coordinates are calculated.

_Demo:_ The entry-point circle functions will need to be revised to accept the two colors, but that should be it—they only need to pass the colors to `plotCirclePoints`. The primitives demo will then of course need to invoke the circle functions with two colors. Pick some nice pairings! 🌈

### Dye Hard with a Vengeance: Multicolor Polygon Fill 🧠💫✨🧐
_Implement a multipoint linear gradient for polygons:_ Modify the `fillPolygon` function in the _primitives_ module so that its third argument is expected to be an _array_ of colors, one color per vertex. The resulting polygon should be filled such that each vertex is painted with exactly the color that positionally corresponds to it in the given arrays, with every other pixel transitioning smoothly across these colors.

Yes, this is not unlike the `vertexColors` feature in _three.js_ geometries and materials. No, this is not a coincidence 💡

_Hint:_ The approach to this gradient is similar to the one for the original four-color rectangle gradient. Calculate vertical gradients based on the vertices’ _y_-coordinates, then as the algorithm scans across a row of pixels, perform a horizontal gradient from the color on the left side to the color on the right.

_Protip:_ Because there should be a one-to-one correspondence between the vertices and colors, you’ll want to enhance the `Edge` object so that it holds the vertices _and_ the corresponding colors of each edge in the polygon.

_Demo:_ Supply correspondingly-sized color arrays for the `fillPolygon` calls in the demo module. Change up the colors and/or the polygons themselves so that you can see how the algorithm looks with different combinations. Note that for certain shapes, the resulting gradient might have some unexpected visual artifacts—those are in fact expected for certain polygons (try to understand why, once you see them).

## Image Processing Filters
The _nanoshop_ and _nanoshop-neighborhood_ sample code shown in class is also part of this repository’s starter code.

For this portion, _each group member must implement filters individually_. The group should coordinate with each other so that the filter buttons include everyone’s work. For larger groups or filters with long names, change to a `select` element with an _Apply Filter_ button so that everything fits.

Thus, if your group has _n_ members, the final submission should have _n_ filters of each type.

### Live Free (of other pixels) or Dye Hard: Single-Pixel Filters
Add one (1) single-pixel filter of your own design, _per group member_, to the _nanoshop_ module. Modify the accompanying _NanoshopDemo_ component to use these new filters.

### A Good Day (in the neighborhood) to Dye Hard: Neighborhood-Pixel Filters
Add one (1) neighborhood-pixel filter of your own design, _per group member_, to the _nanoshop-neighborhood_ module. Modify the _NanoshopNeighborhoodDemo_ component to use these new filters.

Note you’ll end up with a grand total of _2n_ new filters, across the two types. For the _nanoshop-neighborhood_ additions, make sure that your filters _do_ use the neighborhood pixels when computing the filtered color. A “neighborhood” filter that bases its calculations solely on the central pixel will not get credit.

### Note: Yippee Ki Yay Mother Filter
To ensure that the filters aren’t solely simplistic derivatives of the samples (nor each other!), make sure to do one of the following:
* Perform _genuinely different_ computations—don’t just change up constants! (because that is more or less the same filter)
* **or** _parameterize_ the filters—i.e., refactor the code so that you have functions that _return_ filter functions. Here’s an example (which you therefore you can’t use as your own):

```javascript
const uniformScale = coefficient =>
  (x, y, r, g, b, a) => [r * coefficient, g * coefficient, b * coefficient, a]
```

Observe that this is a function that _creates_ variants of the `darken` filter, where the amount by which to brighten/darken a color is given by the `coefficient`.

## Extra Credit: Your Own 2D Canvas Drawing
You might recall that three.js creates a `canvas` element to do its 3D work. The image processing filter examples use the `canvas` element in _2D_ mode. All of the code is there (written by a former student) and if you like, you can write some new code so that the scene being drawn is different. By and large, the Mozilla Developer Network’s [`canvas` tutorial](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial) is enough to get a start.

Make sure that your drawing shows enough nuance and variety to show off the full functionality of the filters that you implement.

## How to Turn it In
Commit the following to your repository:
- Three (3) primitive enhancements
- One (1) single-pixel filter per group member, for a total of _n_ single-pixel filters for an _n_-member group
- One (1) neighborhood-pixel filter per group member, for a total of _n_ neighborhood-pixel filters an _n_-member group
- Corresponding modifications to the web application code that demonstrate these improvements effectively
- (optionally) Code that creates a different `canvas` image for the filters

## Specific Point Allocations
This assignment is scored according to outcomes _1a_, _2c_, _3b_, and _4a_–_4f_ in the [syllabus](http://dondi.lmu.build/spring2026/cmsi3710/cmsi3710-spring2026-syllabus.pdf). For this particular assignment, graded categories are as follows:

| Category | Points | Outcomes |
| -------- | -----: | -------- |
| Linear gradient in a circle | 20 points total | _1a_, _2c_, _3b_, _4a_–_4d_ |
| • Implemented correctly and well | 14 points | |
| • Demonstrated effectively | 6 points | |
| Half-ellipse gradient in a rectangle | 20 points total | _1a_, _2c_, _3b_, _4a_–_4d_ |
| • Implemented correctly and well | 14 points | |
| • Demonstrated effectively | 6 points | |
| Vertex-color gradient in a polygon | 26 points total | _1a_, _2c_, _3b_, _4a_–_4d_ |
| • Implemented correctly and well | 18 points | |
| • Demonstrated effectively | 8 points | |
| Single-pixel filters (individual) | 14 points total | _1a_, _2c_, _3b_, _4a_–_4d_ |
| • Implemented correctly and well | 6 points | |
| • Demonstrated effectively | 4 points | |
| • Genuinely different computations or parameterized | 4 points | |
| Neighborhood filters (individual) | 20 points total | _1a_, _2c_, _3b_, _4a_–_4d_ |
| • Implemented correctly and well | 8 points | |
| • Demonstrated effectively | 6 points | |
| • Genuinely different computations or parameterized | 6 points | |
| • Genuinely uses neighborhood | _zero credit_ (if not done) | |
| Custom `canvas` picture for filter demos | up to 20 points extra credit | _4a_–_4d_ |
| • Well-suited for image filters | reduced extra credit (if not done) | |
| Hard-to-maintain or error-prone code | deduction only | _4b_ |
| Hard-to-read code | deduction only | _4c_ |
| Version control | deduction only | _4e_ |
| Punctuality | deduction only | _4f_ |
| **Total** | **100** |

Note that inability to compile and run any code to begin with will negatively affect other criteria, because if we can’t run your code (or commands), we can’t evaluate related remaining items completely.
