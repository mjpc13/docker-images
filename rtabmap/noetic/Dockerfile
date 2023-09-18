FROM mjpc13/opencv:4.5.1-nonfree AS opencv

# FROM ubuntu:20.04 AS dependencies

# ENV DEBIAN_FRONTEND=noninteractive
# SHELL ["/bin/bash","-c"]

# Install required packages
# RUN apt-get update && apt-get -y install build-essential cmake git libgtk2.0-dev python-dev python3-numpy

FROM ros:noetic-perception AS final
ARG MAKEFLAGS="-j$(nproc)"
LABEL maintainer="Mario Cristovao <mjpc13@protonmail.com>"

ENV DEBIAN_FRONTEND=noninteractive


RUN apt-get purge -y '*opencv*'
#Install custom compilation of OpenCV
COPY --from=opencv /app /usr/local
# RUN ldconfig /app

#Install some ROS packages

# SHELL ["/bin/bash","-c"]

RUN apt-get update && apt-get -y install build-essential cmake git libgtk2.0-dev python-dev python3-numpy python3-pybind11

RUN apt-get install -y ros-$ROS_DISTRO-libg2o ros-$ROS_DISTRO-gtsam ros-$ROS_DISTRO-libpointmatcher

WORKDIR /root/
RUN git clone https://github.com/introlab/rtabmap.git
WORKDIR /root/rtabmap/build/
RUN cmake -DWITH_PYTHON=ON -DBUILD_EXAMPLES=OFF ..
RUN make install -j12

#RUN apt-get update && apt-get install -y \
#  ros-noetic-rtabmap \
#  ros-noetic-rtabmap-ros \
#  && rm -rf /var/lib/apt/lists/

# Install required packages

# RUN apt-get update && apt-get -y install build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev python-dev python3-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev


CMD ["bash"]
