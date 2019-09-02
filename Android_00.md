# Android

08.26

다운로드 https://developer.android.com/studio



IoT를 조정하는 App은 Native로 만들어야함

SDK : 안드로이드 sdk

Configuration - SDK Manager

Android 필요한 버전 설치



Configure - Setting - Appearance&Behavior - System Settings - Android SDK - Android SDK Location

(다시설치) C:\Users\student\에서 Atl 도구 폴더옵션 히든파일도 지우고 다시 설치



삼성드라이버 설치하기



(가상의 머신 만들기)

Configure - AVD Manager - Create Virtual Device - 5.14" WVGA - show advanced settings - RAM 2048 / 512 - finish



(스마트폰)

설정 - 소프트웨어정보 - 빌드번호 연타 - 개발자옵션 활성화

개발자 옵션 - USB디버깅 활성화

출처를 알수없는 앱 설치



start - empty activity - C:\android\AndroidStudioProjects\Hello - Java - (최소)5.1



gradle : 컴파일할때 필요하면 다운로드받아서 메모리에 올려놓고 실행 (메이븐보다 위)

Gradle Scripts - build.gradle -> maven의 pom.xml같은거

xml - UI

MainActivity.java -> Controller



Servlet과 유사 -> 컨테이너와 컨퍼넌트 모델

onCreate -> 처음 시작하는 곳

```java
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
```



- App - java

​	MainActivity

- App  - res(resource)  => 여러가지 관리

​	drawable -> 앱에서 사용하는 모든 이미지 관리

​	**layout**(UI)

​	mipmap(App이 install될 때 위잽 모양 관리)

​	values-string.xml -> 모든 화면에 쓰여있는 것은 여기에 다 써있어야함



- App - mainfests - AndroidManifest.xml

  앱의 이름 ,환경변수, label 등

  ```xml
  <activity android:name=".MainActivity">
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
  
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
  ```

  MainActivity는 아이콘을 누르면 main으로 시작하도록(onCreate 실행)



배경사진 -> drawable에 복사

아이콘 -> mipmap에 복사

```xml
AndroidManifest.xml

<application
       
        android:icon="@mipmap/icon1"
        android:label="Irene"
        android:roundIcon="@mipmap/icon1"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
```



ConstraintLayout click - 오른쪽에 background





Container -> Android를 실행시키기 위함

Activity -> 화면(UI), 이벤트, 스레드 처리, 서비스 등

APK : 3가지가 묶여서 폰으로 감(war같음)



build - build apk -> apk(출처가 없는 앱)

build - generate signed bundle or apk -> app에 등록



6:Logcat : 에러나 로그 출력





create new logcat filter

 debug

logtag : [Debug]



overwrite generate : generate - methods 



Layout : 뷰 그룹(View group)

컨테이너 깔고 위젯 붙이기



-------------



Component Tree : LinearLayout(vertical)





margin, layout weight, padding

