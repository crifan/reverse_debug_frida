# 所有类的所有函数

* 注意：下面代码，没有真正生效，实现我们希望的，hook所有的类的所有的方法
  * 只是记录目前的效果，供参考。


## frida命令

```bash
frida -U -N com.apple.Preferences -l hookAccountLogin_Interceptor_allClassAllMethod.js 
```

## js脚本文件`hookAccountLogin_Interceptor_allClassAllMethod.js`内容

```js
// https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/hook-all-methods-of-all-classes-app-only.js

function get_timestamp()
{
	var today = new Date();
	var timestamp = today.getFullYear() + '-' + (today.getMonth()+1) + '-' + today.getDate() + ' ' + today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds() + ":" + today.getMilliseconds();
	return timestamp;
}

function hook_class_method(class_name, method_name)
{
	var hook = ObjC.classes[class_name][method_name];
		Interceptor.attach(hook.implementation, {
			onEnter: function(args) {
			console.log("[*] [" + get_timestamp() + " ] Detected call to: " + class_name + " -> " + method_name);
		}
	});
}

function run_hook_all_methods_of_classes_app_only(verbose)
{
	console.log("[*] Started: Hook all methods of all app only classes");
	var free = new NativeFunction(Module.findExportByName(null, 'free'), 'void', ['pointer'])
    var copyClassNamesForImage = new NativeFunction(Module.findExportByName(null, 'objc_copyClassNamesForImage'), 'pointer', ['pointer', 'pointer'])
    var p = Memory.alloc(Process.pointerSize)
    Memory.writeUInt(p, 0)
    var path = ObjC.classes.NSBundle.mainBundle().executablePath().UTF8String()
    var pPath = Memory.allocUtf8String(path)
    var pClasses = copyClassNamesForImage(pPath, p)
    var count = Memory.readUInt(p)
    var classesArray = new Array(count)
    for (var i = 0; i < count; i++)
    {
        var pClassName = Memory.readPointer(pClasses.add(i * Process.pointerSize))
        classesArray[i] = Memory.readUtf8String(pClassName)
		var className = classesArray[i]
		if (ObjC.classes.hasOwnProperty(className))
		{
			//console.log("[+] Class: " + className);
			//var methods = ObjC.classes[className].$methods;
			var methods = ObjC.classes[className].$ownMethods;
			for (var j = 0; j < methods.length; j++)
			{
				try
				{
					var className2 = className;
					var funcName2 = methods[j];
					//console.log("[-] Method: " + methods[j]);
					hook_class_method(className2, funcName2);
					if (verbose)
						console.log("[*] [" + get_timestamp() + "] Hooking successful: " + className2 + " -> " + funcName2);
				}
				catch(err)
				{
					console.log("[*] [" + get_timestamp() + "] Hooking Error: " + err.message);
				}
			}
		}
    }
    free(pClasses)
	console.log("[*] Completed: Hook all methods of all app only classes");
}

function hook_all_methods_of_classes_app_only()
{
	setImmediate(run_hook_all_methods_of_classes_app_only,[false])
}

function hook_all_methods_of_classes_app_only(verbose)
{
	setImmediate(run_hook_all_methods_of_classes_app_only,[true])
}

hook_all_methods_of_classes_app_only()
//hook_all_methods_of_classes_app_only("verbose") //for verbose output
```

## 输出

```bash
➜  scripts frida -U -N com.apple.Preferences -l hookAccountLogin_Interceptor_allClassAllMethod.js 
     ____
    / _  |   Frida 16.0.19 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at https://frida.re/docs/home/
   . . . .
   . . . .   Connected to iPhone (id=abdc0dd961c3cb96f5c4afe109de4eb48b88433a)
[*] Started: Hook all methods of all app only classes
[*] [2023-6-23 15:28:28:851] Hooking successful: PreferencesAppController -> - rootController
[*] [2023-6-23 15:28:28:852] Hooking successful: PreferencesAppController -> - appWindow
[*] [2023-6-23 15:28:28:853] Hooking successful: PreferencesAppController -> - profileConnectionDidReceiveProfileListChangedNotification:userInfo:
[*] [2023-6-23 15:28:28:854] Hooking successful: PreferencesAppController -> - profileConnectionDidReceiveRestrictionChangedNotification:userInfo:
[*] [2023-6-23 15:28:28:855] Hooking successful: PreferencesAppController -> - profileConnectionDidReceiveEffectiveSettingsChangedNotification:userInfo:
[*] [2023-6-23 15:28:28:856] Hooking successful: PreferencesAppController -> - profileConnectionDidReceivePasscodeChangedNotification:userInfo:
[*] [2023-6-23 15:28:28:858] Hooking successful: PreferencesAppController -> - clearControllersForSuspendedState
[*] [2023-6-23 15:28:28:858] Hooking successful: PreferencesAppController -> - popToRootOfSettingsSelectGeneral:
[*] [2023-6-23 15:28:28:859] Hooking successful: PreferencesAppController -> - popToRootOfSettingsSelectGeneral:performWithoutDeferringTransitions:
[*] [2023-6-23 15:28:28:860] Hooking successful: PreferencesAppController -> - runTest:options:
[*] [2023-6-23 15:28:28:862] Hooking successful: PreferencesAppController -> - runLoadingTest:withOptions:
[*] [2023-6-23 15:28:28:863] Hooking successful: PreferencesAppController -> - runScrollTest:withOptions:
[*] [2023-6-23 15:28:28:864] Hooking successful: PreferencesAppController -> - runScrollTest:withDelay:withOptions:
[*] [2023-6-23 15:28:28:866] Hooking successful: PreferencesAppController -> - performRecapScrollTest:withOptions:scrollView:
[*] [2023-6-23 15:28:28:867] Hooking successful: PreferencesAppController -> - updateSoftwareUpdateBadgeOnSpecifier:
[*] [2023-6-23 15:28:28:868] Hooking successful: PreferencesAppController -> - _registerForAccessoryNotifications
[*] [2023-6-23 15:28:28:869] Hooking successful: PreferencesAppController -> - application:didFinishLaunchingWithOptions:
[*] [2023-6-23 15:28:28:871] Hooking successful: PreferencesAppController -> - processStateRestorationURL
[*] [2023-6-23 15:28:28:871] Hooking successful: PreferencesAppController -> - application:configurationForConnectingSceneSession:options:
[*] [2023-6-23 15:28:28:873] Hooking successful: PreferencesAppController -> - receivedApplicationWillConnectWindowScene:
[*] [2023-6-23 15:28:28:874] Hooking successful: PreferencesAppController -> - quickActionShortcutItems
[*] [2023-6-23 15:28:28:875] Hooking successful: PreferencesAppController -> - receivedApplicationWillTerminateNotification:
[*] [2023-6-23 15:28:28:884] Hooking successful: PreferencesAppController -> - receivedApplicationDidEnterBackground:
[*] [2023-6-23 15:28:28:886] Hooking successful: PreferencesAppController -> - receivedApplicationWillEnterForegroundNotification:
[*] [2023-6-23 15:28:28:887] Hooking successful: PreferencesAppController -> - receivedApplicationDidBecomeActive:
[*] [2023-6-23 15:28:28:888] Hooking successful: PreferencesAppController -> - receivedApplicationWillResignActiveNotification:
[*] [2023-6-23 15:28:28:890] Hooking successful: PreferencesAppController -> - receivedApplicationContinueUserActivity:
[*] [2023-6-23 15:28:28:890] Hooking successful: PreferencesAppController -> - receivedApplicationResumeNotification:
[*] [2023-6-23 15:28:28:892] Hooking successful: PreferencesAppController -> - receivedApplicationOpenURLNotification:
[*] [2023-6-23 15:28:28:893] Hooking successful: PreferencesAppController -> - processURL:
[*] [2023-6-23 15:28:28:894] Hooking successful: PreferencesAppController -> - processURL:animated:fromSearch:
[*] [2023-6-23 15:28:28:895] Hooking successful: PreferencesAppController -> - processURL:animated:fromSearch:withCompletion:
[*] [2023-6-23 15:28:28:896] Hooking successful: PreferencesAppController -> - _rationalizeAccountSettingsURLDictionary:
[*] [2023-6-23 15:28:28:898] Hooking successful: PreferencesAppController -> - handleAuthKitURLIfNeededFromResourceDictionary:overViewController:
[*] [2023-6-23 15:28:28:898] Hooking successful: PreferencesAppController -> - processAppPrefsURL:
[*] [2023-6-23 15:28:28:899] Hooking successful: PreferencesAppController -> - processShorcutsAppPrefsURL:
[*] [2023-6-23 15:28:28:900] Hooking successful: PreferencesAppController -> - processPrebuddyURL:
[*] [2023-6-23 15:28:28:901] Hooking successful: PreferencesAppController -> - processTVProviderPrefsURL:
[*] [2023-6-23 15:28:28:902] Hooking successful: PreferencesAppController -> - processOtpauthURL:
[*] [2023-6-23 15:28:28:903] Hooking successful: PreferencesAppController -> - generateURL
[*] [2023-6-23 15:28:28:904] Hooking successful: PreferencesAppController -> - _clearSavedPositionURL
[*] [2023-6-23 15:28:28:905] Hooking successful: PreferencesAppController -> - oneTimeJumpURL
[*] [2023-6-23 15:28:28:907] Hooking successful: PreferencesAppController -> - addBusyControllersFromRootController:
[*] [2023-6-23 15:28:28:908] Hooking successful: PreferencesAppController -> - endAndInvalidateBackgroundTask
[*] [2023-6-23 15:28:28:909] Hooking successful: PreferencesAppController -> - controllerFinished:
[*] [2023-6-23 15:28:28:910] Hooking successful: PreferencesAppController -> - _handleBusyControllers
[*] [2023-6-23 15:28:28:911] Hooking successful: PreferencesAppController -> - _suspendControllers
[*] [2023-6-23 15:28:28:912] Hooking successful: PreferencesAppController -> - _handleExternalApplicationChange
[*] [2023-6-23 15:28:28:913] Hooking successful: PreferencesAppController -> - _handleIconChangeForBundleID:
[*] [2023-6-23 15:28:28:914] Hooking successful: PreferencesAppController -> - systemDidWake:
[*] [2023-6-23 15:28:28:915] Hooking successful: PreferencesAppController -> - significantTimeChange
[*] [2023-6-23 15:28:28:916] Hooking successful: PreferencesAppController -> - resetLocale
[*] [2023-6-23 15:28:28:917] Hooking successful: PreferencesAppController -> - defaultImageSnapshotExpiration
[*] [2023-6-23 15:28:28:918] Hooking successful: PreferencesAppController -> - dealloc
[*] [2023-6-23 15:28:28:919] Hooking successful: PreferencesAppController -> - _registerForPowerNotifications
[*] [2023-6-23 15:28:28:920] Hooking successful: PreferencesAppController -> - _deregisterForPowerNotifications
[*] [2023-6-23 15:28:28:921] Hooking successful: PreferencesAppController -> - _accessoryDidConnect:
[*] [2023-6-23 15:28:28:922] Hooking successful: PreferencesAppController -> - accessoryDidDisconnect:
[*] [2023-6-23 15:28:28:923] Hooking successful: PreferencesAppController -> - splitViewController:separateSecondaryViewControllerFromPrimaryViewController:
[*] [2023-6-23 15:28:28:924] Hooking successful: PreferencesAppController -> - splitViewController:collapseSecondaryViewController:ontoPrimaryViewController:
[*] [2023-6-23 15:28:28:925] Hooking successful: PreferencesAppController -> - splitViewControllerDidPopToRootController:
[*] [2023-6-23 15:28:28:926] Hooking successful: PreferencesAppController -> - carrierBundleChange:
[*] [2023-6-23 15:28:28:927] Hooking successful: PreferencesAppController -> - rootDomainConnect
[*] [2023-6-23 15:28:28:929] Hooking successful: PreferencesAppController -> - setRootDomainConnect:
[*] [2023-6-23 15:28:28:930] Hooking successful: PreferencesAppController -> - busyControllers
[*] [2023-6-23 15:28:28:931] Hooking successful: PreferencesAppController -> - setBusyControllers:
[*] [2023-6-23 15:28:28:931] Hooking successful: PreferencesAppController -> - client
[*] [2023-6-23 15:28:28:932] Hooking successful: PreferencesAppController -> - setClient:
[*] [2023-6-23 15:28:28:933] Hooking successful: PreferencesAppController -> - urlManager
[*] [2023-6-23 15:28:28:934] Hooking successful: PreferencesAppController -> - setUrlManager:
[*] [2023-6-23 15:28:28:935] Hooking successful: PreferencesAppController -> - windowSceneClass
[*] [2023-6-23 15:28:28:936] Hooking successful: PreferencesAppController -> - setWindowSceneClass:
[*] [2023-6-23 15:28:28:937] Hooking successful: PreferencesAppController -> - scene
[*] [2023-6-23 15:28:28:938] Hooking successful: PreferencesAppController -> - setScene:
[*] [2023-6-23 15:28:28:939] Hooking successful: PreferencesAppController -> - accountEnumerator
[*] [2023-6-23 15:28:28:940] Hooking successful: PreferencesAppController -> - setAccountEnumerator:
[*] [2023-6-23 15:28:28:941] Hooking successful: PreferencesAppController -> - _uuidRegex
[*] [2023-6-23 15:28:28:942] Hooking successful: PreferencesAppController -> - set_uuidRegex:
[*] [2023-6-23 15:28:28:943] Hooking successful: PreferencesAppController -> - .cxx_destruct
[*] [2023-6-23 15:28:28:945] Hooking successful: PreferenceSceneDelegate -> - scene:willConnectToSession:options:
[*] [2023-6-23 15:28:28:946] Hooking successful: PreferenceSceneDelegate -> - scene:openURLContexts:
[*] [2023-6-23 15:28:28:947] Hooking successful: PreferenceSceneDelegate -> - scene:continueUserActivity:
[*] [2023-6-23 15:28:28:948] Hooking successful: PreferenceSceneDelegate -> - windowScene:performActionForShortcutItem:completionHandler:
[*] Completed: Hook all methods of all app only classes
[iPhone::com.apple.Preferences ]->
```

后续操作Preferences的页面，但是却没有更多的输出。

暂时就这样，有空再深究。
