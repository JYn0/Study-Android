# Android

08.27



* (Hello) 어플의 이미지 만들기, 앱의 실행,정지,버튼 클릭

  ```java
  package com.example.hello;
  
  import android.os.Bundle;
  import android.util.Log;
  import android.view.View;
  import android.widget.Toast;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  public class MainActivity extends AppCompatActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
      }
  
      @Override
      protected void onPostResume() {
          super.onPostResume();
          Toast.makeText(this, "Resume...", Toast.LENGTH_SHORT).show();
      }
  
      @Override
      protected void onPause() { // app이 사라질때
          super.onPause();
          Toast.makeText(this, "Pause...", Toast.LENGTH_LONG).show();
      }
  
      public void clickButton(View view){
          Toast.makeText(this, "click button", Toast.LENGTH_SHORT).show();
          Log.d("[Debug]......","OK");
          Log.i("[Info]......","Information");
          Log.e("[Error]......","Error");
      }
  }
  ```

  ```xml
  activity_main.xml
  
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:background="@drawable/bg1"
      tools:context=".MainActivity">
  
      <TextView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="Hello World!"
          android:textColor="#F5DDDF"
          android:textSize="30sp" />
  
      <Button
          android:id="@+id/button"
          android:layout_width="136dp"
          android:layout_height="80dp"
          android:onClick="clickButton"
          android:text="click" />
  
  </LinearLayout>
  ```

  ```xml
  AndroidManifest.xml
  
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.hello">
  
      <application
          android:allowBackup="true"
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
  
  </manifest>
  ```

  ![a01](https://user-images.githubusercontent.com/50862497/64104427-2aa5f180-cdaf-11e9-9242-9f0ec2e4e164.png)

  

* (P115) click 누르면 button 색 변경하기

  ```java
  package com.example.p115;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.graphics.Color;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.Button;
  
  public class MainActivity extends AppCompatActivity {
  
      Button bt2;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          bt2 = findViewById(R.id.button36);
      }
      public void btclick(View view){
          bt2.setBackgroundColor(Color.RED);
          bt2.setText("Clicked");
      }
  }
  
  ```

  ![a02](https://user-images.githubusercontent.com/50862497/64104758-d18a8d80-cdaf-11e9-88f8-56c27f84501f.png)

* (P158) 왼쪽 버튼을 누르면 하단에 사진, 오른쪽 버튼을 누르면 상단에 로그인/회원가입

  ```java
  package com.example.p158;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.constraintlayout.widget.ConstraintLayout;
  
  import android.os.Bundle;
  import android.view.View;
  import android.widget.ImageView;
  
  public class MainActivity extends AppCompatActivity {
  
      ImageView img;
      ConstraintLayout loginLayer,registerLayer;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          setUi();
      }
  
      private void setUi() {
          img = findViewById(R.id.img); // img 받아오기
          loginLayer = findViewById(R.id.loginLayer);
          registerLayer = findViewById(R.id.registerLayer);
          disable();
      }
  
      public void disable(){ // 안보이게
          loginLayer.setVisibility(View.INVISIBLE);
          registerLayer.setVisibility(View.INVISIBLE);
      }
  
      public void clickBt(View view){ // 모든 버튼 처리하는 곳
          if(view.getId() == R.id.bt1){
              disable();
              img.setImageResource(R.drawable.img1);
          }else if(view.getId() == R.id.bt2){
              disable();
              img.setImageResource(R.drawable.img2);
          }else if(view.getId() == R.id.bt3){
              loginLayer.setVisibility(View.VISIBLE);
              registerLayer.setVisibility(View.INVISIBLE);
          }else if(view.getId() == R.id.bt4){
              loginLayer.setVisibility(View.INVISIBLE);
              registerLayer.setVisibility(View.VISIBLE);
          }
      }
  
  }
  ```

  ![a03](https://user-images.githubusercontent.com/50862497/64105414-27136a00-cdb1-11e9-8461-9d42f1a81703.png)



* (P168) 위,아래 버튼을 눌러 사진 띄우기

  ```java
  package com.example.p168;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.constraintlayout.widget.ConstraintLayout;
  
  import android.os.Bundle;
  import android.view.View;
  import android.widget.FrameLayout;
  import android.widget.ImageView;
  import android.widget.LinearLayout;
  
  public class MainActivity extends AppCompatActivity {
  
      ImageView upimg, downimg;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          setUi();
      }
  
      private void setUi() {
          upimg = findViewById(R.id.upimg);
          downimg = findViewById(R.id.downimg);
      }
  
      public void clickBt(View view){
          if(view.getId() == R.id.up){
              upimg.setImageResource(R.drawable.icon1);
              upimg.setVisibility(View.VISIBLE);
              downimg.setVisibility(View.INVISIBLE);
          }else if(view.getId() == R.id.down){
              downimg.setImageResource(R.drawable.icon1);
              upimg.setVisibility(View.INVISIBLE);
              downimg.setVisibility(View.VISIBLE);
          }
      }
  
  
  }
  ```

  ![a04](https://user-images.githubusercontent.com/50862497/64105573-78bbf480-cdb1-11e9-815f-f40ce1169b3a.png)



