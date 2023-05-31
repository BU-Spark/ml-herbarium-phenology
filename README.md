# ML-Herbarium-Phenology

# Overview
 
The changing climate increases stressors that weaken plant resilience, disrupting forest structure and ecosystem services. Rising temperatures lead to more frequent droughts, wildfires, and invasive pest outbreaks, leading to the loss of plant species. That has numerous detrimental effects, including lowered productivity, the spread of invasive plants, vulnerability to pests, altered ecosystem structure, etc. The project aims to aid climate scientists in capturing patterns in plant life concerning changing climate.

The herbarium specimens are pressed plant samples stored on paper. The specimen labels are handwritten and date back to the early 1900s. The labels contain the curator's name, their institution, the species and genus, and the date the specimen was collected. Since the labels are handwritten, they are not readily accessible from an analytical standpoint. The data, at this time, cannot be analyzed to study the impact of climate on plant life.

The digitized samples are an invaluable source of information for climate change scientists, and are providing key insights into biodiversity change over the last century. Digitized specimens will facilitate easier dissemination of information and allow more people access to data. The project, if successful, would enable users from various domains in environmental science to further studies pertaining to climate change and its effects on flora and even fauna.


## Project Description

The Harvard University Herbaria aims to digitize the valuable handwritten information on herbarium specimens, which contain crucial insights into biodiversity changes in the Anthropocene era. 

The main challenge is to develop a transformer-based optical character recognition (OCR) model using deep learning techniques to accurately **locate and extract the specimen labels on the samples to preserve them digitally**. The secondary task involves **building a plant classifier using taxon labels as ground truth, to supplement the OCR model as a source of a priori knowledge.** The tertiary goal involves **identifying the phenology of the plant specimen under consideration [will be updated after discussing with the client] and possibly predict the biological life cycle stage of the plant**. The successful completion of these objectives will showcase the importance of herbaria in storing and disseminating data for various biological research areas. The ultimate goal is to revive and digitize this valuable information to promote its accessibility to the public and future generations.

## Swin Transformer - Tertiary Task Setup
The original repo can be found [here](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection)
This section of the repository deals with the instance segmentation task. We use the repo stored [here](/projectnb/sparkgrp/ml-herbarium-grp/ml-herbarium-data/Tertiary_Task/Swin-Transformer-Object-Detection/) on the scc server under sparkgrp. 

The current model can be found in `./work_dirs/flowers_36_epochs_update/epoch_36.pth` and the visualized outputs are in `outputs/plants_36_epochs_segm`

Dependencies can be found in `requirements.txt`

<br />

## Usage

### Installation

Please refer to [get_started.md](https://github.com/open-mmlab/mmdetection/blob/master/docs/en/get_started.md) for installation and dataset preparation.

You can also use the script I have written in the event the guide does not work it can be found at `scc_jobs/job_install_packages.sh`.

We then run the file `scc_jobs/job_finetune_2.sh` this will go about downloading the model and finetuning using the data.

The model is [model weight](https://github.com/SwinTransformer/storage/releases/download/v1.0.2/cascade_mask_rcnn_swin_small_patch4_window7.pth) and the config file is [config file](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection/blob/master/configs/swin/cascade_mask_rcnn_swin_small_patch4_window7_mstrain_480-800_giou_4conv1f_adamw_3x_coco.py)

Because the mmdetection repo has to be cloned I have placed the data at `/projectnb/sparkgrp/ml-herbarium-grp/ml-herbarium-data/Tertiary_Task/Swin-Transformer-Object-Detection/data`, when finetuning the model it will need to be place in `/projectnb/sparkgrp/ml-herbarium-grp/ml-herbarium-data/Tertiary_Task/Swin-Transformer-Object-Detection/mmdetection/data`.

### Data Visualization

In order to visualize the data I have created a script `scc_jobs/job_flower_test.sh` that will create the visualizations seen in `outputs/plants_36_epochs_segm`




# ML-Herbarium Tertiary Task EVA

This is based on the [repo](https://github.com/baaivision/EVA/tree/master/EVA-01/det) and instructions can be found there in terms of installing the EVA model. One major issue was that when finetuning the model it went from the base 4 GB to 12 GB. When searching online we found that this maybe due to the code from detectron2 using floating-point 32.

I also have a file under `scc_jobs/create_dataset.py` which can help with creating the plants json file to format needed for EVA.
We use the repo stored [here](/projectnb/sparkgrp/ml-herbarium-grp/ml-herbarium-data/Tertiary_Task/EVA)

Currently I was not able to debug the model in order to test it as I encountred VRAM issues. I would recommend using the guide [here](https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5) in order to format the data into the COCO like style.

Once you have the data in the correct format you can look at changing the configurations in `tools/lazyconfig_train_net.py` or start from scratch and use my code as a guide as to what I have done. I have updated some of the configs that allow you to take the new plant dataset however it may be better to use the original EVA code and paste mine in. As I was also testing with the ballon dataset.


