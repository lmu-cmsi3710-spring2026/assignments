**CMSI 3710** Computer Graphics, Spring 2026

# Assignment 0408
This aspect of your 3D framework adds major flexibility to your 3D objects through the power of vectors and matrices.

## Background Reading
This assignment’s supporting content is virtually all mathematical—[Shirley/Marscher](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870087/View) here we go:
* _Chapter 2: Miscellaneous Math_
* _Chapter 5: Linear Algebra_
* _Chapter 6: Transformation Matrices_
* _Chapter 7: Viewing_

Closer to the code, [Real-Time 3D Graphics with WebGL 2](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870088/View) has _Projection Transform_ and _Perspective Division_, which talk about the concepts behind projection. _The Projection Matrix_ and successive sections walk you through the authors’ own approach to a matrix library and implementing projection.

Depending on what style works better for you, [GLSL Essentials](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870089/View) has equivalents to these as well, emphasizing GLSL rather than mathematics (which is assumed to be understood by the reader—they show what you can do with matrices in GLSL but assume that you know what’s _in_ the matrices). _Section 3: Vertex Shaders_ walks through things end to end. The _Drawing a simple geometry sample_ subsection starts with some sample shader code for how matrices would be used.

The two programming books combined give you two similar but separate examples for matrices and projection, with the same concepts but written in different ways and with different emphases. Pick whatever speaks to your team best—they all lead you in the right direction. And feel free to read further, too.

The sample playground code includes a vector implementation (_vector.js_) with accompanying test suite (_vector.test.js_). It will be useful to understand this code well before going all Morpheus (or Neo?) on your own. And don’t forget the potential conciseness offered by JavaScript [getters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get) and [setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set), as mentioned in the [companion assignment](./static-3d-scene.md).

## For Submission: Enter the Matrix
This assignment focuses on implementing matrix transformations—the core geometric engine of any graphics pipeline. The core functionality should be a distinct, independent module, with the 3D object framework from the [companion assignment](./static-3d-scene.md) meant to use this module to round out the “version 1” functionality of a rudimentary scene library.

### Build the Matrix
Design and implement a computer graphics matrix library with an accompanying suite of unit tests. As with the 3D object framework, no specific design is mandated. However, the following capabilities should be provided:
- A basic 4×4 matrix object that initializes, by default, to the identity matrix
- Matrix multiplication
- Collection of 3D matrix implementations (contributed individually)
  * 3D translation
  * 3D scale
  * 3D rotation about on an arbitrary axis (you may refactor the sample code to fit your matrix object implementation)
  * Orthographic projection
  * Perspective (frustum) projection
- Conversion/convenience functions to prepare the matrix data for direct consumption by WebGL and GLSL

Note that there are enough matrix categories to go around individually, so this is what constitutes the individual credit: each team member will take individual point on one of the above matrix types. Supply a _matrix-credits.md_ file that identifies the specific matrices (and [accompanying tests](#test-the-matrix)) for which each individual team member was primarily responsible.

_All_ of these matrices are required, so matrices that aren’t an individual responsibility will reside with the team.

### Test the Matrix
This item is sufficiently important to deserve its own section:
- A unit test suite based on [Vitest](https://vitest.dev/)

As pure computations with unambiguous correct results and requiring no interaction with the user, matrix functionality is eminently easy to test. [Vitest](https://vitest.dev/)’s approach makes it even more convenient: invoke `npm run test`, keep it running, then write your code and tests. You will see test results as you work. By default, you will only see tests for files changed since the last commit. If you invoke `npm run coverage`, you will see a coverage report showing how much of each module was executed by your test suite. A _coverage_ folder is generated in your repository with even more detailed information.

Like the 4×4 matrix implementations themselves, responsibility for tests is individualized, corresponding to the matrix that a particular team member took point on. Matrix types that are implemented by the team are the responsibility of the team.

### Apply the Matrix
Armed with your newly-minted matrix library, enhance your 3D framework from the [companion assignment](./static-3d-scene.md) in the following ways:
- Give your 3D objects an _instance transformation_: A per-object composition of rotate, scale, and translate matrices that will let you position objects in the scene and within groups. This matrix will be sent to the vertex shader—they are just _stored_ in JavaScript, but GLSL will perform the actual transformation (as it should—that’s why we have graphics hardware!)
- The 3D object grouping functionality from the [companion assignment](./static-3d-scene.md) gets most of its power from the instance transformation: a 3D object group’s members _build their instance transformations on top of the parent’s_. This is the secret to how composite objects “stay together” when the full group is repositioned, rotated, or scaled
- Don’t forget that 3D object groups are _trees_ and can go arbitrarily deeply (e.g., you can group fingers with a palm to form a hand; that hand can be added to an arm; that arm can be added to a body, etc.), so the instance transformations should compose accordingly as well

The key idea here is to have, after this portion is done, a complete “construction set” of sorts for creating, composing, and now transforming your scene objects in a manner that is limited solely by your imagination.

### Use the Matrix
Once you reach this point, you now have all of the pieces needed to create a _complete 3D scene_ from your 3D object and matrix framework (the [companion assignment](./static-3d-scene.md), by itself, is unable to manipulate objects easily once they are created—that’s why it’s static; this assignment makes it dynamic). Use instance transformations to position, rotate, and resize the objects in your scene. Use the composite/group ability to build more complex objects that transform as a unit. Modify these transformations by calling `requestAnimationFrame` repeatedly, changing the matrices, then re-rendering the scene. Welcome to the foundational computations behind [bullet time](https://www.youtube.com/watch?v=I1ZbUs1xwes).

### Project the Matrix
The second major capability afforded by your matrix library will now be the ability to break out of that normalized device coordinates (NDC) 2×2×2 cube thanks to the availability of projection matrices. Integrate a projection matrix into your shader so that you can have full access to a world space that is determined by _you_ rather than WebGL/GLSL. (well, NDC is still there but you’re adding a layer that pushes it under the hood) This will also get you out of a square canvas without otherwise distorting the aspect ratio of your scene!

### Extra Credit: JSON Representation
The pros use tools, not code, to create their 3D objects and scenes—for extra credit, you can take a stab at implementing this capability. (in those tools, code _generates_ objects but those objects, once generated, can be represented as pure data)

Separate your 3D scene from the code by allowing it to be represented in JSON. Find a way to express and process the scene such that one or more of these capabilities is present:

- Objects at different levels of abstraction (3D object, polygon mesh, mesh maker) are supported
  * Mesh maker functions can be called in order to create an object programmatically
  * Alternatively, custom vertices and faces may be listed directly
- Properties that define a current instance transformation are available
- Environment settings such as a viewing volume and projection type can be specified
- Consider looking at established 3D model formats and represent your scenes according to that format—if you do this, your scenes and objects can then be loaded into other applications! Or, vice versa, you can load models from other applications into your own 3D library 🤯

This functionality is somewhat open-ended so extra credit will be given commensurate with how much is (correctly) implemented.

## How to Turn it In
Commit the following to your repository:
- [4×4 matrix object/library](#build-the-matrix)
  * Default to identity matrix
  * Matrix multiplication implementation
  * 3D matrix collection (mostly individual responsibility)
  * Conversion/convenience functions to work with WebGL and GLSL
- [Matrix test suite](#test-the-matrix)
  * Test responsibility aligns with implementation responsibility (i.e., if the team implemented something, the team should implement the tests; if an individual took point on something, the individual should take point on the tests)
  * _matrix-credits.md_ file documents who did what matrix
- [Matrix use in 3D objects](#apply-the-matrix)
  * 3D objects maintain an instance transformation
  * 3D object groups compose children’s instance transformations recursively
- [Matrix use in projection](#project-the-matrix)
  * Orthographic or perspective projection can be applied to the scene (thus breaking it out of the NDC 2×2×2 space)
- (extra credit) Ability to [represent a scene in JSON](#extra-credit-json-representation)

Because you will be committing future assignments to this same repository, make sure to _tag_ the commit that represents this assignment like so:
```bash
$ git tag assignment-0408
$ git push --tags
```

## Specific Point Allocations
This assignment is scored according to outcomes _2a_, _2b_, _3a_, _3c_, _3d_, and _4a_–_4f_ in the [syllabus](http://dondi.lmu.build/spring2026/cmsi3710/cmsi3710-spring2026-syllabus.pdf). For this particular assignment, graded categories are as follows:

| Category | Points | Outcomes |
| -------- | -----: | -------- |
| 4×4 matrix object/library | 30 points total | _2a_, _2b_, _4a_–_4d_ |
| • Default to identity matrix | 2 points | |
| • Matrix multiplication is implemented | 8 points | |
| • Team-based 3D matrix (team’s choice) | 4 points | |
| • Individual-responsibility 3D matrix | 8 points | |
| • Matrix data converts to WebGL and GLSL | 3 points | |
| • Implemented correctly and well | 5 points | |
| Matrix test suite | 25 points total | _2a_, _2b_, _4a_–_4d_ |
| • Default identity tested correctly and well | 1 point | |
| • Default identity test has sufficiently high coverage | 1 point | |
| • Multiplication tested correctly and well | 4 points | |
| • Multiplication test has sufficiently high coverage | 4 points | |
| • Team matrix tested correctly and well | 2 points | |
| • Team matrix test has sufficiently high coverage | 2 points | |
| • Individual matrix tested correctly and well | 4 points | |
| • Individual matrix test has sufficiently high coverage | 4 points | |
| • WebGL and GLSL conversion tested correctly and well | 2 points | |
| • WebGL and GLSL conversion test has sufficiently high coverage | 1 point | |
| • _matrix-credits.md_ lists individual matrix assignments | deduction only if not fulfilled | |
| Matrix use in 3D objects | 30 points total | _2a_, _3a_, _3c_, _3d_, _4a_–_4d_ |
| • Maintain an instance transformation | 10 points | |
| • Groups compose instance transformations | 15 points | |
| • Implemented correctly and well | 5 points | |
| Matrix use in projection | 15 points total | _2b_, _3c_, _3d_, _4a_–_4d_ |
| • Ability to use orthographic or perspective projection | 10 points | |
| • Implemented correctly and well | 5 points | |
| JSON representation of scene | extra credit | _4a_–_4d_ |
| Hard-to-maintain or error-prone code | deduction only | _4b_ |
| Hard-to-read code | deduction only | _4c_ |
| Version control | deduction only | _4e_ |
| Punctuality | deduction only | _4f_ |
| **Total** | **100** |

Note that inability to compile and run any code to begin with will negatively affect other criteria, because if we can’t run your code (or commands), we can’t evaluate related remaining items completely.
