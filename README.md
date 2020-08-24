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

1. Create the conda environment using the file flag
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

###Notes

* If you're having ipython/jupyter notebook issues since it is using the wrong python kernel, [read here](https://stackoverflow.com/questions/37061089/trouble-with-tensorflow-in-jupyter-notebook)

* If you're having version compatibilty issues for keras, tensorflow, etc.. follow this matrix:

| Keras | Tensorflow | Tensorflow-GPU | cuDNN | cuda-toolkit | nvidia-drivers |
|-------|------------|----------------|-------|--------------|----------------|
| 2.0.8 | 1.14       | 1.14           | 7.4   | 10.1         | 418.39         |

* If the python script using the GPU was terminated but the GPU still hasn't cleared its memory

check your GPU processes

```
sudo fuser -v /dev/nvidia*
```

output will look like
```
USER        PID  ACCESS COMMAND
/dev/nvidia0:        root       1256  F...m  Xorg
username   2057  F...m  nvidia-persis...
username   20699 F...m  python
```

kill the python process
```
sudo kill -9 PID
```

##Operation Instructions

1. ssh into the gcloud instance

1. Load the conda environment
`conda activate <env-name>`

1. Mount the dataset to the compute instance
`gcsfuse --implicit-dirs scfly-cv-dataset ./data`

1. Change directory
`cd ~/Mask_RCNN/samples/rooftops`

1. Train

to train a new model:
`python3 rooftops.py train --dataset=../../../data --weights=coco`

to train the last model:
`python3 rooftops.py train --dataset=../../../data --weights=coco`

to view the loss graphs/dashboard, open a new ssh session
```
cd ~/Mask_RCNN
tensorboard --logdir logs/<latest_time_stamp> --bind_all --port 6006
```

and go to the compute engine IP address:6006 (the port has already been exposed)

1. View model

```
cd ~/Mask_RCNN
jupyter notebook
```

navigate to the samples/rooftops directory and view the notebooks there
