#Download base image ubuntu 20.04
FROM ubuntu:20.04

#Install system libraries required to build tools
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get install -y  \
    apt \
    build-essential \
    curl \
    gcc \
    git \
    libcurl4-openssl-dev \
    libssl-dev libv8-dev \
    libbz2-dev \
    udunits-bin \
    libudunits2-* \
    libxml2-dev \
    liblzma-dev \
    openjdk-11-jdk-headless \
    pip \
    python \
    wget \
    unzip \
    && rm -rf /var/lib/apt/lists/*

## update system libraries
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get clean

# Set working directory
WORKDIR /home/worker

# Add user
RUN addgroup --system worker \
    && adduser --system --ingroup worker worker

RUN chown worker:worker -R /home/worker

USER worker

#Install python packages
COPY ./requirements.txt .

RUN pip install -r requirements.txt
#N.B. Pandas v1.3.5 is in the requirements.txt file 

#Install Argparse v1.2.1 
RUN pip install argparse==1.2.1

#Install trimmomatic v0.32
RUN wget https://github.com/usadellab/Trimmomatic/files/5854829/Trimmomatic-0.32.zip \
    && unzip Trimmomatic-0.32.zip \
    && mv Trimmomatic-0.32/trimmomatic-0.32.jar . \
    && mv Trimmomatic-0.32/adapters ./trimmomatic-adapters \
    && rm Trimmomatic-0.32.zip

# Install ABRA v0.97 
RUN wget https://github.com/mozack/abra/releases/download/v0.97/abra-0.97-SNAPSHOT-jar-with-dependencies.jar \
    && mv abra-0.97-SNAPSHOT-jar-with-dependencies.jar abra-0.97.jar 

#Install Picard v1.128
RUN wget https://github.com/broadinstitute/picard/releases/download/1.128/picard-tools-1.128.zip \
    && unzip picard-tools-1.128.zip \
    && rm picard-tools-1.128.zip \
    && mv picard-tools-1.128/picard.jar . 

#Install VarScan v2.4.0
RUN wget https://github.com/dkoboldt/varscan/releases/download/2.4.0/VarScan.v2.4.0.jar


#Install BEDtools v2.26.0 
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.26.0/bedtools-2.26.0.tar.gz \
    && tar -xvzf bedtools-2.26.0.tar.gz \
    && rm bedtools-2.26.0.tar.gz \
    && cd bedtools2 \
    && make

## 8 remaining to install 

# Install BWA v0.7.12 - make NOT WORKING
# RUN wget https://github.com/lh3/bwa/archive/refs/tags/0.7.12.tar.gz \
#     && tar -xvzf 0.7.12.tar.gz \
#     && cd bwa-0.7.12 \
#     && make 

#Install GATK v2.3-9 - Need to find

#Install Platypus v0.7.9.1 - need to find

# This one is not in list but is needed to compile pindel from source
# Install a 2015 version of htslib to build pindel  (as pindel version from 2015)
#The --prefix /home/worker bit doesnt work properly here - potentially have it written incorrectly
# RUN wget https://github.com/samtools/htslib/releases/download/1.2.1/htslib-1.2.1.tar.bz2 \
#     && tar -xjf htslib-1.2.1.tar.bz2 \
#     && rm htslib-1.2.1.tar.bz2 \
#     && cd htslib-1.2.1 \
#     && ./configure \
#     && make \
#     && make install --prefix /home/worker


# #Install Pindel v0.2.5b8 - TO FINISH
# RUN wget https://github.com/genome/pindel/archive/refs/tags/v0.2.5b8.zip \
#     && unzip v0.2.5b8.zip \
#     && rm v0.2.5b8.zip


# #Install Manta v0.29.6 - TO FINISH
# RUN wget https://github.com/Illumina/manta/releases/download/v0.29.6/manta-0.29.6.centos5_x86_64.tar.bz2 \
#     && tar -xjf manta-0.29.6.centos5_x86_64.tar.bz2 \
#     && rm manta-0.29.6.centos5_x86_64.tar.bz2


# #Install Samtools v1.1  - TO FINISH
# RUN wget https://sourceforge.net/projects/samtools/files/samtools/1.1/samtools-1.1.tar.bz2 \
#     && tar -xjf samtools-1.1.tar.bz2 \
#     && rm samtools-1.1.tar.bz2 

# #Install Bcftools v1.0 - TO FINISH
# RUN wget https://sourceforge.net/projects/samtools/files/samtools/1.0/samtools-bcftools-htslib-1.0_x64-linux.tar.bz2 \
#     && tar -xjf samtools-bcftools-htslib-1.0_x64-linux.tar.bz2 \
#     && rm samtools-bcftools-htslib-1.0_x64-linux.tar.bz2

#Install Tabix v0.2.6 (this is potentially in the htslib package but need to check version)

ENV PATH="$PATH:/home/worker"
ENV PATH="$PATH:/home/worker/bedtools2/bin"
RUN echo $PATH










