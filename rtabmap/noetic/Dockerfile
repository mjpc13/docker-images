FROM mjpc13/opencv:4.2.0-nonfree AS opencv

FROM ubuntu:20.04 AS opt_dep
ENV DEBIAN_FRONTEND=noninteractive
ARG MAKEFLAGS=-j$(nproc)

RUN apt-get update && apt-get -y install build-essential cmake git libgtk2.0-dev python-dev python3-numpy python3-pybind11 libeigen3-dev libboost-all-dev

#g2o
WORKDIR /root/
RUN git clone https://github.com/RainerKuemmerle/g2o.git
WORKDIR /root/g2o/build/
RUN cmake -D CMAKE_INSTALL_PREFIX=/app  -DG2O_BUILD_EXAMPLES=OFF .. && make install ${MAKEFLAGS} 

#gtsam
WORKDIR /root/
RUN git clone https://github.com/borglab/gtsam.git
WORKDIR /root/gtsam/build/
RUN cmake -D CMAKE_INSTALL_PREFIX=/app -DGTSAM_BUILD_WITH_MARCH_NATIVE=OFF -DGTSAM_USE_SYSTEM_EIGEN=ON .. && make install ${MAKEFLAGS} 

#octomap
WORKDIR /root/
RUN git clone https://github.com/OctoMap/octomap.git
WORKDIR /root/octomap/build/
RUN cmake -D CMAKE_INSTALL_PREFIX=/app .. && make install ${MAKEFLAGS} 

FROM ros:noetic-perception AS final
LABEL maintainer="Mario Cristovao <mjpc13@protonmail.com>"
ARG MAKEFLAGS=-j$(nproc)
ENV DEBIAN_FRONTEND=noninteractive


RUN apt-get purge -y '*opencv*'
#Install custom compilation of OpenCV
COPY --from=opencv /app /usr/local
COPY --from=opt_dep /app /usr/local
# RUN ldconfig /app

#Install some ROS packages

# SHELL ["/bin/bash","-c"]

RUN apt-get update && apt-get -y install build-essential cmake git libgtk2.0-dev python-dev python3-numpy python3-pybind11 libeigen3-dev

# RUN apt-get install -y ros-$ROS_DISTRO-libg2o ros-$ROS_DISTRO-gtsam ros-$ROS_DISTRO-libpointmatcher


# ---OPTIONAL DEPENDENCIES---

# SLAM backends solvers

# g2o
# WORKDIR /root/
# RUN git clone https://github.com/RainerKuemmerle/g2o.git
# WORKDIR /root/g2o/build/
# RUN cmake -DG2O_BUILD_EXAMPLES=OFF .. && make install

#GTSAM

#OCTOMAP

WORKDIR /root/
RUN git clone https://github.com/introlab/rtabmap.git
WORKDIR /root/rtabmap/build/
RUN cmake -DWITH_PYTHON=ON -DBUILD_EXAMPLES=OFF WITH_CERES=ON ..
RUN make install ${MAKEFLAGS}

RUN apt-get update && apt-get install -y ros-noetic-rtabmap-ros \
  && rm -rf /var/lib/apt/lists/

# Install required packages

# RUN apt-get update && apt-get -y install build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev python-dev python3-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev


CMD ["bash"]
