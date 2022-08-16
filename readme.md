# 🔍 Cellification 🔎 (인공지능을 활용한 세포 계수 분석 서비스)
2022년 1학기 경기대학교 캡스톤 백엔드 개발 😎

# 주제 선정 및 개요
기업 솔이 제안한 인공지능을 활용한 세포 계수 분석 앱 혹은 애플리케이션을 제작해달라는 산합협력 주제를 캡스톤 팀 주제로 선정하여 개발을 진행하였습니다. 살아있는 세포와 죽어있는 세포가 이리저리 퍼져있는 사진을 업로드하면, Cellification이 살아있는 세포와 죽어있는 세포를 딥러닝 기반의 인공지능으로 구분하여 살아있는 세포와 죽어있는 세포의 계수와 비율을 결과로 알려줍니다.

# 기능 설명
- 로그인 / 회원가입 / 회원탈퇴
- 세포 원본 이미지 분석
- 이미지 분석 결과 화면
- 이미지 분석 결과 저장(내부 저장소, DB) 
- 이미지 분석 결과 

# 🖥 기술 스택
- Micro Service Architecture 구조를 활용하여 독립적으로 배포가능한 서비스로 나누어 각각의 서비스를 Restful API로 데이터를 주고받음
- Naver Cloud Platform 리눅스 서버에 각 서비스 배포
- Docker Mariadb로 데이터 관리
- Docker image, container
- Restful API 서버 구현 : Spring Framework, Flask 사용
- 로그인 보안 향상 : JWT 토큰 사용
- 이미지 객체 관리 : AWS S3 객체 스토리지

# 🔨 개발 도구
프론트 엔드
 - Android

백엔드
 - Spring Framework
 - AWS S3 객체 스토리지
 - Naver Cloud Platform
 - Docker
 
인공지능
 - yolor

# DB 명세
User
 - id : 유저 고유값
 - email : 이메일
 - name : 이름
 - userId : 아이디
 - encryptedPwd : 암호화된 비밀번호
Cell
 - id : 세포 고유값
 - totalcell : 전체 세포
 - livecell : 살아있는 세포
 - deadcell : 죽은 세포
 - viability : 전체 세포 중 살아있는 세포의 백분율
 - url : 결과 이미지 저장 url
 - userId : 세포 분석을 진행한 userId
 
# 팀원
| 팀원 | 역할 | 개발환경 
|-----|-----|--------
|조수빈(PM), 김태강| 백엔드 | Spring frameworks, AWS s3, Naver Cloud Platform, Flask
|김민종, 박준후| 안드로이드 | 안드로이드 애플리케이션 (Kotlin)
|한동현| Object Detection | YoLo, torch

# 애플리케이션 구성
01. Welcome, 로그인/회원가입
<img width="389" alt="스크린샷 2022-06-10 오후 12 56 24" src="https://user-images.githubusercontent.com/83891837/172987304-a9885c07-b101-45fa-bb5c-11e3e2a56cd9.png">

02. 이미지 업로드, 분석결과

<img width="225" alt="스크린샷 2022-06-10 오후 2 16 36" src="https://user-images.githubusercontent.com/83891837/172995263-37476d2e-d13b-45f3-b800-63ff81e1602d.png"><img width="225" alt="스크린샷 2022-06-10 오후 2 17 04" src="https://user-images.githubusercontent.com/83891837/172995322-a6b422e9-e7ef-4303-a8db-928fae92d7c6.png"><img width="225" alt="스크린샷 2022-06-10 오후 2 17 29" src="https://user-images.githubusercontent.com/83891837/172995374-b7d0af21-bfb1-478e-a85c-fe6662914e0f.png">

03. DB, 내부 저장소에서 결과 관리
<img width="225" alt="스크린샷 2022-06-10 오후 2 17 53" src="https://user-images.githubusercontent.com/83891837/172995421-c29ccdd7-d513-4353-9869-5d9a5cff9986.png">


# 개발 - 기능 및 문제해결

- [(기능)회원가입, 로그인 인증 with JWT Token.md](https://github.com/KimTaeKang57/CapstoneProject/blob/master/lab/%EA%B0%9C%EB%B0%9C/%EA%B8%B0%EB%8A%A5/(%EA%B8%B0%EB%8A%A5)%ED%9A%8C%EC%9B%90%EA%B0%80%EC%9E%85%2C%20%EB%A1%9C%EA%B7%B8%EC%9D%B8%20%EC%9D%B8%EC%A6%9D%20with%20JWT%20Token.md)
- [(기능, 문제해결)AWS S3, Docker container간 파일 주고받기.md](https://github.com/KimTaeKang57/CapstoneProject/blob/master/lab/%EA%B0%9C%EB%B0%9C/%EA%B8%B0%EB%8A%A5/(%EA%B8%B0%EB%8A%A5%2C%20%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0)AWS%20S3%2C%20Docker%20container%EA%B0%84%20%ED%8C%8C%EC%9D%BC%20%EC%A3%BC%EA%B3%A0%EB%B0%9B%EA%B8%B0.md)
- [(기능, 문제해결)Naver cloud Platform의 유료 Server 사용.md](https://github.com/KimTaeKang57/CapstoneProject/blob/master/lab/%EA%B0%9C%EB%B0%9C/%EA%B8%B0%EB%8A%A5/(%EA%B8%B0%EB%8A%A5%2C%20%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0)Naver%20cloud%20Platform%EC%9D%98%20%EC%9C%A0%EB%A3%8C%20Server%20%EC%82%AC%EC%9A%A9.md)

# 스택 (무료 Ec2 서버가 아닌 Naver Cloud Platform 유료 서버를 사용하였다.)
<img width="637" alt="스크린샷 2022-06-10 오후 1 17 34" src="https://user-images.githubusercontent.com/83891837/172989394-77e0c6e8-288e-4f5f-91a6-07fc5c62f5b7.png">
