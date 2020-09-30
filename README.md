
[![Build Status](https://img.shields.io/badge/platform-android-green)](https://img.shields.io/badge/platform-android-green)
[![Build Status](https://img.shields.io/badge/type-library-blue)](https://img.shields.io/badge/type-library-blue)
[![Licence](https://img.shields.io/cocoapods/l/LivenessSDKOnline?color=red&logo=red)](https://img.shields.io/cocoapods/l/LivenessSDKOnline?color=red&logo=red)


# AndroidOfflineFaceRecSDK
## 📜 Introduction
Brought to you by FaceX.io, OfflineFaceRecSDK  can now be used to integrate on-device Face Recognition into your Android applications



## 📑 Index
* [Documentation](#-documentation)
* [Features](#-features)
* [Installation](#-installation)
  * [AAR](#using--aar--android-archive)
* [How to use](#-how-to-use)
* [Interface](#-interfaces)
   * [onFaceRecognitionListener](#onFaceRecognitionListener)
* [Supported OS & SDK Versions](#-supported-os--sdk-versions)
* [License](#-license)

## 📚 Documentation 

- Step 1: Sign up for a developer account through [FaceX User Portal](https://search.facex.io/#/register)
- Step 2: Navigate to Plans & Payments Tab & Select  [Plans](https://search.facex.io/#/home/payments/plans)
- Step 3: Scroll down to Mobile SDKs and Select Active Liveness
- Step 3: Purchase a plan matching your license requirement after adding required details 
- Step 4: Navigate to Mobile SDK Tab & Select [License History](https://search.facex.io/#/home/mobilesdk/license)
- Step 65: Download the OfflineFaceRec Android SDK from this Github [AndroidOfflineFaceRecSDK](https://github.com/teamfacex/AndroidofflineFaceRecSDK)

## 🌟 Features
- Supports Android 5.0+


## 📲 Installation
* Requires Java8 support/ gradle plugin 3+

#### Using [ AAR  (Android Archive)](https://developer.android.com/studio/projects/android-library)

An **AAR file** contains a software library used for developing Android apps. It is structurally similar to an . APK **file** (Android Package), but it allows a developer to store a reusable component that can be used across multiple different apps. To integrate AAR  into your android project:
- Purchase Active Liveness SDK license from [facex portal](https://search.facex.io).
- Download the `file.json` config file from the portal. Make sure file name is `file.json`.
- Create `assets` directory in the android project ( if not already there) and copy the downloaded `file.json` to `assets` directory.
- Download the latest `FaceRecSDK.aar` release from [here](https://github.com/teamfacex/AndroidofflineFaceRecSDK/releases/latest/download/FaceRecSDK.aar).
- Open android studio and add the liveness SDK to your android project
  - Click  `File > New > New Module`.
  - Click `Import .JAR/.AAR Package` from repo directory then click `Next`.
  -  Enter the location of the `liveness.aar` file in the cloned directory then click `Finish`
- Make sure the library is listed at the top of your `settings.gradle` file, as shown here for a library named "FaceRecSDK":
  -  `include ':app',  ':FaceRecSDK'`
 - Open the app module's `build.gradle` file and add a new line to the `dependencies` block as shown in the following snippet:
   - `dependencies { implementation project(":FaceRecSDK")  }`

## 🐒 How to use
- Make sure to get the camera & Storage permission.
```
  String[] permissionArrays = new String[]{Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE};

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            requestPermissions(permissionArrays, 11111);
        } else {

        }
        
        
 val permissionArrays = arrayOf(Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE)


        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            requestPermissions(permissionArrays, 11111)
        }
```
#### Kotlin

``` 
import com.app.Utils.FaceRecongintion;
import com.app.Utils.onFaceRecognitionListener;

class MainActivity : AppCompatActivity(),onFaceRecognitionListener{
   
    var faceRecongintion: FaceRecongintion? = null

    var vectors: List<String>? = null

    var register: Button? = null
    var search: Button? = null
 
  override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_test)

        faceRecongintion = FaceRecongintion(this, R.id.main_view, this)

        vectors = ArrayList<String>()
        register?.setOnClickListener(View.OnClickListener { faceRecongintion!!.FaceRegistrtion() })

        search?.setOnClickListener(View.OnClickListener { faceRecongintion!!.FaceSearch() })

        val permissionArrays = arrayOf(Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE)


        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            requestPermissions(permissionArrays, 11111)
        }
    }



   override fun onRegisterSuccess(p0: String?, p1: Bitmap?) {
        TODO("Not yet implemented")
        // p0-- encrypted vector
    }
    
     override fun onRegisterError(p0: String?) {
        TODO("Not yet implemented")
        //p0:Register error
    }
    
      override fun onSearchSuccess(p0: Int, p1: String?, p2: Bitmap?) {
        TODO("Not yet implemented")
        
        //p0:position of matching user
        //p1:distance from two users
    }

    override fun onSearchError(p0: String?) {
        TODO("Not yet implemented")
        //p0 --Search Error Message
    }
  
  
```

#### Java

```
import io.facex.liveness.Liveness  
import io.facex.liveness.LivenessListener

public class Mainactivity extends AppCompatActivity implements onFaceRecognitionListener{
  private FaceRecongintion  faceRecongintion;
   Button register, search;
   
   
  
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
      faceRecongintion = new FaceRecongintion(this, R.id.main_view, this);
      
      
       register.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                faceRecongintion.FaceRegistrtion();
            }
        });
        
        
         faceRecongintion.addVectors(vectors); // for input images for search 
         
        

        search.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
            
            // please add vector before calling search
                faceRecongintion.FaceSearch();
            }
        });

}

 
    @Override
    public void onRegisterSuccess(String s, Bitmap bitmap) {

        // s -- encrypted vector

    }

    @Override
    public void onRegisterError(String s) {

        Log.e("onError", s);
        
         // s -- error msg 
    }

    @Override
    public void onSearchSuccess(int i, String s, Bitmap bitmap) {

        // i -- matched postion in provided input array
        // s -- distance with input arry and capture/selcted user image

    }

    @Override
    public void onSearchError(String s) {

        Log.e("onSearchEror", s);
        
        // s -- error msg 

    }
}
```

##### layout file for fragment

```
faceRecongintion.FaceRegistrtion(),  faceRecongintion.FaceSearch(); creates a fragment, so it needs a view to bind the fragment.
Pass in the id of the fragment container using the liveness constructor.
eg: fragment_holder


<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/main_view"
    android:background="@color/black"
    tools:context=".MainActivity">

    .............................
    
</androidx.constraintlayout.widget.ConstraintLayout>
```

## Interfaces

#### onFaceRecognitionListener 

```
Kotlin

ooverride fun onRegisterSuccess(p0: String?, p1: Bitmap?) {
        TODO("Not yet implemented")
        // p0-- encrypted vector
    }
    
     override fun onRegisterError(p0: String?) {
        TODO("Not yet implemented")
        //p0:Register error
    }
    
      override fun onSearchSuccess(p0: Int, p1: String?, p2: Bitmap?) {
        TODO("Not yet implemented")
        
        //p0:position of matching user
        //p1:distance from two users
    }

    override fun onSearchError(p0: String?) {
        TODO("Not yet implemented")
        //p0 --Search Error Message
    }
```



## 📋 Supported OS & SDK Versions
* Android 5.0+
* Java 8

## 👮🏻 License


