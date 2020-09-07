# start with the Docker 'base R' Debian-based image
FROM rocker/r-devel-san

# Remain current and install fst requirements
RUN apt-get update \
	&& apt-get install -y libcurl4-openssl-dev \
	&& apt-get install -y libgit2-dev \ 
	&& apt-get install -y libssl-dev \
	&& apt-get install -y xml2 \
	&& apt-get install -y libssh2-1-dev \
	&& apt-get install -y libxml2-dev

