# from https://github.com/osrf/docker_images/blob/master/ros/melodic/ubuntu/bionic/ros-core/Dockerfile
FROM ubuntu:bionic

# local mirror
RUN sed -i -e 's/archive\.ubuntu\.com/mirror\.binarylane\.com\.au/' /etc/apt/sources.list

# setup timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata && \
    rm -rf /var/lib/apt/lists/*

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    dirmngr \
    gnupg2 \
    && rm -rf /var/lib/apt/lists/*

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros1-latest.list

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

ENV ROS_DISTRO melodic

# install ros packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-melodic-desktop-full=1.4.1-0* \
    git \
    && rm -rf /var/lib/apt/lists/*



# setup entrypoint
COPY ./ros_entrypoint.sh /
RUN chmod a+x /ros_entrypoint.sh

RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> /root/.bashrc

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]