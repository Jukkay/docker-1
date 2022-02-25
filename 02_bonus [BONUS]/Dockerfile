# Valgrindr allows user to run valgrind inside a container.
#
# Prerequisites:
# 1. Docker is installed.
# 2. C program compiles.
# 3. Project directory includes valid Makefile that compiles your program.
#
# Copy this Dockerfile and valgrindr.sh to the project directory. Folder must include valid Makefile for the project.
# Makefile compiles the project inside the container and executes the binary in valgrind with optional arguments.
#
# NOTE: The whole project folder is copied to the container so avoid huge unnecessary files in the folder.
#
# Run to build the image:
# 	bash valgrindr.sh

FROM debian
RUN apt-get update && \
	apt-get upgrade -y && \
	apt-get install build-essential -y && \
	apt-get install valgrind -y && \
	echo \
	"#!/bin/bash \
	make re \
	valgrind $1 $2 $3" > run.sh && \
	rm -rf /var/lib/apt/lists/*
COPY . /
ENTRYPOINT ["/bin/bash", "run.sh"]
