# WebView
## 在AndroidManifest文件下 ，声明应用程序可以访问网络的权限。

    <uses-permission android:name="android.permission.INTERNET"/>
    
## 创建一个 web_view.xml
     
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />
    </LinearLayout>
    
## 创建WebViewActivity(用于启动的Activity)，代码如下：

    public class WebViewActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.web_view);
        WebView myWebView = (WebView) findViewById(R.id.webview);
        String url=getIntent().getExtras().getString("url");
        myWebView.getSettings().setJavaScriptEnabled(true);
        myWebView.loadUrl(url);
     }}
     
 ## 在AndroidManifest文件下，设置被启动Activity的 internt-filter：
      <activity android:name=".WebViewActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.BROWSABLE" />

                <data android:scheme="http"/> //被启动Activity的data部分的scheme只能是 http 开头

            </intent-filter>
        </activity>
        
## 修改activity_main.xml

    <EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:id="@+id/editText"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintBottom_toTopOf="@id/bn" />
    <Button
    android:id="@+id/bn"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="打开浏览器"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent" />
    
## 修改MainActivity 
  
        public class MainActivity extends AppCompatActivity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        Button bn = findViewById(R.id.bn);
        final EditText editText =findViewById(R.id.editText);
        
        //为bn按钮添加一个监听器
        bn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //创建Intent
                Intent intent = new Intent();
                //获取editText中输入的数据
                String data=editText.getText().toString();
                //根据指定字符串解析出Uri对象
                Uri uri = Uri.parse(data);
                //为Intent设置Action属性
                intent.setAction(Intent.ACTION_VIEW);
                //为Intent设置Category属性
                intent.addCategory(Intent.CATEGORY_BROWSABLE);
                //设置Data属性
                intent.setData(uri);
                //启动Activity
                startActivity(intent);
            }
        });
    }}


