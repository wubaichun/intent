# Intent实验

### 自定义webview验证隐式intent的使用

 

\##Intent部分：

 

### 布局代码

 

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

    android:layout_width="match_parent"
    
    android:layout_height="match_parent"
    
    android:orientation="vertical"
    
    android:gravity="center_horizontal">
    
    <EditText
    
        android:layout_width="wrap_content"
    
        android:layout_height="wrap_content"
    
        android:id="@+id/edit_url"
    
        android:hint="输入网址："/>
    
    <Button
    
        android:layout_width="wrap_content"
    
        android:layout_height="wrap_content"
    
        android:id="@+id/btn"
    
        android:text="搜索"/>



</LinearLayout>

 

### java代码

 

package edu.fjnu.intenttourials;



import androidx.appcompat.app.AppCompatActivity;



import android.content.Intent;

import android.net.Uri;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

import android.widget.EditText;



public class MainActivity extends AppCompatActivity {

    EditText editUrl;
    
    Button btn;
    
    @Override
    
    protected void onCreate(Bundle savedInstanceState) {
    
        super.onCreate(savedInstanceState);
    
        setContentView(R.layout.activity_intent);
    
        editUrl = (EditText)findViewById(R.id.edit_url);
    
        btn = (Button)findViewById(R.id.btn);
    
        btn.setOnClickListener(new View.OnClickListener() {
    
            @Override
    
            public void onClick(View view) {
    
                String url = editUrl.getText().toString();
    
                Intent intent = new Intent(Intent.ACTION_VIEW);
    
                intent.setData(Uri.parse(url));
    
                if(intent.resolveActivity(getPackageManager())!=null) {
    
                    startActivity(intent);
    
                    System.out.println("成功");
    
                }
    
                else
    
                    System.out.println("失败");
    
            }
    
        });
    
    }



}

 

\##Mybrowser部分：

 

### 实验要求利用线性布局实现

 

### 布局代码

 

<?xml version="1.0" encoding="utf-8"?>

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"

    xmlns:tools="http://schemas.android.com/tools"
    
    android:layout_width="match_parent"
    
    android:layout_height="match_parent"
    
    android:paddingBottom="16dp"
    
    android:paddingLeft="16dp"
    
    android:paddingRight="16dp"
    
    android:paddingTop="16dp">



    <WebView
    
        android:layout_width="match_parent"
    
        android:layout_height="match_parent"
    
        android:id="@+id/webView">
    
    </WebView>

</RelativeLayout>

 

### java代码

 

package edu.fjnu.mybrowser;



import androidx.appcompat.app.AppCompatActivity;



import android.content.Intent;

import android.net.Uri;

import android.os.Bundle;

import android.webkit.WebResourceRequest;

import android.webkit.WebView;

import android.webkit.WebViewClient;



import java.net.URL;



public class MainActivity extends AppCompatActivity {



    @Override
    
    protected void onCreate(Bundle savedInstanceState) {
    
        super.onCreate(savedInstanceState);
    
        setContentView(R.layout.activity_main);



        Intent intent = getIntent();



        Uri data = intent.getData();
    
        URL url = null;
    
        try{
    
            url = new URL(data.getScheme(),data.getHost(),data.getPath());



        }catch (Exception e){
    
            e.printStackTrace();
    
        }
    
        startBrowser(url);
    
    }



    private void startBrowser(URL url){
    
        WebView webView = findViewById(R.id.webView);
    
        //加载web资源
    
        webView.loadUrl(url.toString());
    
        //覆盖webview默认第三方或者系统默认有拉起打开网页得行为
    
        webView.setWebViewClient(new WebViewClient(){
    
            @Override
    
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
    
                view.loadUrl(url);
    
                return true;
    
            }
    
        });
    
    }

}

 

### 修改AndroidManifest.xml来获取访问游览器的权限

 

```
android:supportsRtl="true"
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```

 

- 本实验通过自定义Webview加载URL来验证隐式Intent的使用
- 本实验包含两个应用：

** 第一个应用：获取URL地址并启动隐式Inten的调用

** 第二个应用：自定义WebView来加载URL

 

### 实验结果截图：

![](./PHOTP\1.png)

![](./PHOTP\2.png)

![](./PHOTP\3.png)