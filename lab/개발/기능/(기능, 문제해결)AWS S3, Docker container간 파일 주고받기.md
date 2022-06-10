# 이미지
- 프로젝트를 진행하면서 세포 계수 분석이 끝난 결과인 이미지 객체들을 어디에 저장하고 어디에서 관리할지 고민하였다. 그러다가 AWS의 Object Storage를 알게되었다.
안드로이드 내부저장소에도 이미지를 관리할 수 있지만, 안드로이드 내부저장소가 아니라 편리하게 이미지 객체를 관리할 공간에 딱 적합한 것이었다..!

안드로이드 개발을 맡은 친구들과 얘기를 진행해보니, 안드로이드에선 결과 이미지를 이미지 파일로 줘도 괜찮지만 열리는 "url"로 알려줘도 안드로이드에서 이미지를 보여줄수 있다는 말을 듣게되었다.

# 어! 그럼 S3에 업로드한 다음에 S3에 올라간 이미지의 Url을 반환해주면 되겠다
- 그래서 어떤식으로 처음에 코드를 진행했냐면... 여기서 Spring Framework와 Flask간의 파일 주고받기를 진행한다.

1. 이미지를 업로드하면 이미지를 Cell-service에서 Flask 서버로 보낸다.
2. Flask 서버에는 인공지능을 실행하는 코드가 들어있기 때문에 Flask 서버에 보낸 이미지를 인공지능 타겟 디렉토리에 저장한다.
3. 디렉토리에 저장된 이미지를 토대로 분석을 진행하고 결과 디렉토리에 저장함.
4. 결과 디렉토리에 저장된 이미지를 다시 Cell-service로 보내고, Cell-sercice에서 AWS S3에 이미지를 업로드하고 url을 받아낸다.
5. {전체 세포, 살아있는 세포, 죽은 세포, 비율, AWS S3 url}를 리턴해주면 안드로이드는 이 url을 통해 이미지를 보여주고, DB에는 url을 String으로 저장한다!!!

# 계획은 먹혔고, Local에선 잘 돌아갔다...
- 그런데 Docker Container로 실행한 이후에 문제가 발생하였다

# 문제 발생
로컬에선 같은 컴퓨터 내부의 디렉토리로 저장하고 S3에 올렸기 때문에 문제가 없었다. 그런데 Server에 Container로 올리는 순간 문제가 발생하였는데, 어느 부분이었나면

### 4번에서 문제가 발생하였다. 
- 원래 코드는 Flask 프로젝트에 저장된 이미지와 .json 파일을 Cell-service에서 파일을 읽어서 결과를 알아오는 방식이었는데, Container로 올린순간 각자 다른 공간이 되었기 때문에, 파일을 읽어올수가 없었던 것이다..
- 이때쯤 Naver Cloud Platform으로 옮겼을 때이고, 또 문제가 발생하여 실망했지만 한편으론 인공지능을 돌렸을때 서버가 죽지는 않는다는 사실에 안도했다.

# 문제 해결
- 그래서 처음에 하기로 했던 방법을 갈아엎고 다시 고안했다.

1.2.3 까지는 동일하다..

4. Cell-service로 결과 이미지를 보내지 않고 그냥 Flask서버에서 AWS S3에 이미지를 업로드하고 url을 받아온다. 그리고 .json 파일에 url도 추가하여 작성하고, Flask 서버에서 .json 파일을 읽어 jsonify를 사용하여 json 형식으로 Cell-service에 반환해준다. Cell-service에서 Flask서버와 Rest Template 방식으로 통신을 하기 때문에 가능한 일이었다.

5. Cell-service에선 리턴받은 결과를 안드로이드, DB에 각각 보낸다!!

### Rest Template을 사용하여 Multipartfile로 이미지를 Cell-service에서 Flask로 보낸다.
<img width="1080" alt="스크린샷 2022-06-10 오후 2 02 17" src="https://user-images.githubusercontent.com/83891837/172993786-532cb945-0f94-4047-8eef-ca6e897929f6.png">

### flask 서버에선 post 방식으로 이미지를 받아서 디렉토리에 저장한다. 그리고 분석을 시작(predict()가 분석 시작 메소드)
<img width="452" alt="스크린샷 2022-06-10 오후 2 03 09" src="https://user-images.githubusercontent.com/83891837/172993869-8a9c77dc-3895-401d-99b9-cc159ccd832a.png">

### result/predict에 결과.jpg 와 결과.json파일이 생성된다. 
- 이미지는 생성되자마자 S3에 업로드하고 받아온 url은 .json 파일에 추가하여 작성한다.

<img width="745" alt="스크린샷 2022-06-10 오후 2 07 38" src="https://user-images.githubusercontent.com/83891837/172994330-6c8efb1b-69ad-4352-bef7-2cdd928c6bab.png">
<img width="835" alt="스크린샷 2022-06-10 오후 2 06 20" src="https://user-images.githubusercontent.com/83891837/172994201-21257ff4-8624-473b-8f36-fb5ea646e7d1.png">

### 저장된 .json 파일을 Flask에서 읽어서 jsonify를 이용해 json 형식으로 값을 리턴해준다.
<img width="524" alt="스크린샷 2022-06-10 오후 2 08 08" src="https://user-images.githubusercontent.com/83891837/172994369-6fb4a0f3-400b-4957-9602-2fa0e4d8d7a1.png">

### 반환받은 결과를 DB에 저장하고 안드로이드에게 리턴해주면 끝
<img width="879" alt="스크린샷 2022-06-10 오후 2 09 17" src="https://user-images.githubusercontent.com/83891837/172994509-7449901b-75bc-48af-9ed5-222d58314243.png">

### 리턴해준 결과
<img width="743" alt="스크린샷 2022-06-10 오후 2 10 03" src="https://user-images.githubusercontent.com/83891837/172994601-767399ef-3a56-4260-9844-bfb62b81c4c6.png">

### 결과 UI
<img width="449" alt="스크린샷 2022-06-10 오후 2 12 01" src="https://user-images.githubusercontent.com/83891837/172994823-610bb7fa-8ad4-4abf-beb8-546ad66203a3.png">
<img width="449" alt="스크린샷 2022-06-10 오후 2 12 09" src="https://user-images.githubusercontent.com/83891837/172994840-f9e612fa-0822-4bec-a0d6-4de281a6554e.png">

