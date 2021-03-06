
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
  * [Adding Dependecies](#adding-dependencies)
* [How to use](#-how-to-use)
   * [Permissions Required](#permissions)
   * [Methods](#method)   
   * [Interpreting the Output](#interpreting-euclidean-distance) 
   * [Implementation](#implementations)  
* [Interface](#-interfaces)
   * [onFaceRecognitionListener](#onFaceRecognitionListener)
* [Supported OS & SDK Versions](#-supported-os--sdk-versions)
* [License](#-license)

## 📚 Documentation 

- Step 1: Contact team@facex.io for an offline license.
- Step 2: Download the OfflineFaceRec Android SDK from this Github [AndroidOfflineFaceRecSDK](https://github.com/teamfacex/AndroidofflineFaceRecSDK)

## 🌟 Features
- Supports Android 5.0+


## 📲 Installation
* Requires Java8 support/ gradle plugin 3+

#### Using [ AAR  (Android Archive)](https://developer.android.com/studio/projects/android-library)

An **AAR file** contains a software library used for developing Android apps. It is structurally similar to an . APK **file** (Android Package), but it allows a developer to store a reusable component that can be used across multiple different apps. To integrate AAR  into your android project:
- Purchase Offline Recognition SDK license from [FaceX](mailto:team@facex.io).

- Download the latest `FaceRecSDK.aar` release from [here](https://github.com/teamfacex/AndroidofflineFaceRecSDK/releases/latest/download/FaceRecSDK.aar).
- Open android studio and add the Recognition SDK to your android project
  - Click  `File > New > New Module`.
  - Click `Import .JAR/.AAR Package` from repo directory then click `Next`.
  -  Enter the location of the `FaceRecSDK.aar` file in the cloned directory then click `Finish`
- Make sure the library is listed at the top of your `settings.gradle` file, as shown here for a library named "FaceRecSDK":
  -  `include ':app',  ':FaceRecSDK'`
 - Open the app module's `build.gradle` file and add a new line to the `dependencies` block as shown in the following snippet:
   - `dependencies { implementation project(":FaceRecSDK")  }`
   
   #### Adding Dependencies


```  
  
* Add Following dependencies in app level build.gradle
    implementation 'com.google.firebase:firebase-ml-vision-face-model:20.0.1'
```

  


## 🐒 How to use
### Permissions
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
### Method
The FaceRecognition SDK provides three main functions
- [Registration Method](#registration-method)- Generation of a Vector from Input (Camera/Gallery)
- [Update Method](#update-method)- Adding of a List<string> of Vectors on SDK 
- [Search Method](#output-of-search-method)- Comparing an Input to Registered String.

#### Registeration Method
The Output of the Registeration method:
- Vector string
- Cropped Image of the Registered person

The Vector string has to be saved in a list<string> to be used as input for Update Method.

#### Update Method
Please note regarding Update Method:
- This method replaces all existing Vectors in SDK with your list
- Everytime the app opens you need to load data using Update Method
 
#### Output of Search Method
After you register vectors on an SDK using Registeration Method or update a string of vectors on your device using update method, you can call the search method.

The output of the Search method is:
 - Position of Matching person in your List<string>
 - Cropped Search image used
 - Euclidean distance between the Search image and registered Image

### Interpreting Euclidean Distance
 The Euclidean distance represents the level of closeness/ Match between the images with a threshold of 0.8 set as the limit
  - If distance is less than 0.8, the images are of same person, but different images
  - If distance is more than 0.8, the images are not a match.
  - If the distance is 0, the images are a perfect match/ the same image was found in the database.
  
 The SDK provides the match with the least distance below the 0.8 distance threshold.
 
### Implementations 
There are two implementations provided:
   * [Using Kotlin](#kotlin)
   * [Using Java](#java)

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
        
        
        //enable camera and storage peermissions
        
         val permissionArrays = arrayOf(Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE)


        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            requestPermissions(permissionArrays, 11111)
        }
        
        
        //Initialize Face Recognition SDK

        faceRecongintion = FaceRecongintion(this, R.id.main_view, this)

 //Calling Registration Method
 // Adding Image input (via Camera/gallery) for Vector generation/ Registering Vectors    
       faceRecongintion!!.FaceRegistrtion()
       
 // Calling Update Method
 // for input images for search & To update input for Search 
 // Vectors to be provided as a List<string>
    
    faceRecongintion.addVectors(vectors); 
    
    
 //Calling Search Method
 // please Ensure to Call Update Method/ Registeration Method for your required database before calling this mehtod 
 //accepts input from camera or gallery
 
       faceRecongintion!!.FaceSearch()

       
    }



   override fun onRegisterSuccess(p0: String?, p1: Bitmap?) {
      
        // p0-- vector
        
          // s -- Registered vector output from SDK and the registered image to be saved by the developer
    }
    
     override fun onRegisterError(p0: String?) {
        
        //p0:Register error
        
    }
    
      override fun onSearchSuccess(p0: Int, p1: String?, p2: Bitmap?) {
        //p0:position of matching user
        //p1:distance from two users
    }

    override fun onSearchError(p0: String?) {
       
        //p0 --Search Error Message
    }
  
  
```

#### Java

```
import com.app.Utils.FaceRecongintion  
import com.app.Utils.onFaceRecognitionListener

public class Mainactivity extends AppCompatActivity implements onFaceRecognitionListener{
  private FaceRecongintion  faceRecongintion;
   Button register, search;
  
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

//Initialize Face Recognition SDK
       faceRecongintion = new FaceRecongintion(this, R.id.main_view, this);

//Calling Registration Method
// Adding Image input (via Camera/gallery) for Vector generation/ Registering Vectors         
         faceRecongintion.FaceRegistrtion();  

//Calling Update Method
// for input images for search & To update input for Search 
//Vectors to be provided as a List<string>

         faceRecongintion.addVectors(vectors); 

//Calling Search Method
// please Ensure to Call Update Method/ Registeration Method for your required database before calling this mehtod 
//accepts input from camera or gallery
            
           faceRecongintion.FaceSearch();
}

//output for Registeration Method 
    @Override
    public void onRegisterSuccess(String s, Bitmap bitmap) {

        // s -- Registered vector output from SDK and the registered image to be saved by the developer

    }

    @Override
    public void onRegisterError(String s) {

        Log.e("onError", s);
        
         // s -- error msg 
    }


//output for Search Method
    @Override
    public void onSearchSuccess(int i, String s, Bitmap bitmap) {
         //Output from the SDK for Search method
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
Pass in the id of the fragment container using the FaceRecongintion constructor.
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


