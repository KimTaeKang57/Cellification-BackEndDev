# 인증 방법 (JWT 토큰 사용)
1. Post -> /authenticate(인증 요구) username= ... & password = ... 입력
2. 서버에서 DB에 저장된 것과 같은지 확인하는 절차
3. 결과가 같다면 Bearer Token 기반의 JWT 토큰을 리턴
4. Client는 리턴받은 JWT 토큰과 함께 원하는 기능을 추가적으로 요청 (Authorization : JWT(Bearer Token)
5. 서버에서 JWT 토큰을 비교하고, JWT 토큰이 확인되면 Client가 요청한 기능의 정보를 처리해줌

## 인증처리 과정

- ApiGateWay-service와 User-service는 개인 Github에 별도로 저장되어있는 application.yml의 token.secret 값을 통해 토큰을 인코딩하고, 디코딩하여 인증여부를 판단한다. 각 service가 인증할 때 사용하는 token.secret 값이 다르면 인증이 허가되지 않으므로 개인 Github의 개인 저장소를 이용한 것이다.

### JWT 토큰 처리과정 1.2에 해당한다
<img width="937" alt="스크린샷 2022-06-10 오후 1 06 59" src="https://user-images.githubusercontent.com/83891837/172988409-a6d50c71-8de7-4a78-970d-7b492aafd3cf.png">

