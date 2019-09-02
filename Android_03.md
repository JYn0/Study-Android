# Android

08.29



* (P213)

  ```java
  package com.example.p213;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.SharedPreferences;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      SharedPreferences sp;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
      }
  
      public void clikbt(View view){
          if(view.getId() == R.id.button){ // save
              sp = getSharedPreferences("ma",MODE_PRIVATE); // MODE_PRIVATE: 저장한 값은 나만 보겠다
              SharedPreferences.Editor editor = sp.edit();
              editor.putString("key01","id01"); // key, value
              editor.commit();
          }else if(view.getId() == R.id.button2){ // load
              sp = getSharedPreferences("ma",MODE_PRIVATE);
              String id = sp.getString("key01","default"); // key, default
              Toast.makeText(this, id, Toast.LENGTH_SHORT).show();
          }
      }
  }
  
  ```

  ![a10](https://user-images.githubusercontent.com/50862497/64108443-bface880-cdb7-11e9-9b1f-b4b4c05fad2c.png)



* (P233)

  ```java
  package com.example.p233;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.view.View;
  import android.widget.ProgressBar;
  import android.widget.SeekBar;
  import android.widget.TextView;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      ProgressBar progressBar;
      SeekBar seekBar;
      TextView textView;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          progressBar = findViewById(R.id.progressBar);
          progressBar.setMax(100);
          seekBar = findViewById(R.id.seekBar);
          textView = findViewById(R.id.textView);
  
  
          seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
              public String number = "";
  
              @Override
              public void onProgressChanged(SeekBar seekBar, int i, boolean b) {
                  progressBar.setProgress(i);
                  textView.setText(Integer.toString(i));
              }
  
              @Override
              public void onStartTrackingTouch(SeekBar seekBar) {
              }
  
              @Override
              public void onStopTrackingTouch(SeekBar seekBar) {
              }
          });
  //        textView.setText(progressBar.getProgress());
  
      }
  
  }
  
  ```

  ![a11](https://user-images.githubusercontent.com/50862497/64108567-15819080-cdb8-11e9-993c-bd17ed464c1e.png)



* (P246)

  ```java
  package com.example.p246;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.os.Bundle;
  import android.view.View;
  
  public class Main3Activity extends AppCompatActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main3);
      }
      public void clickBt(View view){
          // bt를 클릭하면 main2에게 intent를 만들어 보내고 죽음
          Intent intent = new Intent();
          intent.putExtra("nm",100);
          intent.putExtra("st","Hi,Hello");
  
          // main2로 값 보내기
          setResult(RESULT_OK,intent);
  
          finish();
      }
  }
  
  ```

  ![a14](https://user-images.githubusercontent.com/50862497/64109144-a9a02780-cdb9-11e9-9a43-d83f9e35884c.png)



* (P247)

  ```java
  MainActivity.java
  
  package com.example.p247;
  
  import androidx.annotation.Nullable;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.TextView;
  
  public class MainActivity extends AppCompatActivity {
  
      TextView textView;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
      }
  
      public void clickBt(View view){
          Intent intent = new Intent(MainActivity.this, Main2Activity.class);
  
          if(view.getId() == R.id.click1){
              intent.putExtra("img","img1");
          }else if(view.getId() == R.id.click2){
              intent.putExtra("img", "img2");
          }else if(view.getId() == R.id.click3){
              intent.putExtra("img","img3");
          }
          startActivityForResult(intent, 100);
      }
  
      @Override
      protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
          super.onActivityResult(requestCode, resultCode, data);
          if(requestCode==100 && resultCode==RESULT_OK){
              String text = data.getStringExtra("imgname");
              textView.setText(text);
          }
      }
  }
  
  ```

  ```java
  Main2Activity.java
  package com.example.p247;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.EditText;
  import android.widget.ImageView;
  import android.widget.TextView;
  
  import org.w3c.dom.Text;
  
  public class Main2Activity extends AppCompatActivity {
  
      ImageView imageView;
  
      String imgname;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main2);
          imageView = findViewById(R.id.imageView);
  
          Intent intent = getIntent();
          imgname = intent.getStringExtra("img");
  
          if(imgname.equals("img1")){
              imageView.setImageResource(R.drawable.img1);
          }else if(imgname.equals("img2")){
              imageView.setImageResource(R.drawable.img2);
          } else if (imgname.equals("img3")) {
              imageView.setImageResource(R.drawable.img3);
          }
      }
  
      public void clickBt(View view){
          Intent intent = new Intent();
          intent.putExtra("imgname",imgname);
          setResult(RESULT_OK,intent);
          finish();
      }
  }
  
  ```

  ![a15](https://user-images.githubusercontent.com/50862497/64109640-005a3100-cdbb-11e9-9613-6705a3fb0c15.png)

  ![a16](https://user-images.githubusercontent.com/50862497/64109647-06501200-cdbb-11e9-87e9-f00e1a7753c8.png)



* (P285) 세 개 이상의 화면 만들어 전환하기

  ```java
  package com.example.p285;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.content.SharedPreferences;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.CheckBox;
  import android.widget.EditText;
  import android.widget.TextView;
  import android.widget.Toast;
  
  import org.w3c.dom.Text;
  
  public class MainActivity extends AppCompatActivity {
  
      EditText nameText;
      EditText pwdText;
      CheckBox checkBox;
      SharedPreferences sp;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          nameText = findViewById(R.id.nameText);
          pwdText = findViewById(R.id.pwdText);
          checkBox = findViewById(R.id.checkBox);
  
          sp = getSharedPreferences("ma", MODE_PRIVATE);
          if(sp.contains("id")){
              changePage();
          }
      }
  
      public void clickbt(View view){
          if(view.getId() == R.id.loginbt){
              System.out.println(nameText.getText());
              System.out.println(pwdText.getText());
              if((nameText.getText().toString().equals("id"))&&(pwdText.getText().toString().equals("pwd"))){
                  sp = getSharedPreferences("ma", MODE_PRIVATE);
                  SharedPreferences.Editor editor = sp.edit();
                  if(checkBox.isChecked()){
                      editor.putString("id","id");
                      editor.putString("pwd","pwd");
                      editor.commit();
                  }else{
                      editor.clear();
                      editor.commit();
                  }
                  changePage();
              }
          }
      }
  
      public void changePage(){
          Intent intent = new Intent(MainActivity.this, Main2Activity.class);
          startActivity(intent);
      }
  }
  
  ```

  ```java
  package com.example.p285;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.app.AlertDialog;
  import android.app.Dialog;
  import android.content.DialogInterface;
  import android.content.Intent;
  import android.content.SharedPreferences;
  import android.os.Bundle;
  import android.view.View;
  
  public class Main2Activity extends AppCompatActivity {
  
      SharedPreferences sp;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main2);
          sp = getSharedPreferences("ma", MODE_PRIVATE);
      }
  
      public void clickBt(View view){
          if(view.getId() == R.id.button1){
              Intent intent = new Intent(Main2Activity.this, Main3Activity.class);
              startActivity(intent);
          }
      }
  
      @Override
      public void onBackPressed() {
  //        super.onBackPressed();
          AlertDialog.Builder dialog = new AlertDialog.Builder(this);
          dialog.setTitle("Alert");
          dialog.setMessage("Login Page, Logout...?");
  
          dialog.setNegativeButton("NO", new DialogInterface.OnClickListener() {
              @Override
              public void onClick(DialogInterface dialogInterface, int i) {
                  return;
              }
          });
  
          dialog.setPositiveButton("YES", new DialogInterface.OnClickListener() {
              @Override
              public void onClick(DialogInterface dialogInterface, int i) {
                  sp = getSharedPreferences("ma", MODE_PRIVATE);
                  SharedPreferences.Editor editor = sp.edit();
                  editor.clear();
                  editor.commit();
                  finish();
              }
          });
          dialog.show();
      }
  }
  
  ```

  ```java
  package com.example.p285;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.content.SharedPreferences;
  import android.os.Bundle;
  import android.view.View;
  
  public class Main3Activity extends AppCompatActivity {
  
      SharedPreferences sp;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main3);
          sp = getSharedPreferences("ma", MODE_PRIVATE);
      }
  
      public void clickBt(View view){
          if(view.getId() == R.id.buttonM1){
              finish();
          }else if(view.getId() == R.id.buttonL1){
              sp = getSharedPreferences("ma", MODE_PRIVATE);
              SharedPreferences.Editor editor = sp.edit();
              editor.clear();
              editor.commit();
  
              Intent intent = new Intent(Main3Activity.this, MainActivity.class);
              startActivity(intent);
  
          }
      }
  }
  
  ```

  ![a18](https://user-images.githubusercontent.com/50862497/64110587-8c6d5800-cdbd-11e9-8d2c-7358216a5d5b.png)



* (P287) 프래그먼트

  ```java
  MainActivity.java
  
  package com.example.p287;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.graphics.Color;
  import android.os.Bundle;
  import android.util.Log;
  import android.view.View;
  import android.widget.Button;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      View1Fragment view1Fragment;
      View2Fragment view2Fragment;
      View3Fragment view3Fragment;
  
      Button button,button2,button3;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          // 객체 선언
          view1Fragment = (View1Fragment)getSupportFragmentManager().findFragmentById(R.id.fragment);
          // getSupport와 new의 다른점 : mainActivity의 제어 가능
  //        view1Fragment = new View1Fragment(); // mainActivity가 view1Fragment를 제어할 수 없음
          view2Fragment = new View2Fragment();
          view3Fragment = new View3Fragment();
          button = findViewById(R.id.button);
          button2 = findViewById(R.id.button2);
          button3 = findViewById(R.id.button3);
      }
  
      public void setBt(){
          // 버튼 1,2,3 색 바꾸기
  //        button.setBackgroundColor(Color.RED);
  //        button2.setBackgroundColor(Color.BLUE);
  //        button3.setBackgroundColor(Color.YELLOW);
          // 함수 호출은 가능하지만 widgets은 건드릴 수 없다.
          // 다른 쓰레드는 건드릴 수 없음, fragment가 mainActivity를 건드릴 수 없다
          Log.i("*","--------------------");
      }
  
      public void clickBt(View view){
          if(view.getId() == R.id.button){
              onFragmentChange(1);
  //            view1Fragment.setT();
          }else if(view.getId() == R.id.button2){
              onFragmentChange(2);
          }else if(view.getId() == R.id.button3){
              onFragmentChange(3);
          }
      }
  
      public void onFragmentChange(int index){
          if(index == 1){
              // begin~: 화면 바꿔라, replace:container 영역의 fragment를 바꿈
              // getSupport~: activity와 fragment를 왔다갔다 할때
              getSupportFragmentManager().beginTransaction().replace(R.id.container,view1Fragment).commit();
              view1Fragment.setT();
          }else if(index == 2){
              getSupportFragmentManager().beginTransaction().replace(R.id.container,view2Fragment).commit();
          }else if(index == 3){
              getSupportFragmentManager().beginTransaction().replace(R.id.container,view3Fragment).commit();
          }
      }
  }
  
  ```

  ```java
  View1Fragment.java
  package com.example.p287;
  
  import android.content.Context;
  import android.net.Uri;
  import android.os.Bundle;
  
  import androidx.annotation.NonNull;
  import androidx.annotation.Nullable;
  import androidx.fragment.app.Fragment;
  
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.Button;
  import android.widget.TextView;
  
  
  public class View1Fragment extends Fragment {
  
      Button button4, button5;
      TextView textView;
  
      @Override
      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container,
                               @Nullable Bundle savedInstanceState) {
          ViewGroup viewGroup = (ViewGroup) inflater.inflate(R.layout.fragment_view1, container, false);
  
          button4 = viewGroup.findViewById(R.id.button4);
          button5 = viewGroup.findViewById(R.id.button5);
          textView = viewGroup.findViewById(R.id.textView);
  
          button4.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View view) {
                  MainActivity ma = new MainActivity(); // 버튼을 클릭하면 메인 어딘가를 바꾼다
                  ma.setBt();
              }
          });
  
          button5.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View view) {
                  textView.setText("View1 Fragment");
              }
          });
  
          return viewGroup;
      }
  
      public void setT(){
          // fragment의 일정영역 바꾸기
          textView.setText("Main...");
      }
  
  }
  
  ```

  ```java
  View2Fragment.java
  package com.example.p287;
  
  import androidx.lifecycle.ViewModelProviders;
  
  import android.os.Bundle;
  
  import androidx.annotation.NonNull;
  import androidx.annotation.Nullable;
  import androidx.fragment.app.Fragment;
  
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  
  public class View2Fragment extends Fragment {
  
      @Override
      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container,
                               @Nullable Bundle savedInstanceState) {
          return inflater.inflate(R.layout.fragment_view2, container, false);
      }
  
  }
  
  ```

  ```java
  View3Fragment.java
  package com.example.p287;
  
  import android.content.Context;
  import android.net.Uri;
  import android.os.Bundle;
  
  import androidx.annotation.NonNull;
  import androidx.annotation.Nullable;
  import androidx.fragment.app.Fragment;
  
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  
  public class View3Fragment extends Fragment {
  
      @Override
      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container,
                               @Nullable Bundle savedInstanceState) {
          return inflater.inflate(R.layout.fragment_view3, container, false);
      }
  }
  
  ```

  ![a19](https://user-images.githubusercontent.com/50862497/64110961-71e7ae80-cdbe-11e9-9df9-80501a68a8b4.png)



* (P288)

  ```java
  MainActivity.java
  package com.example.p288;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.view.View;
  import android.widget.Button;
  
  public class MainActivity extends AppCompatActivity {
  
      View1Fragment view1Fragment;
      View2Fragment view2Fragment;
      Button button1,button2;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
  //        view1Fragment = (View1Fragment)getSupportFragmentManager().findFragmentById(R.id.fragment);
          view1Fragment = new View1Fragment();
          view2Fragment = new View2Fragment();
      }
  
      public void clickBt(View view){
          if(view.getId() == R.id.button1){
              changeColor(1);
          }else if(view.getId() == R.id.button2){
              changeColor(2);
          }
      }
  
      public void changeColor(int index){
          if(index == 1){
              getSupportFragmentManager().beginTransaction().replace(R.id.container,view1Fragment).commit();
          }else if(index == 2){
              getSupportFragmentManager().beginTransaction().replace(R.id.container,view2Fragment).commit();
          }
      }
  }
  
  ```

  ```java
  View1Fragment.java
  package com.example.p288;
  
  import android.content.Context;
  import android.net.Uri;
  import android.os.Bundle;
  
  import androidx.fragment.app.Fragment;
  
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  
  
  public class View1Fragment extends Fragment {
      @Override
      public View onCreateView(LayoutInflater inflater, ViewGroup container,
                               Bundle savedInstanceState) {
          // Inflate the layout for this fragment
  
          ViewGroup viewGroup = (ViewGroup) inflater.inflate(R.layout.fragment_view1, container, false);
          return viewGroup;
  
  
      }
  
  
  }
  
  ```

  ```java
  View2Fragment.java
  package com.example.p288;
  
  import android.content.Context;
  import android.net.Uri;
  import android.os.Bundle;
  
  import androidx.fragment.app.Fragment;
  
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  
  
  public class View2Fragment extends Fragment {
      @Override
      public View onCreateView(LayoutInflater inflater, ViewGroup container,
                               Bundle savedInstanceState) {
          // Inflate the layout for this fragment
          return inflater.inflate(R.layout.fragment_view2, container, false);
      }
  
  }
  
  ```

  ![a20](https://user-images.githubusercontent.com/50862497/64111123-ddca1700-cdbe-11e9-8207-0cd98ecc8a66.png)

.

