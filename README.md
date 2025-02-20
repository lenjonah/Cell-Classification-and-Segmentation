# Cell Classification and Segmentation

The goal is to segment and classify two different cell types  (HEK and N2a) in fluorescence microscope images. For that purpose the images need to be transformed and labeled. Then, two pretrained convolutional neural networks can be trained for segmentation (cellpose) as well as segmentation and classification (MASK-RCNN). Code originally developed during an internship at the Institute of Developmental Genetics at the Helmholtz Zentrum Munich in 2021. 

The workflow is as follows: 

1. *Data collection & preprocessing:* Obtain two-channel (nuclei and cytoplasm) fluorescence microscope images of two different cell types (HEK and N2a). There are images containing only cells of one type as well as mixed images. Convert the Biodate file output from the microscope to .png images using, e.g., Fiji image. Note that in this conversion the two channels (nuclei and cytoplasm) have to be combined in the right color channels (blue and green). Check the quality of the data for artifacts or out of focus images.

3. *Labeling:* Since no labeled data was originally available, we start by segmenting the images containing only one cell type for the training data. Here, I used the GUI provided by the cellpose developers which is based on the generalist version of the [cellpose model](https://github.com/MouseLand/cellpose) (a U-net type model) for segmenting cytoplasm.    

4. *Segmentation:* Based on the manually labeled images, the cellpose mode can be further finetuned to segment both HEK and N2a cells using the notebook `train_cellpose.ipynb`.
   
   Either using the new weights or the generalist cellpose weights the cellpose algorithm can be run for segmentation using the above notebook. The corresponding output consists of a binary file with a mask 
   for each detected cell. 

6. *Classification:* Based on the segmented images containing only one cell type, respectively, the [MASK-RCNN](https://github.com/matterport/Mask_RCNN) model can be trained for segmentation and classification of mixed images. Using the annotation files generated using the cellpose model, the .json annotation files for the MASK-RCNN model can be created using the notebook `cells_to_coco.ipynb`.
   
   In this format, the images can finally be used to train the MASK-RCNN model for segmentation and classification using the notebook `train_maskrcnn.ipynb`.
   
More details and specifications can be found in the respective notebooks. Note that during my internship this was only a proof-of-principle study. Spending more time on this project and using e.g. hyperparameter searches and data augmentation techniques could definitely improve the performance of the models. For a general introduction to convolutional neural networks and a summary of the results, see the `presentation.pdf` file.
