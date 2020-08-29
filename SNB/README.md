# LiveGraph-SNB-driver

## Dependence
```bash
sudo apt-get update && sudo apt-get -y install openjdk-11-jre curl tar python
curl https://downloads.apache.org/hadoop/common/hadoop-2.10.0/hadoop-2.10.0.tar.gz | tar -xz
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:/bin/java::") && export HADOOP_HOME=$PWD/hadoop-2.10.0
```
- [Thrift 0.12.0](https://thrift.apache.org/)

## Source
Due to the licenses, please download and build the source codes from :
- [ldbc_snb_datagen 0.2.7](https://github.com/ldbc/ldbc_snb_datagen)
- [livegraph_driver](https://github.com/jiguanglizipao/ldbc_snb_implementations)

## Usage
```bash
export LiveGraphSNB=$PWD

# Generate SNB data
cd $LiveGraphSNB/datagen
./run.sh

# Start LiveGraph SNB server
# Wait for importing....

# Run LiveGraph-SNB-driver
cd $LiveGraphSNB/driver
# write $LiveGraphSNB into interactive-benchmark.properties
sed -i "s:LiveGraphSNB:$LiveGraphSNB:g" interactive-benchmark.properties
# change threads and other parameters in interactive-benchmark.properties
./interactive-benchmark.sh
```


# LiveGraph-SNB-server

## Dependence
The binaries are built and tested on `ubuntu 18.04`.

```bash
sudo apt-get update && sudo apt-get -y install libtbb2 libgomp1
```

## Usage
```bash
# Generate SNB data

# Start LiveGraph SNB server
mkdir -p LIVEGRAPH_DATAPATH # on a fast storage like NVME SSD
TZ=UTC ./snb_server LIVEGRAPH_DATAPATH $LiveGraphSNB/datagen/social_network 9090
# Wait for importing....

# Run LiveGraph-SNB-driver
```
