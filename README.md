# 2-2 스프링MVC 2편 - 백엔드 웹 개발 핵심 기술 
MVC 1편의 핵심 원리와 구조 위에 실무 웹 개발에 필요한 모든 활용 기술들을 학습
https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2#

### 전체 목차
1. 타임리프 - 기본 기능
2. 타임리프 - 스프링 통합과 폼
3. 메시지, 국제화
4. 검증1 - Validation
5. 검증2 - Bean Validation
6. 로그인 처리1 - 쿠키, 세션
7. 로그인 처리2 - 필터, 인터셉터
8. 예외 처리와 오류 페이지
9. API 예외 처리
10. 스프링 타입 컨버터
11. 파일 업로드

궁금했던 점 :
JWT 동작 원리
1. 클라이언트에서 서버로 ID/PW로 로그인을 요청한다.
2. 서버에서 검증 과정을 거쳐 해당 유저가 존재하면, Access Token + Refresh Token 을 발급한다.
3. 클라이언트는 요청 헤더에 2번에서 발급받은 Access Token 을 포함하여 API를 요청한다.

---------------
#### 타임리프 - 기본 기능

타임리프 특징
- 서버 사이드 HTML 렌더링 (SSR)
- 네츄럴 템플릿
- 스프링 통합 지원
→ 서버 사이드 HTML 렌더링 (SSR)
: 타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링 하는 용도로 사용된다.
→ 네츄럴 템플릿
: 타임리프는 순수 HTML을 최대한 유지하는 특징이 있다. 타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.
JSP를 포함한 다른 뷰 템플릿들은 해당 파일을 열면, 예를 들어서 JSP 파일 자체를 그대로 웹 브라우저에서 열어보면 JSP 소스코드와 HTML이 뒤죽박죽 섞여서 웹 브라우저에서 정상적인 HTML 결과를 확인할 수 없다. 오직 서버를 통해서 JSP가 렌더링 되고 HTML 응답 결과를 받아야 화면을 확인할 수 있다.
반면에 타임리프로 작성된 파일은 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있다. 물론 이 경우 동적으로 결과가 렌더링 되지는 않는다. 하지만 HTML 마크업 결과가 어떻게 되는지 파일만 열어도 바로 확인할 수 있다.
이렇게 **순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징**을 네츄럴 템플릿(natural templates)이라 한다.
→ 스프링 통합 지원
: 타임리프는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있게 지원한다.

- 자바 8 날짜
    - `<li>yyyy-MM-dd HH:mm:ss = <span th:text="${#temporals.format(localDateTime,'yyyy-MM-dd HH:mm:ss')}"></span></li>`
- URL 링크 : 타임리프에서 URL을 생성할 때는 @{...} 문법을 사용하면 된다.
    - `<li><a th:href="@{/hello}">basic url</a></li>`

쿼리 파라미터
- @{/hello(param1=${param1}, param2=${param2})}
    - /hello?param1=data1&param2=data2
    - () 에 있는 부분은 쿼리 파라미터로 처리된다.

경로 변수
- @{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}
    - /hello/data1/data2
    - URL 경로상에 변수가 있으면 () 부분은 경로 변수로 처리된다.

경로 변수 + 쿼리 파라미터
- @{/hello/{param1}(param1=${param1}, param2=${param2})}
    - /hello/data1?param2=data2
    - 경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.
    

리터럴 대체(Literal substitutions)
- 리터럴(공백) 오류! `<span th:text="hello world!"></span> → <span th:text="|hello ${data}|">`

---------------
#### 타임리프 - 스프링 통합과 폼

th:object : 커맨드 객체를 지정한다.
*{...} : 선택 변수 식이라고 한다. th:object 에서 선택한 객체에 접근한다.
th:field
HTML 태그의 id , name , value 속성을 자동으로 처리해준다.

- th:object="${item}" : <form> 에서 사용할 객체를 지정한다. 선택 변수 식( *{...} )을 적용할 수 있다.*
- *th:field="*{itemName}"
    - *{itemName} 는 선택 변수 식을 사용했는데, ${item.itemName} 과 같다. 앞서 th:object 로 item 을 선택했기 때문에 선택 변수 식을 적용할 수 있다.
    - th:field 는 id , name , value 속성을 모두 자동으로 만들어준다.
---------------
#### 메시지, 국제화
-메시지 적용

messages.properties

label.item=상품
label.item.id=상품 ID
label.item.itemName=상품명
label.item.price=가격
label.item.quantity=수량
page.items=상품 목록
page.item=상품 상세
page.addItem=상품 등록
page.updateItem=상품 수정
button.save=저장
button.cancel=취소


- 타임리프 메시지 적용
타임리프의 메시지 표현식 #{...} 를 사용하면 스프링의 메시지를 편리하게 조회할 수 있다.
예를 들어서 방금 등록한 상품이라는 이름을 조회하려면 #{label.item} 이라고 하면 된다.
- 렌더링 전 <div th:text="#{label.item}"></h2>
- 렌더링 후 <div>상품</h2>

-국제화 적용

messages_en.properties

label.item=Item
label.item.id=Item ID
label.item.itemName=Item Name
label.item.price=price
label.item.quantity=quantity
page.items=Item List
page.item=Item Detail
page.addItem=Item Add
page.updateItem=Item Update
button.save=Save
button.cancel=Cancel


---------------
#### 검증1 - Validation

//검증 오류 결과를 보관

 Map<String, String> errors = new HashMap<>();
 //검증 로직
 if (!StringUtils.hasText(item.getItemName())) { errors.put("itemName", "상품 이름은 필수입니다.");
 }
 if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() >
1000000) {
 errors.put("price", "가격은 1,000 ~ 1,000,000 까지 허용합니다.");
 }
 if (item.getQuantity() == null || item.getQuantity() > 9999) {
 errors.put("quantity", "수량은 최대 9,999 까지 허용합니다.");
 }
 //특정 필드가 아닌 복합 룰 검증
 if (item.getPrice() != null && item.getQuantity() != null) {
 int resultPrice = item.getPrice() * item.getQuantity();
 if (resultPrice < 10000) {
 errors.put("globalError", "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재
값 = " + resultPrice);
 }
 
---------------
#### 검증2 - Bean Validation

검증 애노테이션
- @NotBlank : 빈값 + 공백만 있는 경우를 허용하지 않는다.
- @NotNull : null 을 허용하지 않는다.
- @Range(min = 1000, max = 1000000) : 범위 안의 값이어야 한다.
- @Max(9999) : 최대 9999까지만 허용한다.
  
---------------
#### 로그인 처리1 - 쿠키, 세션

쿠키 동작 방식
쿠키에는 영속 쿠키와 세션 쿠키가 있다.

- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지

세션 동작방식

**클라이언트와 서버는 결국 쿠키로 연결이 되어야 한다.**
- 서버는 클라이언트에 mySessionId 라는 이름으로 세션ID 만 쿠키에 담아서 전달한다.
- 클라이언트는 쿠키 저장소에 mySessionId 쿠키를 보관한다.
**중요**
- 여기서 중요한 포인트는 회원과 관련된 정보는 전혀 클라이언트에 전달하지 않는다는 것이다.
- 오직 추정 불가능한 세션 ID만 쿠키를 통해 클라이언트에 전달한다.
  
---------------
#### 로그인 처리2 - 필터, 인터셉터

필터는 서블릿이 지원하는 수문장이다. 필터의 특성은 다음과 같다.

- 필터 흐름 `HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 컨트롤러\`

필터를 적용하면 필터가 호출 된 다음에 서블릿이 호출된다. 그래서 모든 고객의 요청 로그를 남기는 요구사항이 있다면 필터를 사용하면 된다. 참고로 필터는 특정 URL 패턴에 적용할 수 있다. /* 이라고 하면 모든 요청에 필터가 적용된다.
참고로 스프링을 사용하는 경우 여기서 말하는 서블릿은 스프링의 디스패처 서블릿으로 생각하면 된다.

- 필터 제한 `HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 컨트롤러 //로그인 사용자 HTTP 요청 -> WAS -> 필터(적절하지 않은 요청이라 판단, 서블릿 호출X) //비 로그인 사용자`
  
필터에서 적절하지 않은 요청이라고 판단하면 거기에서 끝을 낼 수도 있다. 그래서 로그인 여부를 체크하기에 딱 좋다.

스프링 인터셉터도 서블릿 필터와 같이 웹과 관련된 공통 관심 사항을 효과적으로 해결할 수 있는 기술이다.
서블릿 필터가 서블릿이 제공하는 기술이라면, 스프링 인터셉터는 스프링 MVC가 제공하는 기술이다. 둘다 웹과 관련된 공통 관심 사항을 처리하지만, 적용되는 순서와 범위, 그리고 사용방법이 다르다.

---------------
#### 예외 처리와 오류 페이지
---------------
#### API 예외 처리
---------------
#### 스프링 타입 컨버터
---------------
#### 파일 업로드
