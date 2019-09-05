# Android

09.02



* (P369)

  ```java
  package com.example.p369;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.ContextCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.annotation.SuppressLint;
  import android.content.BroadcastReceiver;
  import android.content.Context;
  import android.content.Intent;
  import android.content.IntentFilter;
  import android.content.pm.PackageManager;
  import android.net.ConnectivityManager;
  import android.net.NetworkInfo;
  import android.net.Uri;
  import android.os.Bundle;
  import android.util.Log;
  import android.view.View;
  import android.widget.Toast;
  
  import java.util.ArrayList;
  
  public class MainActivity extends AppCompatActivity {
  
  //    @Override
  //    protected void onCreate(Bundle savedInstanceState) {
  //        super.onCreate(savedInstanceState);
  //        setContentView(R.layout.activity_main);
  //        IntentFilter intentFilter = new IntentFilter();
  //        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
  //        // 네트워크가 바뀌는 상황을 받을 것
  //
  //        BroadcastReceiver broadcastReceiver;
  //        broadcastReceiver = new BroadcastReceiver() {
  //            @Override
  //            public void onReceive(Context context, Intent intent) {
  //                // 네트워크 변화가 생기면 받는 곳
  //                String action = intent.getAction();
  //                //네트워크 상황을 보는 manager
  //                ConnectivityManager cManager = null;
  //                NetworkInfo mobile = null;
  //                NetworkInfo wifi = null;
  //                if (action.equals("android.net.conn.CONNECTIVITY_CHANGE")) {
  //                    cManager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);
  //                    mobile = cManager.getNetworkInfo(ConnectivityManager.TYPE_MOBILE);
  //                    wifi = cManager.getNetworkInfo(ConnectivityManager.TYPE_WIFI);
  //                    // manifest -> android.permission.ACCESS_NETWORK_STATE
  //
  //                    if (mobile != null && mobile.isConnected()) {
  //                        Toast.makeText(context, "MOBILE", Toast.LENGTH_SHORT).show();
  //                    } else if (wifi != null && wifi.isConnected()) {
  //                        Toast.makeText(context, "WIFI", Toast.LENGTH_SHORT).show();
  //                    } else {
  //                        Toast.makeText(context, "NOT CONN", Toast.LENGTH_SHORT).show();
  //                    }
  //                }
  //
  //            }
  //        };
  //
  //        // 네트워크 변화에대해 받을 준비가 되어있다
  //        registerReceiver(broadcastReceiver, intentFilter);
  //
  //
  //
  //        String []  targets = {
  //                Manifest.permission.CALL_PHONE
  //        };
  //        checkMyPermissiont(targets);
  //    }
  //
  //    private void checkMyPermissiont(String[] permissions) {
  //        ArrayList<String> targetList = new ArrayList<>();
  //        for(String str : permissions){
  //            int check = ContextCompat.checkSelfPermission(this, str);
  //            if(check == PackageManager.PERMISSION_GRANTED){
  //                Toast.makeText(this,"GRANTED", Toast.LENGTH_SHORT).show();
  //            }else{
  //                Toast.makeText(this,"DENY", Toast.LENGTH_SHORT).show();
  //                if(ActivityCompat.shouldShowRequestPermissionRationale(this,str)){
  //                    Toast.makeText(this,"DESC", Toast.LENGTH_SHORT).show();
  //                }
  //            }
  //        }
  //        String [] targets = new String [targetList.size()];
  //        targetList.toArray(targets);
  //        ActivityCompat.requestPermissions(this,targets,101);
  //
  //    }
  //
  //    @SuppressLint("MissingPermission")
  //    public void clickText(View view) {
  //        Intent intent = null;
  //        intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:01029907425"));
  //
  //        startActivity(intent);
  //    }
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          IntentFilter intentFilter = new IntentFilter();
          intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
          String[] Permissions = {
                  Manifest.permission.CALL_PHONE,
          };
          ActivityCompat.requestPermissions(this, Permissions, 101);
  
  
          BroadcastReceiver boBroadcastReceiver = new BroadcastReceiver() {
              @Override
              public void onReceive(Context context, Intent intent) {
                  String action = intent.getAction();
                  ConnectivityManager connectivityManager = null;
                  NetworkInfo mobile = null;
                  NetworkInfo wifi = null;
                  if (action.equals("android.net.conn.CONNECTIVITY_CHANGE")) {
                      connectivityManager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);
                      mobile = connectivityManager.getNetworkInfo(ConnectivityManager.TYPE_MOBILE);
                      wifi = connectivityManager.getNetworkInfo(ConnectivityManager.TYPE_MOBILE);
                      if (mobile != null && mobile.isConnected()) {
                          Toast.makeText(context, "mobile", Toast.LENGTH_SHORT).show();
                      } else if (wifi != null && wifi.isConnected()) {
                          Toast.makeText(context, "wifi", Toast.LENGTH_SHORT).show();
                      } else {
                          Toast.makeText(context, "not Conn", Toast.LENGTH_SHORT).show();
                      }
                  }
              }
          };
          registerReceiver(boBroadcastReceiver, intentFilter);
      }
  
      public void clickText(View v) {
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
  
  }
  
  ```

  ```xml
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <uses-permission android:name="android.permission.CALL_PHONE" />
  ```

  ![c1](https://user-images.githubusercontent.com/50862497/64325164-9e8c0800-d002-11e9-9be2-beaed6c640c5.png)



* (P427) List

  ```java
  //P427
  package com.example.p427;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.content.Context;
  import android.content.Intent;
  import android.content.pm.PackageManager;
  import android.net.Uri;
  import android.os.Bundle;
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
  
  import org.w3c.dom.Text;
  
  import java.util.ArrayList;
  
  public class MainActivity extends AppCompatActivity
  implements AdapterView.OnItemClickListener {
  
      ArrayList<Item> list;
      ListView listView;
      LinearLayout container;
      ItemAdapter itemAdapter;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          listView = findViewById(R.id.listView);
          container = findViewById(R.id.container);
          listView.setOnItemClickListener(this);
  
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
              ImageView iv1 = myview.findViewById(R.id.imageView);
              TextView iv2= myview.findViewById(R.id.textView);
              TextView iv3 = myview.findViewById(R.id.textView2);
  
              iv1.setImageResource(alist.get(alist.size()-i-1).getImgId());
              iv2.setText(alist.get(alist.size()-i-1).getName());
              iv3.setText(alist.get(alist.size()-i-1).getPhone());
  //            iv3.setText(alist.get(i).getPhone());
              return myview;
          }
      }
  
      public void clickBt(View view){
          getData();
          itemAdapter = new ItemAdapter(list);
          listView.setAdapter(itemAdapter);
      }
  
      public void clickBt2(View view){
          itemAdapter.addItem(new Item("이름11","010-0011-0011",R.drawable.ic_launcher_foreground));
          itemAdapter.notifyDataSetChanged(); // refresh
      }
  
      public void getData(){
  
          // 가상의 데이터
          list = new ArrayList<>();
          list.add(new Item("이름1","010-1111-1234",R.drawable.it01));
          list.add(new Item("이름2","010-2222-1234",R.drawable.it02));
          list.add(new Item("이름3","010-3333-1234",R.drawable.it03));
          list.add(new Item("이름4","010-4444-1234",R.drawable.it04));
          list.add(new Item("이름5","010-5555-8765",R.drawable.it05));
          list.add(new Item("이름6","010-6666-8765",R.drawable.it06));
          list.add(new Item("이름7","010-7777-8765",R.drawable.it07));
          list.add(new Item("이름8","010-8888-8765",R.drawable.it08));
          list.add(new Item("이름9","010-9999-0910",R.drawable.it09));
          list.add(new Item("이름10","010-1010-0910",R.drawable.it10));
      }
  }
  
  ```

  ```java
  //Item.java
  package com.example.p427;
  
  public class Item {
      String name;
      String phone;
      int imgId; // R.drawble.int
  
      public Item() {
      }
  
      public Item(String name, String phone, int imgId) {
          this.name = name;
          this.phone = phone;
          this.imgId = imgId;
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
  
      public int getImgId() {
          return imgId;
      }
  
      public void setImgId(int imgId) {
          this.imgId = imgId;
      }
  }
  
  ```

  ![c2](https://user-images.githubusercontent.com/50862497/64325613-7224bb80-d003-11e9-9c25-318a8e2158c6.png)



* (P436) spinner, ratinBar

  ```java
  package com.example.p436;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.content.Intent;
  import android.content.pm.PackageManager;
  import android.net.Uri;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.AdapterView;
  import android.widget.ArrayAdapter;
  import android.widget.ImageView;
  import android.widget.RatingBar;
  import android.widget.Spinner;
  import android.widget.Toast;
  
  import java.lang.reflect.Array;
  import java.util.ArrayList;
  
  public class MainActivity extends AppCompatActivity
  implements AdapterView.OnItemSelectedListener {
  //    ArrayList<String> list;
      ArrayList<Integer> list;
      Spinner spinner;
      ImageView imageView;
      RatingBar ratingBar;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          spinner = findViewById(R.id.spinner);
          imageView = findViewById(R.id.imageView);
          ratingBar = findViewById(R.id.ratingBar);
          ratingBar.setNumStars(5);
          ratingBar.setStepSize(1);
          ratingBar.setMax(5);
          ratingBar.setRating(0);
  
          getData();
          ArrayAdapter<Integer> adapter = new ArrayAdapter<Integer>(this, android.R.layout.simple_spinner_item,list);
          adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
  
          spinner.setAdapter(adapter); // Adapter에 adapter 넣기
          spinner.setOnItemSelectedListener(this); // 이벤트가 발생하면 내가 처리하겠다
  
          String[] Permissions = {
                  Manifest.permission.CALL_PHONE,
          };
          ActivityCompat.requestPermissions(this, Permissions, 101);
      }
  
      private void getData() {
          list = new ArrayList<>();
          list.add(R.drawable.it01);
          list.add(R.drawable.it02);
          list.add(R.drawable.it03);
          list.add(R.drawable.it04);
          list.add(R.drawable.it05);
          list.add(R.drawable.it06);
          list.add(R.drawable.it07);
          list.add(R.drawable.it08);
          list.add(R.drawable.it09);
          list.add(R.drawable.it10);
  
  
  //        list.add("01074252990");
  //        list.add("01029907425");
      }
  
      @Override
      public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
  //        String str = list.get(i);
  //        Toast.makeText(this, ""+str, Toast.LENGTH_SHORT).show();
  
          int imgcode = list.get(i);
          imageView.setImageResource(imgcode);
          float temp = ratingBar.getRating()+1;
          ratingBar.setRating(temp%6);
          Toast.makeText(this, ""+imgcode, Toast.LENGTH_SHORT).show();
  
  //        int permission = PermissionChecker.checkSelfPermission(this, Manifest.permission.CALL_PHONE);
  //        Intent intent = null;
  //        if(permission == PackageManager.PERMISSION_GRANTED){
  //            intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:01029907425"));
  //            startActivity(intent);
  //        }else{
  //            Toast.makeText(this, "권한부여가 안되었습니다.", Toast.LENGTH_SHORT).show();
  //            return;
  //        }
      }
  
      @Override
      public void onNothingSelected(AdapterView<?> adapterView) {
          // X
      }
  }
  
  ```

  ![c3](https://user-images.githubusercontent.com/50862497/64326163-6c7ba580-d004-11e9-842f-cbf10092dbf5.png)



* (P440) 이름,전화번호,사진 등록하고 전화걸기

  ```java
  package com.example.p440;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.content.Context;
  import android.content.Intent;
  import android.content.pm.PackageManager;
  import android.net.Uri;
  import android.os.Bundle;
  import android.view.LayoutInflater;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.AdapterView;
  import android.widget.ArrayAdapter;
  import android.widget.BaseAdapter;
  import android.widget.EditText;
  import android.widget.ImageView;
  import android.widget.LinearLayout;
  import android.widget.ListView;
  import android.widget.Spinner;
  import android.widget.TextView;
  import android.widget.Toast;
  
  import java.util.ArrayList;
  import java.util.Arrays;
  
  public class MainActivity extends AppCompatActivity
  implements AdapterView.OnItemClickListener, AdapterView.OnItemSelectedListener{
  
      ArrayList<Item> list;
      ArrayList<Integer> plist;
      ListView listView;
      LinearLayout container;
      EditText editText, editText2;
      Spinner spinner;
      ImageView imageView;
  
      ItemAdapter itemAdapter;
  
      int imgcode;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          data();
          getData();
  
          listView = findViewById(R.id.listView);
          container = findViewById(R.id.container);
          listView.setOnItemClickListener(this);
  
          editText = findViewById(R.id.editText);
          editText2 = findViewById(R.id.editText2);
          spinner = findViewById(R.id.spinner);
  
          ArrayAdapter<Integer> adapter = new ArrayAdapter<Integer>(this, android.R.layout.simple_spinner_item, plist);
          adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
  
          spinner.setAdapter(adapter);
          spinner.setOnItemSelectedListener(this);
  
          String[] Permissions = {
                  Manifest.permission.CALL_PHONE,
          };
          ActivityCompat.requestPermissions(this, Permissions, 101);
  
      }
  
      @Override
      public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
          Item item = list.get(i);
          Toast.makeText(this,item.getPhone(), Toast.LENGTH_SHORT).show();
  
          String pnum = list.get(i).getPhone();
  
          int permission = PermissionChecker.checkSelfPermission(this, Manifest.permission.CALL_PHONE);
          Intent intent = null;
          if(permission == PackageManager.PERMISSION_GRANTED){
              intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:"+pnum));
              startActivity(intent);
          }else{
              Toast.makeText(this, "권한부여가 안되었습니다.", Toast.LENGTH_SHORT).show();
              return;
          }
      }
  
      @Override
      public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
          imgcode = plist.get(i);
  //        imageView.setImageResource(imgcode);
          Toast.makeText(this, ""+imgcode, Toast.LENGTH_SHORT).show();
      }
  
      @Override
      public void onNothingSelected(AdapterView<?> adapterView) {
          // X
      }
  
  
      class ItemAdapter extends BaseAdapter{
  
          ArrayList<Item> alist;
  
          public ItemAdapter(ArrayList<Item> alist){
              this.alist = alist;
              System.out.println("ItemAdapter");
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
              return alist.get(i);
          }
  
          @Override
          public long getItemId(int i) {
              return i;
          }
  
          @Override
          public View getView(int i, View view, ViewGroup viewGroup) {
              View myview = null;
              LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
              myview = inflater.inflate(R.layout.layout, container, true);
  
              ImageView iv1 = myview.findViewById(R.id.imageView);
              TextView iv2= myview.findViewById(R.id.textView);
              TextView iv3 = myview.findViewById(R.id.textView2);
  
              iv1.setImageResource(alist.get(i).getImgid());
              System.out.println(alist.get(i).getName());
              iv2.setText(alist.get(i).getName());
              iv3.setText(alist.get(i).getPhone());
  
              return myview;
          }
      }
  
      public void addBt(View view){
          System.out.println("here");
  
          String addName = editText.getText().toString();
          String addPhone = editText2.getText().toString();
  //        int addImgid = spinner.getId();
          System.out.println(addName + " " + addPhone);
          itemAdapter = new ItemAdapter(list);
          itemAdapter.addItem(new Item(addName, addPhone, imgcode));
          itemAdapter.notifyDataSetChanged();
          listView.setAdapter(itemAdapter);
      }
  
      public void data(){
          list = new ArrayList<>();
      }
  
      private void getData() {
          plist = new ArrayList<>();
          plist.add(R.drawable.it01);
          plist.add(R.drawable.it02);
          plist.add(R.drawable.it03);
          plist.add(R.drawable.it05);
          plist.add(R.drawable.it10);
      }
  }
  
  ```

  ```java
  // Item.java
  package com.example.p440;
  
  public class Item {
      String name;
      String phone;
      int imgid;
  
      public Item() {
      }
  
      public Item(String name, String phone, int imgid) {
          this.name = name;
          this.phone = phone;
          this.imgid = imgid;
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
  
      public int getImgid() {
          return imgid;
      }
  
      public void setImgid(int imgid) {
          this.imgid = imgid;
      }
  }
  
  ```

  ![c4](https://user-images.githubusercontent.com/50862497/64326711-6c2fda00-d005-11e9-83b7-657d848d68fe.png)



