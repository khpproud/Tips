# Tips
Trivial tips for developing

- layout File 내 <merge ...> 태그 사용 후 layout 미리 보기 시
    ```
    <merge xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    tools:parentTag="android.widget.LinearLayout"
    tools:orientation="vertical">
    ```
        
- Animation 테스트시 terminal에서 개발자 설정의 Animation speed 조절하기 (0.5 ~ 10)
```
adb shell settings put global window_animation_scale 10
adb shell settings put global transition_animation_scale 10
adb shell settings put global animator_duration_scale 10
```

- SavedStateHandle에 의존하는 ViewModel test 시
```
@Before
fun setup() {
    val savedStateHandle = SavedStateHandle()
    viewModel = SampleViewModel(savedStateHandle)
}
```

1. savedStateHandle을 mocking 하지 않고 직접 초기화하여 기능 테스트(get/set)
2. 내부적으로 Map에 값을 저장/회수하기 때문에 테스트에 오버헤드가 크지 않음
3. ref: https://android.googlesource.com/platform/frameworks/support/+/refs/heads/androidx-master-dev/lifecycle/lifecycle-viewmodel-savedstate/src/main/java/androidx/lifecycle/SavedStateHandle.java?source=post_page---------------------------%2F%2F%2F%2F%2F%2F%2F&autodive=0%2F%2F%2F%2F

- AndroidStudio 4.0 Gradle plugin DSL 변경

~~android {~~
    ~~viewBinding {~~
        ~~enabled = true~~
    ~~}~~
~~}~~


```
android {
    buildFeatures {
        viewBinding = true
    }
}
```

- Kotlin Dagger 

prb) @MapKey + @IntoMap 을 통한 멀티바인딩 injection 시에 
error: [Dagger/MissingBinding] java.util.Map<java.lang.String,? extends (Value type) 에러가 발생
wildcard를 인식하지 못하는 문제

sol) @Injector(paramName : Map<key, @JvmSuppressWildcards value> 와 같이 
wildcard를 suppress하는 어노테이션을 붙여줌

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

Regex Pattern 참고
https://regexr.com/ 매우 유용함.

AndroidX Migrate 시 DataBindingUtil.setContentView(...) 에서 "Cannot infer type ViewDataBinding..." 에러 발생시
=> File -> Invalidate Caches / Restart 해줌

Random Image 
https://source.unsplash.com/random 활용 specific dimension 은 ?w=... 활용

ViewModel 변경 내용(androidx.lifecycle Version 2.2.0-alpha03)
- ViewModelProviders.of()을 사용한 ViewModel 객체 획득시 위 버전 부터 deprecated됨
- 다음과 같이 변경
1. androidx.fragment-ktx import 하여 => by viewModels<(ViewModel 클래스)> { (factory) } 확장 위임 함수 사용
2. ViewModelProvider((ViewModelStoreOwner 객체))[(viewModel 클래스::class.java)] 이용
  => 이 방법 사용시 Fragment 가 아닌 Activity가 Owner로 설정하려면 activity as ViewModelStoreOwner 로 type casting 해줌.
  
AndroidXTest
robolectric 사용시 Android resource 사용 추가
// app/build.gradle
testOptions.unitTests {
    includeAndroidResources = true
}

Mockito dexmaker
1. Mocking final classes& methods: 버전 p 이상 => dexmaker-mockito-inline dependency에 추가

