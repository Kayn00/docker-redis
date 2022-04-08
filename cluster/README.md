# 启动6371-80 十个容器
docker-compose up -d 

# 配置6371-6集群
docker exec -it redis-6371 /bin/bash
redis-cli --cluster create 172.26.0.101:6379 172.26.0.102:6379 172.26.0.103:6379 172.26.0.104:6379 172.26.0.105:6379 172.26.0.106:6379 --cluster-replicas 1

# 6377-80加入集群两个不同方式设置主从
docker exec -it redis-6377 /bin/bash
redis-cli -c -p 6379
CLUSTER MEET 172.26.0.107 6379
CLUSTER MEET 172.26.0.108 6379
# 6378主 6377从
CLUSTER REPLICATE 6378<node-id> 

# 或者
docker exec -it redis-6371 /bin/bash
redis-cli -c  --cluster add-node 172.26.0.109:6379 172.26.0.101:6379
# 6379主 6310从
redis-cli -c --cluster add-node 172.26.0.110:6379 172.26.0.109:6379 --cluster-slave --cluster-master-id  6379<node-id>

# 重新分配哈希槽
redis-cli --cluster reshard 172.26.0.101:6379
输入需要迁移的槽点数
复制粘贴需要被接收的主节点
复制粘贴从哪些节点拿槽，填多个可以从多个槽拿，done结束


redis-cli --cluster create 172.26.0.101:6379 172.26.0.102:6379 172.26.0.103:6379 172.26.0.104:6379 172.26.0.105:6379 172.26.0.106:6379 172.26.0.107:6379 172.26.0.108:6379 172.26.0.109:6379 172.26.0.110:6379 --cluster-replicas 1