Bootstrap:docker  
From:ubuntu:20.04

%labels
MAINTAINER Vanessasaur
SPECIES Dinosaur

%environment
RAWR_BASE=/code
export RAWR_BASE

%runscript
echo "This gets run when you run the image!" 
exec /bin/bash /code/rawr.sh "$@"  

%post  
echo "This section happens once after bootstrap to build the image."  
mkdir -p /code  
apt update
apt-get install -y vim  
echo "RoooAAAAR" >> /code/rawr.sh
chmod u+x /code/rawr.sh  
