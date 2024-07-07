## Docker

### Installation
1. Install [Docker](https://www.docker.com/products/docker-desktop/) desktop.

2. Check version in Terminal.
```bash
docker -v
```

3. Test by pull image.
```bash
docker pull hello-world
```

4. Check all images
```bash
docker images
```

5. Run image
```bash
docker run hello-world
```

### Basic Usage

1. Create file name **Dockerfile**

2. Write base image for run time (Example)
```bash
# ทำการเลือก base image (จาก docker hub) มาเป็นตัว runtime เริ่มต้น เพื่อให้สามารถ run project ได้
# ในทีนี้เราทำการเลือก node image version 18 ออกมา
FROM node:18

# กำหนด directory เริ่มต้นใน container (ตอน run ขึ้นมา)
WORKDIR /usr/src/app

# ทำการ copy file package.json จากเครื่อง local เข้ามาใน container
COPY package.json ./

# ทำการลง dependency node
RUN npm install

# copy file index.js เข้ามาใน container
COPY index.js ./

# ทำการปล่อย port 8000 ออกมาให้ access ได้
EXPOSE 8000

# กำหนด command สำหรับเริ่มต้น run application (ตอน run container)
CMD ["node", "index.js"]
```

3. Build image
```bash
docker build -t <image name> -f <Dockerfile name> <Dockerfile root path>

docker build -t node-server -f ./Dockerfile .
# or
docker build -t node-server .
```

4. Run image (background) or mapping port
```bash
docker run -d -p 8000:8000 node-server
```

5. Docker log
```bash 
docker logs ecstatic_poincare (process name)
```

**Clear docker process**
use when forgot stop running your docker command with ctrl+c
```bash
# check list all running process
docker ps

# kill process
docker rm -f $(docker ps -a -q)
```

### Docker compose
Use when need to run many images in one time

1. Create file name **docker-compose.yml**
```bash
version: '3' # กำหนด docker version
services:
  node-server: # ตั้งชื่อ container (เหมือน --name)
    build: . # ตำแหน่ง dockerfile
    ports:
      - "8000:8000" # map port ออกมา เหมือน -p ใน docker run 
```

2. Run docker compose
```bash
# case when first run and Dockerfile, docker-compose.yml have something change
docker-compose up -d --build

# run 
docker-compose up -d
```

### Docker volume
mapping env from VM(docker) to local

1. In file **docker-compose.yml** 
```bash
...
    db:
        ...
        volumes:
            - mysql_data_test:/var/lib/mysql

volumes:
    mysql_data_test: #กำหนดชื่อ
        driver: local
```

2. Check all docker volumes
```bash
docker volume ls
```

3. Delete docker volume 
```bash
docker volume rm <volume name>
```

### Docker db command
```bash
docker exec -it db sh

mysql -u root -p
```
[Mikelopster Docker](https://docs.mikelopster.dev/c/basic/docker/intro)
[Docker Document](https://docs.docker.com/guides/getting-started/)