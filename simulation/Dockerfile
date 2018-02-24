FROM ubuntu:16.04

#ENV http_proxy http://proxy-us.intel.com:911
#ENV https_proxy http://proxy-us.intel.com:912
#ENV no_proxy intel.com,*.intel.com,127.0.0.1,localhost

RUN apt-get update && \
    apt-get install -y git build-essential ocaml automake autoconf libtool libcurl4-openssl-dev libprotobuf-dev libprotobuf-c0-dev protobuf-compiler curl make g++ unzip wget libssl-dev python module-init-tools

RUN groupadd aesmd && useradd -m -s /bin/bash -g aesmd aesmd

USER aesmd
RUN cd ~ && git clone https://github.com/haitaohuang/linux-sgx.git
RUN cd ~/linux-sgx && git checkout -b docker origin/docker
RUN cd ~/linux-sgx && \
    ./download_prebuilt.sh && \
    make psw_install_pkg

USER aesmd
RUN cd ~/linux-sgx && \
    make sdk_install_pkg

USER root
RUN mkdir /opt/intel && \
    cd /opt/intel && \
    sh -c 'echo yes | /home/aesmd/linux-sgx/linux/installer/bin/sgx_linux_x64_sdk_*.bin'

USER aesmd
RUN cd ~/linux-sgx/SampleCode/SampleEnclave && \
    SGX_DEBUG=1 SGX_MODE=SIM make
ENV LD_LIBRARY_PATH /opt/intel/sgxsdk/lib64

CMD cd /home/aesmd/linux-sgx/SampleCode/SampleEnclave && \
    sh -c 'echo c | ./app'
