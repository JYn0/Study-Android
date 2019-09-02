# Android

08.30



* (P258)

  ```java
  package com.example.p258;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.ContextCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.app.AlertDialog;
  import android.content.DialogInterface;
  import android.content.Intent;
  import android.content.pm.PackageManager;
  import android.net.Uri;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.Toast;
  
  import java.util.ArrayList;
  
  
  public class MainActivity extends AppCompatActivity {
  
      // Pause, Resume, Destroy
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          String[] permissions = {
                  Manifest.permission.CALL_PHONE
          };
  
          checkPermissions(permissions);
      }
  
      public void checkPermissions(String[] permissions) {
          ArrayList<String> targetList = new ArrayList<String>();
  
          for (int i = 0; i < permissions.length; i++) {
              String curPermission = permissions[i];
              int permissionCheck = ContextCompat.checkSelfPermission(this, curPermission);
              if (permissionCheck == PackageManager.PERMISSION_GRANTED) {
                  Toast.makeText(this, curPermission + " 권한 있음.", Toast.LENGTH_LONG).show();
              } else {
                  Toast.makeText(this, curPermission + " 권한 없음.", Toast.LENGTH_LONG).show();
                  if (ActivityCompat.shouldShowRequestPermissionRationale(this, curPermission)) {
                      Toast.makeText(this, curPermission + " 권한 설명 필요함.", Toast.LENGTH_LONG).show();
                  } else {
                      targetList.add(curPermission);
                  }
              }
          }
  
          String[] targets = new String[targetList.size()];
          targetList.toArray(targets);
  
          ActivityCompat.requestPermissions(this, targets, 101);
  
      }
  
      @Override
      protected void onStart() {
          super.onStart();
          Toast.makeText(this,"onStart", Toast.LENGTH_SHORT).show();
      }
  
      @Override
      protected void onResume() {
          super.onResume();
          Toast.makeText(this,"onResume", Toast.LENGTH_SHORT).show();
      }
  
      @Override
      protected void onPause() {
          super.onPause();
          Toast.makeText(this,"onPause", Toast.LENGTH_SHORT).show();
      }
  
      @Override
      protected void onStop() {
          super.onStop();
          Toast.makeText(this,"onStop", Toast.LENGTH_SHORT).show();
      }
  
      @Override
      protected void onRestart() {
          super.onRestart();
          Toast.makeText(this,"onRestart", Toast.LENGTH_SHORT).show();
      }
  
      @Override
      protected void onDestroy() {
          super.onDestroy();
          Toast.makeText(this,"onDestroy", Toast.LENGTH_SHORT).show();
      }
  
      @Override
      public void onBackPressed() {
  //        super.onBackPressed();
          AlertDialog.Builder dialog = new AlertDialog.Builder(this);
          dialog.setTitle("Alert");
          dialog.setMessage("Exit App ?");
  
          dialog.setNegativeButton("NO", new DialogInterface.OnClickListener() {
              @Override
              public void onClick(DialogInterface dialogInterface, int i) {
                  return;
              }
          });
  
          dialog.setPositiveButton("YES", new DialogInterface.OnClickListener() {
              @Override
              public void onClick(DialogInterface dialogInterface, int i) {
                  finish();
              }
          });
          dialog.show();
      }
  // ---------------------------------------------------------------------------------------------
      public void clickBt(View view){
          Intent intent = null;
          switch (view.getId()){
              case R.id.button:
                  intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.naver.com"));
                  break;
              case R.id.button2:
                  intent = new Intent(Intent.ACTION_VIEW, Uri.parse("tel:010-2990-7425"));
                  break;
              case R.id.button3:
  //                intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:010-2990-7425"));
                  int check = PermissionChecker.checkSelfPermission(this, Manifest.permission.CALL_PHONE);
                  if(check == PackageManager.PERMISSION_GRANTED){
                      intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:010-2990-7425"));
                  }else{
                      return;
                  }
                  break;
  
          }
          startActivity(intent);
      }
  }
  
  ```

  ![a17](https://user-images.githubusercontent.com/50862497/64109928-b02f9e80-cdbb-11e9-862a-c4c8ee00a117.png)



* (P289)

  ```java
  package com.example.p289;
  
  import android.os.Bundle;
  
  import com.google.android.material.floatingactionbutton.FloatingActionButton;
  import com.google.android.material.snackbar.Snackbar;
  import com.google.android.material.tabs.TabLayout;
  
  import androidx.viewpager.widget.ViewPager;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.view.Menu;
  import android.view.MenuItem;
  import android.view.View;
  
  import com.example.p289.ui.main.SectionsPagerAdapter;
  
  public class MainActivity extends AppCompatActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          SectionsPagerAdapter sectionsPagerAdapter = new SectionsPagerAdapter(this, getSupportFragmentManager());
          ViewPager viewPager = findViewById(R.id.view_pager);
          viewPager.setAdapter(sectionsPagerAdapter);
          TabLayout tabs = findViewById(R.id.tabs);
          tabs.setupWithViewPager(viewPager);
          FloatingActionButton fab = findViewById(R.id.fab);
  
          fab.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View view) {
                  Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                          .setAction("Action", null).show();
              }
          });
      }
  }
  ```

  ```java
  package com.example.p289.ui.main;
  
  import android.os.Bundle;
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.TextView;
  
  import androidx.annotation.Nullable;
  import androidx.annotation.NonNull;
  import androidx.fragment.app.Fragment;
  import androidx.lifecycle.Observer;
  import androidx.lifecycle.ViewModelProviders;
  
  import com.example.p289.R;
  
  /**
   * A placeholder fragment containing a simple view.
   */
  public class PlaceholderFragment extends Fragment {
  
      private static final String ARG_SECTION_NUMBER = "section_number";
  
      private PageViewModel pageViewModel;
  
      public static PlaceholderFragment newInstance(int index) {
          PlaceholderFragment fragment = new PlaceholderFragment();
          Bundle bundle = new Bundle();
          bundle.putInt(ARG_SECTION_NUMBER, index);
          fragment.setArguments(bundle);
          return fragment;
      }
  
      @Override
      public void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          pageViewModel = ViewModelProviders.of(this).get(PageViewModel.class);
          int index = 1;
          if (getArguments() != null) {
              index = getArguments().getInt(ARG_SECTION_NUMBER);
          }
          pageViewModel.setIndex(index);
      }
  
      @Override
      public View onCreateView(
              @NonNull LayoutInflater inflater, ViewGroup container,
              Bundle savedInstanceState) {
          View root = inflater.inflate(R.layout.fragment_main, container, false);
          final TextView textView = root.findViewById(R.id.section_label);
          pageViewModel.getText().observe(this, new Observer<String>() {
              @Override
              public void onChanged(@Nullable String s) {
                  textView.setText(s);
              }
          });
          return root;
      }
  }
  ```

  ![a21](https://user-images.githubusercontent.com/50862497/64111443-ac9e1680-cdbf-11e9-9ae9-0381a644195b.png)



* (P290)

  ```java
  package com.example.p290;
  
  import android.os.Bundle;
  
  import com.google.android.material.floatingactionbutton.FloatingActionButton;
  import com.google.android.material.snackbar.Snackbar;
  
  import android.view.View;
  
  import androidx.navigation.NavController;
  import androidx.navigation.Navigation;
  import androidx.navigation.ui.AppBarConfiguration;
  import androidx.navigation.ui.NavigationUI;
  
  import com.google.android.material.navigation.NavigationView;
  
  import androidx.drawerlayout.widget.DrawerLayout;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.appcompat.widget.Toolbar;
  
  import android.view.Menu;
  
  public class MainActivity extends AppCompatActivity {
  
      private AppBarConfiguration mAppBarConfiguration;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Toolbar toolbar = findViewById(R.id.toolbar);
          setSupportActionBar(toolbar);
          FloatingActionButton fab = findViewById(R.id.fab);
          fab.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View view) {
                  Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                          .setAction("Action", null).show();
              }
          });
          DrawerLayout drawer = findViewById(R.id.drawer_layout);
          NavigationView navigationView = findViewById(R.id.nav_view);
          // Passing each menu ID as a set of Ids because each
          // menu should be considered as top level destinations.
          mAppBarConfiguration = new AppBarConfiguration.Builder(
                  R.id.nav_home, R.id.nav_gallery, R.id.nav_slideshow,
                  R.id.nav_tools, R.id.nav_share, R.id.nav_send)
                  .setDrawerLayout(drawer)
                  .build();
          NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
          NavigationUI.setupActionBarWithNavController(this, navController, mAppBarConfiguration);
          NavigationUI.setupWithNavController(navigationView, navController);
      }
  
      @Override
      public boolean onCreateOptionsMenu(Menu menu) {
          // Inflate the menu; this adds items to the action bar if it is present.
          getMenuInflater().inflate(R.menu.main, menu);
          return true;
      }
  
      @Override
      public boolean onSupportNavigateUp() {
          NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
          return NavigationUI.navigateUp(navController, mAppBarConfiguration)
                  || super.onSupportNavigateUp();
      }
  }
  
  ```

  ![a22](https://user-images.githubusercontent.com/50862497/64111444-ad36ad00-cdbf-11e9-83a0-a72c28627433.png)



* (P312)

  ```java
  package com.example.p312;
  
  import androidx.annotation.NonNull;
  import androidx.appcompat.app.ActionBar;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.view.Menu;
  import android.view.MenuItem;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
  
      ActionBar actionBar;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          actionBar = getSupportActionBar();
          actionBar.setLogo(R.drawable.icon1);
          actionBar.setDisplayOptions(ActionBar.DISPLAY_SHOW_HOME | ActionBar.DISPLAY_USE_LOGO);
  
      }
  
      @Override
      public boolean onCreateOptionsMenu(Menu menu) {
          getMenuInflater().inflate(R.menu.mymenu,menu);
          return true;
      }
  
      @Override
      public boolean onOptionsItemSelected(@NonNull MenuItem item) {
          if(item.getItemId() == R.id.msetting){
              Toast.makeText(this, "setting", Toast.LENGTH_SHORT).show();
          }else if(item.getItemId() == R.id.mlogin){
              Toast.makeText(this, "login", Toast.LENGTH_SHORT).show();
          }
          return super.onOptionsItemSelected(item);
      }
  }
  
  ```

  ![a24](https://user-images.githubusercontent.com/50862497/64111751-639a9200-cdc0-11e9-8dfd-8e4cad607bc3.png)



* (P351)

  ```
  package com.example.p351;
  
  import androidx.annotation.NonNull;
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.os.Bundle;
  import android.widget.CalendarView;
  import android.widget.TextView;
  import android.widget.TimePicker;
  
  public class MainActivity extends AppCompatActivity {
  
      TextView textView;
      CalendarView calendarView;
      TimePicker timePicker;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
          calendarView = findViewById(R.id.calendarView);
          timePicker = findViewById(R.id.timePicker);
  
          calendarView.setOnDateChangeListener(new CalendarView.OnDateChangeListener() {
              @Override
              public void onSelectedDayChange(@NonNull CalendarView calendarView, int i, int i1, int i2) {
                  String data = i+"/"+ i1+"/"+i2;
                  textView.setText(data);
              }
          });
          timePicker.setOnTimeChangedListener(new TimePicker.OnTimeChangedListener() {
              @Override
              public void onTimeChanged(TimePicker timePicker, int i, int i1) {
                  String time = i+":"+ i1;
                  textView.setText(time);
              }
          });
      }
  }
  
  ```

  ![a23](https://user-images.githubusercontent.com/50862497/64111649-26ce9b00-cdc0-11e9-95ae-1fa8dbc5e071.PNG)



* (P353)

  ```java
  package com.example.p353;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.Intent;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.ImageView;
  import android.widget.ProgressBar;
  import android.widget.TextView;
  
  public class MainActivity extends AppCompatActivity {
  // 버튼을 누르면 백그라운드가 실행되도록
  
      Intent intent; // 서비스 실행시킬때 사용
      TextView textView;
      ImageView imageView;
      ProgressBar progressBar;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
          imageView = findViewById(R.id.imageView);
          progressBar = findViewById(R.id.progressBar);
          progressBar.setMax(9);
      }
  
      public void clickBt(View view){
          intent = new Intent(this, MyService.class);
          startService(intent);
      }
  
      @Override
      protected void onNewIntent(Intent intent) {
          process(intent);
      }
  
      public void process(Intent intent){ // 서비스에서 값을 보내주면 자동으로 받는 부분
          if(intent != null){
              int data = intent.getIntExtra("cmd", 0);
              textView.setText(data+" ");
              progressBar.setProgress(data);
              if(data%2 == 0){
                  imageView.setImageResource(R.drawable.img1);
              }else{
                  imageView.setImageResource(R.drawable.img2);
              }
          }
      }
  
      @Override
      protected void onDestroy() {
          super.onDestroy();
          if(intent != null){
              stopService(intent);
          }
      }
  }
  
  ```

  ```java
  MyService.java
  
  package com.example.p353;
  
  import android.app.Service;
  import android.content.Intent;
  import android.os.IBinder;
  import android.util.Log;
  
  // 화면이 없는 Activity로 생각
  public class MyService extends Service {
      boolean flag = false;
  
      public MyService() {
      }
  
      @Override
      public void onCreate() {  // 실행할때
          super.onCreate();
          Log.d("[MyService]","onCreate......................");
      }
  
      @Override
      public void onDestroy() { // 죽을때
          super.onDestroy();
          Log.d("[MyService]","onDestroy......................");
      }
  
  
      @Override
      public int onStartCommand(Intent intent, int flags, int startId) {
          // Activity가 서비스 실행할때 onCreat하고 다음에 실행, intent는 mainActivity에서 보내줌
          Log.d("[MyService]","onStartCommand......................");
  
          final Intent sendIntent = new Intent(getApplicationContext(), MainActivity.class);
          sendIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_SINGLE_TOP | intent.FLAG_ACTIVITY_CLEAR_TOP);
  
          Runnable run = new Runnable() {
              boolean flag = true;
  
              @Override
              public void run() {
                  for(int i=0;i<10;i++){
                      Log.d("[MyService]","while......................");
                      sendIntent.putExtra("cmd",i);
                      startActivity(sendIntent);
  
                      try {
                          Thread.sleep(1000);
                      } catch (InterruptedException e) {
                          e.printStackTrace();
                      }
  
                      if(flag == false){
                          break;
                      }
                  }
              }
          };
  //        new Thread(run).start();
          Thread t = new Thread(run);
          t.start();
  
          return super.onStartCommand(intent, flags, startId);
      }
  
      @Override
      public IBinder onBind(Intent intent) {
          // TODO: Return the communication channel to the service.
          throw new UnsupportedOperationException("Not yet implemented");
      }
  }
  
  ```

  ![a25](https://user-images.githubusercontent.com/50862497/64112037-297dc000-cdc1-11e9-8de6-eb46402b0f74.png)



* (P360)

  ```java
  package com.example.p360;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.content.ComponentName;
  import android.content.Context;
  import android.content.Intent;
  import android.content.ServiceConnection;
  import android.os.Bundle;
  import android.os.IBinder;
  import android.util.Log;
  import android.view.View;
  import android.widget.ProgressBar;
  import android.widget.SeekBar;
  
  public class MainActivity extends AppCompatActivity {
  
      // Activity가 MyService를 제어한다
      MyService ms;
      ProgressBar progressBar1, progressBar2;
      SeekBar seekBar;
  
  
      ServiceConnection conn = new ServiceConnection() {
          @Override
          public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
              // Binder 가져오기
              MyService.MyBinder mb = (MyService.MyBinder)iBinder;
              // Binder에서 서비스 가져오기
              ms = mb.getService();
          }
  
          @Override
          public void onServiceDisconnected(ComponentName componentName) {
  
          }
      };
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Intent intent = new Intent(this, MyService.class);
          bindService(intent, conn, Context.BIND_AUTO_CREATE); // binding
          // 위에 의해 bind가 불러짐
  
          progressBar1 = findViewById(R.id.progressBar1);
          progressBar1.setMax(10);
          progressBar2 = findViewById(R.id.progressBar2);
          progressBar2.setMax(10);
          seekBar = findViewById(R.id.seekBar);
          seekBar.setMax(10);
      }
  
      public void clickBt1(View view){
          ms.bt1();
      }
      public void clickBt2(View view){
          ms.bt2();
      }
  
      @Override
      protected void onNewIntent(Intent intent) {
          process(intent);
          // 서비스에서 intent에 정보를 넣어 보내면 받는 곳
      }
  
      public void process(Intent intent){ // 서비스에서 값을 보내주면 자동으로 받는 부분
          int data = intent.getIntExtra("cmd", 0);
          int data2 = intent.getIntExtra("cmd2", 0);
  
          if (data != 0) {
              progressBar1.setProgress(data);
          }
          if(data2 != 0) {
              progressBar2.setProgress(data2);
              seekBar.setProgress(data2);
          }
      }
  
  //    @Override
  //    protected void onDestroy() {
  //        super.onDestroy();
  //    }
  }
  
  ```

  ```java
  package com.example.p360;
  
  import android.app.Service;
  import android.content.Intent;
  import android.os.Binder;
  import android.os.IBinder;
  import android.util.Log;
  import android.widget.ProgressBar;
  import android.widget.SeekBar;
  
  public class MyService extends Service {
  
      // 바인더 만들기
      class MyBinder extends Binder {
          public MyService getService(){
              return MyService.this;
          }
      }
  
      IBinder iBinder = new MyBinder();
  
      @Override
      public IBinder onBind(Intent intent) {
          // 바인더 안에서는 MyService를 가져올수있음
          return iBinder;
      }
  
      public void bt1(){
          Log.d("[ms]","bt1------------------------------------------------------------");
          Runnable run = new Runnable() {
              boolean flag = true;
  
              @Override
              public void run() {
  
                  for(int i=0;i<=10;i++){
                      Log.d("[MyService]","while......................");
                      Intent sintent = new Intent(getApplicationContext(), MainActivity.class);
                      sintent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_SINGLE_TOP | Intent.FLAG_ACTIVITY_CLEAR_TOP);
                      sintent.putExtra("cmd", i);
                      startActivity(sintent);
  
                      try {
                          Thread.sleep(500);
                      } catch (InterruptedException e) {
                          e.printStackTrace();
                      }
                  }
              }
          };
          Thread t = new Thread(run);
          t.start();
      }
      public void bt2(){
          Log.d("[ms]","bt2------------------------------------------------------------");
          final Intent sintent = new Intent(getApplicationContext(), MainActivity.class);
          sintent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_SINGLE_TOP | Intent.FLAG_ACTIVITY_CLEAR_TOP);
  
          Runnable run = new Runnable() {
              @Override
              public void run() {
                  for(int i=0; i<10; i++){
                      sintent.putExtra("cmd2",i);
                      startActivity(sintent);
                      try {
                          Thread.sleep(1000);
                      } catch (InterruptedException e) {
                          e.printStackTrace();
                      }
                  }
              }
          };
          Thread t = new Thread(run);
          t.start();
      }
  
  }
  
  ```

  ![a26](https://user-images.githubusercontent.com/50862497/64112021-1a970d80-cdc1-11e9-80ea-e428504daf24.png)

.