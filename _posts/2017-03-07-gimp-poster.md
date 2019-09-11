---
layout: post
title: A GIMP Python plugin that auto-generates a poster with any 5 images
---
#!/usr/bin/env python

# GIMP Python
# This plugin will create a A4 size, 300dpi poster with 5 images and some texts.

import os
from gimpfu import *

def poster(file0, file1, file2, file3, file4, font1, font2, savePath, posterName):

# create an empty image
posterWidth = 2480
posterHeight = 3508
width = 1000
height = 1000
bgHeight = 960
posterImage = gimp.Image(posterWidth, posterHeight, RGB)
gimp.Display(posterImage)

# load background image and the 4 images related with theme
bg1 = pdb.file_jpeg_load(file0, file0)
image0 = pdb.file_jpeg_load(file1, file1)
image1 = pdb.file_jpeg_load(file2, file2)
image2 = pdb.file_jpeg_load(file3, file3)
image3 = pdb.file_jpeg_load(file4, file4)
image4 = image3

# resize the 4 images
pdb.gimp_image_resize(bg1, posterWidth, bgHeight, 0, 0)
pdb.gimp_image_resize(image0, width, height, 0, 0)
pdb.gimp_image_resize(image1, width, height, 0, 0)
pdb.gimp_image_resize(image2, width, height, 0, 0)
pdb.gimp_image_resize(image3, width, height, 0, 0)

# create background layer and fill it with white
layerbg = gimp.Layer(posterImage, “Layer Back”, posterWidth, posterHeight, RGB_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layerbg, 0)
gimp.set_background(255, 255, 255)
pdb.gimp_edit_fill(layerbg, BACKGROUND_FILL)
#pdb.gimp_edit_fill(layerbg, WHITE_FILL)

# make background blurred image layer for title
radius = 50.0
layerbg1 = gimp.Layer(posterImage, “Title Background”, posterWidth, posterHeight/3, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layerbg1, 0)
pdb.gimp_edit_copy(bg1.layers[0])
layerbg1.update(0, 0, posterWidth, bgHeight)
layerbg1.translate(0,-120)
layerbg1 = pdb.gimp_edit_paste(layerbg1, True)
pdb.plug_in_gauss( bg1, layerbg1, radius, radius, 1)

# make background blurred image layer for reasons
layerbg2 = gimp.Layer(posterImage, “Reason Background”, posterWidth, posterHeight/3, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layerbg2, 0)
pdb.gimp_edit_copy(bg1.layers[0])
layerbg2.update(0, 0, posterWidth, bgHeight)
layerbg2.translate(0,posterHeight-bgHeight)
layerbg2 = pdb.gimp_edit_paste(layerbg2, True)
layerbg2 = pdb.gimp_item_transform_flip_simple (layerbg2, 1, True, 500)
layerbg2.translate(0,490)
pdb.plug_in_gauss( bg1, layerbg2, radius, radius, 1)
#layerbg2 = pdb.gimp_image_flip(layerbg2, 1.0, 500.0, 2000.0, 500.0)
#layerbg2 = pdb.gimp_item_transform_flip(layerbg2, ORIENTATION_HORIZONTAL)

# add title
font = ‘Impact’
size = 206
str1 = “WHY AUTUMN IS THE BEST”
str2 = “SEASON”
str3 = “It’s colorful. It’s a season of joy”
gimp.set_foreground((255, 255, 255)) # set color to white
layertxt1 = pdb.gimp_text_fontname(posterImage, None, 240, 240, str1, 10, True, size, PIXELS, font)
layertxt2 = pdb.gimp_text_fontname(posterImage, None, posterWidth/3+110, 480, str2, 10, True, size, PIXELS, font)
layertxt3 = pdb.gimp_text_fontname(posterImage, None, posterWidth/3-220, posterHeight-240, str3, 10, True, size/2, PIXELS, font)
# add content texts
str4 = “Blooming”
str5 = “Passionate”
str6 = “Harvest”
str7 = “Cosy”
font2 = ‘Agency FB’
gimp.set_foreground((50, 50, 50)) # set color to dark grey
layerstr4 = pdb.gimp_text_fontname(posterImage, None, 320, 2440, str4, 2, True, size/2, PIXELS, font2)
layerstr5 = pdb.gimp_text_fontname(posterImage, None, posterWidth/3-220, posterHeight-670, str5, 2, True, size/2, PIXELS, font2)
layerstr6 = pdb.gimp_text_fontname(posterImage, None, 1600, posterHeight-670, str6, 2, True, size/2, PIXELS, font2)
layerstr7 = pdb.gimp_text_fontname(posterImage, None, posterWidth-505, 2440, str7, 2, True, size/2, PIXELS, font2)

# make layer 0
gimp.set_foreground((255, 255, 255))
angle = -3.14/4
posx0, posy0 = 250, 1562
layer0 = gimp.Layer(posterImage, “Layer 0”, width, height, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layer0, 0)
pdb.gimp_edit_copy(image0.layers[0])# layers are all layers, but jpg only has one layer 0
layer0.update(0, 0, width, height)
layer0.translate(posx0,posy0)
layer0 = pdb.gimp_edit_paste(layer0, True)
layer0 = pdb.gimp_item_transform_rotate(layer0, angle, True, 0, 0)

# make layer 1
layer1 = gimp.Layer(posterImage, “Layer 1”, width, height, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layer1, 0)
pdb.gimp_edit_copy(image1.layers[0])
layer1.update(0, 0, width, height)
layer1.translate(posx0+500,posy0-500)
layer1 = pdb.gimp_edit_paste(layer1, True)
layer1 = pdb.gimp_item_transform_rotate(layer1, angle, True, 0, 0)

# make layer 2
layer2 = gimp.Layer(posterImage, “Layer 2”, width, height, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layer2, 0)
pdb.gimp_edit_copy(image2.layers[0])
layer2.update(0, 0, width, height)
layer2.translate(posx0+1000,posy0)
layer2 = pdb.gimp_edit_paste(layer2, True)
layer2 = pdb.gimp_item_transform_rotate(layer2, angle, True, 0, 0)

# make layer 3
layer3 = gimp.Layer(posterImage, “Layer 3”, width, height, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layer3, 0)
pdb.gimp_edit_copy(image3.layers[0])
layer3.update(0, 0, width, height)
layer3.translate(posx0+500,posy0+500)
layer3 = pdb.gimp_edit_paste(layer3, True)
layer3 = pdb.gimp_item_transform_rotate(layer3, angle, True, 0, 0)

# make layer 3 clone
layer4 = gimp.Layer(posterImage, “Layer 4”, posterWidth, posterHeight, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layer4, 0)
pdb.gimp_edit_copy(image4.layers[0])
layer4.update(0, 0, width, height)
layer4.translate(posx0,posy0)
pdb.gimp_edit_paste(layer4, True)
#layer4 = pdb.gimp_item_transform_rotate(layer3, angle, True, 0, 0)

# draw two crossing lines
length = 2545
pdb.gimp_selection_none(posterImage) # de-select all
gimp.set_foreground((90, 90, 90))# set foreground color dark grey
layerline1 = gimp.Layer(posterImage, “Layer Cross Line 1”, posterWidth+1000, posterHeight, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layerline1, 0)
layerline1.update(0, 0, 2*posterWidth, 2*posterHeight)
pdb.gimp_image_select_rectangle(posterImage, 2, posx0, posy0, length, 9)# OPERATION=2 means CHANNEL-OP-REPLACE
pdb.gimp_edit_fill(layerline1, 0) # Fill selected area of drawable with foreground

# adjust crossing lines to center
layerline1 = pdb.gimp_item_transform_rotate(layerline1, 3.14/4, False, posterWidth/2, posy0)
layerline1.translate(62,550)

layerline2 = pdb.gimp_layer_copy(layerline1, True)
posterImage.add_layer(layerline2, 0)
layerline2.translate(-280,-280)

layerline3 = pdb.gimp_layer_copy(layerline1, True)
posterImage.add_layer(layerline3, 0)
layerline3 = pdb.gimp_item_transform_flip_simple (layerline3, 1, False, 2062)

layerline4 = pdb.gimp_layer_copy(layerline2, True)
posterImage.add_layer(layerline4, 0)
layerline4 = pdb.gimp_item_transform_flip_simple (layerline4, 1, False, 2062)

# create thin lines for title
titlew, titleh = 640, 20
gimp.set_foreground((255, 255, 255))
layerline3 = gimp.Layer(posterImage, “Layer Title Line 1”, posterWidth, titleh, RGBA_IMAGE, 100, NORMAL_MODE)
posterImage.add_layer(layerline3, 0)
layerline3.update(0, 0, posterWidth, 10)
#pdb.gimp_image_select_rectangle(posterImage, 0, 240, 500, titlew, titleh)# OPERATION=0 means CHANNEL-OP-ADD
#pdb.gimp_image_select_rectangle(posterImage, 0, 240+posterWidth/2, 500, titlew, titleh)
pdb.gimp_image_select_rectangle(posterImage, 0, 0, 0, titlew, titleh)# OPERATION=0 means CHANNEL-OP-ADD
pdb.gimp_image_select_rectangle(posterImage, 0, 110+posterWidth/2, 0, titlew, titleh)
pdb.gimp_edit_fill(layerline3, 0) # Fill selected area of drawable with foreground
layerline3.translate(270,600)
pdb.gimp_selection_none(posterImage) # de-select all

# clear layer4
pdb.gimp_edit_clear(layer4)
theDrawable = posterImage.active_drawable
posterXcfName = posterName + ‘.xcf’
filename = os.path.join(savePath, posterXcfName)
pdb.gimp_xcf_save(1, posterImage, theDrawable, filename, filename)

posterJpgName = posterName + ‘.jpeg’
jpgName= os.path.join(savePath, posterJpgName)
#pdb.file_jpeg_save(1, posterImage, theDrawable, jpgposterName, jpgposterName, 0.8, 0.5, 1, 0, ‘Poster designed by BAIXUAN WANG’, 3, 1, 0, 0)

# save as jpeg file
pdb.gimp_edit_copy_visible(posterImage)
new_img = pdb.gimp_edit_paste_as_new()
pdb.file_jpeg_save(new_img, new_img.layers[0], jpgName, jpgName, 0.8, 0.5, 1, 0, “Poster designed by BAIXUAN WANG with gimp python”, 3, 1, 0, 0)
pdb.gimp_image_delete(new_img)

register(
“python_fu_poster_bw”,
“BW Poster Maker”,
“This plugin createds a poster with 5 images and texts”,
“Baixuan Wang”,
“Copyright@BaixuanWang”,
“2016”,
“BW Poster Maker”,
“”,
[
# add imput parameter for the function arguments
(PF_FILE, “file0”, “Choose Background”, “”),
(PF_FILE, “file1”, “Choose Image 1”, “”),
(PF_FILE, “file2”, “Choose Image 2”, “”),
(PF_FILE, “file3”, “Choose Image 3”, “”),
(PF_FILE, “file4”, “Choose Image 4”, “”),
(PF_FONT, “font”, “Title Font”, “Impact”),
(PF_FONT, “font”, “Content Font”, “Agency FB”),
(PF_DIRNAME, “savePath”, “Directory to save image”, “C:/”),
(PF_STRING, “posterName”, “Poster Base Name”, ‘poster’),
],
[],
poster, menu=”<Image>/Filters”
)

main()