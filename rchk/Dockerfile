# start with a default Ubuntu image
FROM ubuntu:18.04


# remain current and add some sources
RUN sed -i 's/main/main restricted universe/g' /etc/apt/sources.list
RUN sed -i 's/^# deb-src/deb-src/g' /etc/apt/sources.list

RUN apt-get update \
    && apt-get -y upgrade

# avoid interactive time-zone query
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata

# Install required utilities
RUN apt-get install -y libopenblas-dev libcurl4-openssl-dev libopenmpi-dev openmpi-bin openmpi-common openmpi-doc software-properties-common \
    && apt-get install -y openssh-client openssh-server libssh-dev wget vim git cmake curl python autoconf bzip2 libtool \
    && apt-get install -y libtool-bin python-pip python-dev locales libpcre2-dev libgit2-dev libssl-dev xml2 libssh2-1-dev valgrind


# Install required R dependencies
RUN apt-get build-dep -y r-base-core


# LLVM
RUN apt-get install -y clang-6.0 llvm-6.0-dev clang++-6.0 clang llvm-dev libc++-dev libc++abi-dev \
    && apt-get clean \
    && locale-gen en_US.UTF-8


# wllvm
RUN pip install wllvm


# rchk
RUN cd /opt \
    && git clone git://www.github.com/kalibera/rchk \
    && cd rchk \
    && cd src \
    && make -j `cat /proc/cpuinfo | grep "core id" | wc -l` LLVM=/usr/lib/llvm-6.0 CXX=g++


# download, build r-latest
RUN cd /opt \
    &&   wget -O - -q https://cran.r-project.org/src/base/R-4/R-4.0.2.tar.gz | tar zxvf - \
    && cd /opt/R-4.0.2 \
    && export WLLVM=/usr/local/bin \
    && export LLVM=/usr/lib/llvm-6.0 \
    && export RCHK=/opt/rchk \
    && export CPICFLAGS=-fPIC \
    && export FPICFLAGS=-fPIC \
    && export FCPICFLAGS=-fPIC \
    && export CXXPICFLAGS=-fPIC \
    && export CXX98PICFLAGS=-fPIC \
    && export CXX11PICFLAGS=-fPIC \
    && export CXX14PICFLAGS=-fPIC \
    && export CXX17PICFLAGS=-fPIC \
    && export R_LIBS= \
    && export R_LIBS_SITE= \
    && export R_LIBS_USER= \
    && export R_MAKEVARS_USER= \
    && export R_MAKEVARS_SITE= \
    && /opt/rchk/scripts/build_r.sh


# fix up cmpconfig.inc to use user-defined PKG_BUILD, R_LIBS
# these options were tested on Ubuntu 16.04.2
# they allow to build most but not all packages
RUN cat << EOF > /opt/rchk/scripts/cmpconfig.inc \
    && export CFLAGS="-Wall -g -O0 -fPIC" \
    && export CXXFLAGS="$CFLAGS -I/usr/include/libcxxabi" \
    && export CC=$WLLVM/wllvm \
    && export CXX="$WLLVM/wllvm++ -stdlib=libc++" \
    && export PATH=$LLVM/bin:$PATH \
    && export LLVM_COMPILER=clang \
#  export PKG_BUILD_DIR=`pwd`/packages/build \
#  export R_LIBS=`pwd`/packages/lib \
    && export LD="clang++ -stdlib=libc++"
