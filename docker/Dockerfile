# Copyright (c) Jorge Boucas.
# Distributed under the terms of the Modified BSD License.

FROM jupyter/base-notebook

MAINTAINER Jorge Boucas <jorge.boucas@age.mpg.de>

USER root

# Install all OS dependencies for fully functional notebook server (from jupyter/minimal-notebook)
RUN apt-get update && apt-get install -yq --no-install-recommends \
    git \
    vim \
    jed \
    emacs \
    build-essential \
    python-dev \
    unzip \
    libsm6 \
    pandoc \
    texlive-latex-base \
    texlive-latex-extra \
    texlive-fonts-extra \
    texlive-fonts-recommended \
    texlive-generic-recommended \
    libxrender1 \
    inkscape \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y \
    pkg-config \
    libxml2-dev \
    libcurl4-gnutls-dev \
    libatlas3-base \
    libopenblas-base \
    libfreetype6-dev \
    pigz \
    zlib1g-dev \
    autoconf \
    automake \
    libtool \
    libexpat1-dev \
    libxml2-dev \
    libxslt1-dev \
    ghostscript \
    r-base \
    r-base-dev

RUN cpan Math::CDF HTML::PullParser HTML::Template XML::Simple XML::Parser::Expat XML::Compile::SOAP11 XML::Compile::WSDL11 XML::Compile::Transport::SOAPHTTP Log::Log4perl


# Install all bioinformatics tools
RUN echo 'source("http://bioconductor.org/biocLite.R")' > ~/pack.install.R && \
    echo 'biocLite(c("gplots","Gviz","biomaRt"), ask=FALSE)'  >> ~/pack.install.R && \
    Rscript ~/pack.install.R

# Switch back to jovyan to avoid accidental container runs as root and install user packages
USER $NB_USER

RUN conda install -y psutil
RUN conda create -y -n ipykernel_py2 python=2 ipykernel
ENV PATH /opt/conda/envs/ipykernel_py2/bin/:$PATH
RUN python2 -m ipykernel install --user
RUN /opt/conda/envs/ipykernel_py2/bin/pip install \
    pybedtools \
    scipy \
    seaborn \
    statsmodels \
    Wand

RUN mkdir -p /home/jovyan/work/notebooks \
    /home/jovyan/work/results \
    /home/jovyan/software

RUN cd ~/ && \
    git clone https://github.com/mpg-age-bioinformatics/AGEpy.git && \
    cd AGEpy && \
    python2 setup.py develop
RUN cd ~/software && \
    wget https://github.com/deweylab/RSEM/archive/v1.3.0.tar.gz && \
    tar -xzvf v1.3.0.tar.gz && \
    cd RSEM-1.3.0 && \
    make && make ebseq && make install prefix=~/software
RUN cd ~/software && \
    wget https://github.com/arq5x/bedtools2/releases/download/v2.25.0/bedtools-2.25.0.tar.gz && \
    tar -zxvf bedtools-2.25.0.tar.gz && \
    cd bedtools2 && \
    make && make install prefix=~/software
RUN cd ~/software && \
    wget https://github.com/samtools/samtools/releases/download/1.3.1/samtools-1.3.1.tar.bz2 && \
    tar -jxvf samtools-1.3.1.tar.bz2 && \
    cd samtools-1.3.1 && \
    ./configure --prefix=$HOME/software && \
    make && \
    make prefix=$HOME/software install
RUN cd ~/software && \
    wget http://ccb.jhu.edu/software/stringtie/dl/gffread-0.9.8.Linux_x86_64.tar.gz && \
    tar -zxvf gffread-0.9.8.Linux_x86_64.tar.gz
RUN cd ~/software && \
    wget http://meme-suite.org/meme-software/Databases/motifs/motif_databases.12.15.tgz && \
    tar zxvf motif_databases.12.15.tgz
RUN cd ~/software && \
    wget http://meme-suite.org/meme-software/4.11.2/meme_4.11.2_1.tar.gz && \
    tar -zxvf meme_4.11.2_1.tar.gz && \
    cd meme_4.11.2 && \
    ./configure --prefix=$HOME/software/ --with-url=http://meme-suite.org --enable-build-libxml2 --enable-build-libxslt && \
    make && \
    make test && \
    make prefix=$HOME/software install

ENV PATH ~/software/bin:~/software/RSEM-1.3.0:~/software/gffread-0.9.8.Linux_x86_64:$PATH
