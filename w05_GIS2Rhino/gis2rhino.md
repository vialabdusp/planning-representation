# 3D model - 1/3 GIS to Rhino
[Eric Huntley](@ehuntley) and [Yael Nidam](@yaelnidam)

![stages](./images/Stages.jpg)

This set of three tutorials will utilize GIS data to create a 3D model in rhino and final graphic editing in Illustrator.

### File for This Exercise
[Download the GIS file for this exercise here.](http://web.mit.edu/ehuntley/Public/rhino_workshop.zip)

## ArcMap Setup
Open ArcMap and upload the following layers:
1. contours_clip
2. Somerville_bldg
3. Somerville_sidewalk
4. Somerville_openSpace
5. Study_area_05

** The contour layer was derived from a Digital Elevation Model (DEM), which produces clean and continues contour lines. This method is preferred to the overly detailed contour lines available through municipalities' websites, that are usually not continues or closed polygons.

## Generate topography polygons

### 1. Create line feature from the study area box.
Data Management Tools -> Features -> Feature to Line

Input Features: Study_area_05
Output Feature Class: ~/Box_Line

### 2. Merge box line with contours
Select Contours.
Data Management Tools -> General -> Append

Input Datasets: Box_Line
Target Dataset: contours_clip
Schema Type: NO_TEST (IMPORTANT!!!!)

### 3 .Convert merged box/contours into a polygon layer.
Data Management Tools -> Features -> Feature to Polygon

Input Features: Contours
Output Feature Class: ~/ContoursPolygon

### 4 . Spatially join polygons with contours to obtain elevations
- Right-click ContoursPolygon layer in Table of Contents.
- Joins and Relates -> Join

What do you want to join to this layer?
Join data from another layer based on spatial location.

- Select Contours as the layer to be joined.
- Select "Each polygon will be given a summary..."
- Check the "Maximum" box.

Output: ~/ContoursPolygon_Elev

Congrats! you've successfully created topography polygons.

### 5. Add elevation field to ContoursPolygon_Elev attribute table
This step creates a new field called Elevation in the attribute table and populates it with the values recieved from the max function earlier. This step is needed for 2 reasons: 1. Clarity. 2. Rhino needs this field to read the polygons' elevation.

- Right-click ContoursPolygon_Elev layer in table of Content
- Open attribute table -> Add Field -> Field name: Elevation
- Right-click on Elevation field -> Field Calculator
- Elevation = [Max_Contou]

## Based on topography, Add elevations to building footprints

### 1. Join the building footprints with the elevation data
- Right-click Building Footprints layer in Table of Contents.
- Joins and Relates -> Join

What do you want to join to this layer?
Join data from another layer based on spatial location.

- Select ContoursPolygon_Elev as the layer to be joined.
- Select "Each polygon will be given a summary..."
- Check the "Minimum" box.

Output: ~/BuildingFootprints_Contours

### 2. Add elevation to building height.
Open the attribute table for BuildingFootprints_Contours

- Create new field called "Elevation"
- Right click the field name and open the field calculator.
- Elevation = 30 + [Min_Elevat]

### 3. Check to make sure all fields you will be exporting to CAD include an elevation field.

## Export to CAD
### 1. Select layers to export
In Table of Contents Layers window, right click the ContoursPolygon_Elev layer: Data -> Export to CAD

### 2. Export settings
Input feature: select BuildingFootprints_Contours to add it for CAD export. Create a new folder to save this file as an output from GIS. 

