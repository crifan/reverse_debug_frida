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
        console.log("[-] "+methods[i]);
        console.log("\t[*] Hooking into implementation");
        //eval('var className2 = "'+className+'"; var funcName2 = "'+methods[i]+'"; var hook = eval(\'ObjC.classes.\'+className2+\'["\'+funcName2+\'"]\'); Interceptor.attach(hook.implementation, {   onEnter: function(args) {    console.log("[*] Detected call to: " + className2 + " -> " + funcName2);  } });');
        var className2 = className;
        var funcName2 = methods[i];
        hook_class_method(className2, funcName2);
        console.log("\t[*] Hooking successful");
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
[-] + phoneNumberSupportedWithCompletion:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - textField:shouldChangeCharactersInRange:replacementString:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - tableView:viewForHeaderInSection:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - textFieldShouldReturn:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _keyboardWillHide:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - titleLabel
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _keyboardWillShow:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - loadView
    [*] Hooking into implementation
    [*] Hooking successful
[-] - setUsername:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - sizeCategoryDidChange:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _hasValidCredentials
    [*] Hooking into implementation
    [*] Hooking successful
[-] - username
    [*] Hooking into implementation
    [*] Hooking successful
[-] - tableView:numberOfRowsInSection:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - viewDidLoad
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _tableView
    [*] Hooking into implementation
    [*] Hooking successful
[-] - delegate
    [*] Hooking into implementation
    [*] Hooking successful
[-] - authenticationContext
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _setEnabled:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _tableFooterView
    [*] Hooking into implementation
    [*] Hooking successful
[-] - viewWillAppear:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - tableView:willDisplayCell:forRowAtIndexPath:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - remoteUIController:shouldLoadRequest:redirectResponse:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - .cxx_destruct
    [*] Hooking into implementation
    [*] Hooking successful
[-] - numberOfSectionsInTableView:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - viewDidDisappear:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - traitCollectionDidChange:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - initWithCoder:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - tableView:heightForRowAtIndexPath:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - tableView:viewForFooterInSection:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - tableView:cellForRowAtIndexPath:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - setDelegate:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _tableHeaderView
    [*] Hooking into implementation
    [*] Hooking successful
[-] - tableView:shouldDrawTopSeparatorForSection:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - messageLabel
    [*] Hooking into implementation
    [*] Hooking successful
[-] - initWithNibName:bundle:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - dealloc
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _cancelBarButtonItem
    [*] Hooking into implementation
    [*] Hooking successful
[-] - viewDidLayoutSubviews
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _cancelPasswordDelegateIfNecessary
    [*] Hooking into implementation
    [*] Hooking successful
[-] - context:needsPasswordWithCompletion:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _passwordCell
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _textFieldDidChange:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _nextBarButtonItem
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _updateConstraintsForTraitCollection:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _beginObservingTextFieldDidChangeNotifications
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _beginObservingKeyboardWillShowNotifications
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _beginObservingSizeCategoryNotification
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _usernameCell
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _updateContentInsetWithHeight:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _endObservingSizeCategoryNotification
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _endObservingTextFieldDidChangeNotifications
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _endObservingKeyboardWillShowNotifications
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _akServiceType
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _shouldAnticipatePiggybacking
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _accountsHeaderView
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _cancelButtonSelected:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _nextButtonSelected:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - constrainView:toFillHeaderFooterView:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - allowsAccountCreation
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _actionButtonSelected:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - showServiceIcons
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _stringForFooter
    [*] Hooking into implementation
    [*] Hooking successful
[-] - privacyLinkIdentifiers
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _showOnlyPassword
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _isGreenTeaCapable
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _setUsernameCellWaiting:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _delegate_signInViewControllerDidCancel
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _attemptAuthentication
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _prewarmSignInFlowIfApplicable
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _presentForgotAppleIDPane
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _presentCreateAppleIDPane
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _attemptAuthenticationWithContext:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _isPasswordFieldVisible
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _isRunningInSettings
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _repairCloudAccountWithAuthenticationResults:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _delegate_signInViewControllerDidCompleteWithAuthenticationResults:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _authorizationValueForAuthenticationResults:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _passwordFieldIndexPath
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _setPasswordFieldHidden:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _setAkServiceType:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - _setShouldAnticipatePiggybacking:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - setAllowsAccountCreation:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - setShowServiceIcons:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - canEditUsername
    [*] Hooking into implementation
    [*] Hooking successful
[-] - setCanEditUsername:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - setPrivacyLinkIdentifiers:
    [*] Hooking into implementation
    [*] Hooking successful
[-] - showingPasswordCell
    [*] Hooking into implementation
    [*] Hooking successful
[-] - setShowingPasswordCell:
    [*] Hooking into implementation
    [*] Hooking successful
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
