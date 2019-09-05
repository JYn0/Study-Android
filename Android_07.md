# Android

09.04

## AsyncTask

핸들러를 사용하지않고 좀 더 간단하게 작업하는 방법, 핸들러+스레드

핸들러 - 메인스레드, 서브스레드에서 보내는걸 받는 곳

asyncTask

url 던져주면 is에서 접속해서 스트림 만들고, convet에 주면 알아서 result로 리턴해줌



* (P490)

  ```java
  package com.example.p490;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.app.ProgressDialog;
  import android.os.AsyncTask;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.Button;
  import android.widget.ProgressBar;
  import android.widget.TextView;
  
  public class MainActivity extends AppCompatActivity {
      Button button, button2, button3;
      TextView textView;
      ProgressBar progressBar;
      ProgressDialog progressDialog;
  
      MyTask myTask;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          button = findViewById(R.id.button);
          button2 = findViewById(R.id.button2);
          button3 = findViewById(R.id.button3);
          textView = findViewById(R.id.textView);
          progressBar = findViewById(R.id.progressBar);
          progressDialog = new ProgressDialog(this);
      }
  
      public void clickBt(View view){
          myTask = new MyTask(30);
          myTask.execute(); // 실행(Thread+Handler 같이 동작)
      }
      public void clickBt2(View view){
          myTask.cancel(true);
          //button.setEnabled(true);
      }
      public void clickBt3(View view){
          myTask = new MyTask(10);
          myTask.execute();
      }
  
      // 실행시킬때 넣어주는 args, 스레드가 동작되는 중 발생되는 상태, 스레드가 종료가되고 리턴되는 타입
      class MyTask extends AsyncTask<Integer, Integer, String>{
  
          int cnt;
  
          public MyTask(int cnt){ // 실행시킬때 넣어주는 args 받는부분
              this.cnt = cnt;
          }
  
          @Override
          protected void onPreExecute() {
              progressBar.setMax(cnt);
              button.setEnabled(false);
              textView.setText("Start Task");
              progressDialog.setTitle("Progress");
              progressDialog.show();
              //progressDialog.setCancelable(false);
  
  //            super.onPreExecute();
          }
  
          @Override
          protected String doInBackground(Integer... integers) { // 스레드 시작, 스레드가 동작되는 부분, run, String 리턴
              String result = "";
              int sum = 0;
              for(int i=1; i<=cnt; i++){
                  if(isCancelled() == true){
                      break;
                  }
                  try {
                      Thread.sleep(300);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  sum += i;
                  publishProgress(i); // onProgressUpdate로 값 보내기
              }
              result = sum + "";
              return result;
          }
  
          @Override
          protected void onProgressUpdate(Integer... values) { // 스레드 중, 발생되고있는 내용을 받아서 바로 처리해줌
              progressBar.setProgress(values[0].intValue()); // values는 배열
              textView.setText(values[0].intValue()+"");
  //            super.onProgressUpdate(values);
          }
  
          @Override
          protected void onPostExecute(String s) { // 스레드 후, doInBackground에서 리턴 된 String을 받는 곳
              button.setEnabled(true);
              textView.setText("End Task: "+s);
              progressDialog.dismiss();
  //            super.onPostExecute(s);
          }
      }
  
  }
  
  ```

  ![c11](https://user-images.githubusercontent.com/50862497/64330141-663cf780-d00b-11e9-83ce-c637425c8ef4.png)



* (P499)

  ![그림2](https://user-images.githubusercontent.com/50862497/64330510-114db100-d00c-11e9-90fa-86123fd43ae5.png)

  ```java
  package com.example.p499;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.graphics.Color;
  import android.os.AsyncTask;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.Button;
  import android.widget.TextView;
  
  import java.util.Random;
  
  public class MainActivity extends AppCompatActivity {
  
      Button button;
      TextView textView, textView2, textView4, textView6;
      speedTask speedTask;
      rpmTask rpmTask;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          button = findViewById(R.id.button);
          textView = findViewById(R.id.textView);
          textView2 = findViewById(R.id.textView2);
          textView4 = findViewById(R.id.textView4);
          textView6 = findViewById(R.id.textView6);
      }
      public void clickBt(View view){
          speedTask = new speedTask(30);
          speedTask.execute();
          rpmTask = new rpmTask();
          rpmTask.executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR);
      }
  
      class speedTask extends AsyncTask<Integer,Integer, Integer>{
          int cnt;
          public speedTask(int cnt){
              this.cnt = cnt;
  
          }
  
          @Override
          protected void onPreExecute() {
  //            button.setText("Start");
              button.setEnabled(false);
              button.setText("Stop");
              button.setBackgroundColor(Color.rgb(255,157,157));
          }
  
          @Override
          protected Integer doInBackground(Integer... integers) {
              Random r = new Random();
              int speed = 1;
              for(int i=1; i<=cnt; i++){
                  speed = r.nextInt(140)+11; // 10~150
                  publishProgress(speed);
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
              }
              return speed;
          }
  
          @Override
          protected void onProgressUpdate(Integer... values) {
              textView2.setText(values[0].intValue()+"");
  
              if(values[0].intValue() <= 30){
                  textView6.setText("저속");
              }else if((values[0].intValue() > 30)&&(values[0].intValue() < 100)){
                  textView6.setText("정속");
              }else if(values[0].intValue() >= 100){
                  textView6.setText("과속");
              }else{
                  textView6.setText("--");
              }
          }
  
          @Override
          protected void onPostExecute(Integer integer) {
              button.setEnabled(true);
              button.setText("Start");
              button.setBackgroundColor(Color.rgb(162,176,255));
              rpmTask.cancel(true);
          }
      }
  
      class rpmTask extends AsyncTask<Integer, Integer, Integer>{
  
          public rpmTask(){
  //            this.cnt = cnt;
          }
          @Override
          protected Integer doInBackground(Integer... integers) {
              Random r = new Random();
              int rpm = 6000;
              //for(int i=1; i<=cnt; i++){
              while(isCancelled() != true){
                  rpm = r.nextInt(1000)+6000; // 6000~7000
                  publishProgress(rpm);
                  try {
                      Thread.sleep(3000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
              }
              return rpm;
          }
  
          @Override
          protected void onProgressUpdate(Integer... values) {
              textView4.setText(values[0].intValue()+"");
          }
      }
  }
  
  ```

  ![c12](https://user-images.githubusercontent.com/50862497/64330717-65589580-d00c-11e9-9dfc-78117ff7a37e.png)



* (P511)

  ```java
  package com.example.p511;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.app.ProgressDialog;
  import android.os.AsyncTask;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.TextView;
  
  public class MainActivity extends AppCompatActivity {
      TextView textView;
      ProgressDialog progressDialog;
      HttpTask httpTask;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
          progressDialog = new ProgressDialog(this);
      }
      public void clickBt(View view){
          httpTask = new HttpTask("http://www.naver.com" );
          httpTask.execute();
      }
  
      class HttpTask extends AsyncTask<String,Void, String>{
          // 네트워크를 통해 진행을하려면 무조건 Thread를 사용해야함
          String url;
  
          public HttpTask(String url){
              this.url = url;
          }
  
          @Override
          protected void onPreExecute() {
              progressDialog.setTitle("Http Connecting ...");
              progressDialog.setMessage("Please Wait ...");
              progressDialog.setCancelable(false);
              progressDialog.show();
          }
  
          @Override
          protected String doInBackground(String... strings) { // thread
              String str = HttpHandler.getString(url);
              return str;
          }
  
          @Override
          protected void onPostExecute(String str) {
              progressDialog.dismiss();
              textView.setText(str);
  
          }
      }
  }
  
  ```

  ```java
  // HttpHandler.java
  
  package com.example.p511;
  
  import java.io.BufferedInputStream;
  import java.io.BufferedReader;
  import java.io.IOException;
  import java.io.InputStream;
  import java.io.InputStreamReader;
  import java.net.HttpURLConnection;
  import java.net.MalformedURLException;
  import java.net.URL;
  
  public class HttpHandler {
      public static String getString(String urlstr){
          String result = null;
          URL url = null;
          HttpURLConnection hcon = null;
          InputStream is = null; // 통로
  
          try {
              url = new URL(urlstr);
              hcon = (HttpURLConnection)url.openConnection();
              hcon.setConnectTimeout(10000); // 10초동안 응답없으면 exception 처리
              hcon.setRequestMethod("GET"); // get방식
              is = new BufferedInputStream(hcon.getInputStream()); // 요청하면 inputStream이 만들어짐
              result = convertStr(is);
          } catch (Exception e) {
              e.printStackTrace();
          } finally {
              hcon.disconnect();
          }
          return result;
      }
  
      public static String convertStr(InputStream is){
          BufferedReader br = null;
          br = new BufferedReader(new InputStreamReader(is));
          StringBuilder sb = new StringBuilder();
          String temp;
          try{
              while((temp = br.readLine()) != null){
                  sb.append(temp);
              }
          }catch(Exception e){
              e.printStackTrace();
          }finally {
              try {
                  is.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
  
          return sb.toString();
      }
  }
  
  ```

  ![c13](https://user-images.githubusercontent.com/50862497/64330936-df891a00-d00c-11e9-9bee-188ec42ba981.png)





* (P427)

  ```java
  // MainActivity.java
  package com.example.p427;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.app.ProgressDialog;
  import android.content.Context;
  import android.content.Intent;
  import android.content.pm.PackageManager;
  import android.graphics.Bitmap;
  import android.graphics.BitmapFactory;
  import android.net.Uri;
  import android.os.AsyncTask;
  import android.os.Bundle;
  import android.util.Log;
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.Adapter;
  import android.widget.AdapterView;
  import android.widget.BaseAdapter;
  import android.widget.ImageView;
  import android.widget.LinearLayout;
  import android.widget.ListView;
  import android.widget.TextView;
  import android.widget.Toast;
  
  import org.json.JSONArray;
  import org.json.JSONException;
  import org.json.JSONObject;
  import org.w3c.dom.Text;
  
  import java.io.InputStream;
  import java.net.MalformedURLException;
  import java.net.URL;
  import java.util.ArrayList;
  
  public class MainActivity extends AppCompatActivity
  implements AdapterView.OnItemClickListener {
  
      ArrayList<Item> list;
      ListView listView;
      LinearLayout container;
      ItemAdapter itemAdapter;
      ProgressDialog progressDialog;
      HttpTask httpTask;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          listView = findViewById(R.id.listView);
          container = findViewById(R.id.container);
          listView.setOnItemClickListener(this);
          progressDialog = new ProgressDialog(this);
          list = new ArrayList<>();
  
          String[] Permissions = {
                  Manifest.permission.CALL_PHONE,
          };
          ActivityCompat.requestPermissions(this, Permissions, 101);
      }
  
      @Override
      public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
          Item item = list.get(list.size()-i-1);
          Toast.makeText(this,item.getPhone(), Toast.LENGTH_SHORT).show();
  
          int permission = PermissionChecker.checkSelfPermission(this, Manifest.permission.CALL_PHONE);
          Intent intent = null;
          if(permission == PackageManager.PERMISSION_GRANTED){
              intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:010-2990-7425"));
              startActivity(intent);
          }else{
              Toast.makeText(this, "권한부여가 안되었습니다.", Toast.LENGTH_SHORT).show();
              return;
          }
      }
  
  
      class HttpTask extends AsyncTask<String,Void, String> {
          // 네트워크를 통해 진행을하려면 무조건 Thread를 사용해야함
          String url;
  
          public HttpTask(String url){
              this.url = url;
          }
  
          @Override
          protected void onPreExecute() {
              progressDialog.setTitle("Http Connecting ...");
              progressDialog.setMessage("Please Wait ...");
              progressDialog.setCancelable(false);
              progressDialog.show();
          }
  
          @Override
          protected String doInBackground(String... strings) { // thread
              String str = HttpHandler.getString(url);
              return str;
          }
  
          @Override
          protected void onPostExecute(String str) {
              progressDialog.dismiss();
              Log.d("[JSON]",str);
  //              textView.setText(strings);
  
              JSONArray ja = null;
              try {
                  ja = new JSONArray(str); // JSONArray 객체
                  for(int i=0; i<ja.length();i++){
                      JSONObject jo = ja.getJSONObject(i);
                      String name = jo.getString("name");
                      String phone = jo.getString("phone");
                      String img = jo.getString("img");
                      list.add(new Item(name,phone,img));
                      Log.d("[JO]",""+name+" "+phone+" "+img);
                  }
              } catch (JSONException e) {
                  e.printStackTrace();
              }
  
              itemAdapter = new ItemAdapter(list); // list만큼 getView 호출
              listView.setAdapter(itemAdapter);
          }
      }
  
  
      class ItemAdapter extends BaseAdapter{
          // listView에 붙이기 위함
  
          ArrayList<Item> alist;
  
          public ItemAdapter(ArrayList<Item> alist){
              this.alist = alist;
          }
  
          public void addItem(Item item){
              alist.add(item);
              list = alist;
          }
  
  
  
          @Override
          public int getCount() {
              return alist.size();
          }
  
          @Override
          public Object getItem(int i) {
              return alist.get(i); // 해당하는 객체
          }
  
          @Override
          public long getItemId(int i) {
              return i;
          }
  
          @Override
          public View getView(int i, View view, ViewGroup viewGroup) {
              // data수만큼 호출되면 listView에 들어갈 내용 만들어져서 listView에 들어감
  
              View myview = null;
              LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
              // 화면 만들기
              myview = inflater.inflate(R.layout.layout, container, true);
              // layout에서 container에 myview를 만들어서 리턴해주겠다
  
              // data 넣기
              final ImageView iv1 = myview.findViewById(R.id.imageView);
              TextView iv2= myview.findViewById(R.id.textView);
              TextView iv3 = myview.findViewById(R.id.textView2);
  
              String img = alist.get(i).getImgId();
              img = "http://70.12.60.108/webview/"+img;
              final String temp = img;
  
              Thread t = new Thread(new Runnable() {
                  @Override
                  public void run() {
                      URL url = null;
                      InputStream is = null;
                      try {
                          url = new URL(temp);
                          is = url.openStream();
                          final Bitmap bm = BitmapFactory.decodeStream(is); // 서버에서 이미지를 가져와서
                          runOnUiThread(new Runnable() {
                              @Override
                              public void run() {
                                  iv1.setImageBitmap(bm);
                              }
                          });
                      } catch (Exception e) {
                          e.printStackTrace();
                      }
                  }
              });
              t.start();
  
  //            iv1.setImageResource(alist.get(alist.size()-i-1).getImgId());
              iv2.setText(alist.get(i).getName());
              iv3.setText(alist.get(i).getPhone());
  //            iv3.setText(alist.get(i).getPhone());
              return myview;
          }
  
      }
  
      public void clickBt(View view){
          getData();
      }
  
      public void getData(){
          // 서버에 요청해서 데이터 가져오기기
          String url = "http://70.12.60.108/webview/item.jsp";
          httpTask = new HttpTask(url);
          httpTask.execute();
      }
  }
  
  ```

  ```java
  // Item.java
  package com.example.p427;
  
  public class Item {
      String name;
      String phone;
      String imgId;
  
  
  
      public Item(String name, String phone, String imgId) {
          this.name = name;
          this.phone = phone;
          this.imgId = imgId;
      }
  
      public Item() {
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public String getPhone() {
          return phone;
      }
  
      public void setPhone(String phone) {
          this.phone = phone;
      }
  
      public String getImgId() {
          return imgId;
      }
  
      public void setImgId(String imgId) {
          this.imgId = imgId;
      }
  }
  
  ```

  ```java
  // HttpHandler.java
  package com.example.p427;
  
  import java.io.BufferedInputStream;
  import java.io.BufferedReader;
  import java.io.IOException;
  import java.io.InputStream;
  import java.io.InputStreamReader;
  import java.net.HttpURLConnection;
  import java.net.URL;
  
  public class HttpHandler {
      public static String getString(String urlstr){
          String result = null;
          URL url = null;
          HttpURLConnection hcon = null;
          InputStream is = null; // 통로
  
          try {
              url = new URL(urlstr);
              hcon = (HttpURLConnection)url.openConnection();
              hcon.setConnectTimeout(10000); // 10초동안 응답없으면 exception 처리
              hcon.setRequestMethod("GET"); // get방식
              is = new BufferedInputStream(hcon.getInputStream()); // 요청하면 inputStream이 만들어짐
              result = convertStr(is);
          } catch (Exception e) {
              e.printStackTrace();
          } finally {
              hcon.disconnect();
          }
          return result;
      }
  
      public static String convertStr(InputStream is){
          BufferedReader br = null;
          br = new BufferedReader(new InputStreamReader(is));
          StringBuilder sb = new StringBuilder();
          String temp;
          try{
              while((temp = br.readLine()) != null){
                  sb.append(temp);
              }
          }catch(Exception e){
              e.printStackTrace();
          }finally {
              try {
                  is.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
  
          return sb.toString();
      }
  }
  
  ```

  ```jsp
  <!-- webview/web/item.jsp -->
  <%@ page language="java" contentType="text/json; charset=UTF-8"
      pageEncoding="UTF-8"%>
  [
  {"name":"윤아","phone":"01011111234","img":"it01.jpg"},
  {"name":"Irn","phone":"01022221234","img":"it02.jpg"},
  {"name":"조이","phone":"01033331234","img":"it03.jpeg"},
  {"name":"세아","phone":"01044441234","img":"it04.jpg"},
  {"name":"규현","phone":"01055555678","img":"it05.jpg"},
  {"name":"수근","phone":"01066665678","img":"it06.jpg"},
  {"name":"이린","phone":"01077775678","img":"it07.jpg"},
  {"name":"개개","phone":"01088885678","img":"it08.jpg"},
  {"name":"고양","phone":"01099990910","img":"it09.jpg"},
  {"name":"희철","phone":"01010100910","img":"it10.jpg"}
  ]
  
  ```

  ![c15](https://user-images.githubusercontent.com/50862497/64331720-48bd5d00-d00e-11e9-82b2-fd88315a17f0.png)



사진 : http://70.12.60.108/webview/it01.jpg

http://70.12.60.108/webview/item.jsp



* (P535)

  ```java
  // MainActivity.java
  package com.example.p535;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Context;
  import android.graphics.Bitmap;
  import android.graphics.BitmapFactory;
  import android.os.AsyncTask;
  import android.os.Bundle;
  import android.renderscript.ScriptGroup;
  import android.text.Layout;
  import android.util.Log;
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.Adapter;
  import android.widget.AdapterView;
  import android.widget.BaseAdapter;
  import android.widget.ImageView;
  import android.widget.LinearLayout;
  import android.widget.ListView;
  import android.widget.RatingBar;
  import android.widget.TextView;
  
  import org.json.JSONArray;
  import org.json.JSONException;
  import org.json.JSONObject;
  
  import java.io.InputStream;
  import java.net.HttpURLConnection;
  import java.net.MalformedURLException;
  import java.net.URL;
  import java.util.ArrayList;
  
  public class MainActivity extends AppCompatActivity
  implements AdapterView.OnItemClickListener {
  
      ListView listView;
      LinearLayout container;
  
      ItemAdapter itemAdapter;
      ArrayList<Item> movieList;
  
      HttpTask httpTask;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          listView = findViewById(R.id.listView);
          container = findViewById(R.id.container);
          movieList = new ArrayList<>();
      }
  
      @Override
      public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
          Item item = movieList.get(movieList.size()-i-1);
      }
  
  
  
      class ItemAdapter extends BaseAdapter {
          ArrayList<Item> adapterList;
          public ItemAdapter(ArrayList<Item> adapterList){
              this.adapterList = adapterList;
          }
          public void addItem(Item item){
              adapterList.add(item);
              movieList = adapterList;
          }
  
  
          @Override
          public int getCount() {
              return adapterList.size();
          }
  
          @Override
          public Object getItem(int i) {
              return adapterList.get(i);
          }
  
          @Override
          public long getItemId(int i) {
              return i;
          }
  
          @Override
          public View getView(int i, View view, ViewGroup viewGroup) {
              View movieView = null;
  
              LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
              movieView = inflater.inflate(R.layout.layout, container, true);
  
              final ImageView movieImg = movieView.findViewById(R.id.imageView);
              TextView textTitle = movieView.findViewById(R.id.textView);
              RatingBar ratingBar = movieView.findViewById(R.id.ratingBar);
              ratingBar.setNumStars(5);
              ratingBar.setStepSize(1);
              ratingBar.setMax(5);
              ratingBar.setRating(0);
              ratingBar.setIsIndicator(true);
              TextView textActor = movieView.findViewById(R.id.textView2);
  
              String img = movieList.get(i).getImg();
              img = "http://70.12.60.108/webview/"+img;
  
              final String temp = img;
              Thread t = new Thread(new Runnable() {
                  @Override
                  public void run() {
                      URL url = null;
                      InputStream is = null;
                      try {
                          url = new URL(temp);
                          is = url.openStream();
                          final Bitmap bm = BitmapFactory.decodeStream(is);
                          runOnUiThread(new Runnable() {
                              @Override
                              public void run() {
                                  movieImg.setImageBitmap(bm);
                              }
                          });
                      } catch (Exception e) {
                          e.printStackTrace();
                      }
  
                  }
              });
  
              t.start();
  
              textTitle.setText(movieList.get(i).getTitle());
              ratingBar.setRating(movieList.get(i).getRating());
              textActor.setText(movieList.get(i).getActor());
  
              return movieView;
          }
      }
  
  
      class HttpTask extends AsyncTask<String, Void, String>{
          String url = null;
  
          public HttpTask(String url){
              this.url = url;
          }
  
          @Override
          protected void onPreExecute() {
  
          }
  
          @Override
          protected String doInBackground(String... strings) {
              String str = HttpHandler.getString(url);
              return str;
          }
  
          @Override
          protected void onPostExecute(String s) {
              Log.d("[JSON]",s);
  
              JSONArray ja = null;
              try {
                  ja = new JSONArray(s);
                  for(int i=0; i<ja.length(); i++){
                      JSONObject jo = ja.getJSONObject(i);
                      String title = jo.getString("title");
                      int rating = jo.getInt("rating");
                      String actor = jo.getString("actor");
                      String img = jo.getString("img");
                      movieList.add(new Item(title,rating,actor,img));
                  }
              } catch (JSONException e) {
                  e.printStackTrace();
              }
              itemAdapter = new ItemAdapter(movieList);
              listView.setAdapter(itemAdapter);
          }
      }
  
  
      public void clickBt(View view){
          getData();
      }
  
      public void getData(){
          String url = "http://70.12.60.108/webview/movieItem.jsp";
          httpTask = new HttpTask(url);
          httpTask.execute();
      }
  }
  
  ```

  ```java
  // Item.java
  package com.example.p535;
  
  public class Item {
      String title;
      int rating;
      String actor;
      String img;
  
      public Item(String title, int rating, String actor, String img) {
          this.title = title;
          this.rating = rating;
          this.actor = actor;
          this.img = img;
      }
  
      public Item() {
      }
  
      public String getTitle() {
          return title;
      }
  
      public void setTitle(String title) {
          this.title = title;
      }
  
      public int getRating() {
          return rating;
      }
  
      public void setRating(int rating) {
          this.rating = rating;
      }
  
      public String getActor() {
          return actor;
      }
  
      public void setActor(String actor) {
          this.actor = actor;
      }
  
      public String getImg() {
          return img;
      }
  
      public void setImg(String img) {
          this.img = img;
      }
  }
  
  ```

  ```java
  // HttpHandler.java
  package com.example.p535;
  
  import java.io.BufferedInputStream;
  import java.io.BufferedReader;
  import java.io.IOException;
  import java.io.InputStream;
  import java.io.InputStreamReader;
  import java.net.HttpURLConnection;
  import java.net.URL;
  
  public class HttpHandler {
      public static String getString(String urlstr){
          String result = null;
          URL url = null;
          HttpURLConnection hcon = null;
          InputStream is = null; // 통로
  
          try {
              url = new URL(urlstr);
              hcon = (HttpURLConnection)url.openConnection();
              hcon.setConnectTimeout(10000); // 10초동안 응답없으면 exception 처리
              hcon.setRequestMethod("GET"); // get방식
              is = new BufferedInputStream(hcon.getInputStream()); // 요청하면 inputStream이 만들어짐
              result = convertStr(is);
          } catch (Exception e) {
              e.printStackTrace();
          } finally {
              hcon.disconnect();
          }
          return result;
      }
  
      public static String convertStr(InputStream is){
          BufferedReader br = null;
          br = new BufferedReader(new InputStreamReader(is));
          StringBuilder sb = new StringBuilder();
          String temp;
          try{
              while((temp = br.readLine()) != null){
                  sb.append(temp);
              }
          }catch(Exception e){
              e.printStackTrace();
          }finally {
              try {
                  is.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
  
          return sb.toString();
      }
  }
  
  ```

  ```jsp
  <!-- webview/web/movieItem.jsp -->
  <%@ page language="java" contentType="text/json; charset=UTF-8"
      pageEncoding="UTF-8"%>
  [
  {"title":"분노의질주", "rating":4, "actor":"김도형", "img":"movie1.jpg"},
  {"title":"변신", "rating":3, "actor":"성동일", "img":"movie2.jpg"},
  {"title":"엑시트", "rating":5, "actor":"조정석, 윤아", "img":"movie3.jpg"},
  {"title":"타짜", "rating":2, "actor":"최보근", "img":"movie4.jpg"},
  {"title":"봉오동전투", "rating":4, "actor":"유해진", "img":"movie5.jpg"},
  {"title":"나쁜녀석들", "rating":4, "actor":"마동석", "img":"movie6.jpg"},
  {"title":"알라딘", "rating":4, "actor":"알라딘", "img":"movie7.jpg"},
  {"title":"47미터2", "rating":1, "actor":"상어", "img":"movie8.jpg"},
  {"title":"레드슈즈", "rating":5, "actor":"김재영", "img":"movie9.jpg"},
  {"title":"어바웃타임", "rating":5, "actor":"임지훈", "img":"movie10.jpg"}
  ]
  
  ```

  ![캡처9](https://user-images.githubusercontent.com/50862497/64331239-663df700-d00d-11e9-9a14-c3d67d766e09.PNG)

  ![c14](https://user-images.githubusercontent.com/50862497/64331300-7f46a800-d00d-11e9-907a-9680c062b626.png)









