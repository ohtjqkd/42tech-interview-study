# Web Server vs WAS

web server와 was의 차이점을 알기 위해서는 먼저 정적 페이지(static pages)와 동적 페이지(dynamic pages)의 과정을 알아야한다.  

![static vs dynamic](https://gmlwjd9405.github.io/images/web/static-vs-dynamic.png)

1. Static Pages
    - Web Server는 파일 경로를 받아 경로와 일치하는 'File Contents'를 반환
    - 항상 __동일한__ 페이지를 반환함
    - ex) image, html, css, javascript과 같이 server의 file system 안에 존재하는 파일
2. Dynamic Pages
    - 인자의 내용에 맞게 동적인 contents를 반환

## Web Server와 WAS의 차이
![web server vs was](https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png)

### Web Server의 기능
- HTTP를 기반으로 하여 클라이언트의 요청을 서비스하는 기능을 담당
- 클라이언트의 요청에 따라 두 가지 기능 중 하나를 선택하여 수행
- 기능 1)
    - 정적인 컨텐츠 제공
    - WAS를 거치지 않고 바로 자원을 제공함
- 기능 2)
    - 동적인 컨텐츠 제공을 위한 요청 전달(web application으로 전달하겠지)
    - 클라이언트의 요청을 WAS(?)(web application 아닌가)에 보내고, WAS가 처리한 결과를 클라이언트에 전달

- Web Server의 종류: Apache Server, Nginx 등

### WAS(Web Application Server)
1. WAS의 개념
- DB 조회나 다양한 로직 처리를 요구하는 __동적인__ 컨텐츠를 제공하기 위해 만들어진 Application Server
- HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)
2. WAS의 역할
- Web server 기능들을 구조적으로 분리하여 처리하고자하는 목적으로 제시됨
    - 분산 트랜잭션, 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용
- 현재는 WAS가 가지고 있는 Web Server도 정적인 컨텐츠를 처리하는데 있어서 성능상 큰 차이가 없음
3. WAS의 주요 기능
    1. 프로그램 실행 환경과 DB 접속 기능 제공
    2. 여러 개의 트랜잭션 관리 기능
    3. 업무를 처리하는 비즈니스 로직 수행
4. WAS의 종류: Tomcat, JBoss, Jeus, Web Sphere등

### Web Server가 필요한 이유
- static file들은 html 문서가 클라이언트로 보내질 때 함께 가는 것이 아님
- 클라이언트가 HTML 문서를 받고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하면 그때 static file을 받아옴
- Web Server는 이런 static file을 Application Server까지 가지고 않고 빠르게 보내줄 수 있다.
- Web server가 static file 처리를 전담하도록 기능을 분배하여 서버의 부담을 줄일 수 있음  

### WAS가 필요한 이유
- 사용자의 요청에 맞게 동적 컨텐츠를 제공해야할 필요성이 늘어남
- 만약 Web server만 사용한다면 이러한 다양한 요청에 대비해 무수히 많은 문서를 미리 만들어 놓고 해당 요청에 따라 해당 문서를 응답해주어야하는 매우 비효율적인 과정이 필요해짐
- WAS의 경우 이러한 과정을 하나의 비즈니스 로직으로 대체해 사용자의 요청에 맞는 결과를 동적으로 만들어 제공함으로써 자원을 효율적으로 사용 가능

### WAS와 Web server를 왜 분리하지?
1. 기능을 분리하여 서버 부하를 방지할 수 있음
    - WAS는 DB 조회나 복잡한 비즈니스 로직을 처리하기 때문에 이런 과정이 필요없는 static file 처리는 web server에서 전담으로 처리하여 application의 부담을 줄여 전반적인 서버의 부하를 줄일 수 있다.
2. 보안 강화
    - SSL에 대한 암복호화 처리에 web server를 사용
3. 여러 대의 was를 연결 가능
    - Load Balancing을 위해서 Web Server를 사용
    - fail over, fail back 처리에 유리
    - 대용량 웹 어플리케이션의 경우 Web server와 was를 분리하여 무중단 운영이 가능함
4. 여러 웹 어플리케이션 서비스 가능
    - 하나의 서버(물리적 서버)에서 여러 application을 사용할 수 있음(하나의 web server
결론: 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 web server와 was를 분리한다.