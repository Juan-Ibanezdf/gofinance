docker exec -it gofinance-backend sh

make migrateup


docker images -a
docker container prune
