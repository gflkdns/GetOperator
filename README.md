# GetOperator
一种Android 平台 不需要获取imei imsi 无权限就能获取手机运营商的方法

先贴一下需要权限，然后通过获取 imsi 根据前缀判断运营商的方法：

```java
if (checkPermission(context, Manifest.permission.READ_PHONE_STATE)) {
                TelephonyManager mTelephonyMgr = (TelephonyManager)
                        context.getSystemService(Context.TELEPHONY_SERVICE);
                if (mTelephonyMgr != null) {
                    String imsi = mTelephonyMgr.getSimOperator();
                    if (isEmpty(imsi)) {
                        imsi = mTelephonyMgr.getSubscriberId();
                    }
                    if (!isEmpty(imsi)) {
                        if (imsi.startsWith("46000") || imsi.startsWith("46002")) {
                            return carrierName = "中国移动";
                        } else if (imsi.startsWith("46001")) {
                            return carrierName = "中国联通";
                        } else if (imsi.startsWith("46003")) {
                            return carrierName = "中国电信";
                        }
                    }
                }
            }
```

但随着隐私合规等限制，以上方法由于需要获取imsi，导致不推荐使用了，下面就来推荐一个不需要获取imsi 就能获取运营商的方法。



我们都知道 Android 会根据设备设置的不同，去加载不同的资源文件夹。最典型的，会根据系统的语言去加载不同语言的字符串资源。而 Android 也是可以根据 MCC 和 MNC 加载不同的资源的。而我们就可以利用这一点，通过创建 values-mcc460-mnc00 这种资源文件夹，然后在对应的文件夹，放置不同的运营商名称即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/511babf1118b4fee840e27f5dfbdc391.png)
附带一张国家代码对照表：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6aca7131a5ac471fb64e6f583a25846e.png)
当然其他国家也有：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b3c0728932464a459ecfca9980cad956.png)
https://zh.wikipedia.org/wiki/%E7%A7%BB%E5%8A%A8%E8%AE%BE%E5%A4%87%E7%BD%91%E7%BB%9C%E4%BB%A3%E7%A0%81

最后使用方式非常简单，只是获取下资源字符串就可以了，Android系统会自动导航到正确的运营商名称。

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TextView view = findViewById(R.id.text);
        view.setText(getString(R.string.operator));
    }
}
```
