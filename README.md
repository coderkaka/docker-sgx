[![Build Status](https://travis-ci.org/haitaohuang/docker-sgx.svg?branch=master)](https://travis-ci.org/haitaohuang/docker-sgx)

This is based on original code from https://github.com/sean-jc/linux-sgx/tree/c/docker

Adapted to pull patched SGX PSW and SDK code from https://github.com/haitaohuang/linux-sgx/tree/docker.


The Dockerfiles and run.sh scripts under aesm and sample enable building
and running the AESM and/or SampleEnclave in Docker container(s).  The
containers communicate over the AESM's socket by mounting the directory
containing AESM.socket into all containers.  If the AESMD is running on
the host, then /var/run/aesmd on the host is mounted to /var/run/aesmd
in the SampleEnclave container.  If the AESM is running in a container,
then /tmp/aesmd is mounted into both the AESM and SampleEnclave containers.

A Docker Compose file and run script is also provided to run the AESM and
SampleEnclave in containers using a single command.


