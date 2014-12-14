# Model from https://github.com/geo-data/node-mapcache
#
# This creates an Ubuntu derived base image that installs a recent
# version of Apache as well as the latest repository checkouts of
# Mapproxy. Mapproxy has a  broad range of compile time options enabled 
# and as such this provides a suitable base image for both 
# experimenting with and # deriving production ready images from.
#

FROM ubuntu:14.04

MAINTAINER pamtrak06 <pamtrak06@gmail.com>

# Ensure the package repository is up to date
RUN echo "deb http://ppa.launchpad.net/kubuntu-ppa/backports/ubuntu utopic main" >> /etc/apt/sources.list
RUN apt-get update

# Setting Apache2 environment variables
RUN source /etc/apache2/envvars
# Install Apache devel
RUN sudo apt-get install apache2-threaded-dev
# Install Apache
RUN apt-get install -y apache2

# Apache2 configuration for mapcache
#RUN echo "LoadModule mapcache_module    /usr/lib/apache2/modules/mod_mapcache.so" > /etc/apache2/mods-available/mapcache.load

#RUN echo "<IfModule mapcache_module>" > /etc/apache2/mods-available/mapcache.conf
#RUN echo "   <Directory /etc/apache2/conf-available>" >> /etc/apache2/mods-available/mapcache.conf
#RUN echo "      Require all granted" >> /etc/apache2/mods-available/mapcache.conf
#RUN echo "   </Directory>" >> /etc/apache2/mods-available/mapcache.conf
#RUN echo "   MapCacheAlias /mapcache \"/etc/apache2/conf-available/mapcache.xml\"" >> /etc/apache2/mods-available/mapcache.conf
#RUN echo "</IfModule>" >> /etc/apache2/mods-available/mapcache.conf

# Enable mapcache module
#RUN a2enmod mapcache

# ------------------------------------
# Install python environement
# ------------------------------------
# Install pip
RUN echo "Y" | apt-get install python-pip
# Install python dev
RUN echo "Y" | apt-get install python-dev
# Install setuptools
RUN echo "Y" | apt-get install curl
RUN curl https://bootstrap.pypa.io/ez_setup.py -o - | python
# Install virtualenv
RUN pip install virtualenv
# ------------------------------------

# ------------------------------------
# Install mapproxy dependencies
# ------------------------------------
# Install lib proj
RUN pip install pyproj
# Install lib pil
RUN echo "Y" | sudo apt-get install build-essential libjpeg-dev \
  zlib1g-dev libfreetype6-dev
RUN pip install Pillow
# Install lib yaml
RUN pip install PyYAML
# Install geos
RUN apt-get libgeos++-dev libgeos-3.4.2 libgeos-c1 libgeos-dbg libgeos-dev
# Install Shapely and GEOS (optional)
RUN pip install Shapely
# Install gdal (optional)
RUN apt-get install python-gdal
# Install lxml (optional)
RUN echo "Y" | apt-get install libxml2-dev libxslt1-dev
RUN pip install lxml
# ------------------------------------

# ------------------------------------
# Install mapproxy
# ------------------------------------
RUN pip install MapProxy
# ------------------------------------

# ------------------------------------
# Install third-party
# ------------------------------------
# Install OGC library
RUN pip install OWSLib
# ------------------------------------


# Set path for mapcache file
#RUN cd /etc/apache2/conf-available/
# Download py library to produce mapcache.xml from a wms url
#RUN curl -O https://raw.githubusercontent.com/pamtrak06/mapcache/master/mapcache.py
# Generate mapcache.xml sample
#RUN python mapcache.py --wms http://geo.weather.gc.ca/geomet/?lang=E --prj mapcache

# Download example 
#RUN curl -O https://raw.githubusercontent.com/pamtrak06/mapcache/master/mapcache.xml

# Create temp directory for mapcache tiles
#RUN rm -rf /tmp/mapcache
#RUN mkdir /tmp/mapcache
#RUN chmod 777 /tmp/mapcache

# Configure localhost
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# start apache
RUN apachectl start

#EXPOSE 80
#VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"]
#ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

# test
# curl http://localhost/mapcache/demo

# Install Firefox
#RUN apt-get install -y firefox
