title: NDK Samples学习 [6-1] - camera之basic
tags: [NDK]
date: 2019-03-05 17:40:44
categories: Android
---

[camera-basic](https://github.com/googlesamples/android-ndk/tree/master/camera/basic/)
[README](https://github.com/googlesamples/android-ndk/blob/master/camera/README.md)
该项目演示如何使用c创建camera拍照、预览。

* * *

最低要求：

1.  Android Studio 2.3.0+ ,NDK r15+
2. Android-24+

* * *

项目主要流程：
1. 获取权限
2. 初始化界面，NDKCamera、曝光度、感光度
3. 操作相机，调整曝光度、感光度预览；拍照保存

<!-- more -->

* * *

CameraActivity.java
1. CameraSeekBar 包装SeekBar用于调整相机预览的曝光度、感光度

void setImmersiveSticky() {} //设置全屏显示
public void RequestCamera() {} //请求权限，由c++调用
public void EnableUI(final long[] params){}//由c++调用，并传入曝光度、感光度参数
public void OnPhotoTaken(String fileName) {}//由c++调用，传入拍照图片路径
native static void notifyCameraPermission(boolean granted);//权限变化后
native static void TakePhoto();//调用c++拍照
native void OnExposureChanged(long exposure);//调用c++调整曝光度
native void OnSensitivityChanged(long sensitivity);//调用c++调整感光度
native static void notifyCameraPermission(boolean granted);//通知c++权限变化

android_main.cpp c主入口
```
/*
 * SampleEngine global object
 */
static CameraEngine* pEngineObj = nullptr;
/*
 * 获取CameraEngine单例对象
 */
CameraEngine* GetAppEngine(void) {
  ASSERT(pEngineObj, "AppEngine has not initialized");
  return pEngineObj;
}

/**
 * 根据收到INIT_WINDOW/TERM_WINDOW事件创建或删除camera对象，
 * 使用NativeActivity的应用才会收到
 */
static void ProcessAndroidCmd(struct android_app* app, int32_t cmd) {
  CameraEngine* engine = reinterpret_cast<CameraEngine*>(app->userData);
  switch (cmd) {
    // 窗口初始化
    case APP_CMD_INIT_WINDOW:
      if (engine->AndroidApp()->window != NULL) {
        engine->SaveNativeWinRes(ANativeWindow_getWidth(app->window),
                                 ANativeWindow_getHeight(app->window),
                                 ANativeWindow_getFormat(app->window));
        // 请求权限，创建camera
        engine->OnAppInitWindow();
      }
      break;
    // 窗口关闭
    case APP_CMD_TERM_WINDOW:
      // 删除camera
      engine->OnAppTermWindow();
      ANativeWindow_setBuffersGeometry(
          app->window, engine->GetSavedNativeWinWidth(),
          engine->GetSavedNativeWinHeight(), engine->GetSavedNativeWinFormat());
      break;
    // 窗口配改变
    case APP_CMD_CONFIG_CHANGED:
      engine->OnAppConfigChange();
      break;
    // 失去焦点
    case APP_CMD_LOST_FOCUS:
      break;
  }
}

extern "C" void android_main(struct android_app* state) {
  CameraEngine engine(state);
  pEngineObj = &engine;

  state->userData = reinterpret_cast<void*>(&engine);
  state->onAppCmd = ProcessAndroidCmd;

  // loop waiting for stuff to do.
  while (1) {
    // Read all pending events.
    int events;
    struct android_poll_source* source;

    while (ALooper_pollAll(0, NULL, &events, (void**)&source) >= 0) {
      // Process this event.
      if (source != NULL) {
        source->process(state, source);
      }

      // Check if we are exiting.
      if (state->destroyRequested != 0) {
        LOGI("CameraEngine thread destroy requested!");
        engine.DeleteCamera();
        pEngineObj = nullptr;
        return;
      }
    }
    pEngineObj->DrawFrame();
  }
}

/**
 * 窗口初始化时调用，请求相机相关权限，并创建camera
 */
void CameraEngine::OnAppInitWindow(void) {
  if (!cameraGranted_) {
    // 请求相机相关权限
    RequestCameraPermission();
    return;
  }

  // 旋转角度
  rotation_ = GetDisplayRotation();

  // 创建camera
  CreateCamera();
  ASSERT(camera_, "CameraCreation Failed");

  // 启用曝光度UI
  EnableUI();

  // 预览
  cameraReady_ = true;
  camera_->StartPreview(true);
}

/**
 * Handle APP_CMD_TEMR_WINDOW
 */
void CameraEngine::OnAppTermWindow(void) {
  cameraReady_ = false;
  // 销毁camera
  DeleteCamera();
}

/**
 * Handle APP_CMD_CONFIG_CHANGED
 * 获取旋转角度
 * 旋转角度有变化，销毁camera，再重新创建camera
 */
void CameraEngine::OnAppConfigChange(void) {
  int newRotation = GetDisplayRotation();

  if (newRotation != rotation_) {
    OnAppTermWindow();

    rotation_ = newRotation;
    OnAppInitWindow();
  }
}

```

camera_engine.cpp
```
/**
 * 构造函数，初始camera资源
 * @param app native_app_glue environment
 */
CameraEngine::CameraEngine(android_app* app)

/**
 * 析构造函数，释放camera资源
 */
CameraEngine::~CameraEngine() {
  cameraReady_ = false;
  DeleteCamera();
}

/**
 * 创建后台camera
 */
void CameraEngine::CreateCamera(void) {
  // 没有权限直接返回
  if (!cameraGranted_ || !app_->window) {
    LOGW("Camera Sample requires Full Camera access");
    return;
  }

  int32_t displayRotation = GetDisplayRotation();
  rotation_ = displayRotation;

  // 创建NDKCamera
  camera_ = new NDKCamera();
  ASSERT(camera_, "Failed to Create CameraObject");

  // 旋转角度
  int32_t facing = 0, angle = 0, imageRotation = 0;
  if (camera_->GetSensorOrientation(&facing, &angle)) {
    if (facing == ACAMERA_LENS_FACING_FRONT) {
      imageRotation = (angle + rotation_) % 360;
      imageRotation = (360 - imageRotation) % 360;
    } else {
      imageRotation = (angle - rotation_ + 360) % 360;
    }
  }
  LOGI("Phone Rotation: %d, Present Rotation Angle: %d", rotation_,
       imageRotation);
  ImageFormat view{0, 0, 0}, capture{0, 0, 0};
  camera_->MatchCaptureSizeRequest(app_->window, &view, &capture);

  ASSERT(view.width && view.height, "Could not find supportable resolution");

  // Request the necessary nativeWindow to OS
  bool portraitNativeWindow =
      (savedNativeWinRes_.width < savedNativeWinRes_.height);
  ANativeWindow_setBuffersGeometry(
      app_->window, portraitNativeWindow ? view.height : view.width,
      portraitNativeWindow ? view.width : view.height, WINDOW_FORMAT_RGBA_8888);

  yuvReader_ = new ImageReader(&view, AIMAGE_FORMAT_YUV_420_888);
  yuvReader_->SetPresentRotation(imageRotation);
  jpgReader_ = new ImageReader(&capture, AIMAGE_FORMAT_JPEG);
  jpgReader_->SetPresentRotation(imageRotation);
  jpgReader_->RegisterCallback(this, [this](void* ctx, const char* str) -> void {
    reinterpret_cast<CameraEngine* >(ctx)->OnPhotoTaken(str);
  });

  // now we could create session
  camera_->CreateSession(yuvReader_->GetNativeWindow(),
                         jpgReader_->GetNativeWindow(), imageRotation);
}

// 释放camera
void CameraEngine::DeleteCamera(void) {
  cameraReady_ = false;
  if (camera_) {
    delete camera_;
    camera_ = nullptr;
  }
  if (yuvReader_) {
    delete yuvReader_;
    yuvReader_ = nullptr;
  }
  if (jpgReader_) {
    delete jpgReader_;
    jpgReader_ = nullptr;
  }
}

/**
 * 请求camera相关权限
 *  通过调用CameraActivity.notifyCameraPermission,
 *  授权结果，调用native notifyCameraPermission()
 */
void CameraEngine::RequestCameraPermission() {
  if (!app_) return;

  JNIEnv* env;
  ANativeActivity* activity = app_->activity;
  activity->vm->GetEnv((void**)&env, JNI_VERSION_1_6);

  activity->vm->AttachCurrentThread(&env, NULL);

  jobject activityObj = env->NewGlobalRef(activity->clazz);
  jclass clz = env->GetObjectClass(activityObj);
  env->CallVoidMethod(activityObj,
                      env->GetMethodID(clz, "RequestCamera", "()V"));
  env->DeleteGlobalRef(activityObj);

  activity->vm->DetachCurrentThread();
}
/**
 * 调整曝光度、感光度
 */
void CameraEngine::OnCameraParameterChanged(int32_t code, int64_t val) {
  camera_->UpdateCameraRequestParameter(code, val);
}

/**
 * The main function rendering a frame. In our case, it is yuv to RGBA8888
 * converter
 */
void CameraEngine::DrawFrame(void) {
  if (!cameraReady_ || !yuvReader_) return;
  AImage* image = yuvReader_->GetNextImage();
  if (!image) {
    return;
  }

  ANativeWindow_acquire(app_->window);
  ANativeWindow_Buffer buf;
  if (ANativeWindow_lock(app_->window, &buf, nullptr) < 0) {
    yuvReader_->DeleteImage(image);
    return;
  }

  yuvReader_->DisplayImage(&buf, image);
  ANativeWindow_unlockAndPost(app_->window);
  ANativeWindow_release(app_->window);
}
```

camera_ui.cpp
```
/**
 * 获取旋转角度Retrieve current rotation from Java side
 * 调用CameraActivity.getRotationDegree
 */
int CameraEngine::GetDisplayRotation() {
  ...
}

/**
 * 设置曝光度、感光度
 */
void CameraEngine::EnableUI(void) {
   // 1. 获取曝光度，感光度NDKCamera::GetExposureRange\NDKCamera::GetSensitivityRange
  // 2. 调用CameraActivity.EnableUI设置
}

/**
 * 拍照
 */
void CameraEngine::OnTakePhoto() {
  if (camera_) {
    camera_->TakePhoto();
  }
}

/**
 * 回传照片文件路径，CameraActivity.OnPhotoTaken
 */
void CameraEngine::OnPhotoTaken(const char* fileName) {
  ...
}
/**
 * camera和文件写入权限回调
 */
void CameraEngine::OnCameraPermission(jboolean granted) {
  //有权限则调用OnAppInitWindow初始化
}

/**
 *  native响应权限变化
 */
extern "C" JNIEXPORT void JNICALL
Java_com_sample_camera_basic_CameraActivity_notifyCameraPermission(
    JNIEnv *env, jclass type, jboolean permission) {
  //CameraEngine::OnCameraPermission权限变化
}

/**
 *  native拍照
 */
extern "C" JNIEXPORT void JNICALL
Java_com_sample_camera_basic_CameraActivity_TakePhoto(JNIEnv *env,
                                                      jclass type) {
  //CameraEngine::OnTakePhoto
}

/**
 *  native曝光度变化，调整预览
 */
extern "C" JNIEXPORT void JNICALL
Java_com_sample_camera_basic_CameraActivity_OnExposureChanged(
    JNIEnv *env, jobject instance, jlong exposurePercent) {
  //GetAppEngine()::OnCameraParameterChanged
}

/**
 *  native感光度变化，调整预览
 */
extern "C" JNIEXPORT void JNICALL
Java_com_sample_camera_basic_CameraActivity_OnSensitivityChanged(
    JNIEnv *env, jobject instance, jlong sensitivity) {
  //GetAppEngine()::OnCameraParameterChanged
}
```

camera_manager.cpp   
1. 初始化camera
2. 更新属性
3. 拍照

image_reader.cpp   图片转换相关  
camera_listeners.cpp camera manager相关Listener

* * *

项目功能为：
1.  native监听窗口变化判断调用java请求camera相关权限
2.  native创建NDKCamera，并获取曝光度、感光度属性回调到Java并展示
3.  native预览
4.  java调用native调整预览曝光度、感光度
5. java调用native拍照并回调java提示图片文件路径

* * *
### android-ndk-samples学习目录
部份引用其他大牛的项目
1.  [hello-jni](https://www.jianshu.com/p/a5a6c3fdb622) by 七零八落问号
    演示如何从Java层调用C层代码。

2.  [hello-jniCallback](https://www.jianshu.com/p/499826cb97fc) by 七零八落问
    演示如何在C层回调Java层方法。

3.  [hello-libs](https://www.jianshu.com/p/b3be6b2ee87c) by 七零八落问
    演示如何在Android Studio上使用第三方的C / C++库。

4.  [audio echo](https://blog.csdn.net/foolish0421/article/details/78018926) by hunter800421  
    演示如何如何使用OpenSL ES创建播放器和录音。

5.  [bitmap-plasma](https://blog.csdn.net/foolish0421/article/details/78123165) by hunter800421  
    演示如何使用c代码在bitmap上绘制细胞质效果。

6.1.  [camera-basic](http://blog.520wa.com/2019/03/05/ndksamples-camera-basic/) by  Tu 
    演示如何使用c创建camera拍照、预览。

##### 参考  
[NatvieActivity事件](http://tieba.baidu.com/p/2650541001?traceid=)