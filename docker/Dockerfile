# Use a base image with the desired operating system (Debian)
FROM ubuntu:latest

# Update the package list and install the required packages, including Python 3
RUN apt-get update && apt-get install -y ca-certificates build-essential pkg-config cmake git zlib1g-dev libairspy-dev libusb-1.0-0-dev git

# Create the necessary directories
RUN mkdir -p /goesrecv /goesrecv/config

WORKDIR /goesrecv
# Clone librtlsdr repository
RUN git clone https://github.com/steve-m/librtlsdr.git

# Build librtlsdr
WORKDIR /goesrecv/librtlsdr
RUN mkdir build
WORKDIR /goesrecv/librtlsdr/build
RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr -DINSTALL_UDEV_RULES=ON ..
RUN make -j2 install

# Copy udev rules and blacklist DVB driver
RUN cp ../rtl-sdr.rules /etc/udev/rules.d/
RUN ldconfig
#RUN echo 'blacklist dvb_usb_rtl28xxu' | tee --append /etc/modprobe.d/blacklist-dvb_usb_rtl28xxu.conf

# Build goestools
WORKDIR /goesrecv
RUN git clone --recursive https://github.com/Zalgar/goestools-docker.git
WORKDIR /goesrecv/goestools-docker
RUN mkdir build
WORKDIR /goesrecv/goestools-docker/build
RUN cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local
#RUN make
#RUN make install

# Copy configuration file and test run goesrecv
#WORKDIR /goesrecv
#RUN cp /goesrecv/goestools-docker/etc/goesrecv.conf /goesrecv/config/goesrecv.conf
#CMD ["goesrecv", "-v", "-i", "1", "-c", "/goesrecv/config/goesrecv.conf"]
