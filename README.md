## MvcStudy2

## spring setting
1. spring boot version 2.4.3
2. java 11
3. jar

## dependency
1. thymeleaf
2. web
3. lombok

## 로깅
> 운영 시스템에서는 System.out.println()같은 시스템 콘솔을 사용해서 필요한 정보를 출력하지 않는다. log라이브러리르 사용한다.
1. @Slf4j 어노테이션 사용
2. trace, debug, info, warn, error를 사용 가능하다
3. {}, parameter 바인딩이 가능하다.
ex
```
log.trace( " trace log = {}", name);  //name이 {}로 치환된다.
```
4. 로그의 장점
  * 부가적인 정보를 볼 수 있고, 출력 모양을 결정할 수 있다.
  * 자신이 원하는 정보의 수준을 조절할 수 있다. ( trace debug등등)
  * 로그는 콘소창뿐만 아니라 자신이 원하는 곳에 남길 수 있다.

## PathVariable
1. 리소스 경로에 있는 것을 사용할 수 있다.
ex
```
@GetMapping("/mapping/{userId}")
  public String mappingPath(@PathVariable("userId") String data) {
    log.info("mappingPath userId={}", data);
    return "ok";
  }
```
2. 이름과 파라미터가 같다면 생략 가능

## Post, HTML Form 전송

1. request.getParameter() 사용
```
@Slf4j
@Controller
public class RequestParamController {
 
  @RequestMapping("/request-param-v1")
  public void requestParamV1(HttpServletRequest request, HttpServletResponse response) throws IOException {
    String username = request.getParameter("username");
    int age = Integer.parseInt(request.getParameter("age"));
    log.info("username={}, age={}", username, age);
    response.getWriter().write("ok");
 }
}
```
2. @RequestParam 사용 // 필드명과 파라미터의 명이 같다면 속성 생략 가능
```
@ResponseBody
@RequestMapping("/request-param-v2")
public String requestParamV2( @RequestParam("username") String memberName, @RequestParam("age") int memberAge) {
  log.info("username={}, age={}", memberName, memberAge);
  return "ok";
}
```
3. @ModelAttribute
```
@RequestMapping("/model-attribute-v1")
public String modelAttributeV1(@ModelAttribute HelloData helloData) {
   log.info("username={}, age={}", helloData.getUsername(),helloData.getAge());
   return "ok";
}
```
> 객체를 통해서 받는다면 @RequestParam과 별도의 값으 저장해주는 과정을 생략가능 하다. 

3. @RequestBody
> HTTP 메시지 바디를 편리하게 조회가 가능하다.
```
@ResponseBody
@PostMapping("/request-body-string-v4")
  public String requestBodyStringV4(@RequestBody String messageBody) {
  log.info("messageBody={}", messageBody);
  return "ok";
}
```
4. @ResponseBody
> view를 따로 사용하지 않고 HTTP 메시지 바디에 직접 담아서 전달한다.

## HTTP응답 - 정적 리소스, 뷰 템플릿
1. 정적 리소스 경로
  * /static, /public, /resources, /META-INF/resources
2. 뷰 템플릿 (동적이 가능하다)
  * src/main/resources/templates
