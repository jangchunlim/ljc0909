도커 명령어 모음

도커 이미지 가져오기 : docker pull [image-name]:[tag]

도커 이미지 보기 : docker images

도커 이미지 생성 + 실행 : docker run -name 

도커 이미지 가져오기
아래 명령어를 사용하면 도커 공식 이미지 저장소에서 이미지를 내려받습니다.

$ docker pull [이미지 이름]:[태그]
예제를 보도록 하겠습니다, 그럼 도커 공식 이미지 저장소에서 centos:7 이미지를 다운받게 됩니다.

ex)

$ docker pull centos:7
이미지 확인하기
$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              14.04               7e4b16ae8b23        2 weeks ago         188MB
centos              7                   1e1148e4cc2c        5 weeks ago         202MB
컨테이너 생성하기
이미지로 컨테이너 생성하기

$ docker create [옵션] [이미지 이름]:[태그]
예제를 보겠습니다.

$ docker create -i -t centos:7
-i : 상호 입출력
-t : tty를 활성화하여 bash 쉘을 사용
컨테이너 실행하기
만들어진 도커 컨테이너를 실행하는 명령어 입니다.

$ docker start centos:7
컨테이너 들어가기
도커의 내부쉘로 들어가는 명령어 입니다.

$ docker attach centos:7
컨테이너 만들고 실행하기
위의 생성 -> 실행 -> 들어가기 까지 한번에 해주는 명령어 입니다.

$ docker run [옵션] [이미지 이름]:[태그] 
-i : 상호 입출력
-t : tty를 활성화하여 bash 쉘을 사용
ex)

$ docker run -i -t ubuntu:14.04
보통은 컨테이너를 생성함과 동시에 시작하기 때문에 run 명령어를 더 많이 사용합니다.

컨테이너 목록 확인
만들어진 컨테이너 목록을 확인할 수 있습니다.

$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                NAMES
aab67382350b        ubuntu:14.04        "/bin/bash"         16 minutes ago      Up 16 minutes       0.0.0.0:80->80/tcp   mywebserver
대신 위의 명령어는 현재 실행중인 컨테이너 목록만 확인할 수 있기 때문에, 정지된 컨테이너까지 보고 싶으면, 아래와 같이 -a 옵션을 같이 넣어줍니다.


 
$ docker ps -a
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
컨테이너 이름 변경
$ docker rename [기존 이름] [변경 하고자 하는 이름]
$ docker rename determined_brattain my_container
결과

$ docker rename determined_brattain my_container
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
99627ff61fd7        centos:7            "/bin/bash"         42 hours ago        Exited (0) 42 hours ago                         mycentos
1758dcf92334        ubuntu:14.04        "/bin/bash"         44 hours ago        Exited (255) 25 hours ago                       my_container
컨테이너 삭제
$ docker rm [컨테이너 이름]
결과

$ docker rm mycentos
mycentos
만약 container가 실행 중이면 종료하고 삭제를 하거나 -f 옵션을 이용해서 강제로 삭제를 해야합니다.

$ docker stop mycentos
$ docker rm mycentos
$ docker rm -f centos
한번에 모든 컨테이너를 삭제하려면 prune 명령어를 통해 삭제할 수 있습니다.

$  docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N]
*** -q와 -a 옵션

-q : 컨테이너 ID만 출력
-a : 컨테이너 전체 출력
ex)

$ docker stop $(docker ps -a -q)
$ docker rnm $(docker ps -a -q)
컨테이너 외부에 노출
컨테이너는 가상 머신과 마찬가지로 가상 IP를 할당 받습니다. 테스트를 위해서 network_test란 컨테이너를 만들어 보겠습니다.

기본적으로 도커는 172.17.0.x의 IP부터 순차적으로 할당합니다.

$ docker run -i -t --name network_test ubuntu:14.04
root@7b5213595de1:/# ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:ac:11:00:02
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:17 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1366 (1.3 KB)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

아무 설정을 하지 않으면 기본적으로 컨테이너는 외부에서 접근할 수 없으며 도커가 설치된 호스트에서만 접근 가능합니다.


 
외부에 컨테이너의 어플리케이션을 노출하기 위해서는 eth0의 IP와 포트를 호스트의 IP와 포트에 바인딩해야 합니다.

테스트를 위해 아래의 새로운 컨테이너를 생성하는데 -p 옵션을 이용해 컨테이너의 포트를 호스트의 포트와 바인딩해 연결할 수 있게 설정합니다.

-p [호스트의 포트]:[컨테이너의 포트]
$ docker run -i -t --name mywebserver -p 80:80 ubuntu:14.04
만약 여러개의 포트 설정이 필요하면 -p 옵션을 여러번 사용 할 수 있고, 호스트의 특정 IP를 사용하려면 192.168.0.100:7777:80과 같이 바인딩할 IP와 포트를 직접 명시할 수도 있습니다.
