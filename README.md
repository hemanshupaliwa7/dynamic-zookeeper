# dynamic-zookeeper
Zookeeper Cluster Image

# Create Attachable network for zookeeper cluster
docker network create --driver=overlay --attachable aqua_zookeeper

# Build the image 
docker build -t dynamic-zookeeper .

# Push Image to the local repository
docker tag dynamic-zookeeper aquahub:5000/aquaduct/dynamic-zookeeper
docker push aquahub:5000/aquaduct/dynamic-zookeeper

# Create First Instance of CLuster
Run this on first node:
docker run -d --name zk1 --net=aqua_zookeeper --restart always -p 2181:2181 aquahub:5000/aquaduct/dynamic-zookeeper 1 

Here, 1 denotes the zookeeper id 
Now, we need the container IPAddrees of 1st instance of zookeeper in order to connect to other instances of zkookeeper.

docker inspect zk1 | grep IPAddress 

# Create Second Instance of Cluster 
Run this on Second node:
docker run -d --name zk2 --net=aqua_zookeeper --restart always -p 2181:2181 aquahub:5000/aquaduct/dynamic-zookeeper 2 <ip-address-from above docker command>
  
# Create Third Instance of Cluster 
Run this on Third node:
docker run -d --name zk3 --net=aqua_zookeeper --restart always -p 2181:2181 aquahub:5000/aquaduct/dynamic-zookeeper 3 <ip-address-from above docker command>
  
Like this you can add any number of zookeper instances in the cluster, always procide unique zookeeper id for on each node. And cluster should always have minimum two instances in a cluster, one being Leader and other being Follower
