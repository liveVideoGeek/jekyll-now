---
layout: post
title:  Porting NanoHTTPD on Android
---

# Steps:

## 1. Build nanohttpd-core jar:
* git clone the nanohttpd master source from [github](https://github.com/NanoHttpd/nanohttpd), version 2.3.2@2018/01/23
* build it with maven 3 and JDK 1.8+ in a linux box (if u want to build it in MS windows, pls use cmdline and run 'mvn package -Dmaven.test.skip=true', or else u will fail the UTs and no jar generated) 

## 2. Import nanohttpd-core jar to an Android Studio project 
* launch Android Studio (I tested with MS Windows) and create a empty project
* import the nanohttpd-core to the project:
  - manually copy the nanohttpd-version-SNAPSHOT.jar to the project's libs directory: project top directory -> app -> libs  
  - from the AS project view, locate the jar and click the mouse right button, select 'add as library'
  
## 3. Coding a simple http server
* create a new class in the AS project, saying named SimpleHttpd
* construct the SimpleHttpd like the followings:
```java
import java.io.IOException;
import org.nanohttpd.protocols.http.IHTTPSession;
import org.nanohttpd.protocols.http.NanoHTTPD;
import org.nanohttpd.protocols.http.request.Method;
import org.nanohttpd.protocols.http.response.Response;
import org.nanohttpd.util.ServerRunner;


/**
 * Created by He on 2018/1/23.
 */

public class SimpleHttpd extends NanoHTTPD {
    private final static int PORT = 8888;

    public SimpleHttpd() throws IOException {
        super(PORT);
        start();
        System.out.println( "\nRunning! Point your browers to http://localhost:8080/ \n" );
    }

    @Override
    public Response serve(IHTTPSession session) {
        String msg = "<html><body><h1>Hello server</h1>\n";
        System.out.println("remote ip: " + session.getRemoteIpAddress());
        msg += "<p>We serve request from " + session.getUri() + " !</p>";
        return Response.newFixedLengthResponse( msg + "</body></html>\n" );
    }
}
```
## 4. Launch the server in Android app main class
* MainActivity.java as the followings:
```java
import java.io.IOException;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

public class MainActivity extends AppCompatActivity {
    private SimpleHttpd server;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    public void onResume() {
        super.onResume();
        try {
            server = new SimpleHttpd();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void onPause() {
        super.onPause();
        if(server != null) {
            server = null;
        }
    }
}
```
* modify the AndroidManifest.xml as followings:
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="cn.roobot.simplehttpd">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>
    <uses-permission android:name="android.permission.INTERNET"></uses-permission>
```
## 5. test
* build apk and launch the app from the android device
* run 'curl -G http://ip:8888/hello_world' to see the output 

## 6. References: 
1. [nanohttpd github page](https://github.com/NanoHttpd/nanohttpd)
2. [yet another guide to port nanohttpd to android](http://programminglife.io/android-http-server-with-nanohttpd/)
