## docker_Load_balacner
docker에 생성한 볼륨과 네트워크를 연결하여 로드벨런싱을 직접 구현 해보자!

## 네트워크 3개를 생성
- docker 네트워크 드라이브 유형을 bridge로 설정.
- subnet주소 범위도 각각 설정하고 컨테이너에 할당될 ip주소 범위를 172.100.x.0/24 지정.
- gateway주소 172.100.x.1로 설정
- 네트워크 이름을 custom-net[N]으로 설정

```
sudo docker network create --driver=bridge --subnet 172.100.1.0/24 --ip-range 172.100.1.0/24 --gateway 172.100.1.1 custom-net01
sudo docker network create --driver=bridge --subnet 172.100.2.0/24 --ip-range 172.100.2.0/24 --gateway 172.100.2.1 custom-net02
sudo docker network create --driver=bridge --subnet 172.100.3.0/24 --ip-range 172.100.3.0/24 --gateway 172.100.3.1 custom-net03
```

## 볼륨을 3개 생성
- docker 볼륨을 생성. 볼륨 이름은 volume[N]으로 설정.

```
sudo docker volume create volume01
sudo docker volume create volume02
sudo docker volume create volume03
```

## 웹서버와 네트워크, 볼륨을 각각 연결
- -itd 옵션은 사용자가 컨테이너 내부에서 터미널 세션을 통해 명령어를 입력하고 결과를 볼 수 있게 해주며, 컨테이너 실행을 백그라운드로 실행 시킨다.
- -v 옵션은 호스트에 생성한 볼륨파일 /home/kali/web[N]을 컨테이너의 /usr/share/nginx/html에 마운트 한다.
- --name은 컨테이너 명을 정의한다.
- --net은 생성한 네트워크를 할당한다
- --ip 옵션은 컨테이너의 고정 ip를 할당한다.
- nginx:1.18은 docker 이미지명 이다.

```
sudo docker run -itd -p 8001:80 -v /home/kali/web01:/usr/share/nginx/html --name=web01 --net=custom-net01 --ip 172.100.1.100 nginx:1.18
sudo docker run -itd -p 8002:80 -v /home/kali/web02:/usr/share/nginx/html --name=web02 --net=custom-net02 --ip 172.100.2.100 nginx:1.18
sudo docker run -itd -p 8003:80 -v /home/kali/web03:/usr/share/nginx/html --name=web03 --net=custom-net03 --ip 172.100.3.100 nginx:1.18
```

## 로드밸런싱 구축

```
```

