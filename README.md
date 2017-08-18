# redis-trib_V2.rb
good link between slave and master to create cluster

# Init cluster between 3 servers
redis-server --port 7000 --cluster-enabled yes --dir 7000 --cluster-config-file nodes.conf --cluster-node-timeout 60 --appendonly yes --appendfilename appendonly.aof --dbfilename dump.rdb --logfile redis.log </br>
redis-server --port 7001 --cluster-enabled yes --dir 7001 --cluster-config-file nodes.conf --cluster-node-timeout 60 --appendonly yes --appendfilename appendonly.aof --dbfilename dump.rdb --logfile redis.log </br>

### install ruby
gem install redis

### create cluster
redis-trib_V2.rb create --replicas 1  10.122.102.56:7000 10.108.33.76:7000 10.108.31.200:7000 10.122.102.56:7001 10.108.33.76:7001 10.108.31.200:7001
(--replicas 1 => 1 slave / master)
(if you are 9 hosts, and --replicas 2 ==> 1-3 host = master // 4-9 = slave )

### vérification du cluster
redis-cli -p 7000 cluster nodes

### detail du résultat précédent
Node ID </br>
ip:port </br>
flags: master, slave, myself, fail, ... </br>
if it is a slave, the Node ID of the master </br>
Time of the last pending PING still waiting for a reply. </br>
Time of the last PONG received. </br>
Configuration epoch for this node (see the Cluster specification). </br>
Status of the link to this node. </br>
Slots served...

### lors d'une coupure d'un serveur, le slave du master devient master </br> pour rétablir le master sur le bon serveur, se positinner sur le serveur et le port étant en slave et devant être master.
redis-cli -h 10.108.33.76 -p 7000 </br>
CLUSTER FAILOVER FORCE
