---
layout: post
title: "Surface plots of image data in GNU R"
date: 2006-02-07 13:48
comments: true
categories: 
published: false
---

I needed to plot the intensity profile of some test images for my thesis.  It's really useful to be able to visualise 2D grayscale image data by treating the intensity as a height-field and displaying it in 3D.  There's a variety of ways to do this, but I wanted something that would produce good printed results, so EPS was the best output option.

So here's a recipe for using [GNU R](http://www.r-project.org/) to produce such a plot.

First, generate your image data and crop it appropriately.  For example, I used [ITK](http://www.itk.org/) to generate a Gaussian field.

Then convert it to the `pgm` file format.  The most excellent [ImageMagick](http://www.imagemagick.org/) tools make this a snap (note the '%' is the shell prompt):

    % convert -compress none inputfile.tif inputfile.pgm

The `compress` option ensures the PGM file is written in ASCII format, for easy reading in.

Then, fire up R and do the following (note the '>' character is the default R prompt):

    > sz <- scan("gaussian.pgm", what=integer(0), skip=1, comment.char="#", nmax=2)
    Read 2 items
    > g <- scan("gaussian.pgm", what=integer(0), skip=3, comment.char="#")
    Read 65536 items
    > gi <- matrix(g, sz[1], sz[2])
    > rm(g)

The first line reads the image size from the PGM header into the variable <tt>sz</tt>, the second reads in the actual data as an array.  The third line creates a matrix from the data, with the appropriate size.  The last line removes the temporary array data.

Now you have a matrix object with your image data in it, you can do all sorts of groovy things.  Such as:

    > image(gi)
    > contour(gi, add=TRUE)

[img_assist|fid=13|thumb=1|alt=Gaussian 2D Image Contour plot]

will display it as an image, with a scalar mapping function (which you can customize), along with contour lines showing the isolevels.  But more interesting is this:

    > persp(gi, theta=25, phi=30)

This will produce a 3D contour plot in perspective (the theta and phi parameters rotate the viewing angle).

[img_assist|fid=16|thumb=1|alt=Gaussian 2D Image Perspective plot]

You can also do other handy stuff, like pick out a scanline and do a 2D intensity plot (or a series).
