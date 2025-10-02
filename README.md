# PBR-GS

Physically Based Rendering of Gaussian Splats

## PBR GS PLY File Format Spec V1

Prepared 2025-07-29  
Written By: Andrew Hazelden <andrew@andrewhazelden.com>  
Developed in collaboration with [Eric Paré](eric@xangle.team) and [Didier Muanza](didier.muanza@gmail.com)  

### Overview

The Kartaverse development team has explored various workflows available to extend the existing 3DGS PLY container format. The primary goal is to add PBR shading attributes to support interactive relighting while preserving backwards compatibility with existing 3D Gaussian Splatting training / rendering workflows and tools.

The most promising idea comes from Ben Houston's paper "[PBR extensions to Wavefront OBJ/MTL](https://benhouston3d.com/blog/extended-wavefront-obj-mtl-for-pbr)". We reached out to talk with Ben on 2025-07-29 and got his approval for this PBR GS concept to be scaffolded on-top of his core concepts and naming conventions. Thanks!

### Further Reading Materials

The Library of Congress has a document about the [Wavefront MTL file format](https://www.loc.gov/preservation/digital/formats/fdd/fdd000508.shtml). Check out Wikipedia for insights into the core specifications of the [ply file format](https://en.wikipedia.org/wiki/PLY_(file_format)). Also look at the PDF version of Greg Turk's [The PLY Polygon File Format](https://gamma.cs.unc.edu/POWERPLANT/papers/ply.pdf) document.

### Timecode Addition

To allow sidecar files like audio tracks to sync up with 4DGS sequences, it is useful to add an SMPTE formatted timecode ("00:00:00:00") attribute to the PLY file. One could also include a frame number element if it is useful.

```
property char timecode[11]
```

### PBR Additions

If we add the following PBR pass channels to the PLY file specification we can support fully interactive volumetric relighting of 3DGS assets and scenes:

```
aniso = Anisotropy
anisor = Anisotropy rotation
Ke = Emissive
Pc = Clearcoat thickness
Pcr = Clearcoat roughness
Pm = Metallic
Pr = Roughness
Ps = Sheen
```

It would be feasible to also store the ambient occlusion data that is used in a directX style "rmo" attribute as well.

The standard "f_dc" PLY channels hold the RGB image data on each point-sample:

```
f_dc_0 = red color
f_dc_1 = green color
f_dc_2 = blue color
```

The standard "nx/ny/nz" PLY channels hold the "norm" Normal map (RGB components) on each point sample:

```
nx = norm map red channel
ny = norm map green channel
nz = norm map blue channel
```

### PBR PLY Properties

When these PBR extension for 3DGS ideas are applied, it results in a PBR compatible 3DGS PLY file with the following header properties:
```
ply
format binary_little_endian 1.0
property char timecode[11]
element vertex 3000000
property float x
property float y
property float z
property float nx
property float ny
property float nz
property float aniso
property float anisor
property float ke
property float pc
property float pcr
property float pm
property float pr
property float ps
property float f_dc_0
property float f_dc_1
property float f_dc_2
property float f_rest_0
property float f_rest_1
property float f_rest_2
property float f_rest_3
property float f_rest_4
property float f_rest_5
property float f_rest_6
property float f_rest_7
property float f_rest_8
property float f_rest_9
property float f_rest_10
property float f_rest_11
property float f_rest_12
property float f_rest_13
property float f_rest_14
property float f_rest_15
property float f_rest_16
property float f_rest_17
property float f_rest_18
property float f_rest_19
property float f_rest_20
property float f_rest_21
property float f_rest_22
property float f_rest_23
property float f_rest_24
property float f_rest_25
property float f_rest_26
property float f_rest_27
property float f_rest_28
property float f_rest_29
property float f_rest_30
property float f_rest_31
property float f_rest_32
property float f_rest_33
property float f_rest_34
property float f_rest_35
property float f_rest_36
property float f_rest_37
property float f_rest_38
property float f_rest_39
property float f_rest_40
property float f_rest_41
property float f_rest_42
property float f_rest_43
property float f_rest_44
property float opacity
property float scale_0
property float scale_1
property float scale_2
property float rot_0
property float rot_1
property float rot_2
property float rot_3
end_header
```
