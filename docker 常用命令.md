// 查询所有镜像
docker images

// 查询镜像
docker images |grep 1.0.12.1

// 给镜像打标签 类似git 标签
docker tag bb09a6371196 10.65.4.2:5000/dolphin/dolphin-release:1.0.13.1

// 推送镜像到仓库
docker push 10.65.4.2:5000/dolphin/dolphin-release:1.0.13.1

// 拉取镜像
docker pull 10.65.4.2:5000/hi-dolphin_back_2018.06.26.14.24.57:201806201906

// 导出镜像
docker save -o hi-dolphin_back_2018.06.26.14.24.57.tar 10.65.4.2:5000/hi-dolphin_back_2018.06.26.14.24.57:201806201906

// 导入镜像
docker load --input hi-dolphin_back_2018.06.26.14.24.57.tar
或者 docker load < hi-dolphin_back_2018.06.26.14.24.57.tar

// 查看存在的所有容器
docker ps -a

// 删除该镜像
docker rmi 2eeae8debf3d
docker rmi -f 2eeae8debf3d

// 停止项目容器
docker stop dolphin-release-dev

// 删除项目容器
docker rm dolphin-release-de]v

// 启动项目容器
docker run -d

// 查找项目是否在docker存在
docker ps | grep dolphin-release

// 日志追踪
docker logs -f --tail 1000 2ccf5fa15ab2
//导出日志
docker logs -f 0d6926e7811c>> 20180904.txt
sudo docker logs -f 8c2c52a8361a>> 201801010.txt

// 检查项目内存状态
docker stats dolphin-release-dev

// 重启项目容器
docker ps | grep dolphin-release
docker restart 2eeae8debf3d

// 进入容器
docker exec -it e9ffcad70be9 /bin/bash

// 停止docker运行
docker stop dolphin-release-dev

// 删除容器
docker rm 2eeae8debf3d

// 导出容器
docker export -o test.tar 2eeae8debf3d
或 docker export 2eeae8debf3d > test.tar

// 导入容器
docker import test.tar - dolphin/dolphin-release:1.0.13