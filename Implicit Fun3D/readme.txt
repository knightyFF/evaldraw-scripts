//////////////////////////////////////////////////////////////////////////////////////////////////
// Implicit surfaces viewer 
// By Abdelaziz Nait Merzouk (knight_aziz@hotmail.com)
// Nov, Dec 2008
// 
// TERMES OF USE
// 1- No Warranty: Use at your own risk.
// 2- You can distribute this file to anyone you wish.
// 3- Enjoy.
//
//Description begin///////////////////////////////////////////////////////////////////////////////
//  This is a simple raytracer for implicit surfaces defined by a 3D scalar fonction.
//  The only thing new here is the root finding algorithm. It is an adaptive "marching point" 
// algorithm. It is not robust as AA/IA & bezier/B-spline techniques but gives very good results
// and fails seldom (depending on the degree of the implicit and the settings of the root finder). 
//  The rmaining is classic andcan be found in the litterature.
//  The traversal algorithm works just fine for C2 surfaces (that is, functions that are continuous
// and their first derivative too).
//  There may be holes especially when using min and max (for CSG) one solution
// for this is scaling "correctly" the functions: the function that have the lowest order should be
// multiplied by an appropriate value.(to verify!)
// 
// References: 
// - Jag Mohan Singh and P.J. Narayanan : "Real-Time Ray-Tracing of Impicit Surfaces on the GPU"
//   Tech. Rep. IIIT/TR/2007/72. July 2007.
// - John C. Hart. Sphere tracing: "A geometric method for the antialiased ray tracing of implicit surfaces"
//   The Visual Computer, 12(10):527–545, 1996.
// - Aaron Knoll, Younis Hijazi, Charles D Hansen, Ingo Wald, and Hans Hagen : "Interactive ray
//   tracing of arbitrary implicit functions" In Proceedings of the 2007 Eurographics/IEEE Symposium
//   on Interactive Ray Tracing, 2007.
//
// Links for surfaces' definitions & formulas:
// - http://www.singsurf.org/singsurf/SingSurf.html
// - http://www.oliverlabs.net/suse/index.html
// - http://enriques.mathematik.uni-mainz.de/docs/Eflaechen.shtml
// - http://www.oliverlabs.net/welcome.php
// - http://www.freigeist.cc/gallery.html
// - http://mathworld.wolfram.com/AlgebraicSurface.html
// - http://local.wasp.uwa.edu.au/~pbourke/geometry/
//Description end/////////////////////////////////////////////////////////////////////////////////
//
//Usage begin/////////////////////////////////////////////////////////////////////////////////////
// runs under evaldraw (http://advsys.net/ken/download.htm)
// (assuming azerty keyboard)
// Traversal algorithm tweeks:
//  The default settings works fine in general. If you notice holes or anormal things you can
// change the parameters:
//  - Hold "T" and press "+" or "-" to increase/decrease prediction step multiplier.
//  - Hold "M" and press "+" or "-" to increase/decrease initial step length.
//  - Hold "I" and press "+" or "-" to increase/decrease max number of iterations.
//  - Hold "E" and press "+" or "-" to increase/decrease max error
//  - Press "C" to change scaling of the function by using a log() function. This reduces the number
//    of iterations but may leed the root finder to fail.
//  - Press "D" to see the number of iterations (given by the third value)
//  - Press "B" to change the bounding volume box/sphere (box by default).
//  - Hold right [shift] and move the mouse to change bounding sphere/box size.
// Other controls:
//   Navigation:
//    - Hold left mouse button and move the mouse to pan.
//    - Hold right mouse button and move the mouse to zoom.
//    - Hold left [shift] and move the mouse to turn around (scroll wheel to change distance).
//    - Hold left [ctrl] and move mouse to turn light spot (around 0,0,0) (scroll wheel change dist).
//    - You can render the light spot by setting showlight (see the enum below) to 1.
//   Raytracer params:
//    - Press "S" to show/hide shadows.
//    - Hold "R" and press "+"/"-" to increase/decrease the number of reflexions.
//    - Press "L" to change coloring scheme.
//   Parameters: There are five free parameters for use with the implicit functions. Hold on of
//  (non keypad) keys "1"..."5" and move the mouse. There is also a "time" parameter (in seconds).
//
// Tips:
//  Use 2x2 reder resolution (2D reder resolution in options menu or use F7/shift F7)
//  Use multithreading (F5). drawbacks: -the screen may pop. -The script is automatically recompiled.
//  If the rendering is too slow you can zoom out, change parameters then zoom in
//
//Usage end///////////////////////////////////////////////////////////////////////////////////////
//
//Credits :
// - Ken Silverman (http://advsys.net/ken) who made evaldraw!
// - counting_pine (from JonoF's forum "http://www.jonof.id.au/forum/") for the idea of raytracing 
//   with evaldraw in (x,y,&r,&g,&b) mode.
// - taby (from gamedev.net forums) who suggested to me the dot product colouring scheme.
//////////////////////////////////////////////////////////////////////////////////////////////////