command: redis-server --port 6380 --slaveof redis-master 6379  --requirepass redis2 --masterauth redis1  --appendonly yes

docker network inspect redisNet 

slaveof 172.21.0.2 6379