需要先准备几个工具，

 - 开发工具Android Studio或者IntelliJ IDEA。
[下载地址](http://www.androiddevtools.cn/#android-studio)  ，网站中也包括很多其他工具的下载。
 - 反编译工具
 [下载地址](http://www.androiddevtools.cn/#apk-decompile-tools) 。我没有使用这里的反编译工具，而是使用的[Apk改之理](https://pan.baidu.com/s/1pL133BH)。

用Android Studio编写一个简单的小项目下一章使用。
![功能样式](https://img-blog.csdn.net/2018090611235978?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDEzMjQ5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private EditText mUsername;
    private EditText mPwd;
    private static final String REAL_USERNAME = "282921012";
    private static final String REAL_PWD = "123456";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mUsername = findViewById(R.id.etUsername);
        mPwd = findViewById(R.id.etPwd);
        findViewById(R.id.btnLogin).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        checkLogin();
    }

    private void checkLogin() {
        String username = mUsername.getText().toString();
        String pwd = mPwd.getText().toString();
        if (TextUtils.equals(REAL_USERNAME, username) && TextUtils.equals(REAL_PWD, pwd)) {
            Toast.makeText(this, R.string.login_success, Toast.LENGTH_SHORT).show();
        } else {
            Toast.makeText(this, R.string.login_failed, Toast.LENGTH_SHORT).show();
        }
    }
}
```

实现的功能也很容易，如果输入的账号是282921012，输入的密码是123456，那么提示登录成功，否则提示登录失败。