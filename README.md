# ETL Pipeline Using Prefect

## Project overview
This project is meant to showcase how to develop a basic ETL using Prefect.io- a modern dataflow automation tool.  For the problem statement we use public open dataset of chest X-ray and CT images of patients which are positive or suspected of COVID-19 or other viral and bacterial pneumonias (MERS, SARS, and ARDS) available [here]( https://github.com/ieee8023/covid-chestxray-dataset) 

## Process Overview
#### Extraction:
The extraction module involved setting up of folder structure. Deleting any previous data in folder and pulling new data using git repository into the specified folders.
#### Transform:
The transformation module involved filtering out [PA, AP, AP Supine] X-Ray views from metadata. Checking of which of them images are present. There after segregating them into two sets based on size. Making separate metadata files for these two groups.
#### Load:
The load module saves the metadata files in result folder and moves images into two different buckets based on metadata files.

## Setup:
#### Anaconda
After installing anaconda set up conda environment with python version 3.6 [dependencies compatibility].
```
Conda create –n <name_of_env>  python=3.6 anaconda
Conda activate <name_of_env>
```
Install the following packages
```
conda install -c conda-forge prefect
pip install prefect[viz]
conda install graphviz 
conda install -c conda-forge xorg-libxrender xorg-libxpm
```
Install the following packages for additionally functionality of dask and accessing dcm files.
```
pip install "dask[complete]"
conda install -c conda-forge pydicom 
conda install -c conda-forge gdcm
```
#### Docker and Docker Compose:

For installing docker on ubuntu 18.04 follow the step given @ [here]( https://www.hostinger.in/tutorials/how-to-install-docker-on-ubuntu). Install docker compose using the following commands:
```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)"  -o /usr/local/bin/docker-compose

$ sudo mv /usr/local/bin/docker-compose /usr/bin/docker-compose

$ sudo chmod +x /usr/bin/docker-compose
```
You will not be able to launch prefect server unless your docker compose can run without super user permission. To change permission setting run the following	:
```
$ sudo gpasswd -a $USER docker
$ sudo setfacl -m user:ubuntu:rw /var/run/docker.sock
```
Login to your docker using docker login and pull prefect image using the following command:
```
docker pull prefecthq/prefect:latest-python3.6
```
#### Folder structure setup (Optional):

Create a folder with your project name. Make 3 subfolders in that folder named: results, data and sourcecode. Place the main.ipynb in the sourcecode folder. Go to the results folder and further create 3 subfolder called: images, files and excluded images.

#### Prefect UI
To launch prefect you you will have to create a prefect config.toml file under the dir  ~/.prefect.
In the config file add the following: 
```
[server]
	[server.ui]
graphql_url = http://<IP_Address>:4200/graphql
```

Check your server to make sure port 8080 and port 4200 are open. To start the UI enter the following commands in your terminal
```
prefect backend server
prefect server start
```
go to your browser and enter http:<your_IP_Address>:8080 and you shall be logged onto the prefect UI.
#### Register
Register the flow (last line in main.ipynb) and you can then run the flow from the UI
#### License
Each image has license specified in the metadata.csv file. Including Apache 2.0, CC BY-NC-SA 4.0, CC BY 4.0.
The metadata.csv, scripts, and other documents are released under a CC BY-NC-SA 4.0 license.
