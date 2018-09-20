# Android 9 for enterprise apps practice


**Development Environment**

+ windows 7

+ Android Studio 3.0.1

+ Android api 28

+ Virtul Devices Pixel 2


**Built Environment**

+ �]�mDeviceAdmin

    
�����ݭn�b���Τ��]�mDeviceAdmin�H�K������oComponentName�C


1. �s�W�@��class���~��DeviceAdminReceiverclass
    
    
�����إߦbpackage�U
![](./picture/p1.png)
```
public class MyAdmin extends DeviceAdminReceiver{}
```
2. �bmanifest�����UDeviceAdminReceiver

    
�bmanifest����<application></application>�����[�J���e�G

```

<receiver
            android:name="com.example.user.t0828.MyAdmin"
            android:label="@string/sample_device_admin"
            android:description="@string/sample_device_admin_description"
            android:permission="android.permission.BIND_DEVICE_ADMIN">
            <meta-data
                android:name="android.app.device_admin"
                android:resource="@xml/my_admin" />
            <intent-filter>
                <action android:name="android.app.action.DEVICE_ADMIN_ENABLED" />
            </intent-filter>
        </receiver>

```

<receiver>�U��name���ۤv�� package�W + �~��DeviceAdminReceiver�����O�W�C

alt+enter�blabel�U�s�Wstring�귽�C

�b@string���Ushift+����s�Wdescription�귽�C


�bmeta-data�U��resource�s�Wxml��XML���e�G

```

<device-admin xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-policies>
        <limit-password />
        <watch-login />
        <reset-password />
        <force-lock />
        <wipe-data />
    </uses-policies>
</device-admin>

```

�ԲӪ��v�����e�i�H�bandroid��󤤬d�\�A���U�ӫK����}�l�����γ]�mDeviceOwner�C


* �]�mDeviceOwner

    
�@�B�i�Jadb���O�C�Ҧ�
    
    
�����n�����ۤv��platform-tools��Ƨ��A�̭��|����adb.exe��
�i�H�bandroid studio�� Tools -> Android -> SDK Manager ���� Android SDK Location�d�ߨ�SDK���|�A�bSDK�U�i�H���platform-tools��Ƨ�
�p�G�S���i��O�|���w��platform-tools�C
![](./picture/p3.png)
���U�Ӷ}��cmd�ϥ�cd���O�i�J��platform-tools��Ƨ���m�A��F��m��K����z�Lcmd�i�J Android �t�Ϋ��O�C�Ҧ��i�H���z�L�H�U���O�j�M�w�s�����˸m�G
    
```
    
adb devices
    
```
    
��ܳs��������]�ƹq���u���s���@�x�˸m�����p�i�H������J���O�G

    
```
    
adb shell
    
```
    
�s���������P�@��˸m�i�H��J���O�G
    
```
    
adb -d shell    //�s���@��˸m
    
```
    
```
    
adb -e shell    //�s��������
    
```
    
��J���\�|�X�{$�r���K��}�l�����ε{���]�mDeviceOwner�C
    
    
�]�m���O�G
    
```
    
dpm set-device-owner com.example.user.t0828/.MyAdmin
   
```
    
com.example.user.t0828�n�אּapp��package�W��.MyAdmin�h�אּapp�� extends DeviceAdminReceiver �� class�C�]�w������K����}�l����dpm���O�C
    
�p�G�n���}shell�i�H������J```exit```
    

**The Simplest Sample**


```
 
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DevicePolicyManager mDevicePolicyManager = (DevicePolicyManager) getSystemService(Context.DEVICE_POLICY_SERVICE);
        ComponentName mDPM = new ComponentName(this, MyAdmin.class);
        String[] packages = {this.getPackageName()};

        if (mDevicePolicyManager.isDeviceOwnerApp(this.getPackageName())) {
            mDevicePolicyManager.setLockTaskPackages(mDPM, packages);
            DevicePolicyManager.LOCK_TASK_FEATURE_OVERVIEW);
            startLockTask();
            mDevicePolicyManager.setLockTaskFeatures(mDPM, DevicePolicyManager.LOCK_TASK_FEATURE_HOME);

        } else {
            Toast.makeText(getApplicationContext(),"Not owner", Toast.LENGTH_LONG).show();
        }

```


    