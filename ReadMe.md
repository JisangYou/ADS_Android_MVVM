# ADS04 Android

## 수업 내용

- MVVM 디자인 패턴을 학습

## Code Review

### MainActivity
```Java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //setContentView(R.layout.activity_main);
        MainBinding mainBinding = DataBindingUtil.setContentView(this, R.layout.main); // 데이터 바인딩을 하기위한 코드
        User user = new User("jisang", "hanol");
        mainBinding.setUser(user); // setUser 메소드를 클릭하면,    
                                /*<variable
                                    name="user"
                                    type="com.practice.mvvm.User"/>
                                    이 부분으로 이동함*/
    }
}
```

### User
```Java
public class User { // pojo클래스 형태로 해도되고, javaBeans 객체로 해도 됨.
    public String firstName;
    public String lastName;
    public User(String firstName, String lastName){
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
```

### main.xml
```xml
<!--layout 소문자 태그로 시작 -->
<layout>
    <!-- User user = new User() -->
    <data>
        <variable
            name="user"
            type="com.practice.mvvm.User"/>
    </data>
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:id="@+id/textView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{user.firstName}" />

        <TextView
            android:id="@+id/textView2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{user.lastName}" />
    </LinearLayout>

</layout>
```
### build.gradle (Module:app)

- databinding setting

```Java
apply plugin: 'com.android.application'

android {
   // 생략....
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    //-------------------------
    dataBinding{
        enabled = true
    }
    //-------------------------
}

dependencies {
   //생략...
}

```

## 보충설명

### MVVM

![MVVM](https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvvm.png)

> ViewModel : View를 표현하기 위해 만들어진 View를 위한 Model

- MVVM은 두가지 디자인 패턴을 사용(__Command패턴__과 __Data Binding__)
- 이 두가지 패턴으로 인해 View와 ViewModel은 의존성이 완전히 사라짐.
```java
1. View에 입력이 들어오면 Command 패턴으로 ViewModel에 명령
2. ViewModel은 필요한 데이터를 Model에 요청
3. Model은 ViewModel에 필요한 데이터를 응답
4. ViewModel은 응답 받은 데이터를 가공해서 저장
5. View는 ViewModel과의 Data Binding으로 인해 자동으로 갱신
```

#### CommandPattern

>__커맨드 패턴(Command pattern)이란?__

![커맨드패턴](http://www.jidum.com/upload/ckeditor/2016/10/20161020150657445.png)

```java 
요청을 객체의 형태로 캡슐화하여 사용자가 보낸 요청을 나중에 이용할 수 있도록 매서드 이름, 매개변수 등 요청에 필요한 정보를 저장 또는 로깅, 취소할 수 있게 하는 패턴이다.
커맨드 패턴에는 명령(command), 수신자(receiver), 발동자(invoker), 클라이언트(client)의 네개의 용어가 항상 따른다. 

- 클라이언트 : 커맨드 객체를 생성하고 인보커를 통해 리시버에 전달하여 요청한다.
- 인보커 : 클라이언트의 커맨드 객체를 리시버에 전달한다.
- 리시버 : 요청대로 특정 행동을 수행한다.
- 커맨드 : 어떠한 특정 행동을 담고 있는 추상 객체이다

ex)  
- 고객이 웨이터한테 주문을 한다. (고객=클라이언트)
- 주문 내용은 주문서를 통해 전달된다. (주문서=커맨드 객체)
- 웨이터는 주문서를 카운터에 전달한다. (웨이터=인보커)
- 주방장이 주문서대로 음식을 준비한다. (주방장=리시버)

```
#### databinding

>__데이터바인딩이란?__

- 선언적 레이아웃을 작성하고 애플리케이션 로직과 레이아웃을 바인딩하는 데 필요한 글루 코드를 최소화하는 방법
- 데이터 바인딩은 응용 프로그램 UI와 비즈니스 논리를 서로 연결하는 프로세스
- 바인딩 설정이 올바르고 데이터가 적절한 알림을 제공하는 경우에는 데이터의 값이 변경될 때 데이터에 바인딩된 요소에 변경 내용이 자동으로 반영

- [AndroidDeveloper](https://developer.android.com/topic/libraries/data-binding/index.html?hl=ko)

### 출처

- 출처 : https://magi82.github.io/android-mvc-mvp-mvvm/
- 출처: https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%A7%A8%EB%93%9C_%ED%8C%A8%ED%84%B4
- 출처: http://moonshoo.tistory.com/5 [골드망고 개발 블로그]

## TODO

- databinding으로 예제 만들어서 사용해보기(ex) RecyclerView, fragment등에 적용
- 그동안 배웠던 디자인 패턴 정리 및 적용해보기

## Retrospect

- 중복코드를 사라지게 하고, 데이터의 변경을 자동으로 반영한다는 게 굉장히 매력적인 것 같다.
- 그러나, 간단한 예제만 다루어 보았기에 어떤 장점이 더 있는지, 코드량이 많아지면 어떤 문제 애로사항이 있는지 모르기때문에 예제를 만들어서 공부해봐야겠다. 
- 기존에 배웠던 MVC,MVP하고는 또다른 유형의 디자인 패턴이지만 개인적으로 한번 프로젝트에 적용해보고 싶다. 

## Output
- 생략
