Qiscus SDK [![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Qiscus%20SDK-green.svg?style=true)](https://android-arsenal.com/details/1/4438) [![Build Status](https://travis-ci.org/qiscus/qiscus-sdk-android.svg?branch=master)](https://travis-ci.org/qiscus/qiscus-sdk-android)
======
<p align="center"><img src="https://github.com/qiscus/qiscus-sdk-android/raw/develop/screenshot/device-2016-09-16-102736.png" width="40%" /><img src="https://github.com/qiscus/qiscus-sdk-android/raw/develop/screenshot/device-2016-09-16-102923.png" width="40%" /></p>
Qiscus SDK is a lightweight and powerful android chat library. Qiscus SDK will allow you to easily integrating Qiscus engine with your apps to make cool chatting application.

# Quick Start
### Create a new SDK application in the Dashboard and get application id 
you can get app ID here [http://sdk.qiscus.com](http://sdk.qiscus.com)
### Integrating SDK with an existing app 
Add to your project build.gradle
```groovy
allprojects {
    repositories {
        .....
        maven { url  "http://dl.bintray.com/qiscustech/maven" }
    }
}
```

Then add to your app module build.gradle
```groovy
dependencies {
    compile 'com.qiscus.sdk:chat:1.18.0'
}
```


Check sample apps -> [DragonGo](https://github.com/qiscus/qiscus-sdk-android-sample)

# Authentication 
### Initializing with APP_ID
Init Qiscus at your application class with your application ID
```java
public class SampleApps extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Qiscus.init(this, "yourQiscusAppId");
    }
}
```
### Login or register
Before user can start chatting each other, they must login to qiscus engine.
```java
Qiscus.setUser("user@email.com", "userKey")
      .withUsername("Tony Stark")
      .withAvatarUrl("http://avatar.url.com/handsome.jpg")
      .save(new Qiscus.SetUserListener() {
          @Override
          public void onSuccess(QiscusAccount qiscusAccount) {
              DataManager.saveQiscusAccount(qiscusAccount);
              startActivity(new Intent(this, ConsultationListActivity.class));
          }
          @Override
          public void onError(Throwable throwable) {
              throwable.printStackTrace();
              showError(throwable.getMessage());
          }
      });
```
### Disconnecting/logout 
```java
Qiscus.clearUser();
```
    
# Room Types  
### Group Room
A Group Room is a chat for several users. A user can join the chat only through an invitation.
### 1 on 1 
A 1 on 1 Room is a chat for two users. The chat initiator only need to add the target's messaging username

# 1-to-1 Chat
### Creating and starting 1-to-1 chat 
```java
Qiscus.buildChatWith("jhon.doe@gmail.com")
      .withTitle("Jhon Doe")
      .build(this, new Qiscus.ChatActivityBuilderListener() {
          @Override
          public void onSuccess(Intent intent) {
              startActivity(intent);
          }
          @Override
          public void onError(Throwable throwable) {
              throwable.printStackTrace();
              showError(throwable.getMessage());
          }
      });
```
    

# UI Customization
### Theme Customization 
Boring with default template? You can customized it, try it!, we have more items than those below code, its just example.
```java
Qiscus.getChatConfig()
      .setStatusBarColor(R.color.blue)
      .setAppBarColor(R.color.red)
      .setTitleColor(R.color.white)
      .setLeftBubbleColor(R.color.green)
      .setRightBubbleColor(R.color.yellow)
      .setRightBubbleTextColor(R.color.white)
      .setRightBubbleTimeColor(R.color.grey)
      .setTimeFormat(date -> new SimpleDateFormat("HH:mm").format(date));
```


### UI Source code
Check [CustomChatActivity.java](https://github.com/qiscus/qiscus-sdk-android/blob/develop/app/src/main/java/com/qiscus/dragonfly/CustomChatActivity.java)
<p align="center"><img src="https://github.com/qiscus/qiscus-sdk-android/raw/develop/screenshot/device-2016-09-28-232326.png" width="33%" /><img src="https://github.com/qiscus/qiscus-sdk-android/raw/develop/screenshot/device-2016-09-28-232535.png" width="33%" /><img src="https://github.com/qiscus/qiscus-sdk-android/raw/develop/screenshot/device-2016-09-28-232714.png" width="33%" /></p>
    
# Miscellaneous 
### Rx Java support
```java
// Setup qiscus account with rxjava example
Qiscus.setUser("user@email.com", "password")
      .withUsername("Tony Stark")
      .withAvatarUrl("http://avatar.url.com/handsome.jpg")
      .save()
      .subscribeOn(Schedulers.io())
      .observeOn(AndroidSchedulers.mainThread())
      .subscribe(qiscusAccount -> {
          DataManager.saveQiscusAccount(qiscusAccount);
          startActivity(new Intent(this, ConsultationListActivity.class));
      }, throwable -> {
          throwable.printStackTrace();
          showError(throwable.getMessage());
      });

// Start a chat activity with rxjava example      
Qiscus.buildChatWith("jhon.doe@gmail.com")
      .withTitle("Jhon Doe")
      .build(this)
      .subscribeOn(Schedulers.io())
      .observeOn(AndroidSchedulers.mainThread())
      .subscribe(intent -> {
          startActivity(intent);
      }, throwable -> {
          throwable.printStackTrace();
          showError(throwable.getMessage());
      });
```

License
-------
    Copyright (c) 2016 Qiscus.
    
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
