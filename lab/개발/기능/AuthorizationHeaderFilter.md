# 인증 방법 (JWT 토큰 사용)
0. Apigateway-service와 User-service에서 인증을 진행한다.
1. Post -> /authenticate(인증 요구) email= ... & password = ... 입력
2. 서버에서 DB에 저장된 것과 같은지 확인하는 절차
3. 결과가 같다면 Bearer Token 기반의 JWT 토큰을 리턴
4. Client는 리턴받은 JWT 토큰과 함께 원하는 기능을 추가적으로 요청 (Authorization : JWT(Bearer Token)
5. 서버에서 JWT 토큰을 비교하고, JWT 토큰이 확인되면 Client가 요청한 기능의 정보를 처리해줌

## 인증처리 과정

- ApiGateWay-service와 User-service는 개인 Github에 별도로 저장되어있는 application.yml의 token.secret 값을 통해 토큰을 인코딩하고, 디코딩하여 인증여부를 판단한다. 각 service가 인증할 때 사용하는 token.secret 값이 다르면 인증이 허가되지 않으므로 개인 Github의 개인 저장소를 이용한 것이다.

### JWT 토큰 처리과정 1.2에 해당한다(User-service)
<img width="937" alt="스크린샷 2022-06-10 오후 1 06 59" src="https://user-images.githubusercontent.com/83891837/172988409-a6d50c71-8de7-4a78-970d-7b492aafd3cf.png">

- Client가 로그인을 시도하였을 때의 email과 password 값을 Token에 저장하고 인증처리를 요구한다.
### JWT 토큰 처리과정 3에 해당한다(User-service)
<img width="922" alt="스크린샷 2022-06-10 오후 1 08 40" src="https://user-images.githubusercontent.com/83891837/172988561-8f2cffb2-9a9a-4210-a547-54ced137eb4f.png">

- 인증이 처리되면, 그 결과를 가지고 JWT token을 생선해준다. JWT token은 로그인이 성공한 계정의 정보와 Github의 개인저장소의 token.secret을 HS512 방식으로 암호화한 것과 함께 builder() 메서드를 이용해 만들어낸다. setExpirtaion등과 같은 속성으로 유효기간 등의 세부기능을 설정할 수 있다.
### JWT 처리과정 4에 해당한다.(Apigateway-service)
<img width="922" alt="스크린샷 2022-06-10 오후 1 11 37" src="https://user-images.githubusercontent.com/83891837/172988823-ae9ad57a-9666-42a7-94aa-0d8abf281d69.png">

- Client가 반환받은 JWT 토큰과 함께 기능을 요청하면, 서버 내부에서 JWT 토큰의 유효성을 판단한다. 처음엔 JWT 토큰이 실려 있는지 확인하고, 실려 있다면 인증을 위한 형식으로 바꿔주고 isJwtValid() 메서드를 통해 인증을 진행한다.
### JWT 처리과정 5에 해당한다.(Apigateway-service)
<img width="690" alt="스크린샷 2022-06-10 오후 1 13 07" src="https://user-images.githubusercontent.com/83891837/172988957-eb209ce6-d742-4d28-8ee6-94288cfbfc48.png">

- 마지막으로 Client가 실어준 토큰을 디코딩하여 비교하고 인증여부를 확인한다.(200OK와 함께 인증완료!)
<img width="620" alt="스크린샷 2022-06-10 오후 1 13 46" src="https://user-images.githubusercontent.com/83891837/172989023-f6f1e337-7d8b-41c0-a4c7-f70516ff6ef8.png">
