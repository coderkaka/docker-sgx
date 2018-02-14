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

USER root
RUN mkdir -p /opt/intel && \
    cd /opt/intel && \
    /home/aesmd/linux-sgx/linux/installer/bin/sgx_linux_x64_psw*.bin

USER aesmd
RUN cd ~/linux-sgx && \
    make aesm_install_pkg

USER root
RUN cd /opt/intel && \
    /home/aesmd/linux-sgx/linux/installer/bin/sgx_linux_x64_aesm*.bin

USER aesmd
CMD /opt/intel/sgxaesm/aesm/aesm_service --no-daemon
