# Django uWSGI nginx Postgres docker-compose 一键部署

版本 v0.1


## 产品说明

在任意安装了 docker 和 docker compose 的机器，只要在与
docker-compose.yml 同级的目录下运行 docker-compose up -d
即可一键自动部署 django nginx uwsgi postgres

部署成功后，会生成3个容器，django、nginx、postgres，每个容器映射到本机的 80 8000 5432 端口

为了保证即开即用，数据库（无任何数据）和 django 程序文件已经初始化完成，用户可以根据自己的需求自行调整



## 实现目标

- 保证开发环境和生产环境高度统一
- 配置文件全挂载，方便实时修改
- 将数据库、配置文件、软件环境、代码文件隔离
- 一键部署，帮助新手用户参考，度过困难期



## 使用说明

- --config\       # 配置文件目录，可以实时修改，修改完成后，重启容器即可生效

- --database\     # 数据库文件夹，已经预先初始化一个叫 psndb 的数据库

- --docker-compose.yml    # docker-compose 文件，用来描述容器的启动，容器和宿主机文件的挂载，以及容器相关配置

- --source\       # django 代码的位置

将 database 与 source 解压，然后在安装了 docker 和 docker compose 的机器输入 docker-compose up -d 即可。



## 软件版本

- django:2.0.4

- centos:7.4(django 镜像中)

- python3.6.4(django 镜像中)

- nginx:1.13.12

- postgres:10.3

- docker-compose version 1.21.0

- docker Version:18.03.0-ce



### 重要提示

#### 如何修改数据库密码
- 方法1：
  删除 database 文件夹，然后编辑 docker-compose.yml，注释掉下面2行，填写自己需要的密码
  
  `# - ./config/postgres/postgresql.conf:/var/lib/postgresql/data/postgresql.conf`
  
  `# - ./config/postgres/pg_hba.conf:/var/lib/postgresql/data/pg_hba.conf`
  
  POSTGRES_PASSWORD:newpass123456
  
  运行后，把 django 的 settings.py 中的密码也一并修改，再把注释的内容恢复再运行一遍 docker-compose up -d 即可

- 方法2：
  进入 postgres 容器，然后手动修改密码
  
  `alter user postgres with password 'newpass123456';`
  
  再同步修改掉 docker-compose.yml 以及 django 的 settings.py 中的密码
  

#### django 后台密码
- django 后台 admin 用户的初始化密码 password123，切记需要修改

#### docker-compose.yml 版本无法识别
- 如果你的系统提示，docker-compose.yml 版本无法识别，可能是你的 docker-compose 太旧，把 version 3 改成 2 即可。





