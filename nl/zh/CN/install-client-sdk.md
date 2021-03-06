---

copyright:
  years: 2015, 2017
lastupdated: "2017-01-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

#安装 SDK
{: #mobileanalytics_sdk}

## 安装 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK

目前，Android、iOS、WatchOS、Cordova 和 Web 可以使用 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK。
{: shortdesc}

## 安装 Android 客户端 SDK
{: #install-sdk-android}

[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobilefirstplatform.clientsdk.android/analytics/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobilefirstplatform.clientsdk.android/analytics)

{{site.data.keyword.mobileanalytics_short}} 客户端 SDK 通过 Gradle 进行分发；Gradle 是用于 Android 项目的依赖关系管理器。Gradle 会自动从存储库下载工件，并将其提供给 Android 应用程序。

1. 创建一个 [Android Studio ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://developer.android.com/sdk/index.html){: new_window} 项目或者打开现有的项目。

2. 打开**应用程序模块**中的 `build.gradle` 文件。

  **提示**：Android 项目可能具有两个 `build.gradle` 文件：一个用于项目，另一个用于应用程序模块。请确保使用**应用程序模块**文件。

3. 找到 `build.gradle` 文件的 `Dependencies` 部分，并添加 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK 的编译依赖关系。您的存储库语句应该类似下列代码示例：

	```
            dependencies {
        compile 'com.ibm.mobilefirstplatform.clientsdk.android:analytics:1.+'
    	compile 'com.google.android.gms:play-services-location:10.0.1'
    	// 其他依赖项  
      }
  	```
  	{: codeblock}
	
	第一个依赖项用于 Mobile Analytics 服务客户端 SDK，第二个依赖项用于客户端位置日志记录。仅当启用客户机端位置收集时，才需要第二个依赖项。
    	

4. 通过单击**工具 &gt; Android &gt; 使用 Gradle 文件同步项目**来使用 Gradle 同步项目。

5. 打开 Android 项目的 `AndroidManifest.xml` 文件。您可以在**应用程序 > 清单**中找到此文件。在 `<manifest>` 元素下，添加因特网访问权和位置访问许可权。

	```
	 <uses-permission android:name="android.permission.INTERNET" />
	 <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
	 <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
	```
   如果使用的 SDK 版本不低于 1.2，那么您需要将以下部分放在 `AndroidManifest.xml` 文件的 `<application>` 元素下。
   	
    ```
	 <activity
            android:name="com.ibm.mobilefirstplatform.clientsdk.android.ui.UIActivity"
            android:label="@string/app_name"
            android:launchMode="singleTask">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
	```
   	{: codeblock}
   

您现在已安装 Android 客户端 SDK。接下来，[导入并初始化](sdk.html#initalize-ma-sdk) Analytics 客户端 SDK。   

## 安装 Swift SDK
{: #installing-sdk-ios}

![与 CocoaPods 兼容](https://img.shields.io/cocoapods/v/BMSAnalytics.svg)

使用 {{site.data.keyword.mobileanalytics_full}} SDK，您可以检测移动应用程序。iOS 和 watchOS 可以使用 Swift SDK。

### 开始之前
{: #before-you-begin-ios notoc}

确保正确设置 Xcode。要了解如何设置 iOS 开发环境，请参阅 [Apple Developer Web 站点 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.apple.com/support/xcode/){: new_window}。请阅读有关客户端 SDK Swift Analytics 的 [Xcode 要求 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#requirements){: new_window}。

#### 启用位置 API 
要使位置 API 正常工作，您需要在应用程序的项目文件夹的 Info.plist 文件中添加一个属性，即`隐私 - 位置使用描述`，其值提供添加位置 API 的正确理由，如“应用程序需要启用位置服务”等。

{{site.data.keyword.mobileanalytics_short}} SDK 通过 [CocoaPods ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cocoapods.org/){: new_window} 和 [Carthage ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/Carthage/Carthage#getting-started){: new_window} 进行分发，它们是 Cocoa 项目的依赖关系管理器。CocoaPods 和 Carthage 会自动从存储库下载工件，并将其提供给您的应用程序。选择 CocoaPods 或 Carthage：

#### CocoaPods
{: #cocoapods notoc}

1. 遵循 GitHub 上的 [{{site.data.keyword.Bluemix_notm}} Mobile Services Swift SDK 指示信息 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#cocoapods){: new_window} 以使用 Cocoapods 安装 `BMSAnalytics` 并将其添加到 pod 文件。 
	
2. 在已安装 iOS 客户端 SDK 之后，[导入并初始化](sdk.html#initalize-ma-sdk) Analytics 客户端 SDK。   

#### Carthage
{: #carthage notoc}

如果您不使用 CocoaPods，那么可以使用 [Carthage ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos){: new_window} 将框架添加到项目。

1. 遵循 GitHub 上的 [Carthage 安装指示信息 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics/tree/development#carthage){: new_window} 以安装 `BMSAnalytics`。

2. 在已安装 iOS 客户端 SDK 之后，[导入并初始化](sdk.html#initalize-ma-sdk) Analytics 客户端 SDK。


## 安装 Cordova 插件
{: #installing-sdk-cordova}

使用 {{site.data.keyword.mobileanalytics_full}} Cordova 插件，您可以检测移动应用程序。 

1. 创建一个 [Cordova ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://cordova.apache.org/#getstarted){: new_window} 项目或者打开现有的项目。

2. 将 Android 和 iOS 平台添加到您的 Cordova 应用程序。从命令行运行以下一个或全部两个命令。当前支持 Cordova-CLI V6.3.0 或更低版本：
   
   Android：

	 ```
	 cordova platform add android@5.2.2
	```
	 {: codeblock}
	
   iOS：
   	
	```
	cordova platform add ios
```
   {: codeblock}
	
3. 如果您已添加了 Android 平台，您必须向 Cordova 应用程序的 `config.xml` 文件添加支持的最低 API 级别。打开 `config.xml` 文件，并将以下行添加到 `<platform name="android">` 元素：

	```
	<platform name="android">  
  	 <preference name="android-minSdkVersion" value="15"/>
  	<preference name="android-targetSdkVersion" value="23"/>
  	 <!-- add minimum and target Android API level declaration -->
  	</platform>
	```
   {: codeblock}

 *minSdkVersion* 值必须为 V`15` 或更高版本。请参阅 [Android 平台指南 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cordova.apache.org/docs/en/latest/guide/platforms/android/){: new_window} 以了解有关 Android SDK 受支持的 *targetSdkVersion* 的最新信息。

4. 如果已添加了 iOS 操作系统，请使用目标声明更新 `<platform name="ios">` 元素：

	```
	<platform name="ios">
    <preference name="deployment-target" value="8.0"/>
     <!-- add deployment target declaration -->
     </platform>
	```
	{: codeblock}

5. 添加 `bms-core` 插件。
 	
	 ```
	 cordova plugin add bms-core
	 ```
	 {: codeblock}

6. 通过运行以下命令验证插件是否安装成功：
	
	```
	cordova plugin list
	```
	{: codeblock}
	
7. [配置 Android 和 iOS 环境 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.npmjs.com/package/bms-core#4-configuring-your-platform){: new_window}。

8. 您现在已安装 Cordova 插件并已配置环境。接下来，[导入并初始化](sdk.html#initalize-ma-sdk) Analytics 客户端 SDK。

#### 早于 cordova bms-core 插件版本 (>2.4.+) 的启用位置服务功能。
9. 对于 Cordova-ios 应用程序，要使 Cordova-ios API 正常工作，您需要在应用程序的项目文件夹的 Info.plist 文件中添加一个属性，即`隐私 - 位置使用描述`，其值提供添加位置 API 的正确理由，如“应用程序需要启用位置服务”等。

10. 对于 Cordova-android 应用程序，要使位置 API 正常工作，请将以下内容放入应用程序 AndroidManifest.xml 文件中。
	```
	 <uses-permission android:name="android.permission.INTERNET" />
	 <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
	 <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
	```
	{: codeblock}
	
   然后，需要将以下部分放入 `AndroidManifest.xml` 文件的 `<application>` 元素下。
   	```
	 <activity
            android:name="com.ibm.mobilefirstplatform.clientsdk.android.ui.UIActivity"
            android:label="@string/app_name"
            android:launchMode="singleTask">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
	```
	{: codeblock}

## 安装 Web 插件
{: #web-sdk-cordova}

{{site.data.keyword.mobileanalytics_full}} SDK 支持检测 Web 应用程序。

1. 确保您已安装 Web 服务器和浏览器（例如，Chrome 和 Firefox）。
2. 创建新的 Web 应用程序，或使用现有 Web 应用程序，并将此 [WebSDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-web-analytics/) 放入要由应用程序代码访问的项目中。
3. 通过在 Web 应用程序的 `index.html` 文件中添加以下脚本来添加 Web 插件：
	
	```Html
  	<script src="bms-clientsdk-web-analytics/bmsanalytics.js"></script>
	```
	{: codeblock}
或者，使用模块装入程序 requirejs 来添加 Web 插件。用作参考 API 的名称与所使用的自变量名称 (`BMSAnalytics`) 相同。
	
   ```Javascript
	  require.config({
	   'paths': {
	   'bmsanalytics': 'bms-clientsdk-web-analytics/bmsanalytics'
	}
	});

		<script src="bms-clientsdk-web-analytics/bmsanalytics.js"></script>
    ```
 	    {: codeblock}

	或者，使用模块装入程序 requirejs 来添加 Web 插件。用作参考 API 的名称与所使用的自变量名称 (`BMSAnalytics`) 相同。 
	
	  ```Javascript
	 	require.config({
	    'paths': {
	        'bmsanalytics': 'bms-clientsdk-web-analytics/bmsanalytics'
	    	}
		});
	  ```
	{: codeblock}




# 相关链接
{: #rellinks notoc}

## SDK
{: #sdk notoc}
* [Android SDK ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-analytics){: new_window}  
* [iOS SDK ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics){: new_window}
* [Cordova 插件核心 SDK ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.npmjs.com/package/bms-core){: new_window}
* [Web SDK ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-web-analytics/){: new_window}
## API 参考
{: #api notoc}
* [REST API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobile-analytics-dashboard.{DomainName}/analytics-service/){:new_window}
