#Google Cloud Instructions

The tree directory in the `scfly-cv-dataset` bucket is as follows:

├── scfly-cv-dataset
│   ├── train
│   │   ├── image1
│   │   ├── image2
│   │   ├── json
│   │   │   ├── image1_annotations.json
│   │   │   ├── image2_annotations.json
│   ├── val
│   │   ├── image3
│   │   ├── image4
│   │   ├── json
│   │   │   ├── image3_annotations.json
│   │   │   ├── image4_annotations.json

##Fresh Installation Instructions

1. ssh into the gcloud instance

1. Install the conda environment. [Here are the instructions](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart). Only follow till step 8, as making the conda environment will require a specific instruction.

1. Download the Mask-RCNN library
`git clone https://github.com/niniack/Mask_RCNN.git`

1. Change directory to Mask_RCNN
`cd Mask_RCNN`

1. Create the conda environment
`conda create --name <env-name> --file working-gpu.txt`

1. Activate the conda environment
`conda activate <env-name>`

1. Run the setup script
`python3 setup.py install`

1. Check for GPU details. If the previous instruction fails, there is an issue with the nvidia driver. Follow the remaining instructions
`nvidia-smi`

(If the above fails)
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-driver-418
```
(Check again)
`nvidia-smi`

1. Make a data directory for gcsfuse to connect the bucket to
`cd ~ && mkdir data`


##Operation Instructions

1. ssh into the gcloud instance

1. Load the conda environment
`conda activate <env-name>`

1. Mount the dataset to the compute instance
`gcsfuse --implicit-dirs scfly-cv-dataset ./data`

1. change directory
`cd Mask_RCNN/samples/rooftops`

1. training

to train a new model:
`python3 rooftops.py train --dataset=../../../data --weights=coco`

load up tensorboard
