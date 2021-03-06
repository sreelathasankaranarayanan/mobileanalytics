---

copyright:
  years: 2015, 2017
lastupdated: "2018-01-18"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

# 檢測應用程式
{: #mobileanalytics_sdk}

## 檢測應用程式以使用 {{site.data.keyword.mobileanalytics_short}} Client SDK

{{site.data.keyword.mobileanalytics_full}} SDK 可讓您檢測行動應用程式。
{: shortdesc}

{{site.data.keyword.mobileanalytics_short}} 可讓您收集下列種類的資料，而且各需要不同程度的檢測：


- 預先定義的資料 - 此種類包括適用於所有應用程式的一般用法及裝置資訊。在此種類內的是指出應用程式使用數量、頻率或持續時間的裝置 meta 資料（作業系統和裝置模型）及用法資料（作用中使用者和應用程式階段作業）。在應用程式中起始設定 {{site.data.keyword.mobileanalytics_short}} SDK 之後，會自動收集預先定義的資料。

- 應用程式日誌訊息 - 此種類可讓開發人員在應用程式內新增程式碼行，以記載自訂訊息來協助開發和除錯。開發人員會將嚴重性/詳細層次指派給每一個日誌訊息，而且後續可以依指派的層次過濾訊息，或透過配置應用程式忽略低於給定記載層次的訊息來保留儲存空間。若要收集應用程式日誌資料，您必須在應用程式內起始設定 {{site.data.keyword.mobileanalytics_short}} SDK，以及為每一個日誌訊息新增一行程式碼。

- 自訂事件 - 此種類包括您自行定義及應用程式特有的資料。這個資料代表您應用程式內發生的事件，例如頁面檢視、按鈕點選或應用程式內採購。除了在應用程式中起始設定 {{site.data.keyword.mobileanalytics_short}} SDK 之外，您還必須為您要追蹤的每一個自訂事件新增一行程式碼。 

SDK 目前適用於 Android、iOS、WatchOS、Cordova 及 Web。

## 識別服務認證 API 金鑰值
{: #analytics-clientkey}

識別設定 Client SDK 之前的 **API 金鑰**值。需要有「API 金鑰」，才能起始設定 Client SDK。

1. 開啟 {{site.data.keyword.mobileanalytics_short}} 服務儀表板。
1. 展開**檢視認證**，以顯示「API 金鑰」值。當您起始設定 {{site.data.keyword.mobileanalytics_short}} Client SDK 時，需要「API 金鑰」值。


## 起始設定應用程式來收集分析
{: #initalize-ma-sdk}

起始設定應用程式，以啟用將日誌傳送至 {{site.data.keyword.mobileanalytics_short}} 服務。

1. 匯入 Client SDK。
	- Android
	
		將下列 `import` 陳述式新增至專案檔開頭處：
		
		```
		import com.ibm.mobilefirstplatform.clientsdk.android.core.api.*;
	import com.ibm.mobilefirstplatform.clientsdk.android.analytics.api.*;
	import com.ibm.mobilefirstplatform.clientsdk.android.logger.api.*;
  	```
		{: codeblock}
  
	- iOS
		
		Swift SDK 適用於 iOS 及 watchOS。將下列 `import` 陳述式新增至 `AppDelegate.swift` 專案檔開頭處，以匯入 `BMSCore` 和 `BMSAnalytics` 架構：
	
		```Swift
		import BMSCore
		import BMSAnalytics
		```
		{: codeblock}  
   
	- Cordova
			
		從您的 Cordova 應用程式根目錄，執行下列指令以新增 Cordova 外掛程式：
	
		```Javascript
		cordova plugin add bms-core
		```
		{: codeblock}  

	- Web
			
		將以 Script 形式提供的 SDK 新增至 index.html，以新增 Web 外掛程式：
	
		```Html
		<script src="/path/to/bms-clientsdk-web-analytics/bmsanalytics.js"></script>
		```
		{: codeblock} 	 	

	 	或者，使用模組載入器 requirejs 來新增 Web 外掛程式： 
	
	 	```Javascript
	 	require.config({
	    'paths': {
	        'bmsanalytics': '/path/to/bms-clientsdk-web-analytics/bmsanalytics'
	    	}
		});
		require(['bmsanalytics'], function(BMSAnalytics) {
		 BMSAnalytics.send(); 
		}
		``` 	
		{: codeblock}  

2. 在應用程式中起始設定 {{site.data.keyword.mobileanalytics_short}} Client SDK。

	- Android
		
		在 Android 應用程式中主要活動的 `onCreate` 方法中或最適合您專案的位置中新增起始設定碼，以在 Android 應用程式中起始設定 {{site.data.keyword.mobileanalytics_short}} Client SDK。
	
		```Java
		BMSClient.getInstance().initialize(getApplicationContext(), BMSClient.REGION_US_SOUTH); // Make sure that you point to your region
		```
		{: codeblock}
	
		您必須起始設定 `BMSClient` 與 **bluemixRegion** 參數。在起始設定程式中，**bluemixRegion** 值指定您所使用的 {{site.data.keyword.Bluemix_notm}} 部署，例如 `BMSClient.REGION_US_SOUTH` 及 `BMSClient.REGION_UK`。
    	    <!-- , or `BMSClient.REGION_SYDNEY`.--> 
    
	- iOS
	    
		請先使用下列程式碼來起始設定 `BMSClient` 類別。將起始設定碼放在應用程式委派的 `application(_:didFinishLaunchingWithOptions:)` 方法中或最適合您專案的位置中。
		
		```Swift 
		BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
		```
		{: codeblock}
	
		您必須起始設定 `BMSClient` 與 **bluemixRegion** 參數。在起始設定程式中，**bluemixRegion** 值指定您所使用的 {{site.data.keyword.Bluemix_notm}} 部署（例如，`BMSClient.Region.usSouth` 或 `BMSClient.Region.unitedKingdom`）。
	    <!-- , or `BMSClient.Region.Sydney`. -->
    
	- Cordova
	  
		起始設定 **BMSClient** 及 **BMSAnalytics**。您將需要 [**API 金鑰**](#analytics-clientkey)值。
	
		```Javascript
		BMSClient.initialize(BMSClient.REGION_US_SOUTH); //Make sure you point to your region	
		```
		{: codeblock}
		
		若要使用 {{site.data.keyword.mobileanalytics_short}} Client SDK，您必須起始設定 `BMSClient` 與 **bluemixRegion** 參數。在起始設定程式中，**bluemixRegion** 值指定您所使用的 {{site.data.keyword.Bluemix_notm}} 部署（例如，`BMSClient.REGION_US_SOUTH` 或 `BMSClient.REGION_UK`）。
	    <!-- , or `BMSClient.REGION_SYDNEY`. -->
    
	- Web

	    使用「API 金鑰」值，在應用程式碼內起始設定 Client SDK，以記錄用量分析及應用程式階段作業。
	    ```javascript
	    BMSAnalytics.Client.initialize(BMSAnalytics.Client.REGION_US_SOUTH);
	    ```
	    {: codeblock}
		
		若要使用 {{site.data.keyword.mobileanalytics_short}} Client SDK，您必須起始設定 `BMSAnalytics.Client` 與 **bluemixRegion** 參數。在起始設定程式中，**bluemixRegion** 值指定您所使用的 {{site.data.keyword.Bluemix_notm}} 部署（例如，`BMSAnalytics.Client..REGION_UK` 或 `BMSAnalytics.Client..REGION_US_SOUTH`）。
	    <!-- , or `BMSClient.REGION_SYDNEY`. -->
    
3. 您可以使用應用程式物件，並提供您的應用程式名稱，來起始設定 Analytics。 

	您為應用程式所選取的名稱 (`your_app_name_here`) 會在 {{site.data.keyword.mobileanalytics_short}} 主控台中顯示為應用程式名稱。應用程式名稱是用來作為過濾器，以在儀表板中搜尋應用程式日誌。當您跨平台（例如，Android 及 iOS）使用相同的應用程式名稱時，不論是從哪個平台傳送日誌，都可以看到同名應用程式的所有日誌。

	您還需要 [API 金鑰](#analytics-clientkey)值。

- Android
		
	```Java
		// In this code example, Analytics is configured to record lifecycle events.
		Analytics.init(getApplication(), "your_app_name_here", apiKey, hasUserContext, collectLocation, Analytics.DeviceEvent.ALL);
	```
    {: codeblock}
		
	**附註：**請將 `hasUserContext` 的值設為 **true** 或 **false**。如果是 false（預設值），每一台裝置都會計算為一個作用中使用者。當 `hasUserContext` 為 false 時，[`Analytics.setUserIdentity("username")`](sdk.html#android-tracking-users) 方法（可讓您追蹤每台裝置中積極使用您應用程式的使用者數目）無法運作。如果是 true，則每次使用 [`Analytics.setUserIdentity("username")`](sdk.html#android-tracking-users) 時都會視為作用中使用者。當 `hasUserContext` 為 true 時，沒有預設的使用者身分，因此必須設為移入作用中使用者圖表。`Analytics.logLocation()` 方法可讓應用程式傳送裝置位置，將在 `collectLocation` 設為 true 時運作。 
		
		
	
- iOS
		
	```Swift 
		Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, collectLocation: true, deviceEvents: .lifecycle, .network)
	```
    {: codeblock}
 	
- watchOS
		 	
	```Swift
		Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here",collectLocation: true, deviceEvents: .network)
	```
    {: codeblock}
	 	
    選用 `deviceEvents` 參數會自動收集裝置層次事件的分析。
		
    將 `hasUserContext` 的值設為 **true** 或 **false**。如果是 false（預設值），每一台裝置都會計算為一個作用中使用者。當 `hasUserContext` 為 false 時，[`Analytics.userIdentity = "username"`](sdk.html#ios-tracking-users) 方法（可讓您追蹤每台裝置中積極使用您應用程式的使用者數目）無法運作。如果 `hasUserContext` 是 true，則每次使用 [`Analytics.userIdentity = "username"`](sdk.html#ios-tracking-users) 時都會計算為一個作用中使用者。當 `hasUserContext` 為 true 時，沒有預設的使用者身分，因此必須設為移入作用中使用者圖表。`Analytics.logLocation()` 方法可讓應用程式傳送裝置位置，將在 `collectLocation` 設為 true 時運作。 

- watchOS
	
    您可以使用 `Analytics.recordApplicationDidBecomeActive()` 及 `Analytics.recordApplicationWillResignActive()` 方法，來記錄 WatchOS 上的裝置事件。
	  
    將下列這一行新增至 ExtensionDelegate 類別的 `applicationDidBecomeActive()` 方法：
	 
	```Javascript
		Analytics.recordApplicationDidBecomeActive()
	```
    {: codeblock}
	
    將下列這一行新增至 ExtensionDelegate 類別的 `applicationWillResignActive()` 方法：
	 
	```Javascript
		Analytics.recordApplicationWillResignActive()
	```
    {: codeblock}	
		
- Cordova
		
	```Javascript
	Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
	```
    {: codeblock}	
		
    將 `hasUserContext` 的值設為 **true** 或 **false**。如果是 false（預設值），每一台裝置都會計算為一個作用中使用者。當 `hasUserContext` 為 false 時，`Analytics.setUserIdentity("username")` 方法（可讓您追蹤每台裝置中積極使用您應用程式的使用者數目）無法運作。如果是 true，則每次使用 `Analytics.setUserIdentity("username")`(sdk.html#android-tracking-users) 時都會計算為一個作用中使用者。當 `hasUserContext` 為 true 時，沒有預設的使用者身分，因此必須設為移入作用中使用者圖表。`Analytics.logLocation()` 方法可讓應用程式傳送裝置位置，將在 `collectLocation` 設為 true 時運作。 
	
- Web
		
	```Java		
		// In this code example, Analytics is configured to record allevents.
		BMSAnalytics.initialize(appName, apiKey, hasUserContext, BMSAnalytics.DeviceEvent.ALL);
	```
    {: codeblock}
		
    將 `hasUserContext` 的值設為 **true** 或 **false**。如果是 false（預設值），每一台裝置都會計算為一個作用中使用者。當 `hasUserContext` 為 false 時，[`BMSAnalytics.setUserIdentity("username")`](sdk.html#web-tracking-users) 方法（可讓您追蹤每台裝置中積極使用您應用程式的使用者數目）無法運作。如果是 true，則每次使用 [`BMSAnalytics.setUserIdentity("username")`](sdk.html#web-tracking-users) 時都會計算為一個作用中使用者。當 `hasUserContext` 為 true 時，沒有預設的使用者身分，因此必須設為移入作用中使用者圖表。	

4. 您現在已起始設定應用程式來收集分析。接下來，您可以[傳送分析資料](sdk.html#app-monitoring-gathering-analytics)至 {{site.data.keyword.mobileanalytics_short}} 服務。


## 收集用量分析
{: #app-monitoring-gathering-analytics}

您可以配置 {{site.data.keyword.mobileanalytics_short}} Client SDK 來記錄用量分析，並且將記錄的資料傳送至 {{site.data.keyword.mobileanalytics_short}} 服務。

使用下列 API 來開始記錄及傳送用量分析：

- Android
	
	```
	// Disable recording of usage analytics (for example, to save disk space)
	// Recording is enabled by default
	Analytics.disable();
	// Enable recording of usage analytics
	Analytics.enable();
	// Send recorded usage analytics to the Mobile Analytics Service
	Analytics.send(new ResponseListener() {
	            @Override
                    public void onSuccess(Response response) {
                        // Handle Analytics send success here.
    }
    @Override
            public void onFailure(Response response, Throwable throwable, JSONObject jsonObject) {
                // Handle Analytics send failure here.
            }
        });
	```
	{: codeblock}
		
	用於記載事件的用法分析範例：
		
	```
	// Log a custom analytics event
	JSONObject eventJSONObject = new JSONObject();
	eventJSONObject.put("customProperty" , "propertyValue");
	Analytics.log(eventJSONObject);
	```
	{: codeblock}


- iOS (Swift)

	```
	// Disable recording of usage analytics (for example, to save disk space)
	// Recording is enabled by default
	Analytics.isEnabled = false
	// Enable recording of usage analytics
	Analytics.isEnabled = true
	// Send recorded usage analytics to the Mobile Analytics Service
	Analytics.send(completionHandler: { (response: Response?, error: Error?) in
	    if let response = response {
	        // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
            }
        })
	```
	{: codeblock}
	
	用於記載事件的用法分析範例：
	
	```Swift
	// Log a custom analytics event
	let eventObject = ["customProperty": "propertyValue"]
	Analytics.log(metadata: eventObject)
	```
	{: codeblock}

- Cordova

	```JavaScript
	// Enable usage analytics recording
	BMSAnalytics.enable();
	// Disable usage analytics recording
	BMSAnalytics.disable();
	// Send recorded usage analytics to the {{site.data.keyword.mobileanalytics_short}} Service
	BMSAnalytics.send(
		function(response) {
			console.log('success: ' + response);
		},
	function (err) {
		console.log('fail: ' + err);
		});
  ```
	{: codeblock}
	
	用於記載事件的用法分析範例：
	
	```JavaScript
	// Log a custom analytics event
	var eventObject = {"customProperty": "propertyValue"}
	BMSAnalytics.log(eventObject)
	```
	{: codeblock}

	當您在開發 Cordova 應用程式時，請使用原生 API 來啟用應用程式生命週期事件記錄。
 
- Web
		
	```
	// Disable recording of usage analytics (for example, to save disk space)
	// Recording is enabled by default
	BMSAnalytics.disable();
	// Enable recording of usage analytics
	BMSAnalytics.enable();
	// Send recorded usage analytics to the Mobile Analytics Service
	BMSAnalytics.send().then(function(result) {
         	//Handle Success here
        }, function(err) {
                //Handle Failure here
        });;
	```
	{: codeblock}
		
	用於記載事件的用法分析範例：
		
	```JavaScript
	// Log a custom analytics event
	var eventObject = {"customProperty": "propertyValue"}
	BMSAnalytics.log(eventObject)
	```
	{: codeblock}
  
## 啟用、配置及使用日誌程式

{{site.data.keyword.mobileanalytics_full}} Client SDK 提供的記載架構類似於您可能熟悉的其他記載架構，例如 `java.util.logging` 或 `log4j`。記載架構支援多個每一套件日誌程式實例、不同的記載層次、應用程式損毀的堆疊追蹤擷取等。

您也可以配置將記載的資料儲存至應用程式執行所在的裝置，並且於稍後將這些裝置日誌傳送至「{{site.data.keyword.mobileanalytics_short}} 服務」。

<!-- Initialization has to happen first to be able to collect logs and send them to the {{site.data.keyword.mobileanalytics_short}} service. -->

{{site.data.keyword.mobileanalytics_short}} Client SDK 記載架構支援下列具有建議使用準則的記載層次（依最不詳細到最詳細程度列出）：

  * `FATAL` - 用於無法復原的損毀或停滯。`FATAL` 層次是保留用於記載無法復原的錯誤，這對使用者而言就像應用程式損毀
  * `ERROR` - 用於非預期的異常狀況或非預期的網路通訊協定錯誤
  * `WARN` - 用來記載非視為嚴重錯誤的用法警告（例如使用已淘汰的 API 或慢速網路回應）
  * `INFO` - 用於報告起始設定事件以及可能重要但不緊急的其他資料
  * `DEBUG` - 用於報告除錯陳述式，以協助開發人員解決應用程式問題報告


### 記載層次情境
{: #log-level-scenario notoc}

日誌程式層次配置成 `FATAL` 時，日誌程式會擷取未捕捉的異常狀況，但不會擷取任何導致損毀事件的日誌。您可以設定更詳細的日誌程式層次，確保也一併擷取可能會導致 `FATAL` 日誌程式項目（例如 `WARN` 及 `ERROR`）的日誌。

日誌程式層次設定成 `DEBUG` 時，您還會取得 Mobile Analytics Client SDK 日誌，包括在傳送日誌時。

<!-- **Note:** Find full Logger API references for each platform at [SDKs, samples, API reference](sdks-samples-apis.html). The Logger API is part of the--> <!--{{site.data.keyword.mobileanalytics_short}} Client SDK Core. -->


### 日誌程式用法範例
{: #sample-logger-usage notoc}

**附註：**請先確定您已檢測應用程式以使用 {{site.data.keyword.mobileanalytics_short}} Client SDK，然後使用記載架構。
 
下列程式碼 Snippet 顯示「日誌程式」用法範例：

- Android
	
	```
	// Configure Logger to save logs to the device so that they 
	// can later be sent to the Mobile Analytics service
	// Disabled by default; set to true to enable
	Logger.storeLogs(true);
	// Change the minimum log level (optional)
	// The default setting is Logger.LEVEL.DEBUG
	Logger.setLogLevel(Logger.LEVEL.INFO);
	// Create two logger instances
	// You can create multiple log instances to organize your logs
	Logger logger1 = Logger.getLogger("logger1");
	Logger logger2 = Logger.getLogger("logger2");
	// Log messages with different levels
	// Debug message for feature 1
	// Info message for feature 2
	logger1.debug("debug message"); 
	//the logger1.debug message is not logged because the logLevelFilter is set to Info
	logger2.info("info message");
	// Send logs to the Mobile Analytics Service
	Logger.send(new ResponseListener() {
		@Override
                    public void onSuccess(Response response) {
                        // Handle Logger send success here.
                    }

                    @Override
            public void onFailure(Response response, Throwable throwable, JSONObject jsonObject) {
                // Handle Logger send failure here.
                    }
                });        
	```
	{: codeblock}


- iOS (Swift)
	
	```
	// Configure Logger to save logs to the device so that they 
	// can later be sent to the Mobile Analytics service
	// Disabled by default; set to true to enable
	Logger.isLogStorageEnabled = true
	// Change the minimum log level (optional)
	// The default setting is LogLevel.debug
	Logger.logLevelFilter = LogLevel.info
	// Create two logger instances
	// You can create multiple log instances to organize your logs
	let logger1 = Logger.logger(name: "feature1Logger")
	let logger2 = Logger.logger(name: "feature2Logger")
	// Log messages with different levels
	logger1.debug(message: "debug message for feature 1") 
	// The logger1.debug message is not logged because the
// logLevelFilter is set to info
logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service
Logger.send(completionHandler: { (response: Response?, error: Error?) in
	        if let response = response {
	        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
	    }
	})
	```
	{: codeblock}

	**提示：**基於隱私權考量，您可以針對使用發行模式建置的應用程式停用「日誌程式」輸出。「日誌程式」類別預設會將日誌列印至 Xcode 主控台。在您目標的建置設定中，將 `-D RELEASE_BUILD` 旗標新增至發行建置配置的**其他 Swift 旗標**區段。
    

- Cordova

	```
	// Enable persisting logs
	BMSLogger.storeLogs(true);
	// Set the minimum log level to be printed and persisted
	BMSLogger.setLogLevel(BMSLogger.INFO);
	var logger1 = BMSLogger.getLogger("logger1");
	var logger2 = BMSLogger.getLogger("logger2");   
	// Log messages with different levels
	logger1.debug ("debug message");
	logger2.info ("info message");
	// Send persisted logs to the {{site.data.keyword.mobileanalytics_short}} Service
	BMSLogger.send();
	BMSAnalytics.send();
	```
	{: codeblock}

- Web

	```
	// Enable persisting logs
	BMSAnalytics.Logger.storeLogs(true);
	// Set the minimum log level to be printed and persisted   
	// log levels in descending verbose level trace,debug,log,info,warn,error,fatal,analytics  
	BMSAnalytics.Logger.setLogLevel('error');
	// Log messages with different levels
	BMSAnalytics.Logger.debug ("debug message");
	BMSAnalytics.Logger.info ("info message");
	// Send persisted logs to the {{site.data.keyword.mobileanalytics_short}} Service
	BMSAnalytics.Logger.send();
	BMSAnalytics.send().then(function(result) {
		//Handle Success here
        }, function(err) {
         	//Handle Failure here
	 });;
	```
	{: codeblock}

<!--## Enabling the {{site.data.keyword.mobileanalytics_short}} Client SDK internal logs
{: #enable-logger-sdklogs notoc}

The {{site.data.keyword.mobileanalytics_short}} Client SDK provides a better development experience by not unnecessarily increasing the console output with its internal debug messages. Therefore, by default, internal log messages that are produced by the {{site.data.keyword.mobileanalytics_short}} SDK with the DEBUG level are not printed. You can enable printing of all internal log messages of the {{site.data.keyword.mobileanalytics_short}} Client SDK with the following API:

- Android
	```
	Logger.setSDKDebugLoggingEnabled(true);
	```
	{: codeblock}

- iOS - Swift
	```
	Logger.sdkDebugLoggingEnabled = true
	```
	{: codeblock}
-->

## 位置資料記載 
{: #location-logging}

可能會透過這個提供的 API 從應用程式記載行動裝置的位置。
```
Analytics.logLocation();
 
``` 
此 API 可讓應用程式將其位置以緯度、經度形式傳送至 appsession 之間的伺服器。除了位置記載 API SDK 的這個明確呼叫之外，還會針對起始 AppSession 環境定義及 userswitch AppSession（亦即，在應用程式階段作業之間切換使用者）環境定義，傳送每個 App-Session 的裝置位置。如前所述，起始設定 SDK 時，需要啟用位置 API。

**附註：**若要呼叫此位置記載 API，請在 SDK 起始設定中將 `collectLocation` 參數設為 true，如下所示。
```
Analytics.initialize(appName, apiKey,  hasUserContext, collectLocation, [BMSAnalytics.ALL])
		
```





## 進行網路要求
{: #network-requests}

您可以配置 {{site.data.keyword.mobileanalytics_short}} Client SDK 來[進行網路要求](/docs/mobile/sdk_network_request.html)。您務必要已經起始設定 `BMSClient` 及 `BMSAnalytics`，並且匯入 Client SDK。

<!--
#### Android
{: #android-network-requests notoc}

**Note:** This code snippet assumes that you have [imported the Client SDKs](#android-import).

```
public void makeGetCall(){
    Thread thread = new Thread(new Runnable() {
        @Override
        public void run() {
            try  {
                Request request = new Request("http://httpbin.org/get", "GET");
                    request.send(null, null);
            } catch (Exception e) {
                // Handle failure here.
            }
        }
    });
    thread.start();
}
```
	{: codeblock}

-->

<!-- 
#### Swift 3.0
{: #ios-network-requests notoc}

 ```Swift
 	// Make a network request
	let customResourceURL = "<your resource URL>"
	let request = Request(url: customResourceURL, method: HttpMethod.GET)

	let callBack:BMSCompletionHandler = {(response: Response?, error: Error?) in
   	if error == nil {
       	    print ("response:\(response?.responseText), no error")
    	  } else {
       	    print ("error: \(error)")
    	}
	}
	request.send(completionHandler: callBack)
 ```
	{: codeblock}
 
 -->

<!-- Commenting out bmsurlsession
```
// Make a network request
let urlSession = BMSURLSession(configuration: .default, delegate: nil, delegateQueue: nil)
var request = URLRequest(url: URL(string: "http://httpbin.org/get")!)
request.httpMethod = "GET"
request.allHTTPHeaderFields = ["foo":"bar"]

urlSession.dataTask(with: request) { (data: Data?, response: URLResponse?, error: Error?) in
    if let httpResponse = response as? HTTPURLResponse {
        logger.info(message: "Status code: \(httpResponse.statusCode)")
    }
    if data != nil, let responseString = String(data: data!, encoding: .utf8) {
        logger.info(message: "Response data: \(responseString)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
}.resume()
```
	{: codeblock}
-->

<!--
#### Swift 2.2
{: ios-swift22-network-requests}

```Swift
 	// Make a network request
	let customResourceURL = "<your resource URL>"
	let request = Request(url: customResourceURL, method: HttpMethod.GET)

	let callBack:BMSCompletionHandler = {(response: Response?, error: NSError?) in
   	if error == nil {
       	    print ("response:\(response?.responseText), no error")
    	  } else {
       	    print ("error: \(error)")
    	}
	}
	request.send(completionHandler: callBack)
 ```
	{: codeblock}
-->
<!--
```
// Make a network request
let urlSession = BMSURLSession(configuration: .defaultSessionConfiguration(), delegate: nil, delegateQueue: nil)
let request = NSMutableURLRequest(URL: NSURL(string: "http://httpbin.org/get")!)
request.HTTPMethod = "GET"
request.allHTTPHeaderFields = ["foo":"bar"]

urlSession.dataTaskWithRequest(request) { (data: NSData?, response: NSURLResponse?, error: NSError?) in
    if let httpResponse = response as? NSHTTPURLResponse {
        logger.info(message: "Status code: \(httpResponse.statusCode)")
    }
    if data != nil, let responseString = NSString(data: data!, encoding: NSUTF8StringEncoding) {
        logger.info(message: "Response data: \(responseString)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
}.resume()
```
	{: codeblock}
-->
<!--
#### Cordova
{: #cordova-network-requests notoc}

```
var success = function(data){
     console.log("success", data);
 }
 var failure = function(error)
     {console.log("failure", error);
 }
 var request = new BMSRequest("<your application route>", BMSRequest.GET);
 request.send(success, failure);
```
	{: codeblock}

-->


## 報告損毀分析
{: #report-crash-analytics}

將分析及日誌資訊傳送至 {{site.data.keyword.mobileanalytics_short}}，即可查看[應用程式損毀資料](app-monitoring.html#monitor-app-crash)。

`Analytics.send()` 方法會在**損毀**頁面上移入**損毀概觀**及**損毀**表格。本節中的圖表是使用起始設定並傳送分析處理程序所啟用；不需要特殊配置。

`Logger.send()` 方法會在**疑難排解**頁面上移入**損毀摘要**及**損毀詳細資料**表格。您必須讓應用程式儲存及傳送日誌以移入本節中的圖表，方法是在應用程式碼中新增其他陳述式：


- Android

	`Logger.storeLogs(true);`
	<!-- * `Logger.setLogLevel(Logger.LEVEL.FATAL); // or greater` -->
	
	請參閱 Android [日誌程式用法範例](sdk.html##sample-logger-usage)。


- iOS

	`Logger.isLogStorageEnabled = true`
	<!-- * `Logger.logLevelFilter = LogLevel.Fatal // or greater` -->
	
	請參閱 iOS [日誌程式用法範例](sdk.html##sample-logger-usage)。


- Cordova

	 `BMSLogger.storeLogs(true);`
	<!-- * `Logger.logLevelFilter = LogLevel.Fatal // or greater` -->

	請參閱 Cordova [日誌程式用法範例](sdk.html##sample-logger-usage)。

- Web

	`BMSAnalytics.Logger.storeLogs(true);`

## 追蹤作用中使用者
{: #trackingusers}

如果您的應用程式可以辨識裝置上的唯一使用者，則您可以選擇性地追蹤有多少位使用者正在積極使用您的應用程式，方法是將作用中使用者的使用者名稱傳遞至 {{site.data.keyword.mobileanalytics_short}}。 

使用 `hasUserContext=true` 來起始設定 {{site.data.keyword.mobileanalytics_short}}，以啟用使用者追蹤。否則，{{site.data.keyword.mobileanalytics_short}} 對於每台裝置只會擷取一位使用者。 


- Android

	請新增下列程式碼，以在使用者登入時進行追蹤：
	
	```
	Analytics.setUserIdentity("username");
```
	{: codeblock}
	
	<!--Add the following code for when the user logs out:
	
	```
	Analytics.clearUserIdentity();
	```
	{: codeblock}
	-->

- iOS (Swift)

	請新增下列程式碼，以在使用者登入時進行追蹤：
	
	```
	Analytics.userIdentity = "username"
```
	{: codeblock}
	
	<!--
	Add the following code for when the user logs out:
	
	```
	Analytics.userIdentity = nil
	```
	{: codeblock}
	-->


- Cordova
	
	請新增下列程式碼，以在使用者登入時進行追蹤：
	
	```
	BMSAnalytics.setUserIdentity("username");
```
	{: codeblock}

- Web

	請新增下列程式碼，以在使用者登入時進行追蹤：
	
	```
	BMSAnalytics.setUserIdentity("username");
```
	{: codeblock}

## 應用程式內意見分析
{: #In-App}

「應用程式內意見分析」可讓**使用者及測試者**將豐富的環境定義意見提供給應用程式擁有者。**應用程式擁有者**會根據應用程式用法，取得其使用者的即時意見。**開發人員**會根據即時洞察來實作變更。

呼叫下列 API 來檢測行動應用程式，以進入意見模式。 

- Android

    ```
    ImageButton feedback =(ImageButton)findViewById(R.id.Feedback);
        feedback.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Analytics.triggerFeedbackMode();
            }
        });
    ```	
	{: codeblock}
	
<!--## Configuring MobileFirst Platform Foundation servers to use the {{site.data.keyword.mobileanalytics_short}} service (optional)
{: #configmfp notoc}
  If you are using MobileFirst Platform Foundation Server V7 and higher, you can configure it to use the {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.mobileanalytics_short}} service to store analytics and logged data. 
  
  Configure MobileFirst Platform V7.0, V7.1, and V8.0 Beta servers to send analytics data to the {{site.data.keyword.mobileanalytics_short}} service on {{site.data.keyword.Bluemix_notm}}.

  1. Visit the dashboard for the {{site.data.keyword.mobileanalytics_short}} service where you want to send analytics data and note the browser URL for the dashboard.
  2. Determine the value for reporting analytics, as follows:
  	1. Get the {{site.data.keyword.mobileanalytics_short}} service route from the dashboard URL. The {{site.data.keyword.mobileanalytics_short}} service route is the part of the URL before ``/analytics/console/dashboard``.  

  		For example, if the dashboard URL is: `http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde`
      {: screen}

  		then the Analytics Service Route is
      `http://mobile-analytics-dashboard.ng.bluemix.net`
      {: screen}

  	2. Get the [Client API Key](#analytics-clientkey) value from the Analytics dashboard.

  	You will use the {{site.data.keyword.mobileanalytics_short}} service route and API Key to construct a URL to report analytics data, depending on the version of your MobileFirst Platform server. -->

<!--  3. Copy the URL for your {{site.data.keyword.mobileanalytics_short}} service dashboard. Include the `instanceId` query parameter, for example:
      `http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=1212345abcde`
      {: screen}

  4. Configure the values for the MobileFirst Platform server JNDI properties, as follows:-->

<!--    ### MobileFirst Platform V7.0
    {: #mfp70 notoc}

        The format of the `wl.analytics.url` JNDI property is: `<Analytics Service Route>/analytics-service/rest/data?tenant=<Client API Key>`
        {: screen}

        The `wl.analytics.console.url` JNDI property is the Analytics service dashboard URL.

        #### Example of MobileFirst Platform V7.0 JNDI property values
        {: #example-mfp70-jndi-values notoc}

        For a MobileFirst Platform project named `Myproj7000`, based on the examples in steps 2 and 3, replace the current JNDI property values in the `server.xml` file with:
        ```
        <jndiEntry jndiName="MyProj7000/wl.analytics.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/rest/data?tenant=12345abcde"'/>
        ```
        {: screen}
        ```
        <jndiEntry jndiName="MyProj7000/wl.analytics.console.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde"'/>
        ```
        {: screen}

    ### MobileFirst Platform V7.1
    {: #mfp71 notoc}

      The format of the `wl.analytics.url` JNDI property is: `<Analytics Service Route>/analytics-service/rest/v2/<Client API Key>`
      {: screen}

      #### Example of MobileFirst Platform JNDI V7.1 property values
      {: #example-mfp71-jndi-values notoc}

      For a MobileFirst Platform project named `MyProj7101`, replace the current JNDI property values in the `server.xml` file with:
      ```
      <jndiEntry jndiName="MyProj7101/wl.analytics.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/rest/v2/data?tenant=12345abcde"'/>
      ```
      {: screen}
      ```
      <jndiEntry jndiName="MyProj7101/wl.analytics.console.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde"'/>
      ```
      {: screen}

      ### MobileFirst Platform V8.0
      {: #mfp80 notoc}

      The format of the `mfp.analytics.url` JNDI property is:
      `<Analytics Service Route>/analytics-service/rest/v2/<Client API Key>`  

    #### Example MobileFirst Platform V8.0 Beta JNDI property values
      {: example-mfp80b-jndi-values}

      For a MobileFirst Platform project named `mfp`, which is the default, replace the current JNDI property values in the `server.xml` file with:
      ```
      <jndiEntry jndiName="mfp/mfp.analytics.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/rest/data?tenant=12345abcde"'/>
      ```
      {: screen}
      ```
      <jndiEntry jndiName="mfp/mfp.analytics.console.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde"'/>
      ```
      {: screen}
-->
<!-- #### Ignored events and event retention
{: #ignored-events notoc}
The {{site.data.keyword.mobileanalytics_short}} service saves the following data for ignored events and event retention:
  
<dl>	
<dt>Ignored events</dt>
	<dd>`ANALYTICS_SDK_CFG_IGNORE_AppPushAction: true`
		  `ANALYTICS_SDK_CFG_IGNORE_ServerLog: true`
		  `ANALYTICS_SDK_CFG_IGNORE_NetworkTransaction: true`
		  `ANALYTICS_SDK_CFG_IGNORE_NetworkTransactionSummarizedHourly: true`
		  `ANALYTICS_SDK_CFG_IGNORE_PushNotification: true`
		  `ANALYTICS_SDK_CFG_IGNORE_PushSubscription: true`</dd>
</dt>
<dt>Event retention</dd>
<dd>
`ANALYTICS_TTL_AlertNotification: 14d`<br>
`ANALYTICS_TTL_CustomData: 7d`<br>
`ANALYTICS_TTL_Device: 90d`<br>
`ANALYTICS_TTL_AppLog: 3d`<br>
`ANALYTICS_TTL_AppSession: 7d`
</dd>
</dt>
</dl>
  
-->


## 下一步
{: #what-to-do-next notoc}

您現在可以移至「{{site.data.keyword.mobileanalytics_short}} 主控台」來查看用量分析（例如，使用應用程式的新裝置及裝置總數）。您也可以<!--[creating custom charts](app-monitoring.html#custom-charts),-->[設定警示](app-monitoring.html#alerts)及[監視應用程式損毀](app-monitoring.html#monitor-app-crash)來監視應用程式。


# 相關鏈結
{: #rellinks notoc}

## API 參考資料
{: #api notoc}

* [REST API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobile-analytics-dashboard.{DomainName}/analytics-service/){:new_window}
