---
title: Projects
---


## Space-divison-based Geometry Compression

This is my current project that I am working on while attending Georgia Tech.

The main idea behind this concept is that, given a set of coordinates, we can create an `n x n` grid within
the extent of those coordinates. As `n` increases, there will be centers of grid cells that
tend to the initial set of coordinates. Once we have a "fine-enough" grid, we can create an
index over the grid (using [space-filling curves](https://en.wikipedia.org/wiki/Space-filling_curve)),
and take only the cells that align to a coordinate (or set of close coordinates) to form a 1D array of integers.
Since we have an array of integers (resembling an [inverted index](https://en.wikipedia.org/wiki/Inverted_index)),
we can use integer-specific compression techniques to reduce the size of this array.

Some of the work that I've done on this can be seen here: [spress](https://github.com/UFOKN/spress), [flatpoints](https://github.com/program--/flatpoints).

## GEOM
GEOM is a geometry and topology engine written in Fortran. It is an exploratory project at manipulating
geometry in [SoA](https://en.wikipedia.org/wiki/AoS_and_SoA#Structure_of_arrays) format,
as opposed to the traditional [AoS](https://en.wikipedia.org/wiki/AoS_and_SoA#Array_of_structures)
format represented by the [simple features](https://en.wikipedia.org/wiki/Simple_Features) standard.
The goal of this is to explore the ways that geometry operations can take advantage of SIMD instructions.

## rapture (Raster Capture)
[rapture](https://github.com/program--/rapture) is a tool written in Go for visualizing large point datasets as rasters.
It draws inspiration from similar tooling, such as [datashader](https://datashader.org) and [rasterly](https://github.com/plotly/rasterly).

## R Packages
I maintain various R packages. A (non-exhaustive) list of them is as follows:
- [fipio](https://github.com/UFOKN/fipio)
- [hilbert](https://github.com/program--/hilbert)
- [blaeu](https://github.com/program--/blaeu)
