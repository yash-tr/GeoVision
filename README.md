![ubuntu label](https://img.shields.io/badge/Ubuntu-20.04-brightgreen)
[![build passing label](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/TIERS/wildnav/actions/workflows/python-app.yml)
# WildNav: GNSS-Free Localization in the Wild

Check out our paper at the [TIERS website](https://tiers.utu.fi/paper/marius2022vision). Access a sample [dataset here](https://utufi.sharepoint.com/:f:/s/msteams_0ed7e9/EsXaX0CKlpxIpOzVnUmn8-sB4yvmsxUohqh1d8nWKD9-BA?e=gPca2s).

###  Abstract

Considering the accelerated development of Unmanned Aerial Vehicles (UAVs) applications in both industrial and research scenarios, there is an increasing need for localizing these aerial systems in non-urban environments, using GNSS-Free, vision-based methods. Our paper proposes a vision-based localization algorithm that utilizes deep features to compute geographical coordinates of a UAV flying in the wild. The method is based on matching salient features of RGB photographs captured by the drone camera and sections of a pre-built map consisting of georeferenced open-source satellite images. Experimental results prove that vision-based localization has comparable accuracy with traditional GNSS-based methods, which serve as ground truth. Compared to state-of-the-art Visual Odometry (VO) approaches, our solution is designed for long-distance, high-altitude UAV flights.

### Overview

The main advantage of Wildnav is its capability of matching drone images (left) with georeferenced satellite images (right). The aim is for this to work for drones flying in non-urban areas, in environments with sparse features.

<div align=center>
<img src="assets/overview/project_overview.png" width="800px">
<p align="center">Vision-based localization algorithm overview </p>
</div>

<div align=center>
<img src="assets/overview/good_match_1.png" width="800px">
<p align="center"> Robust against rotation </p>
</div>

<div align=center>
<img src="assets/overview/good_match_2.png" width="800px">
<p align="center"> Able to visually identify the drone pose even in environments with sparse feature </p>
</div>

<div align=center>
<img src="assets/overview/good_match_3.png" width="800px">
<p align="center"> The drone image (left) can be taken from a very different perspective, compared to the matched satellite image(right) </p>
</div>

<div align=center>
<img src="assets/overview/good_match_4.png" width="800px">
<p align="center"> Successful visual-based localization in non-urban areas </p>
</div>



## Run it yourself

The algorithm was tested on Ubuntu 20.04 with Python 3.10. Nevertheless, it should work with other versions as well.


   0. **(Highly recommended)** Create a new python3 virtual environment
      ```
      python3 -m venv env 
      ```
      Activate it
      ```
      source env/bin/activate
      ```
   1. Clone the repo
      ```
      git clone git@github.com:TIERS/wildnav.git
      ```
   3. Install superglue dependencies:
      ```
      cd wildnav
      git submodule update --init --recursive
      ```
   3. Install python dependencies
      ```
      pip3 install -r requirements.txt
      ```
   4. Run the localization script (see below to add a dataset, but you can try with a few samples already in the repo)
      ```
      cd src
      python3 wildnav.py
      ```


## Add your own drone image dataset

Before running the code, you need to have a dataset, reference images (map) 

   1. Add your drone photos to ```assets/query```. Feel free to use our dataset from [here](https://utufi.sharepoint.com/:f:/s/msteams_0ed7e9/EsXaX0CKlpxIpOzVnUmn8-sB4yvmsxUohqh1d8nWKD9-BA?e=gPca2s).

   2. Add your satellite map images to ```assets/map``` together with a csv file containing geodata for the images (see ```assets/map/map.csv```) 

   3. Run python script to generate csv file containing photo metadata with GNSS coordinates
      ```
      python3 extract_image_meta_exif.py
      ```
   4. Run wildnav algorithm
      ```
      python3 wildnav.py
      ```
   
## Common problems and fixes

1. Runtime error due to incompatible version of ```torch``` installed
Error message: "NVIDIA GeForce RTX 3070 with CUDA capability sm_86 is not compatible with the current PyTorch installation. The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_70.If you want to use the NVIDIA GeForce RTX 3070 GPU with PyTorch."

**Fix:**

Uninstall current torch installation:
```
pip3 uninstall torch
```
Follow instructions on the official [pytorch](https://pytorch.org/get-started/locally/) website to install the right version of torch for your system (it depends on your graphics card and CUDA version).

2. No dedicated GPU available on your system.

**Fix:**


The algorithm can run, albeit much slower, on CPU. Simply change ```force_cpu``` flag in ```src/superglue_utils.py``` to ```True```.

**NOTE**: If you encounter any problems which are not listed here, please open a new issue in this repository. We will try to fix it as soon as possible.

## Datasets

Photographs used for experimental validation of the algorithm can be found [here](https://utufi.sharepoint.com/:f:/s/msteams_0ed7e9/EsXaX0CKlpxIpOzVnUmn8-sB4yvmsxUohqh1d8nWKD9-BA?e=gPca2s).

<div align=center>
<img src="assets/overview/experiments_flight_area.png" width="800px">
<p align="center">Satellite view of the flight zone (highlighted rectangle). The yellow pin is
located at 60.403091° latitude and 22.461824° longitude </p>
</div>


## Results

<div align=center>

|           	| Total 	| Localized 	| MAE (m) 	|
|:---------:	|:-----:	|:---------:	|:-------:	|
| Dataset 1 	|  124  	|  77 (62%) 	|  15.82  	|
| Dataset 2 	|   78  	|  44 (56%) 	|  26.58  	|

</div>



<div align=center>
<img src="assets/overview/dataset_1_abs_coord.png" width="800px">
<p align="center">Dataset 1 absolute coordinates of localized photographs </p>
</div>

<div align=center>
<img src="assets/overview/dataset_1_error.png" width="800px">
<p align="center">Dataset 1 localization error </p>
</div>

<div align=center>
<img src="assets/overview/dataset_2_abs_coord.png" width="800px">
<p align="center">Dataset 2 absolute coordinates of localized photographs</p>
</div>

<div align=center>
<img src="assets/overview/dataset_2_error.png" width="800px">
<p align="center">Dataset 2 localization error </p>
</div>

<div align=center>
<img src="assets/overview/error_comparison.png" width="800px">
<p align="center">Error comparison </p>
</div>

## Contact
Feel free to send me an email at mmgurg@utu.fi if you have any questions about the project.
