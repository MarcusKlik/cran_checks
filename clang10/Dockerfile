# start with a default Ubuntu image
FROM ubuntu:18.04

# Remain current
RUN apt-get update \
    && apt-get -y upgrade

# Install fst requirements
RUN apt-get install -y libcurl4-openssl-dev software-properties-common \ 
    && apt-get install -y libgit2-dev \
    && apt-get install -y libssl-dev xml2 libssh2-1-dev libxml2-dev wget

RUN apt-get install -y valgrind


# Install llvm10
RUN mkdir ~/tmp \
    && cd ~/tmp \
    && wget https://apt.llvm.org/llvm.sh \
    && chmod +x llvm.sh \
    && ./llvm.sh 10 \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get update


# prerequisites for R
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata


# Install latest R 4.0
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/'
RUN apt-get update
RUN apt-get install -y r-base


# use clang-10 for package building
RUN mkdir ~/.R \
    && echo "CC = /usr/bin/clang-10" > ~/.R/Makevars \
    && echo "CXX = /usr/bin/clang++-10" >> ~/.R/Makevars \
    && echo "CXX1X = /usr/bin/clang++-10" >> ~/.R/Makevars \
    && echo "CXX11 = /usr/bin/clang++-10" >> ~/.R/Makevars \
    && echo "CXX14 = /usr/bin/clang++-10" >> ~/.R/Makevars \
    && echo "CXX17 = /usr/bin/clang++-10" >> ~/.R/Makevars \
    && echo 'CFLAGS="-g -O3 -Wall -pedantic"' > ~/.Renviron \
    && echo 'FFLAGS="-g -O2 -mtune=native -Wall -pedantic"' >> ~/.Renviron \
    && echo 'CXXFLAGS="-g -O3 -Wall -pedantic -frtti"' >> ~/.Renviron \
    && echo 'CPPFLAGS="-isystem /usr/local/clang/include"' >> ~/.Renviron \
    && echo 'LDFLAGS="-L/usr/local/clang/lib64 -L/usr/local/lib64"' >> ~/.Renviron
