# Android

08.28





- (P169) SMS 입력 화면 만들고 글자의 수 표시하기

  ```java
  package com.example.p169;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.text.Editable;
  import android.text.TextWatcher;
  import android.view.View;
  import android.widget.EditText;
  import android.widget.TextView;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      EditText editText;
      TextView textView;
      TextView byteLabel;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          editText = findViewById(R.id.editText);
          byteLabel=findViewById(R.id.textView);
          editText.addTextChangedListener(tw);
          editText.setHorizontallyScrolling(false);
      }
  
      public void clickB(View view){
          if(view.getId() == R.id.sendB){
              Toast.makeText(this, editText.getText(), Toast.LENGTH_SHORT).show();
              editText.setText("");
          }else if(view.getId() == R.id.closeB){
              Toast.makeText(this, "Close", Toast.LENGTH_SHORT).show();
              System.exit(0);
          }
      }
  
      TextWatcher tw = new TextWatcher() {
          @Override
          public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2) {
  
          }
  
          @Override
          public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
              int a = editText.length();
              byteLabel.setText(a+" / 80 바이트");
          }
  
          @Override
          public void afterTextChanged(Editable editable) {
  
          }
      };
  
  }
  
  ```

  ![a05](https://user-images.githubusercontent.com/50862497/64105973-347d2400-cdb2-11e9-8b67-2d7ad528a5f1.png)



- (P178) Switch Button Event, RadioButton, CheckBox, EditText

  ```java
  package com.example.p178;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.graphics.Color;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.Button;
  import android.widget.CheckBox;
  import android.widget.CompoundButton;
  import android.widget.EditText;
  import android.widget.RadioButton;
  import android.widget.Switch;
  import android.widget.TabWidget;
  import android.widget.TextView;
  import android.widget.Toast;
  import android.widget.ToggleButton;
  
  public class MainActivity extends AppCompatActivity
  implements View.OnClickListener{ // MainActivity에서 onclick을 처리할것이다
      Button bt;
      RadioButton radioButton,radioButton2;
      CheckBox checkBox,checkBox2;
      Switch switch1;
      ToggleButton toggleButton;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          bt = findViewById(R.id.button);
          radioButton = findViewById(R.id.radioButton);
          radioButton2 = findViewById(R.id.radioButton2);
          checkBox = findViewById(R.id.checkBox);
          checkBox2 = findViewById(R.id.checkBox2);
          toggleButton = findViewById(R.id.toggleButton);
          switch1 = findViewById(R.id.switch1);
  
          final EditText editText,editText3,editText4;
  
          editText = findViewById(R.id.editText);
          editText3 = findViewById(R.id.editText3);
          editText4 = findViewById(R.id.editText4);
  
          editText.setOnFocusChangeListener(new View.OnFocusChangeListener() {
              @Override
              public void onFocusChange(View view, boolean b) {
  //                editText.setFocusable(true);
                  if(b == true){
                      editText.setHint("typing your name");
                  }else{
                      editText.setHint("");
                  }
              }
          });
  
          // switch에 이벤트가 들어오면 ()안에서 처리
          switch1.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
              @Override
              public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
                  // 이름없는 class, 이벤트처리를 위한 클래스
                  if(b == true){
                      bt.setBackgroundColor(Color.RED);
                      Toast.makeText(MainActivity.this, "Click ", Toast.LENGTH_SHORT).show();
                  }else{
                      bt.setBackgroundColor(Color.BLUE);
                  }
              }
          });
  
          toggleButton.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
              @Override
              public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
                  if(b == true){
                      toggleButton.setBackgroundColor(Color.RED);
                  }else{
                      toggleButton.setBackgroundColor(Color.BLUE);
                  }
              }
          });
  
          // bt에 이벤트가 들어오면 this(MainActivity)의 onClick이 처리할것이다
          bt.setOnClickListener(this);
  
      }
  
      @Override
      public void onClick(View view) {
  
          Toast.makeText(this, "Click "+ checkBox.isChecked()+radioButton.isChecked(),
                  Toast.LENGTH_SHORT).show();
  
      }
  }
  ```

  ![a06](https://user-images.githubusercontent.com/50862497/64106391-0f3ce580-cdb3-11e9-8cd6-834af477110f.png)



- (P200) 터치 이벤트

  ```java
  package com.example.p200;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.res.Configuration;
  import android.os.Bundle;
  import android.view.GestureDetector;
  import android.view.KeyEvent;
  import android.view.MotionEvent;
  import android.view.View;
  import android.widget.TextView;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      View view, view2;
      TextView textView;
      GestureDetector gestureDetector;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          setUI();
      }
  
      private void setUI(){
          view = findViewById(R.id.view);
          view2 = findViewById(R.id.view2);
          textView = findViewById(R.id.textView);
          gestureDetector = new GestureDetector(this, new GestureDetector.OnGestureListener() {
              @Override
              public boolean onDown(MotionEvent motionEvent) {
                  printMsg("onDown() 호출");
                  return true;
              }
  
              @Override
              public void onShowPress(MotionEvent motionEvent) {
                  printMsg("onShowPress() 호출");
              }
  
              @Override
              public boolean onSingleTapUp(MotionEvent motionEvent) {
                  printMsg("onSingleTabUp() 호출");
                  return true;
              }
  
              @Override
              public boolean onScroll(MotionEvent motionEvent, MotionEvent motionEvent1, float v, float v1) {
                  printMsg("onScroll() 호출"+v+","+v1);
                  return true;
              }
  
              @Override
              public void onLongPress(MotionEvent motionEvent) {
                  printMsg("onLongPress() 호출");
              }
  
              @Override
              public boolean onFling(MotionEvent motionEvent, MotionEvent motionEvent1, float v, float v1) {
                  printMsg("onFling() 호출"+v+","+v1);
                  return true;
              }
          });
  
          view.setOnTouchListener(new View.OnTouchListener() {
              @Override
              public boolean onTouch(View view, MotionEvent motionEvent) {
                  // view영역 안에서 손을 눌렀거나 눌린상태로 움직이거나 떼어지는거 검출
                  int action = motionEvent.getAction();
                  float curX = motionEvent.getX();
                  float curY = motionEvent.getY();
                  if(action == motionEvent.ACTION_DOWN){
                      printMsg("DOWN"+curX+" "+curY);
                  }else if(action == motionEvent.ACTION_MOVE){
                      printMsg("MOVE"+curX+" "+curY);
                  }else if(action == motionEvent.ACTION_UP){
                      printMsg("UP"+curX+" "+curY);
                  }
                  return true;
              }
          });
  
          view2.setOnTouchListener(new View.OnTouchListener() {
              @Override
              public boolean onTouch(View view, MotionEvent motionEvent) {
                  // gestureDetector에게 현재 발생한 event를 넘겨준다
                  gestureDetector.onTouchEvent(motionEvent);
  
                  return true;
              }
          });
      }
  
      private void printMsg(String str){
          textView.append(str+"\n");
          textView.setText(str+"\n"+textView.getText());
      }
  
      @Override
      public boolean onKeyDown(int keyCode, KeyEvent event) { // smartphone의 버튼(back, home, volume...)
          if(keyCode == KeyEvent.KEYCODE_BACK){
              Toast.makeText(this, "Back...", Toast.LENGTH_SHORT).show();
              return true; // app 꺼지지 않음
          }
          return false;
  //        return super.onKeyDown(keyCode, event);
      }
  
      @Override
      public void onConfigurationChanged(Configuration newConfig) { // 방향전환
          super.onConfigurationChanged(newConfig);
          if(newConfig.orientation == Configuration.ORIENTATION_LANDSCAPE){
              showToast("방향 : ORIENTATION_LANDSCAPE");
          }else if(newConfig.orientation == Configuration.ORIENTATION_PORTRAIT){
              showToast("방향 : ORIENTATION_PORTRAIT");
          }
      }
  
      public void showToast(String str){
          Toast.makeText(this, str, Toast.LENGTH_SHORT).show();
      }
  }
  
  ```

  ![a07](https://user-images.githubusercontent.com/50862497/64106650-938f6880-cdb3-11e9-88ce-c37470c02f4f.png)



- (P212) X

  ```java
  package com.example.p212;
  
  import androidx.annotation.NonNull;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.os.PersistableBundle;
  import android.view.View;
  import android.widget.Button;
  import android.widget.EditText;
  import android.widget.QuickContactBadge;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
      String appID;
      EditText editText;
      Button button,button2;
  
      @Override
      protected void onCreate(final Bundle savedInstanceState) {
          // savedInstanceState 상태를 bundle에 저장해두는것
          // Bundle : 앱이 살아있는동안 특정한 곳에 저장해둠, 화면이 움직이거나 화면이 없어졌다가 다시 띄울때 임시로 저장하는 것
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          editText = findViewById(R.id.editText);
          button = findViewById(R.id.button);
  
          button.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View view) {
                  String data = editText.getText().toString();
                  savedInstanceState.putString("save",data); // save로 저장
              }
          });
  
          button2 = findViewById(R.id.button2);
          button2.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View view) {
                  String data =null;
                  if(savedInstanceState != null){
                      data = savedInstanceState.getString("ID");
                  }else{
                      data = "Empty";
                  }
                  Toast.makeText(MainActivity.this, data, Toast.LENGTH_SHORT).show();
              }
          });
      }
  
      @Override
      public void onSaveInstanceState(Bundle outState) {
          super.onSaveInstanceState(outState);
          Toast.makeText(this, "onSaveInstanceState", Toast.LENGTH_SHORT).show();
          outState.putString("ID","id01");
      }
  }
  
  ```



* (P217)

  ```java
  package com.example.p217;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.app.AlertDialog;
  import android.content.DialogInterface;
  import android.os.Bundle;
  import android.view.Gravity;
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.TextView;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
      }
  
      public void toast(View view){
  
          LayoutInflater inflater = getLayoutInflater();
          View tview = inflater.inflate(R.layout.toast, (ViewGroup) findViewById(R.id.tlayout));
          Toast toast = new Toast(this);
          toast.setGravity(Gravity.CENTER,0,-100);
          toast.setDuration(Toast.LENGTH_LONG);
          toast.setView(tview);
          toast.show();
  
  //        Toast toast = Toast.makeText(this,"hi",Toast.LENGTH_LONG);
  //        toast.setGravity(Gravity.TOP|Gravity.TOP, 0, 300);
  //        toast.show();
      }
      public void snack(View view){
  
      }
      public void dialog(View view){
          AlertDialog.Builder builder = new AlertDialog.Builder(this);
          builder.setTitle("my dialog");
  //        builder.setMessage("Do you Exit App ?");
          builder.setIcon(R.drawable.icon1);
  
          // Dialog 내 버튼 추가 긍정/부정 버튼 기능 정의
          builder.setNegativeButton("NO", new DialogInterface.OnClickListener() {
              @Override
              public void onClick(DialogInterface dialogInterface, int i) {
                  Toast.makeText(MainActivity.this, "Continue", Toast.LENGTH_SHORT).show();
              }
          });
          builder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
              @Override
              public void onClick(DialogInterface dialogInterface, int i) {
                  Toast.makeText(MainActivity.this, "BYE", Toast.LENGTH_SHORT).show();
                  finish();
              }
          });
  
          LayoutInflater inflater = getLayoutInflater();
          View tview= inflater.inflate(R.layout.toast, (ViewGroup) findViewById(R.id.tlayout));
          TextView tv = tview.findViewById(R.id.textView);
          tv.setText("Do you wanna Exit App..?");
  
  //        builder.setView(tview);
  //        builder.show();
  
          AlertDialog dialog = builder.create();
          dialog.setCancelable(false); // 모달
          dialog.show();
      }
      public void progress(View view){
  
      }
  }
  
  ```

  ![a08](https://user-images.githubusercontent.com/50862497/64107878-6d1efc80-cdb6-11e9-934a-f9b16a62a133.png)



* (P228)

  ```java
  package com.example.p228;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.app.ProgressDialog;
  import android.content.DialogInterface;
  import android.os.Bundle;
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.ProgressBar;
  import android.widget.TextView;
  
  public class MainActivity extends AppCompatActivity {
  
      ProgressBar progressBar;
      ProgressDialog progressDialog;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          progressBar = findViewById(R.id.progressBar);
          progressBar.setMax(100);
      }
      public void bar(View view){
          if(view.getId() == R.id.button){
              progressBar.setProgress(progressBar.getProgress()+10);
          }else if(view.getId() == R.id.button2){
              progressBar.setProgress(progressBar.getProgress()-10);
  
          }
      }
      public void dialog(View view){
          if(view.getId() == R.id.button3){
              progressDialog = new ProgressDialog(MainActivity. this);
              progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
              progressDialog.setMessage("Progres ...");
  
              progressDialog.setButton(progressDialog.BUTTON_NEGATIVE, "Cancel", new DialogInterface.OnClickListener() {
                  @Override
                  public void onClick(DialogInterface dialogInterface, int i) {
                      progressDialog.dismiss();
                  }
              });
  
              progressDialog.setCancelable(false);
              progressDialog.show();
          } else if(view.getId() == R.id.button4){
              progressDialog.dismiss();
  
          }
      }
  }
  
  ```

  ![a09](https://user-images.githubusercontent.com/50862497/64108128-09e19a00-cdb7-11e9-8b33-da95f4d0bd96.png)



* (P234)

  ```java
  package com.example.p234;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.app.AlertDialog;
  import android.app.ProgressDialog;
  import android.content.DialogInterface;
  import android.os.Bundle;
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.EditText;
  import android.widget.ImageView;
  import android.widget.ProgressBar;
  import android.widget.RadioButton;
  
  public class MainActivity extends AppCompatActivity {
  
      ProgressDialog progressDialog;
      ImageView imgView;
      ProgressBar progressBar;
      RadioButton rb1, rb2, rb3;
      EditText editText;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          imgView = findViewById(R.id.imgView);
          progressBar = findViewById(R.id.progressBar);
          progressBar.setIndeterminate(false);
      }
  
      public void btn(View view){
  
          if(view.getId() == R.id.button0){
              AlertDialog.Builder builder = new AlertDialog.Builder(this);
  
              LayoutInflater inflater = getLayoutInflater();
              final View oview= inflater.inflate(R.layout.option_layout, (ViewGroup) findViewById(R.id.olayout));
  
              builder.setView(oview);
              builder.setPositiveButton("Click", new DialogInterface.OnClickListener() {
                  @Override
                  public void onClick(DialogInterface dialogInterface, int i) {
                      rb1 = oview.findViewById(R.id.rb1);
                      rb2 = oview.findViewById(R.id.rb2);
                      rb3 = oview.findViewById(R.id.rb3);
  
                      if(rb1.isChecked() == true){
                          imgView.setImageResource(R.drawable.img1);
                      }else if(rb2.isChecked() == true){
                          imgView.setImageResource(R.drawable.img2);
                      }else if (rb3.isChecked() == true){
                          imgView.setImageResource(R.drawable.img3);
                      }else{
                          imgView.setImageResource(R.drawable.ic_launcher_foreground);
                      }
  
                      editText = oview.findViewById(R.id.editText);
                      String num = editText.getText().toString();
  
                      if(num.equals("")){
                          progressBar.setProgress(0);
                      }else{
                          progressBar.setProgress(Integer.parseInt(num));
                      }
                  }
              });
              builder.setCancelable(false);
              builder.show();
          }
      }
  
  }
  
  ```

  ![a12](https://user-images.githubusercontent.com/50862497/64108890-11a23e00-cdb9-11e9-9d6e-c19fe83d808a.png)

  ![a13](https://user-images.githubusercontent.com/50862497/64108897-1666f200-cdb9-11e9-9c53-d02695ea2a23.png)



.

