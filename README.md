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

### MultiValueMap

- 맵과 유사한데 하나의 키에 여러 값을 받을 수 있다

- HTTP header, HTTP 쿼리 파라미터와 같이 하나의 키에 여러 값을 받을 때 사용한다

- key1=value1&key2=value2

### 주의

#### 파라미터 이름만 사용

`request-param?username=`

- 파라미터 이름만 있고 값이 없는 경우 ->    빈 문자로 통과

#### 기본형에 null 입력

`request-param` 요청

```java
    @ResponseBody
@RequestMapping("/request-param-required")
public String requestParamRequired(
@RequestParam(required = true) String username,
@RequestParam(required = false) Integer age
        ){
        log.info("username={}, age={}",username,age);
        return"ok";
        }
```

### 디폴트 설정

- required 필요 없음
- 빈문자 경우에도 디폴트 값으로 적용된다

```java
    @ResponseBody
@RequestMapping("/request-param-default")
public String requestParamDefault(
@RequestParam(required = true, defaultValue = "guest") String username,
@RequestParam(required = false, defaultValue = "-1") Integer age
        ){
        log.info("username={}, age={}",username,age);
        return"ok";
        }
```

### HTTP 요청 파라미터 @ModelAttribute

```java
    @ResponseBody
@RequestMapping("/model-attribute-v1")
public String modelAttributeV1(@ModelAttribute HelloData helloData){
        log.info("username={}, age={}",helloData.getUsername(),helloData.getAge());
        log.info("helloData={}",helloData);
        return"ok";
        }

@ResponseBody
@RequestMapping("/model-attribute-v2")
public String modelAttributeV2(HelloData helloData){
        log.info("username={}, age={}",helloData.getUsername(),helloData.getAge());
        log.info("helloData={}",helloData);
        return"ok";
        }
```

### @RequestBody 는 생략 불가능

- 스프링은 @ModelAttribute, @RequestParam 해당 생략시 다음과 같은 규칙을 적용한다
- String, int, Integer = @RequestParam
- 나머지 @ModelAttribute
- 따라서 @RequestBody 생략시 @ModelAttribute 적용

```java
@ResponseBody
@PostMapping("/request-body-json-v5")
public HelloData requestBodyJsonV5(@RequestBody HelloData helloData){
        log.info("username={}, age={}",helloData.getUsername(),helloData.getAge());
        return helloData;
        }
```

## HTTP 응답 - 정적 리소스, 뷰 템플릿

- 정적 리소스

- 뷰 템플릿

- HTTP 메시지

### 뷰 템플릿

```java

@RestController
public class ResponseViewController {
    @RequestMapping("/response-view-v1")
    public ModelAndView responseViewV1() {
        ModelAndView mav = new ModelAndView("response/hello")
                .addObject("data", "hello!");
        return mav;
    }

    @RequestMapping("/response-view-v2")
    public String responseViewV2(Model model) {
        model.addAttribute("data", "hello2");
        return "response/hello";
    }
}
```

### HTTP 응답 HTTP API 메시지 바디에 직접 입력

#### JSON

```java
@GetMapping("/response-body-json-v1")
public ResponseEntity<HelloData> responseBodyJsonV1(){
        HelloData helloData=new HelloData();
        helloData.setUsername("userA");
        helloData.setAge(27);
        return new ResponseEntity<>(helloData,HttpStatus.OK);
        }

@ResponseStatus(HttpStatus.OK)
@ResponseBody
@GetMapping("/response-body-json-v2")
public HelloData responseBodyJsonV2(){
        HelloData helloData=new HelloData();
        helloData.setUsername("userA");
        helloData.setAge(27);
        return HelloData
        }
```

### HTTP 메시지 컨버터

우선순위

- `ByteArrayHttpMessageConverter`

- `StringHttpMessageConverter`

- `MappingJackson2HttpMessageConverter`

  

