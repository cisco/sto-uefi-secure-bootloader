FROM --platform=linux/amd64 ubuntu:jammy

RUN sed -i 's/# deb-src/deb-src/' /etc/apt/sources.list && \
    apt update -y && \
    DEBIAN_FRONTEND=noninteractive apt install -y devscripts git-buildpackage software-properties-common gcc-aarch64-linux-gnu && \
    apt build-dep -y shim tar wget

# Download and extract the upstream release tarball for shim
RUN wget https://github.com/rhboot/shim/releases/download/16.1/shim-16.1.tar.bz2
RUN mkdir /shim-build
RUN tar -xvf shim-16.1.tar.bz2 --directory /shim-build
WORKDIR /shim-build/shim-16.1

# Add our public certificate
ADD Cisco_Virtual_UEFI_SubCA_v3.cer .

# Create build directories
RUN mkdir build-aarch64

# Add our SBAT data
ADD sbat.csv data/sbat.csv

# Always use the cross-compiler to ensure reproducible builds across all host architectures.
RUN make -C build-aarch64 ARCH=aarch64 VENDOR_CERT_FILE=../Cisco_Virtual_UEFI_SubCA_v3.cer \
        TOPDIR=.. -f ../Makefile \
        CROSS_COMPILE=aarch64-linux-gnu-

# Output hash of shimaa64.efi
RUN sha256sum build-aarch64/shimaa64.efi

# Final shim for installation
RUN mkdir /shim-build/install
RUN cp build-aarch64/shimaa64.efi /shim-build/install/
RUN cp build-aarch64/fbaa64.efi /shim-build/install/
RUN cp build-aarch64/mmaa64.efi /shim-build/install/