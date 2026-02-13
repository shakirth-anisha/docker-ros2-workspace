FROM ros:humble
ARG USERNAME=ros2user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME
RUN useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

# Add sudo support
RUN apt-get -y update
RUN apt-get install -y sudo
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME
RUN chmod 0440 /etc/sudoers.d/$USERNAME
RUN apt-get update && apt-get upgrade -y

# Install a few important dependencies
RUN apt-get install -y \
    ament-cmake \
    ccls \
    python3-colcon-common-extensions \
    python3-pip \
    vim \
    clang \
    clang-format \
    clang-tidy \
    ros-humble-turtlesim \
    ros-humble-rqt*

RUN echo "source /opt/ros/humble/setup.bash" >> /home/$USERNAME/.bashrc
ENV SHELL /bin/bash

# Set the default user
USER $USERNAME
CMD ["/bin/bash"]
