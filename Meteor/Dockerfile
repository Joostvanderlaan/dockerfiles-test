FROM ubuntu

# File Author / Maintainer
MAINTAINER Joost van der Laan "joostvanderlaan@gmail.com"

RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get -y upgrade

#### BASE END
 
RUN apt-get install -y openssl vim tmux git-core curl
RUN apt-get install -y python-software-properties python
RUN add-apt-repository ppa:chris-lea/node.js
RUN echo "deb http://us.archive.ubuntu.com/ubuntu/ precise universe" >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade
RUN apt-get install -y nodejs
RUN curl https://install.meteor.com | sh -x
RUN npm install -g meteorite
RUN mkdir -p /var/www

RUN git clone git@github.com:SachaG/Microscope.git
RUN cp -r Microscope/* /var/www

CMD cd /var/www mrt