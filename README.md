# Android-exist-SD
android 20範例

sdroot = Environment.getExternalStorageDirectory(); 取得外部SD卡
progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER); 轉圈圈

**********************************************************************
package com.example.iii_user.ming14v2;

import android.app.ProgressDialog;
import android.os.Environment;
import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.TextView;

import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;

public class MainActivity extends AppCompatActivity {
    private TextView tv;
    private File sdroot,approot;
    private ProgressDialog progressDialog;
   // private UIhandler uIhandler;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tv=(TextView)findViewById(R.id.tv);
        sdroot = Environment.getExternalStorageDirectory();
        approot = new File(sdroot,"Android/data/"+getPackageName()+"/");
        if(!approot.exists()){
            approot.mkdirs();
        }
        progressDialog = new ProgressDialog(this);
        progressDialog.setTitle("DownLoad...");
        progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
    }
    public void test1(View view){
        progressDialog.show();
        new Thread(){
            @Override
            public void run() {
                dotest1("http://tw.yahoo.com");
            }
        }.start();
    }
    private void dotest1(String urlString){
        byte[] buf =new byte[4096];
        int len;
        try {
            URL url = new URL("http://pdfmyurl.com/?url="+urlString);
           HttpURLConnection conn= (HttpURLConnection)url.openConnection();
            conn.connect();
            FileOutputStream fout = new FileOutputStream(new File(approot,"ming3.pdf"));

            InputStream in = conn.getInputStream();
            while ((len =in.read(buf)) != -1){
                fout.write(buf,0,len);
            }

            in.close();
            fout.flush();
            fout.close();
            Log.i("ming","save ok");
        } catch (Exception e) {
            Log.i("ming","ERROR");
        }
       // uIhandler.sendEmptyMessage(0);
        progressDialog.dismiss();
    }
****************************************************
//    private class UIhandler extends Handler{
//        @Override
//        public void handleMessage(Message msg) {
//            super.handleMessage(msg);
//
//            if(progressDialog.isShowing()){
//                progressDialog.dismiss();
//            }
//        }
//    }
}
