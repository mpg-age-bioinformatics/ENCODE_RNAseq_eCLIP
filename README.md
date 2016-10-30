# ENCODE_RNAseq_eCLIP

This is a *Jupyter* notebook using *Python*, *R* and *bash* to integrate RNAseq and eCLIP
data from ENCODE. It uses an RNAseq data set of KHSRP-shRNA on K562 cells and respective
shRNA-mock control. It uses as well an eCLIP data set on KHSRP.

This pipeline comes with a *Docker* image containing all required software.

#### Installation

You will need to have [Docker](https://www.docker.com) and [git](https://www.atlassian.com/git/tutorials/install-git) installed.

Start by cloning this repository into your home directory:

```bash
cd ~/
git clone https://github.com/mpg-age-bioinformatics/ENCODE_RNAseq_eCLIP
```

Then build the Docker image:

```bash
cd ~/ENCODE_RNAseq_eCLIP
sudo docker build -t encode_rnaseq_eclip .
```

#### Running without Docker

Users who wish to run the Jupyter notebook without Docker should install
[Conda](http://conda.pydata.org) and make sure they have installed [Jupyter](http://jupyter.org).

Afterwards one should be able to follow the installation in [docker/Dockerfile](docker/Dockerfile).

#### Usage

The analysis pipeline was tested on a Docker image running with 8gb of memmory and 4 cpus.  

Start your container with:

```bash
sudo docker run -d -p 8888:8888 \
-e GRANT_SUDO=yes -e NB_UID=1000 --user root \
-e PASSWORD="YOURPASS" -e USE_HTTPS=yes \
-v ~/ENCODE_RNAseq_eCLIP/notebooks:/home/jovyan/work/notebooks \
-v ~/ENCODE_RNAseq_eCLIP/results:/home/jovyan/work/results \
--name encode_rnaseq_eclip \
-i -t encode_rnaseq_eclip
```

Instead of ```"YOURPASS"``` type in a password of your choice.

Then, on your web browser connect to [https://localhost:8888](https://localhost:8888).

You will be requested to accepted the self-signed certificate and to give in the password you used in ```"YOURPASS"```.

#### Running the Jupyter Notebook

In the Notebook Dashboard navigate to find the notebook: clicking on its name will open it in a new browser tab.

Click on the menu Help -> User Interface Tour for an overview of the Jupyter Notebook App user interface.

You can run the notebook document step-by-step (one cell a time) by pressing shift + enter.

You can run the whole notebook in a single step by clicking on the menu Cell -> Run All.

To restart the kernel (i.e. the computational engine), click on the menu Kernel -> Restart. This can be useful to start over a computation from scratch (e.g. variables are deleted, open files are closed, etc...).

#### Stopping, restarting, and removing the Docker container

Stop the container with ```docker stop encode_rnaseq_eclip``` and restart it with ```docker start encode_rnaseq_eclip```.

The container can be removed with ```docker rm encode_rnaseq_eclip```.

#### Removing Docker images

You can remove all images of removed containers with ```docker rmi $(docker images -q)```.

MAC users will maybe need to remove the contents of ```~/Library/Containers/com.docker.docker/Data/``` in order to free space not released after the removal of the images: ```rm -rf ~/Library/Containers/com.docker.docker/Data/*```.
