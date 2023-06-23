# 单个类的所有函数

## hook类`AAUISignInViewController`的所有函数

### Frida命令

```log
frida -U -F -p 18533 -l ./hookAccountLogin_Interceptor.js
```

### js文件`hookAccountLogin_Interceptor.js`内容

```js
// https://raw.githubusercontent.com/noobpk/frida-ios-hook/master/frida-ios-hook/frida-scripts/hook-all-methods-of-specific-class.js
function hook_class_method(class_name, method_name)
{
    var hook = ObjC.classes[class_name][method_name];
        Interceptor.attach(hook.implementation, {
            onEnter: function(args) {
            console.log("[*] Detected call to: " + class_name + " -> " + method_name);
        }
    });
}

function run_hook_all_methods_of_specific_class(className_arg)
{
    console.log("[*] Started: Hook all methods of a specific class");
    console.log("[+] Class Name: " + className_arg);
    //Your class name here
    var className = className_arg;
    //var methods = ObjC.classes[className].$methods;
    var methods = ObjC.classes[className].$ownMethods;
    for (var i = 0; i < methods.length; i++)
    {
        console.log("  "+methods[i]);
        // console.log("\t[*] Hooking into implementation");
        //eval('var className2 = "'+className+'"; var funcName2 = "'+methods[i]+'"; var hook = eval(\'ObjC.classes.\'+className2+\'["\'+funcName2+\'"]\'); Interceptor.attach(hook.implementation, {   onEnter: function(args) {    console.log("[*] Detected call to: " + className2 + " -> " + funcName2);  } });');
        var className2 = className;
        var funcName2 = methods[i];
        hook_class_method(className2, funcName2);
        // console.log("\t[*] Hooking successful");
    }
    console.log("[*] Completed: Hook all methods of a specific class");
}

function hook_all_methods_of_specific_class(className_arg)
{
    setImmediate(run_hook_all_methods_of_specific_class,[className_arg])
}

//Your class name goes here
hook_all_methods_of_specific_class("AAUISignInViewController")
```

### 输出

最开始输出：

```log
✘ crifan@licrifandeMacBook-Pro  ~/dev/dev_root/iosReverse/AppleStore/Preferences_app/dynamicDebug/frida  frida -U -F -l ./hookAccountLogin_Interceptor.js -p 18533
     ____
    / _  |   Frida 16.0.10 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at https://frida.re/docs/home/
   . . . .
   . . . .   Connected to iPhone (id=abdc0dd961c3cb96f5c4afe109de4eb48b88433a)
Attaching...
[*] Started: Hook all methods of a specific class
[+] Class Name: AAUISignInViewController
  + phoneNumberSupportedWithCompletion:
  - textField:shouldChangeCharactersInRange:replacementString:
  - tableView:viewForHeaderInSection:
  - textFieldShouldReturn:
  - _keyboardWillHide:
  - titleLabel
  - _keyboardWillShow:
  - loadView
  - setUsername:
  - sizeCategoryDidChange:
  - _hasValidCredentials
  - username
  - tableView:numberOfRowsInSection:
  - viewDidLoad
  - _tableView
  - delegate
  - authenticationContext
  - _setEnabled:
  - _tableFooterView
  - viewWillAppear:
  - tableView:willDisplayCell:forRowAtIndexPath:
  - remoteUIController:shouldLoadRequest:redirectResponse:
  - .cxx_destruct
  - numberOfSectionsInTableView:
  - viewDidDisappear:
  - traitCollectionDidChange:
  - initWithCoder:
  - tableView:heightForRowAtIndexPath:
  - tableView:viewForFooterInSection:
  - tableView:cellForRowAtIndexPath:
  - setDelegate:
  - _tableHeaderView
  - tableView:shouldDrawTopSeparatorForSection:
  - messageLabel
  - initWithNibName:bundle:
  - dealloc
  - _cancelBarButtonItem
  - viewDidLayoutSubviews
  - _cancelPasswordDelegateIfNecessary
  - context:needsPasswordWithCompletion:
  - _passwordCell
  - _textFieldDidChange:
  - _nextBarButtonItem
  - _updateConstraintsForTraitCollection:
  - _beginObservingTextFieldDidChangeNotifications
  - _beginObservingKeyboardWillShowNotifications
  - _beginObservingSizeCategoryNotification
  - _usernameCell
  - _updateContentInsetWithHeight:
  - _endObservingSizeCategoryNotification
  - _endObservingTextFieldDidChangeNotifications
  - _endObservingKeyboardWillShowNotifications
  - _akServiceType
  - _shouldAnticipatePiggybacking
  - _accountsHeaderView
  - _cancelButtonSelected:
  - _nextButtonSelected:
  - constrainView:toFillHeaderFooterView:
  - allowsAccountCreation
  - _actionButtonSelected:
  - showServiceIcons
  - _stringForFooter
  - privacyLinkIdentifiers
  - _showOnlyPassword
  - _isGreenTeaCapable
  - _setUsernameCellWaiting:
  - _delegate_signInViewControllerDidCancel
  - _attemptAuthentication
  - _prewarmSignInFlowIfApplicable
  - _presentForgotAppleIDPane
  - _presentCreateAppleIDPane
  - _attemptAuthenticationWithContext:
  - _isPasswordFieldVisible
  - _isRunningInSettings
  - _repairCloudAccountWithAuthenticationResults:
  - _delegate_signInViewControllerDidCompleteWithAuthenticationResults:
  - _authorizationValueForAuthenticationResults:
  - _passwordFieldIndexPath
  - _setPasswordFieldHidden:
  - _setAkServiceType:
  - _setShouldAnticipatePiggybacking:
  - setAllowsAccountCreation:
  - setShowServiceIcons:
  - canEditUsername
  - setCanEditUsername:
  - setPrivacyLinkIdentifiers:
  - showingPasswordCell
  - setShowingPasswordCell:
[*] Completed: Hook all methods of a specific class
[iPhone::PID::18533 ]->
```

后续操作app页面，触发：

```log
...
[*] Detected call to: AAUISignInViewController -> - textField:shouldChangeCharactersInRange:replacementString:
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - textField:shouldChangeCharactersInRange:replacementString:
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - textField:shouldChangeCharactersInRange:replacementString:
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - textField:shouldChangeCharactersInRange:replacementString:
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - textField:shouldChangeCharactersInRange:replacementString:
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
```

继续操作app页面：输入账号，点击下一步按钮，输出：

```log
[*] Detected call to: AAUISignInViewController -> - _attemptAuthentication
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - authenticationContext
[*] Detected call to: AAUISignInViewController -> - _akServiceType
[*] Detected call to: AAUISignInViewController -> - _shouldAnticipatePiggybacking
[*] Detected call to: AAUISignInViewController -> - _isRunningInSettings
[*] Detected call to: AAUISignInViewController -> - _attemptAuthenticationWithContext:
[*] Detected call to: AAUISignInViewController -> - _setEnabled:
[*] Detected call to: AAUISignInViewController -> - textField:shouldChangeCharactersInRange:replacementString:
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - _textFieldDidChange:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - _keyboardWillShow:
[*] Detected call to: AAUISignInViewController -> - _updateContentInsetWithHeight:
[*] Detected call to: AAUISignInViewController -> - _keyboardWillHide:
[*] Detected call to: AAUISignInViewController -> - _updateContentInsetWithHeight:
[*] Detected call to: AAUISignInViewController -> - _setUsernameCellWaiting:
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _prewarmSignInFlowIfApplicable
[*] Detected call to: AAUISignInViewController -> - showServiceIcons
[*] Detected call to: AAUISignInViewController -> - context:needsPasswordWithCompletion:
[*] Detected call to: AAUISignInViewController -> - _setPasswordFieldHidden:
[*] Detected call to: AAUISignInViewController -> - _isPasswordFieldVisible
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - _passwordFieldIndexPath
[*] Detected call to: AAUISignInViewController -> - numberOfSectionsInTableView:
[*] Detected call to: AAUISignInViewController -> - numberOfSectionsInTableView:
[*] Detected call to: AAUISignInViewController -> - tableView:numberOfRowsInSection:
[*] Detected call to: AAUISignInViewController -> - _showOnlyPassword
[*] Detected call to: AAUISignInViewController -> - tableView:heightForRowAtIndexPath:
[*] Detected call to: AAUISignInViewController -> - tableView:heightForRowAtIndexPath:
[*] Detected call to: AAUISignInViewController -> - tableView:cellForRowAtIndexPath:
[*] Detected call to: AAUISignInViewController -> - _passwordFieldIndexPath
[*] Detected call to: AAUISignInViewController -> - _passwordCell
[*] Detected call to: AAUISignInViewController -> - tableView:shouldDrawTopSeparatorForSection:
[*] Detected call to: AAUISignInViewController -> - tableView:heightForRowAtIndexPath:
[*] Detected call to: AAUISignInViewController -> - tableView:willDisplayCell:forRowAtIndexPath:
[*] Detected call to: AAUISignInViewController -> - tableView:shouldDrawTopSeparatorForSection:
[*] Detected call to: AAUISignInViewController -> - _setEnabled:
[*] Detected call to: AAUISignInViewController -> - _hasValidCredentials
[*] Detected call to: AAUISignInViewController -> - _setUsernameCellWaiting:
[*] Detected call to: AAUISignInViewController -> - tableView:shouldDrawTopSeparatorForSection:
[*] Detected call to: AAUISignInViewController -> - tableView:shouldDrawTopSeparatorForSection:
[*] Detected call to: AAUISignInViewController -> - _keyboardWillShow:
[*] Detected call to: AAUISignInViewController -> - _updateContentInsetWithHeight:
[*] Detected call to: AAUISignInViewController -> - _keyboardWillShow:
[*] Detected call to: AAUISignInViewController -> - _updateContentInsetWithHeight:
```
