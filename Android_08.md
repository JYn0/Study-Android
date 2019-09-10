

# Android

09.10



Android 4대 Component

activity, broadcast receiver, content provider(data 공유), service



## 로그인하기

간단한 서버 만들기(Spring할 시간 없어서 간단하게)



get방식

http://70.12.60.108/webview/login.jsp?id=aa&pwd=11



* (P536)

  ```java
  // MainActivity.java
  
  package com.example.p536;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.fragment.app.FragmentActivity;
  
  import android.content.Intent;
  import android.os.AsyncTask;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.EditText;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      EditText editText, editText2;
      HttpTask httpTask;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          editText = findViewById(R.id.editText);
          editText2 = findViewById(R.id.editText2);
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
              String id = editText.getText().toString();
              if(s.equals("0")){
                  Intent intent = new Intent(MainActivity.this, Main2Activity.class);
                  intent.putExtra("id",id);
                  startActivity(intent);
              }else{
                  System.out.println("XXX "+s);
              }
          }
      }
  
      public void clickBt(View view){
          String id = editText.getText().toString();
          String pwd = editText2.getText().toString();
  
          String url = "http://70.12.60.108/webview/login.jsp?id="+id+"&pwd="+pwd;
          httpTask = new HttpTask(url);
          httpTask.execute();
      }
  
  }
  
  ```

  ```java
  // Main2Activity.java
  
  package com.example.p536;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.os.Bundle;
  import android.widget.TextView;
  
  public class Main2Activity extends AppCompatActivity {
  
      TextView textView;
  
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main2);
          textView = findViewById(R.id.textView);
          Intent intent = getIntent();
          String id = intent.getStringExtra("id");
          textView.setText(id+"님 환영합니다.");
      }
  }
  
  ```

  ```jsp
  <!-- login.jsp -->
  <%@ page language="java" contentType="text/html; charset=EUC-KR"
      pageEncoding="EUC-KR"%>
  
  <% 
  	String id = request.getParameter("id");
  	String pwd = request.getParameter("pwd");
  	String result = "0";
  	if(id.equals("aa") && pwd.equals("11")){
  		result = "0";
  	}else{
  		result = "1";
  	}
  	out.print(result);
  %>
  ```

  ![](https://user-images.githubusercontent.com/50862497/64601536-1342b600-d3f8-11e9-885f-20b0bf6d67cf.png)

  ![d1](https://user-images.githubusercontent.com/50862497/64601849-a845af00-d3f8-11e9-9f8e-640f6226d2ad.png)







## 데이터베이스

(크롬에는)

IndexedDB -> JSON Object로 데이터 넣고빼기

Web SQL -> sql로 데이터 넣고빼기

(폰에는)

SQLWrite -> Web SQL과 동일



데이터베이스(create, drop) -> 테이블 -> 데이터 추가(insert)



* (P548)

  ```java
  // MainActivity.java
  
  package com.example.p548;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.database.Cursor;
  import android.database.sqlite.SQLiteDatabase;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.TextView;
  
  
  public class MainActivity extends AppCompatActivity {
  // DataBase 및 table 생성
      TextView textView;
      DatabaseHelper databaseHelper;
      SQLiteDatabase sqLiteDatabase;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
  
      }
  
      public void create(View view){
          String name = "emp"; // table 이름
          println("createDatabase 호출됨.");
          // DB 생성
          databaseHelper = new DatabaseHelper(this); // db 생성
          sqLiteDatabase = databaseHelper.getWritableDatabase();
  
          println("데이터베이스 생성함 : " );
  
  
          // table 생성
          if (sqLiteDatabase == null) {
              println("데이터베이스를 먼저 생성하세요.");
              return;
          }
  
          sqLiteDatabase.execSQL("create table if not exists " + name + "("
                  + " _id integer PRIMARY KEY autoincrement, "
                  + " name text, "
                  + " age integer, "
                  + " mobile text)");
  
          println("테이블 생성함 : " + name);
      }
  
      public void insert(View view){
          println("insertRecord 호출됨.");
  
          if (sqLiteDatabase == null) {
              println("데이터베이스를 먼저 생성하세요.");
              return;
          }
  
          sqLiteDatabase.execSQL("insert into emp"
                  + "(name, age, mobile) "
                  + " values "
                  + "('John', 20, '010-1000-1000')");
  
          println("레코드 추가함.");
      }
  
      public void select(View view){
          println("executeQuery 호출됨.");
  
          Cursor cursor = sqLiteDatabase.rawQuery("select _id, name, age, mobile from emp", null);
          int recordCount = cursor.getCount();
          println("레코드 개수 : " + recordCount);
  
          for (int i = 0; i < recordCount; i++) {
              cursor.moveToNext();
  
              int id = cursor.getInt(0);
              String name = cursor.getString(1);
              int age = cursor.getInt(2);
              String mobile = cursor.getString(3);
  
              println("레코드 #" + i + " : " + id + ", " + name + ", " + age + ", " + mobile);
          }
  
          cursor.close();
      }
  
      public void println(String data){
          textView.append(data + "\n");
      }
  }
  
  ```

  ```java
  // DatabaseHelper.java
  
  package com.example.p548;
  
  import android.content.Context;
  import android.database.sqlite.SQLiteDatabase;
  import android.database.sqlite.SQLiteOpenHelper;
  import android.util.Log;
  
  public class DatabaseHelper extends SQLiteOpenHelper {
      // db 생성
      public static String NAME = "employee.db"; // db의 이름
      public static int VERSION = 1; // employee.db의 버전
  
      public DatabaseHelper(Context context) {
          super(context, NAME, null, VERSION);
          // context : MainActivity
      }
  
      public void onCreate(SQLiteDatabase db) {
          println("onCreate 호출됨");
          // table 생성
          String sql = "create table if not exists emp("
                  + " _id integer PRIMARY KEY autoincrement, "
                  + " name text, "
                  + " age integer, "
                  + " mobile text)";
  
          db.execSQL(sql);
      }
  
      public void onOpen(SQLiteDatabase db) {
          println("onOpen 호출됨");
      }
  
      public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
          println("onUpgrade 호출됨 : " + oldVersion + " -> " + newVersion);
  
          if (newVersion > 1) {
              db.execSQL("DROP TABLE IF EXISTS emp");
          }
      }
  
      public void println(String data) {
          Log.d("DatabaseHelper", data);
      }
  }
  
  ```

  ![d2](https://user-images.githubusercontent.com/50862497/64602373-a6c8b680-d3f9-11e9-9749-25f8b7c21da4.png)



* (PMemo)

  ![d3](https://user-images.githubusercontent.com/50862497/64602437-c5c74880-d3f9-11e9-95e3-ef842b58ea3c.png)

  

  diary table 만들기

  int no, string date, title, name, content

  

  ```java
  // MainActivity.java
  
  package com.example.pmemo;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.os.AsyncTask;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.EditText;
  
  public class MainActivity extends AppCompatActivity {
      EditText editText, editText2;
      HttpTask httpTask;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          editText = findViewById(R.id.editText);
          editText2 = findViewById(R.id.editText2);
      }
  
      class HttpTask extends AsyncTask<String, Void, String> {
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
              String id = editText.getText().toString();
              if(s.equals("0")){
                  Intent intent = new Intent(MainActivity.this, Main2Activity.class);
                  intent.putExtra("id",id);
                  startActivity(intent);
              }else{
                  System.out.println("XXX "+s);
              }
          }
      }
  
      public void clickBt(View view){
          String id = editText.getText().toString();
          String pwd = editText2.getText().toString();
  
          String url = "http://70.12.60.108/webview/login.jsp?id="+id+"&pwd="+pwd;
          httpTask = new HttpTask(url);
          httpTask.execute();
      }
  
  }
  
  ```

  ```java
  // Main2Activity.java
  
  package com.example.pmemo;
  
  import android.content.Intent;
  import android.database.Cursor;
  import android.database.sqlite.SQLiteDatabase;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.EditText;
  import android.widget.TextView;
  
  import androidx.annotation.Nullable;
  import androidx.appcompat.app.AppCompatActivity;
  
  public class Main2Activity extends AppCompatActivity {
  
      TextView textView, textView2;
      EditText editText3, editText4;
  
      DatabaseHelper databaseHelper; // db 생성
      SQLiteDatabase sqLiteDatabase; // table 생성
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main2);
          textView = findViewById(R.id.textView);
          textView2 = findViewById(R.id.textView2);
          editText3 = findViewById(R.id.editText3);
          editText4 = findViewById(R.id.editText4);
          Intent intent = getIntent();
          String name = intent.getStringExtra("id");
  //        textView.setText(id+"님 환영합니다.");
          textView.setText(name);
  
          ///////////////////////////////////////////////////////////////////////////////////////////
  
          String dbname = "memo"; // table 이름
          // DB 생성
          databaseHelper = new DatabaseHelper(this); // db 생성
          sqLiteDatabase = databaseHelper.getWritableDatabase();
  
          // table 생성
          if (sqLiteDatabase == null) {
              System.out.println("데이터베이스를 먼저 생성하세요.");
              return;
          }
          sqLiteDatabase.execSQL("create table if not exists memo("
                  + " _no integer PRIMARY KEY autoincrement, "
                  + " date text,"
                  + " title text,"
                  + " name text, "
                  + " content text)");
          System.out.println("테이블 생성함 : " + dbname);
      }
  
      public void clickCalendar(View view){
          Intent calintent = new Intent(Main2Activity.this, Main3Activity.class);
          startActivityForResult(calintent, 101);
      }
  
      @Override
      protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
          super.onActivityResult(requestCode, resultCode, data);
          if(requestCode==101 && resultCode==RESULT_OK){
              String date = data.getStringExtra("cdate");
  //            System.out.println("date: " + date);
              textView2.setText(date);
          }
      }
  
      public void clickSave(View view){
  
          if (sqLiteDatabase == null) {
              System.out.println("데이터베이스를 먼저 생성하세요.");
              return;
          }
  
          String date = textView2.getText().toString();
          String title = editText3.getText().toString();
          String name = textView.getText().toString();
          String content = editText4.getText().toString();
  
  
          sqLiteDatabase.execSQL("insert into memo"
                  + "(date, title, name, content) "
                  + " values "
                  + "('" + date + "', '" + title + "', '" + name + "', '" + content + "')");
      }
  
      public void clickList(View view){
  
          Intent listintent = new Intent(Main2Activity.this, Main4Activity.class);
          startActivityForResult(listintent, 104);
  
      }
  }
  
  ```

  ```java
  // Main3Activity.java
  
  package com.example.pmemo;
  
  import androidx.annotation.NonNull;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.os.Bundle;
  import android.widget.CalendarView;
  
  public class Main3Activity extends AppCompatActivity {
      CalendarView calendarView;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main3);
          calendarView = findViewById(R.id.calendarView);
  
          calendarView.setOnDateChangeListener(new CalendarView.OnDateChangeListener() {
              @Override
              public void onSelectedDayChange(@NonNull CalendarView calendarView, int i, int i1, int i2) {
                  String date = i+""+ i1+""+i2;
                  System.out.println(date);
                  Intent cintent = new Intent(Main3Activity.this, Main2Activity.class);
                  cintent.putExtra("cdate",date);
                  setResult(RESULT_OK,cintent);
                  finish();
              }
          });
      }
  
  }
  
  ```

  ```java
  // Main4Activity.java
  
  package com.example.pmemo;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.database.Cursor;
  import android.database.sqlite.SQLiteDatabase;
  import android.os.Bundle;
  import android.widget.TextView;
  
  
  public class Main4Activity extends AppCompatActivity {
      TextView textView3;
      SQLiteDatabase sqLiteDatabase;
      DatabaseHelper databaseHelper; // db 생성
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main4);
          textView3 = findViewById(R.id.textView3);
          databaseHelper = new DatabaseHelper(this); // db 생성
          sqLiteDatabase = databaseHelper.getWritableDatabase();
          Cursor cursor = sqLiteDatabase.rawQuery("select _no, date, title, name, content from memo", null);
          int recordCount = cursor.getCount();
  
          for (int i = 0; i < recordCount; i++) {
              cursor.moveToNext();
              int no = cursor.getInt(0);
              String date = cursor.getString(1);
              String title = cursor.getString(2);
              String name = cursor.getString(3);
              String content = cursor.getString(4);
  
              println("레코드 #" + i + " : " + date + ", " + title + ", " + name + ", " + content);
          }
  
          cursor.close();
      }
  
      public void println(String data){
          textView3.append(data + "\n");
      }
  }
  
  ```

  ![d4](https://user-images.githubusercontent.com/50862497/64603274-18553480-d3fb-11e9-8efb-566823593e37.png)

  





