# Android

09.03



* (P458)

  ```java
  package com.example.p458;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.view.View;
  import android.webkit.WebSettings;
  import android.webkit.WebView;
  import android.webkit.WebViewClient;
  import android.widget.TextView;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      WebView webView;
      TextView textView;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          webView = findViewById(R.id.webView);
          webView.setWebViewClient(new WebViewClient());
  
          // js이름으로 등록
          webView.addJavascriptInterface(new JS(),"js");
  
          WebSettings webSettings = webView.getSettings();
          webSettings.setJavaScriptEnabled(true); // WebView에 자바스크립트를 실행하기위한 셋팅
  
          textView = findViewById(R.id.textView);
          webView.loadUrl("http://m.naver.com");
      }
      class JS{
          JS(){}
          @android.webkit.JavascriptInterface
          public void webclick(String str){
              textView.setText(str);
              Toast.makeText(MainActivity.this, ""+str, Toast.LENGTH_SHORT).show();
          }
      }
      public void click(View view){
          if(view.getId() == R.id.button){
              webView.loadUrl("http://www.daum.net");
          }else if(view.getId() == R.id.button2){
              webView.loadUrl("http://70.12.60.108/webview");
          }else if(view.getId() == R.id.button3){
              webView.loadUrl("javascript:s('cmd')");
          }
      }
  }
  
  ```

  ```html
  index.html
  
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="EUC-KR">
  <title>Insert title here</title>
  <script>
  function s(data){
      document.getElementById('id02').innerHTML = 'Web view Event'+data;
  }
  </script>
  </head>
  <body>
  <h1>Web View Test</h1>
  <h2 id="id02">Sample Data</h2> 
  <button onclick="window.js.webclick('web')">Click</button>
  </body>
  </html>
  ```

  ![c5](https://user-images.githubusercontent.com/50862497/64327405-95049f00-d006-11e9-816b-67507013e3fb.png)

  

  

* (P465)

  ```java
  package com.example.p465;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.view.WindowManager;
  import android.widget.SeekBar;
  import android.widget.TextView;
  import android.widget.Toast;
  
  import org.w3c.dom.Text;
  
  public class MainActivity extends AppCompatActivity {
      SeekBar seekBar;
      TextView textView;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          seekBar = findViewById(R.id.seekBar);
          textView = findViewById(R.id.textView);
  
          // SeekBar 움직임에 따라 이벤트 처리
          seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
              @Override
              public void onProgressChanged(SeekBar seekBar, int i, boolean b) {
                  textView.setText("SeekBar Value:"+i);
                  setBright(i);
              }
  
              @Override
              public void onStartTrackingTouch(SeekBar seekBar) {
                  Toast.makeText(MainActivity.this, "start", Toast.LENGTH_SHORT).show();
              }
  
              @Override
              public void onStopTrackingTouch(SeekBar seekBar) {
                  Toast.makeText(MainActivity.this, "stop", Toast.LENGTH_SHORT).show();
              }
          });
      }
      public void setBright(int value){
          if(value < 10){
              value = 10;
          }
          WindowManager.LayoutParams params = getWindow().getAttributes();
          params.screenBrightness = (float)value/10;
          getWindow().setAttributes(params);
      }
  
  }
  
  ```

  ![c6](https://user-images.githubusercontent.com/50862497/64327702-1c521280-d007-11e9-88a5-116dea14e834.png)



* (P474)

  ```java
  package com.example.p474;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.util.Log;
  import android.view.View;
  import android.widget.Button;
  import android.widget.TextView;
  
  public class MainActivity extends AppCompatActivity {
      TextView textView,textView2;
      Button button, button2;
      boolean flag1=true, flag2=true;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
          textView2 = findViewById(R.id.textView2);
          button = findViewById(R.id.button);
          button2 = findViewById(R.id.button2);
      }
  
      //Thread t1 = new Thread(new Runnable() {
       Runnable r1 = new Runnable(){
          @Override
          public void run() {
              for(int i=1; i<=10; i++){
                  try {
                      Thread.sleep(500);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  Log.d("[T]","-----"+i);
  
                  // 서브 쓰레드가 메인 쓰레드를 제어할 수 없다
                  // textView.setText(i+"");
                  final int temp = i;
                  runOnUiThread(new Runnable() {
                      @Override
                      public void run() {
                          textView.setText(temp+"");
                      }
                  });
              }
              runOnUiThread(new Runnable() {
                  @Override
                  public void run() {
                      button.setEnabled(true);
                  }
              });
  
              // button.setEnabled(true);
          }
      };
      //Thread t2 = new Thread(new Runnable() {
      Runnable r2 = new Runnable(){
          @Override
          public void run() {
              for(int i=11; i<=20; i++){
                  try {
                      Thread.sleep(500);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  Log.d("[T]","*****"+i);
                  final int temp = i;
                  runOnUiThread(new Runnable() {
                      @Override
                      public void run() { // Thread 안에 Thread를 만들어서 main과 연동하도록
                          textView2.setText(temp+"");
                      }
                  });
              }
              runOnUiThread(new Runnable() {
                  @Override
                  public void run() {
                      button2.setEnabled(true);
                  }
              });
          }
      };
  
      public void clickB1(View view){
  
          Thread t1 = new Thread(r1);
          t1.start(); // t1의 run 함수 동작
          button.setEnabled(false);
          // 멀티쓰레드 안하고 여기서하면 다른기능 동시에 동작 불가
  //        for(int i=1; i<=100; i++){
  //            try {
  //                Thread.sleep(500);
  //            } catch (InterruptedException e) {
  //                e.printStackTrace();
  //            }
  //            Log.d("[T]","@@@@@"+i);
  //        }
      }
      public void clickB2(View view){
          Thread t2 = new Thread(r2);
          t2.start();
          button2.setEnabled(false);
      }
  }
  
  ```

  ![c7](https://user-images.githubusercontent.com/50862497/64328124-dd708c80-d007-11e9-8d12-65208706e6e5.png)



* (P478) Thread와 Handler 연동하기

  ```java
  package com.example.p478;
  
  import androidx.annotation.NonNull;
  import androidx.appcompat.app.AlertDialog;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.app.ProgressDialog;
  import android.content.DialogInterface;
  import android.os.Bundle;
  import android.os.Handler;
  import android.os.Message;
  import android.util.Log;
  import android.view.View;
  import android.widget.Button;
  import android.widget.ProgressBar;
  import android.widget.TextView;
  
  public class MainActivity extends AppCompatActivity {
      // (Handler에서는 MainThread를 제어할 수 있다)
      TextView textView;
      CountHandler countHandler; // 구현한거
      Button button;
  
      Handler handler; // 원래 있는거
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
          countHandler = new CountHandler();
          button = findViewById(R.id.button);
          handler = new Handler();
      }
  
      Runnable r = new Runnable(){
          @Override
          public void run() {
              for(int i=1; i<=10; i++){
                  try {
                      Thread.sleep(500);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  Log.d("[T]","-----"+i);
                  Message message = countHandler.obtainMessage(); // 핸들러에서 메시지 꺼내기
                  Bundle bundle = new Bundle();
                  bundle.putInt("cnt",i); // 번들에 값 넣기
                  message.setData(bundle); // 메시지에 다시 번들 넣기
                  countHandler.sendMessage(message); // countHandler에게 메시지 보내기
                  // 번들에 넣었다가 보내기
              }
  
              handler.post(new Runnable() { // handler의 ref는 mainThread라서 mainThread를 제어할 수 있다 / 유지보수에는 안좋음음
                 @Override
                  public void run() {
                      button.setEnabled(true);
                  }
              });
  
  //            // button.setEnabled(true);
  //            Message message = countHandler.obtainMessage();
  //            Bundle bundle = new Bundle();
  //            // bundle.putBoolean("flag", true);
  //            bundle.putInt("cmd",1);
  //            message.setData(bundle);
  //            countHandler.sendMessage(message);
          }
      };
  
      class CountHandler extends Handler{
          @Override
          public void handleMessage(@NonNull Message msg) { // 메시지가 msg로 들어옴
              Bundle bundle = msg.getData();
              int value = bundle.getInt("cnt");
              textView.setText(value+"");
  
  //            int cmd = bundle.getInt("cmd");
  //            if(cmd == 1){
  //                button.setEnabled(true);
  //            }
  
          }
      }
  
      public void clickBt(View view){
          Thread t = new Thread(r);
          t.start(); // start는 만든함수가 아니어서 값을 넣을 수 없음
          button.setEnabled(false);
      }
  
      public void clickBt2(View view){
          final ProgressDialog progressDialog = new ProgressDialog(this);
  
          AlertDialog.Builder dialog = new AlertDialog.Builder(this);
          dialog.setTitle("dialog");
          dialog.setMessage("5 seconds ...");
          dialog.setPositiveButton("NEXT", new DialogInterface.OnClickListener() {
              @Override
              public void onClick(DialogInterface dialogInterface, int i) {
                  progressDialog.show();
  
                  handler.postDelayed(new Runnable() {
                      @Override
                      public void run() {
                          progressDialog.dismiss();
                          textView.setText("NEXT OK"); // 5초 후 실행
                      }
                  },5000);
              }
          });
          dialog.show();
      }
  }
  
  ```

  ![c8](https://user-images.githubusercontent.com/50862497/64328583-ab135f00-d008-11e9-90a8-15880513c420.png)



* (P485)

  ```java
  package com.example.p485;
  
  import androidx.annotation.NonNull;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.os.Handler;
  import android.os.Looper;
  import android.os.Message;
  import android.util.Log;
  import android.view.View;
  import android.widget.TextView;
  // 스레드로 메시지 전송하기
  public class MainActivity extends AppCompatActivity {
      TextView textView;
      MyThread myThread;
      MyHandler myHandler; // mainAcitiviry꺼
  
      // Main Thread
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
          myThread = new MyThread(); // 버튼을 클릭하면 Thread가 하나 만들어짐, Main이 돌면 Sub 만들어두기
          myHandler = new MyHandler();
      }
  
      public void clickBt(View view){
  //        Message message = Message.obtain();
  //        message.obj = 10;
  //        myThread.threadHandler.sendMessage(message); //MainThread에서 Sub에게 send
          myThread.setCnt(20);
          myThread.start();
      }
  
      class MyHandler extends Handler{
          // 여기다 값을 주고
  
          @Override
          public void handleMessage(@NonNull Message msg) {
              Bundle bundle = msg.getData();
              int cnt = bundle.getInt("cnt");
              textView.setText(cnt+"");
          }
      }
  
      // Sub Thread
      class MyThread extends Thread{
  //        ThreadHandler threadHandler;
  
          int cnt;
          public MyThread(){
  //            threadHandler = new ThreadHandler();
          }
          public void setCnt(int cnt){
              this.cnt = cnt;
          }
  
  
          @Override
          public void run() {
  //            super.run();
              for (int i = 0; i < cnt; i++) {
                  try {
                      Thread.sleep(300);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  Log.d("[T2]", "------------------" + i);
                  final int temp = i;
                  Message message = myHandler.obtainMessage();
                  Bundle bundle = new Bundle();
                  bundle.putInt("cnt",temp);
                  message.setData(bundle);
                  myHandler.sendMessage(message);
  //                Looper.prepare();
  //                Looper.loop();
              }
          }
      }
  }
  
  ```

  ![c9](https://user-images.githubusercontent.com/50862497/64328806-178e5e00-d009-11e9-9500-f9a03131afe1.png)



* (P488)

  ![그림1](https://user-images.githubusercontent.com/50862497/64329423-34776100-d00a-11e9-91b0-a736958a28a7.png)

  ```java
  package com.example.p488;
  
  import androidx.annotation.NonNull;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.graphics.Color;
  import android.os.Bundle;
  import android.os.Handler;
  import android.os.Message;
  import android.util.Log;
  import android.view.View;
  import android.widget.Button;
  import android.widget.TextView;
  
  import java.util.Random;
  import java.util.Scanner;
  
  public class MainActivity extends AppCompatActivity {
  
      Button button;
      TextView textView2, textView4, textView6;
  
      Random random = new Random();
  
      SpeedThread speedThread;
      RPMThread rpmThread;
      AvgThread avgThread;
      SpeedHandler speedHandler;
      RPMHandler rpmHandler;
      AvgHandler avgHandler;
  
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          button = findViewById(R.id.button);
          textView2 = findViewById(R.id.textView2);
          textView4 = findViewById(R.id.textView4);
          textView6 = findViewById(R.id.textView6);
  
          speedThread = new SpeedThread();
          rpmThread = new RPMThread();
          avgThread = new AvgThread();
          speedHandler = new SpeedHandler();
          rpmHandler = new RPMHandler();
          avgHandler = new AvgHandler();
  
      }
  
      public void clickBt(View view){
          button.setBackgroundColor(Color.rgb(255,157,157));
          button.setText("STOP");
          button.setEnabled(false);
  
  //        Thread t = new Thread(r);
  //        t.start();
          speedThread.start();
          rpmThread.start();
          avgThread.start();
      }
  
      class SpeedHandler extends Handler {
          @Override
          public void handleMessage(@NonNull Message msg) {
              Bundle bundle = msg.getData();
              int speed = bundle.getInt("speed");
              textView2.setText(speed+"");
          }
      }
      class RPMHandler extends Handler {
          @Override
          public void handleMessage(@NonNull Message msg) {
              Bundle bundle = msg.getData();
              int rpm = bundle.getInt("rpm");
              textView4.setText(rpm+"");
          }
      }
      class AvgHandler extends Handler{
          @Override
          public void handleMessage(@NonNull Message msg) {
              Bundle bundle = msg.getData();
              int avg = bundle.getInt("avg");
              textView6.setText(avg+"");
          }
      }
  
      class SpeedThread extends Thread{
          public SpeedThread(){
          }
          @Override
          public void run() {
              for (int i = 0; i < 10; i++) {
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  final int speed = random.nextInt(80)+20;
                  avgThread.setSpeed(speed); // AVGThread로 보내기
                  // SpeedHandler로 보내기
                  Message message = speedHandler.obtainMessage();
                  Bundle bundle = new Bundle();
                  bundle.putInt("speed",speed);
                  message.setData(bundle);
                  speedHandler.sendMessage(message);
              }
          }
      }
  
      class RPMThread extends Thread{
          public RPMThread(){
          }
          @Override
          public void run() {
              for (int i = 0; i < 10; i++) {
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  final int rpm = random.nextInt(1000)+5000;
                  avgThread.setRpm(rpm);
                  Message message = rpmHandler.obtainMessage();
                  Bundle bundle = new Bundle();
                  bundle.putInt("rpm",rpm);
                  message.setData(bundle);
                  rpmHandler.sendMessage(message);
              }
          }
      }
  
      class AvgThread extends Thread{
          public AvgThread(){
          }
          int speed, rpm;
          public void setSpeed(int speed){
              this.speed = speed;
          }
          public void setRpm(int rpm){
              this.rpm = rpm;
          }
  
          @Override
          public void run() {
              for (int i = 0; i < 10; i++) {
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  final int avg = rpm/speed;
                  Message message = avgHandler.obtainMessage();
                  Bundle bundle = new Bundle();
                  bundle.putInt("avg",avg);
                  message.setData(bundle);
                  avgHandler.sendMessage(message);
              }
          }
      }
  
  
      /*Runnable r = new Runnable(){
          @Override
          public void run() {
              for(int i=1; i<=10; i++){
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  Log.d("[T]","-----"+i);
  
                  final int speed = random.nextInt(80)+20; // 20~100
                  final int rpm = random.nextInt(1000)+6000; // 6000~7000
                  final int avg = rpm / speed;
  
                  runOnUiThread(new Runnable() {
                      @Override
                      public void run() {
                          textView2.setText(speed+"");
                          textView4.setText(rpm+"");
                          textView6.setText(avg+"");
                      }
                  });
              }
  
              runOnUiThread(new Runnable() {
                  @Override
                  public void run() {
                      button.setEnabled(true);
                  }
              });
          }
      };*/
  }
  
  ```

  ![c10](https://user-images.githubusercontent.com/50862497/64329116-aa2efd00-d009-11e9-9ce2-d79a501a1306.png)



