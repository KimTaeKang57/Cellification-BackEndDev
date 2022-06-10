# AWS EC2

Local에서 작동이 잘 되는지 확인 하고 서비스들을 하나하나 Docker image로 만들어 Docker hub에 push하여 run 하여 Container화 시켰다.
우리 Cellification은 마이크로서비스 아키텍처 기능을 사용해 개발을 진행하였는데, Local로 진행하였을 때는 같은 network 상이어서 문제없이 통신이 이루어졌지만 Docker Container들 끼리의 
네트워크 통신이 이루어지지 않았다. 그래서 서버안에서 자체적으로 network를 만들어 프로젝트들간 네트워크 통신을 하였다.

### network 이름 : ecommerce-network
- 현재 network에 연결되어있는 마이크로서비스 아키텍처들 (명령어 : docker inspect network ecommerce-network)

<img width="700" alt="스크린샷 2022-06-10 오후 1 31 12" src="https://user-images.githubusercontent.com/83891837/172990665-94eb2081-738f-44b0-be7c-50f521edd7d0.png">

# 문제 발생
- AWS의 무료 Ec2 서버를 사용하다 보니, 로그인 회원가입등 가벼운 기능은 정상적으로 작동이되었지만 세포 계수 분석을 진행하는 딥러닝 기반의 인공지능을 진행할때에는 어김없이 서버가 죽어버리는 모습을 볼 수 있었다.
### 요청이 몇십분 단위로 길어지고 나오는 결과..
<img width="700" alt="스크린샷 2022-06-10 오후 1 33 20" src="https://user-images.githubusercontent.com/83891837/172990881-993b7c77-8e37-4a09-9377-dd5e0f3ef1f9.png">

- 일단 본인의 개발환경은 Mac m1칩을 사용한 개발환경이었다. 인공지능 파트를 담당한 한동현 학생은 Cuda, Cudnn등을 활용하여 GPU를 사용하였기 때문에 오랜시간이 걸리지 않았지만, Mac m1은 GPU 자체가 없는 환경이기 때문에 무조건 CPU를 활용하여야 했다. (그런데 로컬에서도 1분30초~2분이 걸렸는데... 무료 Ec2 서버는 당연히...)

그래서 무료 Ec2 서버를 사용하지 않고! 30만 크레딧을 지원받은 Naver Cloud Platform의 유료 서버를 이용하기로 하였다

# 문제 해결
일단 충분한 vCPU와 RAM이 필요하였기 때문에 비교적 좋은 성능의 서버를 선택하였다.
Centos -7.3.64 [Standard] 4vCPU, 4GB Mem, 50GB Disk [g1] : 월 10만원에 가까운 요금이었던 것 같다.

### 무료 서버가 아닌 유료서버를 이용한 결과 : 로컬과 같은 속도가 나오진 않지만 1분30초 안에 요청이 반환되었다!!
<img width="600" alt="스크린샷 2022-06-10 오후 1 38 45" src="https://user-images.githubusercontent.com/83891837/172991382-16e2de14-a1a8-40a8-82eb-a582ec8d1504.png">
