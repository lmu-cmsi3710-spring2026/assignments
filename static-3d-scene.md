**CMSI 3710** Computer Graphics, Spring 2026

# Assignment 0330
Alright, time to go “under the hood.” As mentioned in class, the overall spirit of the remaining 3D assignments is to give you a taste of what it’s like to build your very own 3D graphics library. Design decisions are yours to make, as long as you implement the requested functionality.

## Background Reading
Our recommended texts now take center stage—make sure you get the hang of accessing them! Links are available in the [Content > Books](https://brightspace.lmu.edu/d2l/le/content/295613/Home?itemIdentifier=D2L.LE.Content.ContentObject.ModuleCO-3870083) section in Brightspace.

### Concepts and Theory
These sections/chapters of the [Shirley/Marscher textbook](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870087/View) will set the foundational tone for the rest of the semester:
* _Chapter 1: Introduction_ lays down some big-picture and background ideas
* _Chapter 2: Miscellaneous Math_ will come up frequently—for this specific assignment, take note of the sections on trigonometry and various curves and surfaces
* _Chapter 12: Data Structures for Graphics_ has been mentioned before, but is now worth revisiting with an eye toward implementing these for yourselves. Sections 12.1 and 12.2 are likely of particular interest

### Programming and Technology
The central technologies that we’ll be picking up are WebGL—the web flavor of the GL graphics library (which may well be a redundant phrase)—and the GL Shading Language (GLSL). That’s where our two programming books come in, respectively:
* [Real-Time 3D Graphics with WebGL 2](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870088/View) provides another view of the same core material, but now with a specific leaning toward WebGL and JavaScript. Turn to this book for alternative sample code to what you are given in the class. You will see overlap between the code I give you and the code in this book—think of this as a “second opinion” on how to build a graphics application directly on top of WebGL
* [GLSL Essentials](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870089/View) focuses on—surprise!—GLSL. Chapter 1 provides one more perspective of the graphics pipeline (sometimes it’s good to read about the same concept from multiple sources); Chapter 2 is **not to be missed** as it introduces you to the core pieces of the language; and the beginnings of Chapters 3 and 4 will come in handy as they center on vertex and fragment shaders, respectively

On the non-graphics-specific front, if you haven’t seen these before it might be useful to look into JavaScript [getters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get) and [setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set). These mechanisms are syntactic sugar that can make functions look like standard properties: i.e., instead of `object.getProperty()` and `object.setProperty(value)` for some `object`, defining getters and setters will let you write `object.property` and `object.property = value` while still invoking functions to return and set `property`’s value. The sample _vector_ object shows the use of `get`; `set` is similar and should be pretty easy to pick up based on the MDN links above or other online sources.

### Reference
The reference books [OpenGL Programming Guide](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870090/View) (red), [OpenGL Shading Language](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870091/View) (orange), and [WebGL Programming Guide](https://brightspace.lmu.edu/d2l/le/content/295613/viewContent/3870367/View) function as companions to the programming books, but deeper. Use these books if there is a function, constant, symbol, or other concept that you need to learn about in depth. Although these books aren’t exactly like encyclopedias or dictionaries, one tends to use them that way: instead of reading them end to end, you can go straight to their indices, look up the term you’re interested in, then read the corresponding pages.

## For Submission: Scenes and Shapes
The focus of this assignment is the creation of a library for specifying 3D scenes. There is an intentional design-it-yourself element here, so rather than providing _starter_ code, you are instead given _sample_ code—it is meant to show you raw functionality and nothing else. You are not obligated to follow their structure. Design decisions are yours to make. [three.js](https://threejs.org) can be viewed as an example of what a full-featured version of such a library could look like. For this course, we are aiming for a subset of what [three.js](https://threejs.org) can do.

### Envision a Scene
It’s a good idea to have _some_ notion of what you want to render by the end of the semester. Create a stub web app that will eventually hold the final version; for now, you can use it to demonstrate and test the work listed below.

### Implement Scenes _Your_ Way
Implement your own version of a scene construct. You can be “inspired” by [three.js](https://threejs.org)’s scene but can build it your own way. Cherry-pick anything from the sample code as needed. This construct should:
* Take care of the setup that is needed for WebGL
* Provide add/remove functionality for objects in the scene
* Handle rendering of the scene onto a `canvas` element

### Define a 3D Object Construct
Design and implement a JavaScript construct for representing a 3D object (i.e., the thing that gets added to/removed from your scene). You may use classes or plain objects, but as mentioned we are leaving the design specifics up to you.
* The construct should accept a mesh data structure (see below) to define its shape
* The construct should have an approach for capturing/storing its visual/material characteristics—at a minimum, it should support a single solid color
* The construct should have a _composite_ mechanism—in other words, there should be a way to cluster/group 3D objects together

These features should all sound familiar—[three.js](https://threejs.org) has them! This time, though, you’re building them yourself. Oh yeah, have we already said that these design choices are up to you? 🧐

The group implementation should allow for arbitrary depth, and of course the drawing code should be able to handle this without a problem. Yes, you are implementing a tree. And now you know why the data structures course is a prerequisite to this one.

### Define a Polygon Mesh Data Structure
Design and implement a data structure for representing a _polygon mesh_. As we have seen, a polygon mesh captures a 3D object’s geometry. Again, you may use the ad hoc “proto-mesh” from the sample code as a basis for your version, but the design specifics are up to you. Your data structure should:
* Represent vertices and triangles
* Have a strategy for storing corresponding vertex data, like normals, colors, or texture coordinates (think custom geometry)
* Allow easy conversion into wireframe (`GL_LINES`) or solid (`GL_TRIANGLES`) rendering

### Define a “Mesh Maker” Library
> With extra credit opportunity—see below!

As a helper module for your 3D framework, build a library of “mesh maker” functions: i.e., functions that create different object shapes (analogous to [three.js](https://threejs.org)’s geometries). Once more, design choices are flexible—for instance, you may use the JavaScript prototype mechanism to create “subclasses.” Alternatively, you may prefer to take a factory approach (i.e., similar to what the sample code currently does in _shapes.js_). At a minimum for this assignment, implement the following 3D shapes:

* A _sphere_—this is such a satisfying, iconic polyhedron to code up from first principles that we want to make sure that every group does this. Choose appropriate parameters, such as the number of slices or rows
* As _individual work_, have each group member implement a shape of their own. Make sure to coordinate which shape each person is building! You can also look ahead to your eventual scene to decide which shapes you’d like to implement. Some shapes are more difficult than others; in particular, group members who tackle the following shapes will get extra credit for both themselves and the group:
    - An _extrude_ shape that creates a polygon mesh by extruding a given set of vertices
    - A _lathe_ or _revolve_ shape that creates a polygon mesh by rotating a given set of vertices about the _y_-axis
* Include a _shape-credits.md_ file that identifies the specific shape(s) for which each individual group member was responsible

Some additional ground rules apply for individual shapes, in order to keep the level of effort sufficiently equitable:
* Like the custom geometry from the earlier assignment, the individual shape must consist of at least six (6) non-coplanar triangles—in other words, it cannot be flat _with a single exception_: a [regular polygon](https://en.wikipedia.org/wiki/Regular_polygon) with a custom number of sides
* Some non-flat shapes also consist of fewer than six non-coplanar triangles: the tetrahedron and the pyramid, for example. If you pick these, you must implement _one more_ non-flat shape 
* The box/cube has been broken down so thoroughly in our course materials that the effort for that alone would be significantly less than for, say, a cylinder. Thus, individual students who opt for a box/cube _must also implement one more shape_

When building your mesh’s triangles, remember to adhere to the convention that the triangle’s vertices should be listed in _counterclockwise_ order when viewed from their “outside.” This will affect lighting when that gets implemented later on.

Unless you have a compelling reason to do otherwise, center your shapes about the origin. You can also orient and size them in a standard way (i.e., no need to specify parameters like rotations or dimensions). The [companion assignment](./matrix-library.md) will handle positioning (translation), rotation, and scaling.

Although this can be seen as a general-purpose library, feel free to pick your shapes in a way that will facilitate the eventual scene that you’d like to render. You don’t have to render that scene yet (there’s more to do before we can get there!) but you can look ahead to that when picking your shapes.

In summary, _n_-member groups will be implementing _n + 1_ or more shapes (sphere + _n_ or more individual). The likely “or more” would be from the individual student who implements shapes like a tetrahedron, pyramid, or box/cube, because they will need to implement one more on top of that.

### Extra Credit: Test Suite
Not all graphics code is easily testable, but some of it is. Courtesy of Vite, the repository is set up to run [Vitest](https://vitest.dev)-style tests, and some examples are provided in the playground sample code. Groups that implement a test suite will receive extra credit commensurate with the test suite’s coverage and quality.

## How to Turn it In
Commit the following to your repository:
- [Stub web app](#envision-a-scene)
  * Demonstrates the functionality listed below
  * Doesn’t have to be a specific scene or use case (yet)
- [Scene construct/framework](#implement-scenes-your-way)
  * Handles WebGL setup and connection to `canvas`
  * Can add/remove 3D objects as needed
  * Renders the objects on a `canvas` element
- [3D object framework](#define-a-3d-object-construct)
  * Can handle different shapes
  * Can store a single color, with room to expand later
  * Can represent composite objects (i.e., grouping)
- [Polygon mesh data structure](#define-a-polygon-mesh-data-structure)
  * Stores vertices and triangles
  * Room to eventually store normals, colors, or texture coordinates
  * Can produce wireframe or solid renders
- [“Mesh maker” library](#define-a-mesh-maker-library)
  * A sphere
  * At least one individual shape per group member (extra credit for extrusion or lathing functionality)
  * No need to position, rotate, or scale—see the [companion assignment](./matrix-library.md) for this
  * _shape-credits.md_ file documents who did what shape
- (extra credit) [Test suite](#extra-credit-test-suite) for any or all of these functionalities

Because you will be committing future assignments to this same repository, make sure to _tag_ the commit that represents this assignment like so:
```bash
$ git tag assignment-0330
$ git push --tags
```

## Specific Point Allocations
This assignment is scored according to outcomes _1b_, _1c_, _2b_, _3a_, _3c_, _3d_, and _4a_–_4f_ in the [syllabus](http://dondi.lmu.build/spring2026/cmsi3710/cmsi3710-spring2026-syllabus.pdf). For this particular assignment, graded categories are as follows:

| Category | Points | Outcomes |
| -------- | -----: | -------- |
| Stub web app demonstrates the library so far | 5 points | _3c_, _4a_–_4d_ |
| Scene construct/framework | 15 points total | _2b_, _3c_, _3d_, _4a_–_4d_ |
| • Handles WebGL setup and connection to `canvas` | 4 points | |
| • Can add/remove 3D objects as needed | 4 points | |
| • Renders the objects on a `canvas` element | 4 points | |
| • Implemented correctly and well | 3 points | |
| 3D object framework | 15 points total | _1c_, _3a_, _4a_–_4d_ |
| • Handles different shapes | 2 points | |
| • Stores at least a single color | 2 points | |
| • Can build group/composite trees | 8 points | |
| • Implemented correctly and well | 3 points | |
| Polygon mesh data structure | 25 points total | _1b_, _3a_, _4a_–_4d_ |
| • Stores vertices and triangles | 10 points | |
| • Design will accommodate future values | 5 points | |
| • Produces wireframe or solid renders | 5 points | |
| • Implemented correctly and well | 5 points | |
| “Mesh maker” library | 40 points total | _3a_, _4a_–_4d_ |
| • Sphere is implemented | 20 points | |
| • Individual shapes are implemented (two for team members who do the stated exceptions) | 10 points | |
| • Individual shape follows given specifications | deduction only if not fulfilled | |
| • _shape-credits.md_ lists individual shape assignments | deduction only if not fufilled | |
| • Implemented correctly and well | 10 points | |
| Test suite | extra credit | _4a_–_4d_ |
| Hard-to-maintain or error-prone code | deduction only | _4b_ |
| Hard-to-read code | deduction only | _4c_ |
| Version control | deduction only | _4e_ |
| Punctuality | deduction only | _4f_ |
| **Total** | **100** |

Note that inability to compile and run any code to begin with will negatively affect other criteria, because if we can’t run your code (or commands), we can’t evaluate related remaining items completely.
