# OpenCV Java for Android

## NOTE
After clone this repository, remove the folder **doc**. It's not part of the android app project.

## Introduction
Follow the steps below to create a new android app from scratch with support for OpenCV Java.

If you want to skip the procedures, just clone this repository and update the **jniLibs** symbolic link at **app/src/main**

## Steps
1. Download the OpenCV Android SDK from https://github.com/opencv/opencv/releases and unzip it.

2. Create a new android app using the Android Studio. 

![new project](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/01_new_project.jpg)

![project_properties](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/02_project_properties.png)

![project_properties ii](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/03_project_properties_2.png)

3. Create a symbolic link for the native OpenCV compiled libs inside the app project

```ln -s OpenCV-android-sdk/sdk/native/libs <project_folder>/app/src/main/jniLibs```

4. Import as a new module (File -> New -> Import Module...) the OpenCV Java source into the project. 

![import module](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/04_import_module.png)

5. Open the **OpencvLibrary/build.gradle** file and adjust the parameters: **compileSdkVersion**, **minSdkVersion** and **targetSdkVersion** as configured in the **app/build.grade** file.

6. Still in the **OpencvLibrary/build.gradle** file change the property **res.srcDirs** to **res.srcDirs = ['res']**. This is required to avoid a build error for OpenCV 4.

Press **Sync Now**

![opencv build gradle](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/05_opencv_build_gradle.png)

7. Set the OpencvLibrary module as a dependency of the android app. Select the **app** level in the left panel, go to File -> Project Structure..., select the module **app**, click on the tab Dependecies, '+' button and select Module Dependency

![module dependency](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/07_module_dependency.png)

 Select **:OpencvLibrary**
 
![module dependency i](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/08_module_dependency_1.png)

![module dependency ii](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/09_module_dependency_2.png)

8. Open the **app/res/layout/activity_main.xml** file and add an **ImageView** after the TextView.

```
    <ImageView
        android:id="@+id/sample_image"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="@id/sample_text" />
```

![activity main xml](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/10_activity_main_xml.png)

9. Copy any image to **app/res/drawable** with the name **test_image.jpg**

![test image jpg](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/11_test_image_jpg.png)

10. Open the **MainActivity.java** file

Add to the **Static {...}** section the following

```
        if (BuildConfig.DEBUG) {
            OpenCVLoader.initDebug();
        }
```

![load opencv](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/12_load_opencv.png)

On the **onCreate()** method add

```
        // OpenCV
        Bitmap src = BitmapFactory.decodeResource(getApplicationContext().getResources(), R.drawable.test_image);

        Mat image = new Mat();
        Utils.bitmapToMat(src, image);

        Mat grayImage = new Mat();
        Imgproc.cvtColor(image, grayImage, Imgproc.COLOR_BGR2GRAY, CvType.CV_32S);

        final Bitmap bitmap = Bitmap.createBitmap(grayImage.cols(), grayImage.rows(), Bitmap.Config.ARGB_8888);

        Utils.matToBitmap(grayImage, bitmap);

        ImageView iv = (ImageView) findViewById(R.id.sample_image);
        iv.setImageBitmap(bitmap);
```

11. Run the application and you must get the image converted to grayscale.

![app run](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/13_app_run.png)

12. It's also supposed you see in the Logcat the OpenCV initialization

![opencv init](https://raw.githubusercontent.com/kleysonr/opencv_android/master/doc/14_opencv_init.png)





