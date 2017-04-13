# blog

#  ButterKnife简介

# ButterKnife是通过注解的方式进行Android控制和方法的绑定，对于控件而言，无需通过findViewById来进行实例化，而是通过注解。对于一些回调方法，比如按钮的点击事件，无需通过绑定监听器，也可以通过注解的方式来完成。通过注解的方式对控制和回调方法进行绑定，可以有效的减少重复性的工作，简化代码，而将精力用于功能的实现。最新版本是8.4.0 ，以下内容都是都是基于8.
。
ButterKnife Github主页：https：//github.com/JakeWharton/butterknife

在AndroidStudio中如何使用ButterKnife？

Github上ButterKnife的主页有详细介绍。
首先在项目的build.gradle中：

buildscript{
    repositories{
        mavenCentral()
    }
    dependencies{
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
    }
}
然后在module的build.gradle文件中：

apply plugin: 'android-apt'
dependencies{
    compile 'com.jakewharton:butterknife:8.4.0' 
    apt 'com.jakewharton:butterknife-compiler:8.4.0'
}
要按照上面的教程去配置毕业文件，要不然代码即使编译通过，但是在运行时会报空指针错误的。

布局xml

下面通过一个简单的登录页面来展示一下如何在活动和片段中使用ButterKinfe。页面布局login.xml代码如下：

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"        
    xmlns:tools="http://schemas.android.com/tools"    
    android:layout_width="match_parent"    
    android:layout_height="match_parent"    
    android:orientation="vertical"        
    tools:context="com.example.administrator.butterknifetest.MainActivity">     
    <EditText        
        android:id="@+id/et_username"        
        android:layout_width="match_parent"         
        android:layout_height="wrap_content" />    
    <EditText        
        android:id="@+id/et_pwd"        
        android:layout_width="match_parent"         
        android:layout_height="wrap_content" />    
    <Button        
        android:id="@+id/btn_login"        
        android:layout_width="match_parent"         
        android:layout_height="wrap_content"        
        android:text="login" />    
    <Button        
        android:id="@+id/btn_cancel"        
        android:layout_width="match_parent"         
        android:layout_height="wrap_content"        
        android:text="cancel" />
</LinearLayout>
4.在活动中控制和回调方法的绑定

在活动中通过以下方式进行控制的绑定，相关的解释在代码注释中。

package com.example.administrator.butterknifetest;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import butterknife.BindView;
import butterknife.ButterKnife;
import butterknife.OnClick;

public class MainActivity extends AppCompatActivity {    
    //通过BindView标注对控件进行绑定，相当于findViewById    
    @BindView(R.id.et_username)    
    EditText mUsernameEt;    
    @BindView(R.id.et_pwd)    
    EditText mPwdEt;    
    @BindView(R.id.btn_login)    
    Button mLoginBtn;    
    @BindView(R.id.btn_cancel)    
    Button mCancelBtn;    

    @Override    
    protected void onCreate(Bundle savedInstanceState) {            
        super.onCreate(savedInstanceState);        
        setContentView(R.layout.activity_main);        
        //绑定目标Activity        
        ButterKnife.bind(MainActivity.this);        
        //通过BindView标注了的控件可以直接使用            
        mUsernameEt.setText("user name");
    }

    /**     
    * 绑定Button的回调方法，注解要与Button的回调方法名onClick相同,但是注解中OnClick第一个字母大写     
    * Button绑定的方法名可以自己任意取，比如与登录相关的doLogin()     
    */    
    @OnClick({R.id.btn_login,R.id.btn_cancel})
    public  void doLogin(View v){
        switch (v.getId()){
            case R.id.btn_login:                
                Toast.makeText(getApplicationContext(),"login click",Toast.LENGTH_SHORT).show();
                break;
            case R.id.btn_cancel:
                Toast.makeText(getApplicationContext(),"cancel click",Toast.LENGTH_SHORT).show();;
                break;
            default:
                break;
        }
    }
}
在片段中如何绑定控件和回调方法

在片段中的onCreateView（）方法中对控件进行绑定

View view = inflater.inflate(R.layout.fragment_layout,container,false);
ButterKnife.bind(view);
回调方法的绑定与活动中的相同。

总结

通过ButterKnife注释的方式对控制进行绑定，减少重复的findViewById方法的使用，特别是对于控制比较多的页面，更能体现出注解的优势。对于回调方法的注解，个人感觉，如果回调方法只有一个并且是大家熟知的，像点击事件onClick方法可以通过ButterKnife进行注解。如果是有的控件有多个回调方法，像SeekBar，进行回调方法的注解反而看起来不直观。另外还可以通过Butterknife对ListView， GridView的适配器进行注解，有时间我再加上。
