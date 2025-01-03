# Cell Classification and Segmentation

Two convolutional neural networks are finetuned for the classification and segmentation of different cell types in microscope images. Code originally created during an internship at the Institute of Developmental Genetics at the Helmholtz Zentrum Munich in 2021. 

The workflow is as follows: 

1. Obtain two-channel (nuclei and cytoplasm) microscope images of two different cell types (HEK and N2a).
2. Convert the Biodate file output from the microscope to .png images using, e.g., Fiji image. Note that in this conversion the two channels (nuclei and cytoplasm) have to be combined in the right color channels (blue and green).
3. Train the [cellpose model](https://github.com/MouseLand/cellpose) (a U-net model) for the segmentation of the cells. For that, training data have to be manually segmented by hand. Here, it proved very easy to use the GUI provided by the cellpose developers. Then, the network can be trained using the notebook
```   
	train_cellpose.ipynb
```
Either using the new weights or the original cellpose weights the cellpose algorithm can be performed for segmentation using the above notebook. The segmentation consists of a binary file with a mask for each detected cell. 
4. Based on the segmented images of the different cell types, train [MASK-RCNN](https://github.com/matterport/Mask_RCNN) for segmentation and classification of mixed images. Using the annotation files created in the above fashion, the .json annotation files for the Mask RCNN model can be created using the notebook
```
	cells_to_coco.ipynb
```
5. In this format, the images can finally be used to train the Mask-RCNN model for segmentation and classification using the notebook 
```
	train_maskrcnn.ipynb
```
More details and specifications can be found in the respective notebooks. For a summary of the results, see the `presentation.pdf` file.
