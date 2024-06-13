# run "docker pull ubuntu:jammy" before building. 
FROM ubuntu:jammy

RUN sed -i 's/# deb-src/deb-src/' /etc/apt/sources.list && \
    apt update -y && \
    DEBIAN_FRONTEND=noninteractive apt install -y devscripts git-buildpackage software-properties-common && \
    apt build-dep -y shim tar wget

# Download and extract the upstream source tarball for shim
RUN wget https://github.com/rhboot/shim/releases/download/15.8/shim-15.8.tar.bz2
RUN mkdir /shim-build
RUN tar -xvf shim-15.8.tar.bz2 --directory /shim-build
WORKDIR /shim-build/shim-15.8

# Add our public certificate
ADD Cisco_Virtual_UEFI_SubCA_v3.der .

# Create build directory
RUN mkdir build-x64

# Add our SBAT data
ADD sbat.csv data/sbat.csv

# Build 64-bit
RUN make -C build-x64 ARCH=x86_64 VENDOR_CERT_FILE=../Cisco_Virtual_UEFI_SubCA_v3.der \
    TOPDIR=.. -f ../Makefile

# Output hash of shimx64.efi
RUN sha256sum build-x64/shimx64.efi

# Final shim for installation
RUN mkdir /shim-build/install
RUN cp build-x64/shimx64.efi /shim-build/install
RUN cp build-x64/fbx64.efi /shim-build/install
RUN cp build-x64/mmx64.efi /shim-build/install
