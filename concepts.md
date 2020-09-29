Concepts
===

The Python Imaging Library handles raster images; that is, rectangles of pixel data.

# Bands
An image can consist of one or more bands of data. The Python Imaging Library allows you to store several bands in a single image, provided they all have the same dimensions and depth.

To get the number and names of bands in an image, use the getbands method.

# Mode
The mode of an image defines the type and depth of a pixel in the image. The current release supports the following standard modes:

* **1** (1-bit pixels, black and white, stored with one pixel per byte)

* **L** (8-bit pixels, black and white)

* **P** (8-bit pixels, mapped to any other mode using a colour palette)

* **RGB** (3x8-bit pixels, true colour)

* **RGBA** (4x8-bit pixels, true colour with transparency mask)

* **CMYK** (4x8-bit pixels, colour separation)

* **YCbCr** (3x8-bit pixels, colour video format)

* **I** (32-bit signed integer pixels)

* **F** (32-bit floating point pixels)

PIL also provides limited support for a few special modes, including **LA** (L with alpha), **RGBX** (true colour with padding) and **RGBa** (true colour with premultiplied alpha). However, PIL doesn’t support user-defined modes; if you to handle band combinations that are not listed above, use a sequence of Image objects.

You can read the mode of an image through the [mode]() attribute. This is a string containing one of the above values.

# Size
You can read the image size through the size attribute. This is a 2-tuple, containing the horizontal and vertical size in pixels.

# Coordinate System
The Python Imaging Library uses a Cartesian pixel coordinate system, with (0,0) in the upper left corner. Note that the coordinates refer to the implied pixel corners; the centre of a pixel addressed as (0, 0) actually lies at (0.5, 0.5).

> FIXME: Add illustration!

Coordinates are usually passed to the library as 2-tuples (x, y). Rectangles are represented as 4-tuples, with the upper left corner given first. For example, a rectangle covering all of an 800x600 pixel image is written as (0, 0, 800, 600).

# Palette
The palette mode (“P”) uses a colour palette to define the actual colour for each pixel.

# Info
You can attach auxiliary information to an image using the [info]() attribute. This is a dictionary object.

How such information is handled when loading and saving image files is up to the file format handler (see the chapter on [*Image File Formats*]()). Most handlers add properties to the **info** attribute when loading an image, but ignore it when saving images.

# Filters
For geometry operations that may map multiple input pixels to a single output pixel, the Python Imaging Library provides four different resampling filters.

## NEAREST
Pick the nearest pixel from the input image. Ignore all other input pixels.

## BILINEAR
Use linear interpolation over a 2x2 environment in the input image. Note that in the current version of PIL, this filter uses a fixed input environment when downsampling.

## BICUBIC
Use cubic interpolation over a 4x4 environment in the input image. Note that in the current version of PIL, this filter uses a fixed input environment when downsampling.

## ANTIALIAS
(New in PIL 1.1.3). Calculate the output pixel value using a high-quality resampling filter (a truncated sinc) on all pixels that may contribute to the output value. In the current version of PIL, this filter can only be used with the resize and thumbnail methods.

Note that in the current version of PIL, the ANTIALIAS filter is the only filter that behaves properly when downsampling (that is, when converting a large image to a small one). The BILINEAR and BICUBIC filters use a fixed input environment, and are best used for scale-preserving geometric transforms and upsamping.