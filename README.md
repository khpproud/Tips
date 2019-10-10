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

