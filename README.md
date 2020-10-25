# Docker-compose-node-app

docker compose 기능을 이용한 간단한 node-redis app.

## Getting Started

```shell script
docker-compose up
```

--------------------------------------------------

### docker-compose

다중 컨테이너 도커 애플리케이션을 정의하고 실행하기 위한 도구이다.

레디스 클라이언트가 작동하려면 레디스 서버가 켜져있어야 하기 때문에 먼저 레디스 서버를 위한
컨테이너를 실행하고 node.js 를 위한 컨테이너를 실행해야 한다.

```shell script
docker run redis
docker build -t kkangil/docker-compose-app ./
docker run kkangil/docker-compose-app
``` 

node 서버를 실행할때 레디스 서버에 연결할 수 없다는 에러가 발생한다.

서로 다른 컨테이너에 있는데 컨테이너 사이에는 아무런 설정없이는 접근을 할 수 없기에
node.js 앱에서 레디스 서버에 접근을 할 수 없다.
이때 멀티 컨테이너 상황에서 쉽게 네트워크를 연결시켜주기 위해서 Docker Compose 를 이용하면 된다.

### 도커 환경에서 레디스 클라이언트 생성시 주의 사항

보통 도커를 사용하지 않는 환경에서는 레디스 서버가 작동되고 있는 곳의 host 옵션을 URL 로 주면 되지만,
도커 compose 를 사용할 때는 host 옵션을 docker-compose.yml 파일에 명시한 컨테이너 이름으로 주면된다.

### docker-compose.yml

```markdown
version: docker compose 의 버전
services: 실행하려는 컨테이너들을 정의
├── redis-server: 컨테이너 이름
│   └── image: 컨테이너에서 사용하는 이미지
├── node-app: 컨테이너 이름
│   └── build: 해당 디렉토리에 있는 Dockerfile 사용
│   └── ports: 포트 맵핑 로컬포트:컨테이너포트
```

### docker-compose 명령어

```shell script
docker-compose build  # 이미지를 빌드하기만 하며 컨테이너를 시작하지는 않음.
docker-compose up # 이미지가 존재하지 않는 경우에만 빌드하며, 컨테이너 시작
docker-compose up --build # 필요치 않을때도 강제로 빌드하며, 컨테이너 시작
docker-compose up --no-build  # 이미지 빌드 없이 컨테이너 시작(이미지 없을시 실패)
```

### docker compose 컨테이너 멈추기

docker compose 를 통해 작동시킨 컨테이너들을 한꺼번에 중단 시키려면 
docker-compose down 으로 한다.

```shell script
docker-compose down
```

중단 시키기 위해서 두개의 터미널을 사용해야 하는데, 두개의 터미널을 사용하지 않고 하나만 사용하고 싶다면
 -d: Detached 모드로서 앱을 백그라운드에서 실행시킨다.
 해당 모드는 앱에 나오는 output 을 표출하지는 않는다.
 
 ```shell script
docker-compose up -d
```
