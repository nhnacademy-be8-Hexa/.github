## Hexa Bookstore

[Hexa Bookstore 주소](https://hexabook.shop/)

## Service Introduction
Spring boot를 이용한 온라인 서점 플랫폼 입니다.

## 팀원
<div align="center">

|고은|김성재|김수아|안민재|
|:---:|:---:|:---:|:---:|
|<img style="height: 200px; width: 200px;" src="https://avatars.githubusercontent.com/u/133118296?v=4"/>|<img style="height: 200px; width: 200px;" src="https://avatars.githubusercontent.com/u/104749176?v=4"/>|<img style="height: 200px; width: 200px;" src="https://avatars.githubusercontent.com/u/144919371?v=4"/>|<img style="height: 200px; width: 200px;" src="https://avatars.githubusercontent.com/u/180361980?v=4"/>|
| 쿠폰 | 이미지 저장 <br/> (Object Storage) | CI/CD <br/> (github action) | 민감정보 보안 |
| NHN Log & Crash | 포인트 <br/> 페이지 구현 | 도서 CRUD <br/> 도서 조회 top 5  | 이미지 저장 <br/> (Local,ImageManager) | 
| 쿠폰API 명세서 작성| 메세지 큐 <br/> (Rabbit MQ) | 관리자페이지 회원 관리 | Oauth2, JWT(나머지) | 
| | | 주문 상세 조회<br/> 주문 상태 관리 | Spring batch <br/> (사용자 등급 조정) | 

|이규빈|조나현|조승주|채노아|
|:---:|:---:|:---:|:---:|
|<img style="height: 200px; width: 200px;" src="https://github.com/nhnacademy-be8-Hexa/.github/blob/main/profile/busan.jpg?raw=true"/>|<img style="height: 200px; width: 200px;" src="https://avatars.githubusercontent.com/u/95014596?v=4"/>|<img style="height: 200px; width: 200px;" src="https://avatars.githubusercontent.com/u/168888761?s=400&u=fe82e7f81db55f64ac90560a5332e47f706563dc&v=4"/>|<img style="height: 200px; width: 200px;" src="https://avatars.githubusercontent.com/u/104444048?v=4"/>|
| ELK Stack | CI/CD </br>(무중단 배포)| 로그인 JWT<br/>Spring Security | 사용자 휴면 전환<br/>(Spring Batch) |
| 카테고리 | 장바구니 | 주문하기 (주문서 작성) | CI/CD<br/>(Shell Script, <br/> Health Check) |
| 도서 api </br> 외부 도서 검색 </br> 도서 좋아요 | 태그 | 결제, 환불<br/>Toss Payments API | 리뷰(이미지 포함) |
| spring cloud | testcode (front-end) | | API 명세(Rest Docs) |

</div>

## ERD Cloud
https://www.erdcloud.com/team/gxWqy4hXfkZrX27Sc


## CI / CD

![image](https://github.com/nhnacademy-be8-Hexa/.github/blob/d923873a574aaae3914b34563d7c287842b9ac6c/profile/CI_CD.png)

> GitHub Actions를 통한 CI를 진행하고, 테스트가 성공 시 Merge가 이루어집니다.
> feature를 통해 각 인원이 담당한 기능을 각각의 브랜치로 두고, deploy에 Pull Request를 요청한 뒤 미리 지정한 2~3명의 인원이 함께 Code Review 진행, main 브랜치로 병합할 경우 CI/CD가 이루어지도록 Git Flow를 구성했습니다.
> SonarLint, SonarQube를 통해 정적 코드 분석을 진행하며, 이에 따른 Test Coverage를 저장합니다.



## System Architecture

![image](https://github.com/nhnacademy-be8-Hexa/.github/blob/main/profile/Architecture.png?raw=true)

> 1. 클라이언트의 요청은 NginX를 통해서 들어오고, 로드밸런서에서 Round Robin 방식으로 2개의 Front Server에 순서대로 보냅니다.(Sticky Session 적용)
> 2. Front Server
     >   -	Front Server는 SSL을 적용하여 https 환경으로 구성하였으며, 확장성을 고려한 설계로 OAuth 2.0을 이용한 회원가입/로그인 기능을 구현하였습니다.
>   -	redis server를 session으로 사용합니다. 장바구니, JWT token 저장 등에 사용됩니다.
>   -	필요한 요청을 API Gateway를 통해 처리하고, Gateway는 해당 요청에 대해 처리되어야 하는 서비스 API로 요청을 보낸다.
> 3. Services
     >   -	Gateway, shop, Auth server는 모두 eureka server에 등록되어, Gateway에 도착한 요청을 적절히 라우팅 처리합니다.
>   -	Shoppingmall Server는 두 개의 서버 (포트)로 구성되어 있으며, 이에 대한 요청은 Gateway에서 Round Robin 방식으로 로드밸런싱하여 요청합니다.
>   -	Shoppingmall, Coupon, Auth는 MySQL (개발환경 및 테스트 환경에서는 H2 Database)과 통신하여 CRUD 를 수행합니다.
>   -	Shop Server에서 '쿠폰 지급' 기능에서는 요청 과부하를 Queue를 활용하여 막기 위해 별도로 마련된 RabbitMQ 서버에 전달하여 요청합니다.
>   -	Auth Server에서는 JWT를 활용하여 인증 요청이 오는 경우, 토큰 인증, 토큰 재발급을 시행합니다.
>   -	Shop Server에서 리뷰, 도서 저장 등 이미지 저장 요청이 오는 경우 NHN Cloud에 업로드합니다.


<div align=center><h2>📚 STACKS</h2></div>

<div align=center> 
  <img src="https://img.shields.io/badge/java-007396?style=for-the-badge&logo=java&logoColor=white"> 
  <br>
  <img src="https://img.shields.io/badge/springboot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white">
  <img src="https://img.shields.io/badge/springsecurity-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white">
  <br>
  
  <img src="https://img.shields.io/badge/html5-E34F26?style=for-the-badge&logo=html5&logoColor=white"> 
  <img src="https://img.shields.io/badge/css-1572B6?style=for-the-badge&logo=css3&logoColor=white"> 
  <img src="https://img.shields.io/badge/javascript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> 
  <img src="https://img.shields.io/badge/thymeleaf-005F0F?style=for-the-badge&logo=thymeleaf&logoColor=black"> 
  
  <br>

  <img src="https://img.shields.io/badge/apachemaven-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white">
  <img src="https://img.shields.io/badge/elasticstack-005571?style=for-the-badge&logo=elasticstack&logoColor=white">
  <img src="https://img.shields.io/badge/nginx-009639?style=for-the-badge&logo=nginx&logoColor=white">
  <img src="https://img.shields.io/badge/rabbitmq-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white">  
  <img src="https://img.shields.io/badge/sonarqube-4E9BCD?style=for-the-badge&logo=sonarqube&logoColor=white"> 
  <img src="https://img.shields.io/badge/sonarlint-CB2029?style=for-the-badge&logo=sonarlint&logoColor=white"> 
  <br>
  
  <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> 
  <img src="https://img.shields.io/badge/redis-FF4438?style=for-the-badge&logo=redis&logoColor=white"> 
  <img src="https://img.shields.io/badge/jsonwebtokens-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white"> 
  <br>
  
  
  <br>
  
  <img src="https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white">
  <img src="https://img.shields.io/badge/git-F05032?style=for-the-badge&logo=git&logoColor=white">
  <img src="https://img.shields.io/badge/githubactions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white">
  <br>
</div>



