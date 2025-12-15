# Cubic Bézier Curve Simulation

## Project Overview

This project shows an interactive cubic Bézier curve created using only HTML, CSS, and JavaScript. The curve is drawn on a canvas and changes its shape in real time when the user moves the mouse.

The curve is calculated manually using mathematical formulas instead of built-in functions. Two control points are attached to a small physics system that behaves like a spring, so the curve follows the mouse smoothly with a slight delay and bounce. This makes the movement look natural instead of rigid.

Overall, the project helps in understanding how Bézier curves work, how simple physics can be used for smooth motion, and how interactive graphics can be built using basic web technologies.
<img width="1781" height="651" alt="image" src="https://github.com/user-attachments/assets/03d9ecc1-6459-4c71-b0e2-4559decb0986" />


## Technical Architecture

The project is divided into a few small parts so the code stays simple and easy to understand. As we can see all parts:
**Vector Mathematics->**
This part handles basic 2D math like adding vectors, subtracting them, scaling, and finding direction or length. These operations are used everywhere in the project, especially for curve calculation and motion.
**Physics Engine->**
The physics system controls how the control points move. Each control point behaves like it is connected to a spring. Instead of jumping directly to the mouse position, it moves smoothly with delay and damping, which gives a natural elastic effect.

**Bézier Math->**
This section contains the formulas for calculating points on a cubic Bézier curve. For different values of t, it finds the exact position on the curve and also calculates the direction of the curve at that point.

**Input Handling->**
User input is taken from the mouse (or device motion if supported) and converted into target positions for the physics system. This allows the curve to react smoothly to user movement.

Together, these parts work as a simple pipeline:**( input → physics → math → drawing)**, which makes the project easy to maintain and understand.

## Module Details

**1. Vector Mathematics (V)**

Responsibilities:
-This module handles basic 2D vector operations used throughout the project, such as addition, subtraction, scaling, length, and normalization.

Key formulas:

Length: ∣v∣=x2+y2

Normalized vector: v^=∣v∣v​

Example operations provided:

```
V.add(a, b)      // returns a + b
V.sub(a, b)      // returns a - b
V.mul(a, s)      // scalar multiply
V.length(a)      // magnitude
V.normalize(a)   // unit vector
```

**2. Physics Engine (`createPhysicsNode`)**
Each control point behaves like a mass connected to a spring. Instead of moving instantly, the point smoothly follows a target position with damping.


Behavior per update (Key Formulas):

- Compute displacement: `x = pos - target`
- Compute spring force (Hooke's law): `F = -k * x`, where `k` is stiffness (typical value used: 0.05).
- Integrate acceleration into velocity: `vel += F * dt` (mass assumed 1 for simplicity).
- Apply damping: `vel *= damping` (typical damping: 0.91).
- Integrate velocity into position: `pos += vel * dt`.

The node exposes a `setTarget` method so input handlers can move the spring's anchor point each frame.

**3. Bézier Mathematics (BezierMath)**
The curve is calculated manually using the cubic Bézier equation for values of 
𝑡where(t from 0 to 1).
$$
B(t) = (1-t)^3P_0 + 3(1-t)^2tP_1 + 3(1-t)t^2P_2 + t^3P_3
$$
Derivative (tangent):
The first derivative, which gives the tangent (direction of motion) along the curve, is
$$
B'(t) = 3(1-t)^2(P_1 - P_0) + 6(1-t)t(P_2 - P_1) + 3t^2(P_3 - P_2)
$$

**4. Input Handling (Web)**
Mouse movement (or device orientation on supported devices) is used to update the target positions of the control points, which are then processed by the physics engine to create smooth and responsive motion.

## Rendering & Visuals

- Curve rendering: The curve is sampled at small increments (for example, `dt = 0.005`) and line segments are drawn between successive samples for a smooth appearance.
- Tangent visualization: At selected parameter values the derivative is computed, normalized, and drawn as a short line (often colored red) to indicate local direction.
- Control points: Each control point is rendered as a small filled circle. These are the visible handles driven by the physics nodes.

## Performance Considerations
- Here minimized allocations in the render loop: prefer in-place updates and reuse vectors when possible.
- Vector calculations are kept lightweight and reused where possible.
- The number of curve samples and tangent lines is limited to maintain smooth performance while keeping the visuals clear.

## Usage Instructions

1. Download and Open index.html in a modern desktop browser and move the mouse to interact.

2. Run on desktop: Open the file in Chrome, Firefox, or Safari. Move the mouse to interact; control points will follow with spring dynamics as visual.


## Implementation Notes
- The physics uses a fixed stiffness value (0.05) and damping (0.91) by default; these are tunable constants.
- For simplicity the physics nodes assume unit mass; if mass is required, divide acceleration accordingly when integrating.
- The Bézier math is intentionally separate from rendering so the same math utilities can be used for collision tests or path sampling outside the visualizer.

## References
-This project is based on standard cubic Bézier curve mathematics, Hooke’s law for spring motion, and basic Euclidean vector calculus.
1.Bézier curves: Bézier, P. (1972). Numerical Control: Mathematics and Applications.


