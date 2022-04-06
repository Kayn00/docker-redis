docker run --restart=always -p 6373:6379 --name redis6373 -v /Users/lx/docker-redis/cluster/redis6373.conf:/etc/redis/redis.conf -d redis:3.0.0 redis-server /etc/redis/redis.conf --appendonly yes

./redis-cli -h 127.0.0.1 -p 6371 cluster addslots $(seq 0 5000)
./redis-cli -h 127.0.0.1 -p 6372 cluster addslots $(seq 5001 10000)
./redis-cli -h 127.0.0.1 -p 6373 cluster addslots $(seq 10001 16383)