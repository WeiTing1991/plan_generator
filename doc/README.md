# GOGOGO

This repo is to make a curve/points to a custom json format for this project
it will be within in rhino and grasshopper CAD environment.


## Getting Started

### Setup 1
try to figure out how to make a function that in src folder .py
and then import it in grasshopper (I kind of forgot but please search of it)
>[!NOTE]:
> use rhino 7 and try to use the latest/modern way and start with a simple points.

- make a rhino file and grasshopper file in `sciprt` folder
- put a simple .py in `src` folder.
- make a simple grasshopper python component and try to import the function from `src` folder.

Please find this [lib](https://docs.python.org/3/library/pathlib.html) for the path handling.

Some references:
- [1](https://discourse.mcneel.com/t/importing-and-using-a-custom-grasshopper-component-in-rhino-python/167200/3)
- [2](https://discourse.mcneel.com/t/import-file-in-python/177564)

- [how to do error handling in python](https://www.geeksforgeeks.org/python-exception-handling/)

>[!IMPORTANT]:
Those i just quick search and I will try to find more. also if you have any just put it into markdown.

### Setup 2
make a simple function store grasshopper points in the python dict.
...

### Setup 3
make a simple function store grasshopper points in the python dict.
...

### Setup 5

In the end, I will like to have a data structure that looks like this:
Please find this [here](https://www.geeksforgeeks.org/how-to-convert-python-dictionary-to-json/)

The data structure:

```python
# type hinting
output_data = { INT: 
               { "point": List[float],
                 "axis" :{ "x": List[float], "y": List[float]}, },
```

```json

// now for frame sytle, in the future we do Quaternions.

{
  "0": {
    "point": [0.0, 0.0, 0.0 ],
    "axis" :{ "x": [0.0, 0.0, 0.0], "y": [0.0, 0.0, 0.0]},
  },
  "1": {
    "point": { "x": 0.0, "y": 0.0, "z": 0.0 },
    "axis" :{ "x": [0.0, 0.0, 0.0], "y": [0.0, 0.0, 0.0]},
  },

  ...

}

```

### Setup 10
robot motion simulation
maybe with compas_fab or other lib.


```
some NOTE:

1 If (x, y, z, w) is a Quaternion
In computer graphics, robotics, and game engines, quaternions 
( 𝑥 , 𝑦 , 𝑧 , 𝑤)
(x,y,z,w) represent rotations in 3D space.

(x, y, z, w) is a unit quaternion that defines an orientation.
The first three zeros 
[ 0 , 0 , 0 ]
[0,0,0] might be padding or unused components.
🔹 Example in Game Engines (Unity, Unreal, etc.):

Quaternion(x, y, z, w) = rotation
Vector3(x, y, z) = position
2 If (x, y, z, w) is a 4D Homogeneous Point
Another interpretation is that (x, y, z, w) represents a 4D point in homogeneous coordinates used in 3D transformations:

If w = 1, it’s a position (a physical point in space).
If w = 0, it’s a direction (a vector, not a position).
The first three zeros [0,0,0] could be empty values or an unused dimension.
🔹 Example in 3D Graphics (OpenGL, Blender, etc.):

P = (x, y, z, w=1) = A point in 3D space
V = (x, y, z, w=0) = A direction vector

Extract Rotation Axis & Angle from Quaternion
A quaternion represents a rotation in 3D space:

𝑄 = ( 𝑥 , 𝑦 , 𝑧 , 𝑤)
Q=(x,y,z,w)
where:

( 𝑥 , 𝑦 , 𝑧)
(x,y,z) defines the axis of rotation, but it needs normalization.
𝑤
w is related to the rotation angle.
Formula to extract axis and angle:
Rotation angle (θ in radians):
𝜃 = 2 ⋅ arccos � ( 𝑤) θ=2⋅arccos(w) 
Rotation axis (unit vector): axis = ( 𝑥 , 𝑦 , 𝑧) sin � ( 𝜃 / 2) axis= sin(θ/2) (x,y,z) � (Only if sin � ( 𝜃 / 2) ≠ 0 sin(θ/2)  =0 to avoid division by zero)

import numpy as np

def quaternion_to_axis_angle(q):
    x, y, z, w = q
    angle = 2 * np.arccos(w)  # Rotation angle in radians
    sin_half_theta = np.sqrt(1 - w**2)  # sin(theta/2)
    
    if sin_half_theta < 1e-6:  # Avoid division by zero (small rotation case)
        axis = (1, 0, 0)  # Default axis if angle is near 0
    else:
        axis = (x / sin_half_theta, y / sin_half_theta, z / sin_half_theta)

    return axis, np.degrees(angle)  # Convert angle to degrees for readability

# Example quaternion (rotation by 90 degrees around Y-axis)
q = (0, 1, 0, np.cos(np.radians(45)))  # 90-degree rotation

axis, angle = quaternion_to_axis_angle(q)
print(f"Rotation Axis: {axis}")
print(f"Rotation Angle: {angle} degrees")
```
