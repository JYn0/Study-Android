# Android

09.11



## 음악, 동영상 재생

* (P641)

  ```java
  package com.example.p641;
  
  import androidx.appcompat.app.AppCompatActivity;
  
  import android.media.MediaPlayer;
  import android.net.Uri;
  import android.os.Bundle;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.MediaController;
  import android.widget.VideoView;
  
  import java.io.IOException;
  
  public class MainActivity extends AppCompatActivity {
      MediaPlayer mediaPlayer;
      VideoView videoView;
      int position = 0;
      public static final String VIDEO_URL = "https://sites.google.com/site/ubiaccessmobile/sample_video.mp4";
      
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
  //        playAudio();
          videoView = findViewById(R.id.videoView);
  
          MediaController mc = new MediaController(this);
          videoView.setMediaController(mc);
      }
  
      @Override
      protected void onDestroy() {
          super.onDestroy();
          if(mediaPlayer != null){
              releaseMedia(); // back button 누르면 바로 finish
          }
      }
  
      public void releaseMedia(){
          if(mediaPlayer != null){
              mediaPlayer.release();
          }
      }
  
      public void playAudio(){
          releaseMedia();
          mediaPlayer = MediaPlayer.create(this, R.raw.kalimba);
          mediaPlayer.start();
      }
  
      public void play(View view){
          playAudio();
      }
      public void stop(View view){
          mediaPlayer.stop();
      }
      public void pause(View view){ }
      public void replay(View view){
          Uri uri = Uri.parse("android.resource://"+getPackageName()+"/"+R.raw.sample_video);
          videoView.setVideoURI(uri);
          videoView.requestFocus();
          videoView.start();
      }
  }
  
  ```

  ![e1](https://user-images.githubusercontent.com/50862497/64679593-00db8180-d4b7-11e9-9c6b-4d49585d7eae.png)





## GPS로 나의 위치 확인하기

* (P673)

  ```java
  package com.example.p673;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.content.Context;
  import android.content.Intent;
  import android.content.pm.PackageManager;
  import android.location.Location;
  import android.location.LocationListener;
  import android.location.LocationManager;
  import android.net.Uri;
  import android.os.Bundle;
  import android.widget.TextView;
  import android.widget.Toast;
  
  public class MainActivity extends AppCompatActivity {
      TextView textView;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
  
          setContentView(R.layout.activity_main);
          textView = findViewById(R.id.textView);
  
          String[] Permissions = {
                  Manifest.permission.ACCESS_FINE_LOCATION,
          };
          ActivityCompat.requestPermissions(this, Permissions, 101);
  
          int permission = PermissionChecker.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION);
          if(permission == PackageManager.PERMISSION_GRANTED){
              Toast.makeText(this, "Accept", Toast.LENGTH_SHORT).show();
              startLocationService();
              return;
          }else{
              Toast.makeText(this, "권한부여가 안되었습니다.", Toast.LENGTH_SHORT).show();
              return;
          }
      }
  
      public void startLocationService() {
          LocationManager manager =
                  (LocationManager) getSystemService(Context.LOCATION_SERVICE);
  
          try {
  //            Location location = manager.getLastKnownLocation(LocationManager.GPS_PROVIDER);
              // GPS : Emulator, NETWORK : real
              Location location = manager.getLastKnownLocation(LocationManager.NETWORK_PROVIDER);
  
              if (location != null) {
                  double latitude = location.getLatitude();
                  double longitude = location.getLongitude();
                  String message = "" + latitude + ", " + longitude;
                  textView.setText(message);
              }
  
              GPSListener gpsListener = new GPSListener();
              long minTime = 10000;
              float minDistance = 0;
  
              manager.requestLocationUpdates(
                      LocationManager.NETWORK_PROVIDER, minTime, minDistance, gpsListener);
  
              Toast.makeText(getApplicationContext(), "", Toast.LENGTH_SHORT).show();
  
          } catch(SecurityException e) {
              e.printStackTrace();
          }
      }
  
      class GPSListener implements LocationListener {
          public void onLocationChanged(Location location) {
              Double latitude = location.getLatitude();
              Double longitude = location.getLongitude();
  
              String message = ""+ latitude + ", "+ longitude;
              textView.setText(message);
          }
  
          public void onProviderDisabled(String provider) { }
  
          public void onProviderEnabled(String provider) { }
  
          public void onStatusChanged(String provider, int status, Bundle extras) { }
      }
  }
  
  ```

  ```xml
      <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
  ```

  ![e2](https://user-images.githubusercontent.com/50862497/64679636-151f7e80-d4b7-11e9-80a9-ac6741bc29aa.png)





* (P674)

  ```java
  package com.example.p674;
  
  import androidx.fragment.app.FragmentActivity;
  
  import android.os.Bundle;
  
  import com.google.android.gms.maps.CameraUpdateFactory;
  import com.google.android.gms.maps.GoogleMap;
  import com.google.android.gms.maps.OnMapReadyCallback;
  import com.google.android.gms.maps.SupportMapFragment;
  import com.google.android.gms.maps.model.LatLng;
  import com.google.android.gms.maps.model.MarkerOptions;
  
  public class MapsActivity extends FragmentActivity implements OnMapReadyCallback {
  
      private GoogleMap mMap;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_maps);
          // Obtain the SupportMapFragment and get notified when the map is ready to be used.
          SupportMapFragment mapFragment = (SupportMapFragment) getSupportFragmentManager()
                  .findFragmentById(R.id.map);
          mapFragment.getMapAsync(this);
      }
  
  
      /**
       * Manipulates the map once available.
       * This callback is triggered when the map is ready to be used.
       * This is where we can add markers or lines, add listeners or move the camera. In this case,
       * we just add a marker near Sydney, Australia.
       * If Google Play services is not installed on the device, the user will be prompted to install
       * it inside the SupportMapFragment. This method will only be triggered once the user has
       * installed Google Play services and returned to the app.
       */
      @Override
      public void onMapReady(GoogleMap googleMap) {
          mMap = googleMap;
  
          // Add a marker in Sydney and move the camera
          LatLng sydney = new LatLng(37.3002037, 127.009515);
          mMap.addMarker(new MarkerOptions().position(sydney).title("Marker in Sydney"));
  //        mMap.moveCamera(CameraUpdateFactory.newLatLng(sydney));
          mMap.animateCamera(CameraUpdateFactory.newLatLngZoom(sydney, 15));
      }
  
  }
  
  ```

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.p674">
  
      <!--
           The ACCESS_COARSE/FINE_LOCATION permissions are not required to use
           Google Maps Android API v2, but you must specify either coarse or fine
           location permissions for the 'MyLocation' functionality.
      -->
      <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
  
      <application
          android:allowBackup="true"
          android:icon="@mipmap/ic_launcher"
          android:label="@string/app_name"
          android:roundIcon="@mipmap/ic_launcher_round"
          android:supportsRtl="true"
          android:theme="@style/AppTheme">
  
          <!--
               The API key for Google Maps-based APIs is defined as a string resource.
               (See the file "res/values/google_maps_api.xml").
               Note that the API key is linked to the encryption key used to sign the APK.
               You need a different API key for each encryption key, including the release key that is used to
               sign the APK for publishing.
               You can define the keys for the debug and release targets in src/debug/ and src/release/.
          -->
          <meta-data
              android:name="com.google.android.geo.API_KEY"
              android:value="@string/google_maps_key" />
  
          <activity
              android:name=".MapsActivity"
              android:label="@string/title_activity_maps">
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
  
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
      </application>
  
  </manifest>
  ```

  ```
  apply plugin: 'com.android.application'
  
  android {
      compileSdkVersion 29
      buildToolsVersion "29.0.2"
      defaultConfig {
          applicationId "com.example.p674"
          minSdkVersion 22
          targetSdkVersion 29
          versionCode 1
          versionName "1.0"
          testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
      }
      buildTypes {
          release {
              minifyEnabled false
              proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
          }
      }
  }
  
  dependencies {
      implementation fileTree(dir: 'libs', include: ['*.jar'])
      implementation 'androidx.appcompat:appcompat:1.0.2'
      implementation 'com.google.android.gms:play-services-maps:16.1.0'
      testImplementation 'junit:junit:4.12'
      androidTestImplementation 'androidx.test:runner:1.1.1'
      androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.1'
  }
  
  ```

  ![e5](https://user-images.githubusercontent.com/50862497/64680155-36cd3580-d4b8-11e9-9c0e-b9f66646a00e.png)

  



* (P676)

  ```java
  package com.example.p675;
  
  import androidx.appcompat.app.AppCompatActivity;
  import androidx.core.app.ActivityCompat;
  import androidx.core.content.PermissionChecker;
  
  import android.Manifest;
  import android.content.Context;
  import android.content.Intent;
  import android.content.pm.PackageManager;
  import android.location.Location;
  import android.location.LocationListener;
  import android.location.LocationManager;
  import android.os.Bundle;
  import android.util.Log;
  import android.widget.TextView;
  import android.widget.Toast;
  
  import com.google.android.gms.maps.CameraUpdateFactory;
  import com.google.android.gms.maps.GoogleMap;
  import com.google.android.gms.maps.MapsInitializer;
  import com.google.android.gms.maps.OnMapReadyCallback;
  import com.google.android.gms.maps.SupportMapFragment;
  import com.google.android.gms.maps.model.BitmapDescriptorFactory;
  import com.google.android.gms.maps.model.LatLng;
  import com.google.android.gms.maps.model.MarkerOptions;
  
  import org.w3c.dom.Text;
  
  public class MainActivity extends AppCompatActivity {
      SupportMapFragment mapFragment;
      GoogleMap map;
      TextView textView;
      MarkerOptions myLocationMarker;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          textView = findViewById(R.id.textView);
  
          mapFragment = (SupportMapFragment) getSupportFragmentManager().findFragmentById(R.id.map);
          mapFragment.getMapAsync(new OnMapReadyCallback() {
              @Override
              public void onMapReady(GoogleMap googleMap) {
                  Log.d("Map", "지도 준비됨.");
                  map = googleMap;
  //                map.setMaxZoomPreference(15);
                  map.setMyLocationEnabled(true);
                  startLocationService();
              }
          });
  
          try {
              MapsInitializer.initialize(this);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          String[] Permissions = {
                  Manifest.permission.ACCESS_FINE_LOCATION,
          };
          ActivityCompat.requestPermissions(this, Permissions, 101);
  
  
          int permission = PermissionChecker.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION);
          if(permission == PackageManager.PERMISSION_GRANTED){
              Toast.makeText(this, "Accept", Toast.LENGTH_SHORT).show();
              return;
          }else{
              Toast.makeText(this, "권한부여가 안되었습니다.", Toast.LENGTH_SHORT).show();
              return;
          }
      }
  
  
  
      public void startLocationService() {
          LocationManager manager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);
  
          try {
              Location location = manager.getLastKnownLocation(LocationManager.GPS_PROVIDER);
              if (location != null) {
                  double latitude = location.getLatitude();
                  double longitude = location.getLongitude();
                  String message = "" + latitude + "" + longitude;
                  showCurrentLocation(latitude, longitude);
                  textView.setText(message);
              }
  
              GPSListener gpsListener = new GPSListener();
              long minTime = 10000;
              float minDistance = 0;
  
              manager.requestLocationUpdates(
                      LocationManager.NETWORK_PROVIDER, minTime, minDistance, gpsListener);
  
              Toast.makeText(getApplicationContext(), "", Toast.LENGTH_SHORT).show();
  
          } catch(SecurityException e) {
              e.printStackTrace();
          }
      }
  
      class GPSListener implements LocationListener {
          public void onLocationChanged(Location location) {
              Double latitude = location.getLatitude();
              Double longitude = location.getLongitude();
  //            showCurrentLocation(latitude, longitude);
              String message = ""+ latitude + ""+ longitude;
              showCurrentLocation(latitude, longitude);
              textView.setText(message);
          }
  
          public void onProviderDisabled(String provider) { }
  
          public void onProviderEnabled(String provider) { }
  
          public void onStatusChanged(String provider, int status, Bundle extras) { }
      }
  
      private void showCurrentLocation(Double latitude, Double longitude) {
          LatLng curPoint = new LatLng(latitude, longitude);
          map.animateCamera(CameraUpdateFactory.newLatLngZoom(curPoint, 15));
  
          showMyLocationMarker(curPoint);
      }
  
      private void showMyLocationMarker(LatLng curPoint) {
          if (myLocationMarker == null) {
              myLocationMarker = new MarkerOptions();
              myLocationMarker.position(curPoint);
              myLocationMarker.title("● 내 위치\n");
              myLocationMarker.snippet("● GPS로 확인한 위치");
              myLocationMarker.icon(BitmapDescriptorFactory.fromResource(R.drawable.mylocation));
              map.addMarker(myLocationMarker);
          } else {
              myLocationMarker.position(curPoint);
          }
      }
  }
  
  ```

  ```xml-dtd
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.p675">
      <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
      <application
          android:allowBackup="true"
          android:icon="@mipmap/ic_launcher"
          android:label="@string/app_name"
          android:roundIcon="@mipmap/ic_launcher_round"
          android:supportsRtl="true"
          android:theme="@style/AppTheme">
  
          <meta-data
              android:name="com.google.android.geo.API_KEY"
              android:value="AIza**KEY**lXYkPQ0"/>
  
          <activity android:name=".MainActivity">
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
  
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
      </application>
  
  </manifest>
  ```

  ![e12](https://user-images.githubusercontent.com/50862497/64681549-e3a8b200-d4ba-11e9-89d5-deeae4fb0cdb.png)





* (P676)

  * 지도를 화면에 출력하고 현재 위치를 찍는다.

    서버에 현재 위치에서 오차범위 .00X 받아서 지도를 계속 이동 시킨다.

    Network를 통해 서버에 요청 -> Async Task

  ```
  
  ```

  