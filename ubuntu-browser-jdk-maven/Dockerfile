# docker build -t browser-jdk-maven:1.0 .
# MAINTAINER jzhang
FROM ubuntu:18.04
RUN apt-get update && apt-get install -y sudo vim wget

#firefox
RUN apt-get update && apt-get install -y firefox

#chrome 
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN dpkg -i google-chrome-stable_current_amd64.deb; apt-get -fy install
RUN apt-get install -y dbus-x11 packagekit-gtk3-module libcanberra-gtk-module

# jdk
RUN cd /tmp && \
wget --no-check-certificate -c --header "Cookie: oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/10.0.2+13/19aef61b38124481863b1413dce1855f/jdk-10.0.2_linux-x64_bin.tar.gz" && \ 
tar -xvzf /tmp/jdk-10.0.2_linux-x64_bin.tar.gz -C /opt && \ 
rm -rf /tmp/jdk-10.0.2_linux-x64_bin.tar.gz

ENV JAVA_HOME=/opt/jdk-10.0.2 \ 
    PATH=${PATH}:/opt/jdk-10.0.2/bin:/opt/jdk-10.0.2
 
#maven
RUN apt-get install -y maven

# Replace 1000 with your user / group id
 RUN export uid=1000 gid=1000 && \
    mkdir -p /home/gre && \
    echo "gre:x:${uid}:${gid}:gre,,,:/home/gre:/bin/bash" >> /etc/passwd && \
    echo "gre:x:${uid}:" >> /etc/group && \
    echo "gre ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/gre && \
    chmod 0440 /etc/sudoers.d/gre && \
    chown ${uid}:${gid} -R /home/gre && \
    cp /root/.bashrc /home/gre/.bashrc && \
    cp /root/.profile /home/gre/.profile

RUN mkdir /var/run/dbus/

USER gre
ENV HOME /home/gre
CMD sudo dbus-daemon --system --fork && /usr/bin/firefox

# RUN apt-get -qy autoremove
