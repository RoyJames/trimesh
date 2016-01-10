# trimesh #
[![Build Status](https://travis-ci.org/mikedh/trimesh.svg?branch=master)](https://travis-ci.org/mikedh/trimesh)

Python (2.7-3.*) library for loading and utilizing triangular meshes. The goal of the library is to provide a fully featured Trimesh object which allows for easy manipulation and analysis, in the style of the excellent Polygon object in the Shapely library.

### Features ###
* Import binary/ASCII STL, Wavefront, and OFF
* Import formats using assimp (if pyassimp installed)
* Import STEP files as meshes (if STEPtools Inc. Author Tools installed)
* Import 2D or 3D vector paths from DXF or SVG files
* Export meshes as binary STL, COLLADA, or OFF
* Preview meshes (requires pyglet)
* Internal caching of computed values which are automatically cleared when vertices or faces are changed
* Fast loading of binary and ASCII STL files (on 234,230 face mesh, was 24.5x faster than assimp)
* Calculate face adjacencies quickly (for the same 234,230 face mesh .248 s)
* Calculate cross sections (.146 s)
* Split mesh based on face connectivity using networkx (4.96 s) or graph-tool (.584 s)
* Calculate mass properties, including volume, center of mass, and moment of inertia (.246 s)
* Find coplanar groups of faces (.454 s)
* Fix triangle winding to be consistent 
* Fix normals to be oriented 'outwards' using ray tests
* Calculate whether or not a point lies inside a watertight mesh using ray tests
* Find convex hulls of meshes (.21 s)
* Compute a rotation/translation/tessellation invariant identifier for meshes (from an FFT of the radius distribution)
* Merge duplicate meshes from identifier
* Determine if a mesh is watertight (manifold)
* Repair single triangle and single quad holes
* Uniformly sample the surface of a mesh
* Find ray-mesh intersections
* Boolean operations on meshes (intersection, union, difference) if OpenSCAD is installed
* Voxelize watertight meshes
* Unit conversions
* Create meshes by extruding 2D profiles
* Numerous utility functions, such as transforming points, unitizing vectors, grouping rows, etc. 

### Installation ###

The easiest way to install is:
```bash
$ sudo pip install trimesh
```

### Optional Dependencies ###

Basic functionality is available immediately. Some functions (ray queries, polygon handling, mesh creation, viewer windows, boolean operations, additional importers) require additional libraries:
```bash
$ sudo apt-get install cmake openscad libspatialindex* 
$ sudo pip install pyglet shapely git+https://github.com/robotics/assimp_latest.git git+https://github.com/Toblerity/rtree.git svg.path meshpy
```

### Quick Start ###

Here is an example of loading a cube from file and colorizing its faces.

```python
import numpy as np
import trimesh

# load a file by name or from a buffer
mesh = trimesh.load_mesh('./models/featuretype.STL')

# is the current mesh watertight?
print(mesh.is_watertight)

# since the mesh is in fact watertight, it means there is a 
# volumetric center of mass which we can set as the origin for our mesh
mesh.vertices -= mesh.center_mass

# find groups of coplanar adjacent faces
facets, facets_area = mesh.facets(return_area=True)

# set each facet to a random color
for facet in facets:
    mesh.visual.face_colors[facet] = trimesh.color.random_color()

# preview mesh in an opengl window if you installed pyglet with pip 
mesh.show()
```

In the mesh view window, dragging rotates the view, ctl + drag pans, mouse wheel scrolls, 'z' returns to the base view, 'w' toggles wireframe mode, and 'c' toggles backface culling.
