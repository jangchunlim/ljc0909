도커 명령어 모음

도커 이미지 가져오기 : docker pull [image-name]:[tag]

도커 이미지 보기 : docker images

도커 이미지 생성 + 실행 : docker run -name 

도커 컨테이너 나가기(컨테이너 종료하지 않고 나가기) : control + p + q, exit

도커 컨테이너 생성 : docker create -i -t centos:7
-i : 상호 입출력
-t : tty 활성화 하여 bash 쉘 사용

도커 컨테이너 실행 : docker start centos:7

도커 컨테이너 들어가기 : docker attach centos:7

도커 컨테이너 만들고 실행 : docker run [옵션] [이미지 이름]:[태그] 

도커 컨테이너 목록 확인 : docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
99627ff61fd7        centos:7            "/bin/bash"         42 hours ago        Exited (0) 42 hours ago                         mycentos
1758dcf92334        ubuntu:14.04        "/bin/bash"         43 hours ago        Exited (255) 25 hours ago                       determined_brattain
CONTAINER ID : 컨테이너에게 자동으로 할당되는 고유한 ID
IMAGE : 컨테이너를 생성할 때 사용된 이미지 이름
COMMAND : 컨맨드는 컨테이너가 시작될 때 실행될 명렁어, 기본은 /bin/bash 명령어라 명령을 쓸 수 있습니다.
CREATED : 컨테이너가 생성되고 난 뒤 흐른 시간
STATUS : 컨테이너의 상태 ex) Up(실행 중), Exited(종료), Pause(일시 중지)
PORTS : 컨테이너가 개방한 포트와 호스트에 연결한 포트
NAMES : 컨테이너의 고유한 이름, --name 옵션으로 이름을 설정하지 않으면 도커 엔진이 임의의로 설정

도커 컨테이너 삭제 : docker rm mycentos

도커 컨테이너 정지 : docker stop mycentos

도커 포트 할당 : docker run -i -t --name mywebserver -p 80:80 ubuntu:14.0
도커 아이피 할당 : docker run -i -t -p 3306:3306 -p 192.168.0.100:7777:80 ubuntu:14.04
