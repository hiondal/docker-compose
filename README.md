# Docker-compose를 이용한 Build/Deploy

## 작업환경 구성
- sc, subrecommend, subride-front 프로젝트 remote git repo에 푸시  
  - github에 remote git repo 생성  
  - local git 생성 및 remote git 연결  
    ```
    git init
    git checkout -B main
    git remote add origin {remote git repo 주소}  
    ``` 
  - Push
    ```
    git add . && git commit -m "release" && git push -u origin main 
    ```

- 작업 VM 로그인 
  본인 OS 계정으로 로그인
  ssh 로그인 설정은 [여기](https://github.com/cna-bootcamp/cna-handson/blob/main/prepare/%EB%A1%9C%EC%BB%AC%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD%EA%B5%AC%EC%84%B1.md#ssh-login-%EC%84%A4%EC%A0%95)를 참고 하세요. 
  ```
  ssh user00
  ```

- 작업 디렉토리 생성
  ```
  mkdir -p ~/work && cd ~/work
  ```

- Docker compose 관련 파일 clone
  ```
  git clone https://github.com/hiondal/docker-compose.git 
  cd docker-compose
  ```
- 파일 내용 변경  
  본인의 OS계정으로 값을 변경  
  ```
  sed -i'' "s/user00/user01/g" .env
  ```
  ```
  sed -i'' "s/user00/user01/g" docker-compose.yml
  ```
- 환경변수 파일 수정  
  .env 파일을 열고 아래 내용 수정  
  ```
  #Image Organization: Organization 수정 필요 
  IMAGE_ORG=hiondal
  IMAGE_VERSION=2.0.0

  #Frontend service: 포트 번호 수정-30{OS계정 끝 2자리}
  FRONT_HOST=http://user00.13.215.90.232.nip.io:3000
  FRONT_PORT=3000

  #Git Config Git: URL, USERNAME, TOKEN 수정 필요 
  GIT_URL=https://github.com:443/hiondal/subride-config.git
  GIT_USERNAME=hiondal
  GIT_BRANCH=main
  GIT_TOKEN=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
  ENCRYPT_KEY=CL4cboqlIweOqt93wjzZi/qjCxcSOYAMgzdKiy6cG2Y=

  #SCG:외부포트수정: 190{OS계정 끝 2자리}
  SCG_HOST=http://user00.13.215.90.232.nip.io:19000
  SCG_PORT=19000

  #Frontend service: 포트 번호 수정-30{OS계정 끝 2자리}
  FRONT_HOST=http://user01.13.215.90.232.nip.io:3000
  FRONT_PORT=3000

  #SCG:외부포트수정: 190{OS계정 끝 2자리}
  SCG_PORT=19000
  ```

## sc, subrecommend, subride-front 소스 클론  
먼저 PC에서 작업한 sc, subrecommend 프로젝트를 remote git repo에 푸시 해야 함  
hiondal은 본인의 Git organization으로 변경해야 함  
```
git clone https://github.com/hiondal/sc.git 
```

```
git clone https://github.com/hiondal/subrecommend.git 
```

```
git clone https://github.com/hiondal/subride-front.git 
```

## 기존에 비정상 종료된 container 삭제  
```
docker-compose down --remove-orphans
```

## Build jar 
```
docker-compose -f buildjar.yml up
```

## Build/Push container image 
```
docker-compose build
```
본인 login id로 docker hub 로그인  
```
docker login -u hiondal
```

```
docker-compose push
```

## Run container
기존 실행 중인 container 모두 중지  
```
docker ps -a
docker stop {id 또는 name}
```

기존 로컬의 이미지 모두 삭제  
```
docker container prune 
docker image prune -a
```

```
docker-compose up -d
```

상태 보기
```
docker-compose ps
```


## 테스트
subride-front 주소로 브라우저로 접근하여 테스트 함  

## Stop container  
```
docker-compose down 
```


