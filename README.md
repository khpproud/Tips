# Tips
Trivial tips for developing

- Espresso Test

prb) typeText("...") 입력시 한글이 입력되는 문제 & Upper & Lower case combined(ex: Title)

sol) replaceText("...") 로 대체

- Unit test with Mockito

@RunWith(MockitoJUnitRunner::class) 사용시

1. MockitoAnnotations.initMocks(Object)선언 불필요 
2. 사용되지 않는 Stubbing Object에 대한 Exception 발생

* MocktioJUnit.StrictStubs 사용 고려: Mockito v3에는 default가 될 예정
"Make sure to try out MockitoJUnitRunner.StrictStubs which automatically detects stubbing argument mismatches and is planned to be the default in Mockito v3"

Mockito final class 생성시
- v2 이상에서 지원은 하지만 default는 deactivated임. 아래와 같이 추가
1. Create the file org.mockito.plugins.MockMaker in either src/test/resources/mockito-extensions/ or src/mockito-extensions/. 
2. Add this line to the file: mock-maker-inline. With this modification we now can mock a final class.

- Regex Pattern 참고
https://regexr.com/ 매우 유용함.
