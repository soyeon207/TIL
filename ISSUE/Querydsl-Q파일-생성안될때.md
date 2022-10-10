# Querydsl Q파일 생성 안 될 때 

### 문제 상황 
분명 회사 컴퓨터로는 Q파일 생성이 너무 잘 되는데, 집 컴퓨터로 했을 때에 갑자기 Querydsl Q파일이 안생겼다. ㅠㅠ 

<img width="576" alt="스크린샷 2022-10-10 오후 6 59 03" src="https://user-images.githubusercontent.com/39696812/194841316-5692466f-0196-4b7e-b069-1d60928c965e.png">

진짜 눈물 좀 많이 흘렸음 ㅠㅠㅠㅠㅠㅠㅠ 실행도 못하고 ,,,, 

### 해결 방법
일단 시도 해본 방식이 

1. Gradle > Build and run using & Run tests using Intellij IDEA 로 바꾸기 
2. Enable annotation processing 활성화 시키기
3. 다른 방법 등등 ...

근데 뭔가 다른 컴퓨터에서는 정상적으로 작동하는데 내 컴퓨터에서만 정상적으로 작동 안한다는 건 버전 문제일 것 같다 ! 라고 생각하고 로그를 뜯어보고 있는데 
Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0. 
이런 로그를 발견했다. 

다른 블로그에서는 그냥 무시해도 된다고 하긴 했는데, 그래도 한 번 해볼까 해서 Gradle 버전을 바꿔보니깐 성공 !

Preferences > Build, Execution, Deployment > Gradle > Gradle JVM 버전이 원래 corretto-16 이였는데 1.8.0 으로 낮춰줬더니 정상적으로 작동했다. 
저번에도 버전때문에 이슈가 있었는데 QueryDSL 이 정상적으로 작동하지 않는 버전이 있는 것 같음 
