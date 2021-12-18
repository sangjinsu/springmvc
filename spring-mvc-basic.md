# 스프링 MVC - 기본 기능

### 매핑 정보 

- @RestController
  - @Controller 는 반환값이 String이면 뷰 이름으로 인식한다 그래서 뷰를 찾고 뷰가 랜더링 된다 
  - @RestController는 반환값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다 

### 테스트 

로그가 출력되는 포멧 

- 시간, 로그 레벨, 프로세스 아이디, 쓰레드 명, 클래스명, 로그 메시지

로그 레벨 설정 

level: trace > debug > info > warn > error

```
# 전체 로그 레벨 설정 디폴드 info
logging.level.root = info

# hello.springmvc 패키지와 그 하위 로그 레벨 설정
logging.level.hello.springmvc=debug
```

- 개발 서버 debug
- 운영 서버 info



### 로그 사용시 장점

- 쓰레드 정보, 클래스 이름 같은 부가 정보를 함께 보고 출력 모양 조정

- 로그 레벨에 따라 개발 서버에서는 모든 로그 출력, 운영 서버에서는 출력하지 않는 등 로그를 상황에 따라 조절 

- sout 보다 성능이 좋음 

  

### 요청 매핑

```
hello.springmvc.basic.requestmapping.MappingController
```

- 다양한 요청 매핑 확인 

  