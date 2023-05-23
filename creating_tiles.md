# Creating image tiles for deep learning applications
For the final project in my remote sensing class, we were assigned to find a dataset and use it to train and test different convolutional neural networks. There was just one problem - it's very difficult to find labelled datasets of image tiles for this purpose. Sure, there's EuroSat and BigEarthNet, but options are much more limited if you have a specific location in mind that isn't in the Global North, which is what my colleague Tiffany Tran and I were planning to do. Tiffany's doctoral research is on secondary cities in Indonesia, and we couldn't find any datasets to use.

That left creating our own. We found a satellite image of our chosen Indonesian city, Cirebon. I figured the steps to creating a dataset from this would be easily googleable. I was wrong. In my search, I couldn't find any instructions on how exactly to create my own labeled dataset of satellite image tiles. After much trial and error, I figured it out. And I share these steps with you below.

Goal: Create a dataset of satellite image tiles. These tiles will be divided into folders and labeled of the land use feature they contain. For this example, we will be dividing a satellite image of Cirebon, Indonesia into tiles labeled either "urban" or "non-urban" based on their associated land use.

Before we begin, you will need:
* **A high-quality satellite image.** We will be using a satellite image of Cirbon taken by Sentinel-2 and provided by [Copernicus](https://dataspace.copernicus.eu/). Select an image with very low cloud cover. Our selected image from August 19, 2022 has about 3% cloud cover. Make sure to download this image in the highest resolutation possible and as a georeferenced TIFF. For this example, we will only be using the RGB channels.

* **Shapefile(s) for labelling.** We will be using [NYU's Atlas of Urban Expansion](http://atlasofurbanexpansion.org/data)'s 2014 polygon of the estimated urban extent of Cirebon.

* Access to QGIS. [QGIS](https://www.qgis.org/en/site/) is a free and open source GIS software. These steps could likely be replicated in ArcGIS, although some steps may have different names or processes.

**Step 1: Create a Grid**
Open your satellite image and label shapefile into QGIS.

Before we create a grid, we need to know the pixel size of pixels in our satellite image. Open the properties of the satellite image by right-clicking on the layer and navigating to Properties > Information.

Under information, scroll down until you find the pixel size. Note down this number. For our satellite image, the pixel size is 0.0002658692000000030618,-0.0002641357688113412814.

<img_src="images/remote_sensing/step1_aa.png>

Create a grid by clicking on Vector > Research Tools > Create Grid.

<img_src="images/remote_sensing/step1_a.png>

In the pop up box, enter the following information:

Grid type: Rectangle (Polygon)

Grid Extent: Use the drop-down arrow to naviate to Calculate from Layer > select the satellite image layer.

Horizontal Spacing: Here's where the pixel size we previously noted comes in. The horizontal spacing is calculated as:

Horizontal Pixel Size (absolute value) * Height or Width of Tiles in Pixels = Horizontal spacing in decimal degrees

For our image, this is:

|0.0002658692000000030618| * 64 = 0.0170156288

Note, most models are designed to work with images 256 pixels square or more. Due to the limited resolution of our image, if we created tiles of this size, we would have very few tiles left to work with. We are therefore reducing our tiled images to 64 pixels square. This is a tradeoff: reducing the resolution of our image times may limit the accuracy of our later models. However, this will give us many more image tiles to work with.

Vertical Spacing: Same thing, but for the vertical pixel size:

Vertical Pixel Size (absolute value) * Height or Width of Tiles in Pixels = Vertical spacing in decimal degrees

For our image, this is:

|-0.0002641357688113412814| * 64 = 0.0169046892

Horiztonal Overlay: Leave as default, 0.00

Vertical Overlay: Leave as default, 0.00

Grid CRS: Leave as the default (CRS of the other layers, 4326)

Grid: If you want to save the grid as a layer for future use, use the drop-down arrow to navigate to Save to File. Otherwise, leave as default, Create temporary layer.

Click Run.

<img_src="images/remote_sensing/step1_b.png>

Adjust your grid's symbology so that it's fill is transparent so that you can see the satellite image underneath. Right click on the grid layer > Properties > Symbology and adjust.

<img_src="images/remote_sensing/step1_c.png>

Now we have a grid covering our satellite image and shapefile. In the next step, we'll select grid cells for our labelled tiles.

<img_src="images/remote_sensing/step1_d.png>

**Step 2: Select Grid Cells**
In this step, we will use masks to trim our grid down into the cells we want for our final tiles.

We will walk through three whittling steps. First, we'll trim our satellite image of areas that don't fit into the grid. Next, we'll work on removing clouds from our images. Finally, we'll extract out images that are within urban and non-urban areas.

*Part A: Fitting the Grid to our Satellite Image*
You may notice that our grid extends beyond the borders of our satellite image. This is no good. We can't use the edges of the satellite image that don't quite fill the grid. So, we will need to trim our satellite image of these extra cells.

<img_src="images/remote_sensing/step2_aa.png>
<img_src="images/remote_sensing/step2_a.png>

To do this, we will create a mask layer that will cover the cells we *do* want to keep.

To create a layer navigate to Layer > Create Layer > New Shapefile layer.
Set up the layer.

<img_src="images/remote_sensing/step2_b.png>

To draw the layer, click on the pencil to activate editing. Then click "Add Polygon Feature."
Drag to draw a large rectangle over the grid cells we do want to keep. Left-click to finish the shape.
It's okay if the rectangle doesn't completely line up with the grid's lines. Any grid cells that the the new layer touches will be kept, so draw accordingly.
Click the pencil again and save the changes to your mask layer.

<img_src="images/remote_sensing/step2_c.png>

Now we will identify grid cells that fall within our desired area. 

Open the attribute table of our mask layer. Open the field calculator, and create a field named is_mask. Give this field a value of 1.

Navigate to Vector > Data Management Tools > Join Attributes by Location.

Set up the following options:
Target layer: Select the grid
Join layer: Select the mask layer.
Join type: Select "Take attributed of first matching feature only"
Geometric predicate: Select 'Intersects'
Click Run.

You now have a new layer named Joined Layer. Take a look at its attribute table. Notice how some cells are labeled as is_mask = 1 while some are NULL? The NULL cells are those outside of our mask layer. We want to remove them.

Click the pencil to enter eidting mode. Next, select "Select features using an expression". Enter the following condition: "is_mask IS NULL". You have selected all the rows that have NULL for is_mask, i.e. the cells that fall outside of the mask. Select "Delete selected features."

Now your joined layer is trimmed to our desired extent. Click the pencil again to save the joined layer.

Now we will trim our satellite image. Navigate to Raster > Extraction > Clip Raster by Mask Layer.

Set up the clip.
Input layer: Select the satellite image.
Mask layer : Select the joined layer we just created and trimmed.
Output extent : Select "Mask Layer Extent"

Click run. You now have a new satellite image layer that is trimmed to the extent of the filled grid. You may need to turn off the original satellite image layer to see it.

Rename your new trimmed satellite image. You can remove your original satellite image.
Remove the original grid layer and grid mask layer.
Rename the joined layer to "Grid" and adjust its symbology to be like the original grid layer.

*Part B: Removing Clouds*
Using a similar process to part A, we're going to remove the clouds from our image by creating a mask layer of clouds and then removing parts of our satellite image that contain those clouds or their shadows.

To start with, we'll create a cloud mask layer. Navigate to Layer > Create Layer > New Shapefile layer.
Set up the layer. We will make this a point layer.

<img_src="images/remote_sensing/step2_cloud_a.png>

To draw the layer, click on the pencil to activate editing. Then click "Add Point Feature."
For each grid cell that contains a significant amount of cloud cover or cloud shadow, place one point on that grid cell. These will be the grid cells we do want to keep. In this method, making the decision for how much cloud/shadow cover to accept is a subjective one - use your best judgement. Generally, the less cloud cover the better.

Click the pencil again and save the changes to your mask layer.

<img_src="images/remote_sensing/step2_cloud_b.png>

Now we will identify grid cells that don't contain clouds or their shadows. 

Open the attribute table of our cloud layer. Open the field calculator, and create a field named is_cloud. Give this field a value of 1.

<img_src="images/remote_sensing/step2_cloud_c.png>

Navigate to Vector > Data Management Tools > Join Attributes by Location.

Set up the following options:
Target layer: Select the grid
Join layer: Select the cloud mask layer.
Join type: Select "Take attributed of first matching feature only"
Geometric predicate: Select 'Intersects'
Click Run.

<img_src="images/remote_sensing/step2_cloud_d.png>

You now have a new layer named Joined Layer. We want to remove cells whose value is_cloud = 1, and keep cells whose is_cloud value is NULL. Click the pencil to enter eidting mode. Next, select "Select features using an expression". Enter the following condition: "is_cloud IS 1". You have selected all the rows that have 1 for is_cloud, i.e. the cells that have clouds in them. Select "Delete selected features."

<img_src="images/remote_sensing/step2_cloud_d.png>

Now your joined layer has trimmed out grid cells with clouds. Click the pencil again to save the joined layer.

Before we trim out satellite image, we need to dissolve our joined layer into a layer with a single shape, rather than the multiple cells it is currently made up of. To do this, go to Vector > Geoprocessing Tools > Dissolve.

Now we have a new layer named Dissolve.

Now we will trim our satellite image. Navigate to Raster > Extraction > Clip Raster by Mask Layer.

Set up the clip.
Input layer: Select the satellite image.
Mask layer : Select the Dissolve layer we just created.
Output extent : Select "Mask Layer Extent"

Click run. You now have a new satellite image layer that is trimmed to exclude all the cells with clouds. You may need to turn off the original satellite image layer to see it.

Rename your new trimmed satellite image. You can remove your previous satellite image.

Before our next step, we will trim our grid to be made up of only cells without clouds.

Navigate to Vecotr > Geoprocessing Tools > Intersection.

<img_src="images/remote_sensing/step2_cloud_j.png>

Remove the original grid layer. Rename the intersection layer to "Grid" and adjust its symbology to be like the original grid layer.

<img_src="images/remote_sensing/step2_cloud_k.png>

*Part C: Selecting Urban and Non-Urban Tiles*
Now we want to select out grid cells that are within urban and non-urban areas, based on our labelled shapefile (in yellow in the image below). 

<img_src="images/remote_sensing/step2_urban_a.png>

Similar to our previous steps, we will use a mask layer to label areas as within the urban area, and then clip the satallite image to the extent od that mask. But luckily for us, we already have the mask layer ready - in our layered shapefile.

Frist, we create a varaible is_urban to label whether a cell is urban or not. Open the attribute table of our labeled shapefile (i.e. the Cirebon urban boundary polygon). Open the field calculator, and create a field named is_urban. Give this field a value of 1.

<img_src="images/remote_sensing/step2_urban_b.png>

Navigate to Vector > Data Management Tools > Join Attributes by Location.

Set up the following options:
Target layer: Select the grid
Join layer: Select the labeld shapefile (urban boundary polygon).
Join type: Select "Take attributed of first matching feature only"
Geometric predicate: Select 'Intersects'
Click Run.

You now have a new layer named Joined Layer. Duplicate this layer. Name one of the duplicates "urban_mask" and the other "non_urban_mask."

Now we will trim urban_mask to only include urban cells. Open urban_mask's attribute table. Click the pencil to enter editing mode. Next, select "Select features using an expression". Enter the following condition: "is_urban IS NULL". You have selected all the cells that do not contain the urban area. Select "Delete selected features." Click the pencil to exit out of editing mode.

Now you have a layer of only urban grid cells.

<img_src="images/remote_sensing/step2_urban_c.png>

Now we will do the opposide for non_urban_mask and trin it to only include non-urban cells. Open non_urban_mask's attribute table. Click the pencil to enter editing mode. Next, select "Select features using an expression". Enter the following condition: "is_urban IS 1". You have selected all the cells that contain the urban area. Select "Delete selected features." Click the pencil to exit out of editing mode.

Now you have a layer of only non-urban grid cells.

<img_src="images/remote_sensing/step2_urban_d.png>

In the next step, we will clip the satellite image to the urban and non-urban mask layers, and save these images as labeled tiles.

**Step 3: Label Tiles**
We will start by labelling and extracting only urban tiles.

First, we will clip our satellite image to the extent of the urban tiles. Navigate to Raster > Extraction > Clip Raster by Mask Layer.

Set up the clip.
Input layer: Select the satellite image.
Mask layer : Select the urban_mask layer we just created. *Note: This time, we will click the "iterate" button (two green arrows) to create a separate image tile for each grid cell.*
Output extent : Select "Mask Layer Extent"

<img_src="images/remote_sensing/step3_urban_a.png>

Click run. It may take a few seconds to run. You now have many new layers, one for each individual sattelite image, trimmed to a grid cell.Shift+Click to select all these new layers. Right-click and select "Group Selected". Minimize the group and rename them "urban_tiles."

<img_src="images/remote_sensing/step3_urban_b.png>
<img_src="images/remote_sensing/step3_urban_c.png>

Now we will export each of these urban tiles and label them accordingly.

Open the Python console. Copy in the following script, replacing with your parameters as appropriate:

```
import os
outfolder = r'path/to/folder/for/tiles' # path to folder you want to save images to
label = "urban" # label for the tiled images - in this case, "urban"
group_name = "urban_tiles" # name we gave the group of urban tiles

# Get the layer group by name
group = QgsProject.instance().layerTreeRoot().findGroup(group_name)

# Iterate over the layers in the group
n = 1

for layer in group.children():
    layer = layer.layer()
    extent = layer.extent()
    width, height = layer.width(), layer.height()
    renderer = layer.renderer()
    provider=layer.dataProvider()
    crs = layer.crs().toWkt()
    pipe = QgsRasterPipe()
    pipe.set(provider.clone())
    pipe.set(renderer.clone())
    out_file = os.path.join(outfolder, f"{label}_{n}.jpg")
    n = n+1
    file_writer = QgsRasterFileWriter(out_file)
    opts = ["COMPRESS=JPEG"]
    file_writer.setCreateOptions(opts)
    file_writer.writeRaster(pipe, width, height, extent, layer.crs())

print('Done')

```
Click run. Take a look at your folder - filled with labelled images! 

<img_src="images/remote_sensing/step3_urban_d.png>

We will repeat this process with the non-urban images.

First, we will clip our satellite image to the extent of the non-urban tiles. Navigate to Raster > Extraction > Clip Raster by Mask Layer.

Set up the clip.
Input layer: Select the satellite image.
Mask layer : Select the non_urban_mask layer we just created. *Note: We will click the "iterate" button (two green arrows) to create a separate image tile for each grid cell.*
Output extent : Select "Mask Layer Extent"

<img_src="images/remote_sensing/step3_nonurban_a.png>

Click run. It may take a while to run. You now have many new layers, one for each individual satellite image, trimmed to a grid cell. Shift+Click to select all these new layers. Right-click and select "Group Selected". Minimize the group and rename them "non_urban_tiles."

Now we will export each of these non-urban tiles and label them accordingly.

Open the Python console. Copy in the following script, replacing with your parameters as appropriate:

```
import os
outfolder = r'path/to/folder/for/tiles' # path to folder you want to save images to
label = "non_urban" # label for the tiled images - in this case, "non_urban"
group_name = "non_urban_tiles" # name we gave the group of tiles

# Get the layer group by name
group = QgsProject.instance().layerTreeRoot().findGroup(group_name)

# Iterate over the layers in the group
n = 1

for layer in group.children():
    layer = layer.layer()
    extent = layer.extent()
    width, height = layer.width(), layer.height()
    renderer = layer.renderer()
    provider=layer.dataProvider()
    crs = layer.crs().toWkt()
    pipe = QgsRasterPipe()
    pipe.set(provider.clone())
    pipe.set(renderer.clone())
    out_file = os.path.join(outfolder, f"{label}_{n}.jpg")
    n = n+1
    file_writer = QgsRasterFileWriter(out_file)
    opts = ["COMPRESS=JPEG"]
    file_writer.setCreateOptions(opts)
    file_writer.writeRaster(pipe, width, height, extent, layer.crs())

print('Done')

```
Click run. Take a look at your folder - filled with labelled images! 

<img_src="images/remote_sensing/step3_nonurban_b.png>
