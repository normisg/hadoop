# Prerequisites
- docker
- docker-compose
- python3 + virtualenv

# Run map-reduce and spark jobs locally (plot.txt.gz is my large file)
```bash
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
python ./mr.py -r local plot.txt.gz
python ./spark.py plot.txt.gz
```

# Test hadoop in docker
```bash
git clone git@github.com:big-data-europe/docker-hadoop.git
cd docker-hadoop
docker-compose pull
docker-compose up -d
docker ps -a | grep namenode
docker cp plot.txt.gz namenode:/tmp/
docker exec -it namenode bash
hadoop fs -mkdir /test
hadoop fs -Ddfs.blocksize=64M -put /tmp/plot.txt.gz /test
hadoop fsck /test/plot.txt.gz -files -blocks
hadoop fs -rm /test/plot.txt.gz
```