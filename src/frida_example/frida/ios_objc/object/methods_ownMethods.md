# $methods和$ownMethods

## `NSXPCConnection`

对于`ObjC`的`NSXPCConnection`的实例对象：

```bash
argSelfObj:  <NSXPCConnection: 0x154d09250> connection from pid 46847 on mach service named com.apple.ak.auth.xpc
```

### `NSXPCConnection`的`$methods`

代码：

```js
const connMethods = argSelfObj.$methods;
console.log("connMethods=", connMethods);
```

输出：

```bash
connMethods= + geo_createGEODaemonToMapsPushDaemonConnection,+ geo_withMapsPushDaemon:errorHandler:,+ beginTransaction,+ endTransaction,+ _handoffCurrentReplyToQueue:block:,+ currentConnection,+ fromPBCodable:,+ CKSQLiteClassName,+ CA_automaticallyNotifiesObservers:,+ CA_setterForProperty:,+ CA_getterForProperty:,+ CA_encodesPropertyConditionally:type:,+ CA_CAMLPropertyForKey:,+ version,+ load,+ implementsSelector:,+ instancesImplementSelector:,+ setVersion:,+ accessInstanceVariablesDirectly,+ automaticallyNotifiesObserversForKey:,+ _createMutableOrderedSetValueGetterWithContainerClassID:key:,+ cancelPreviousPerformRequestsWithTarget:selector:object:,+ _createMutableArrayValueGetterWithContainerClassID:key:,+ setKeys:triggerChangeNotificationsForDependentKey:,+ _createMutableSetValueGetterWithContainerClassID:key:,+ _createOtherValueGetterWithContainerClassID:key:,+ _createOtherValueSetterWithContainerClassID:key:,+ _createValuePrimitiveGetterWithContainerClassID:key:,+ _createValuePrimitiveSetterWithContainerClassID:key:,+ replacementObjectForPortCoder:,+ cancelPreviousPerformRequestsWithTarget:,+ _createValueGetterWithContainerClassID:key:,+ _createValueSetterWithContainerClassID:key:,+ _shouldAddObservationForwardersForKey:,+ _keysForValuesAffectingValueForKey:,+ classForKeyedUnarchiver,+ keyPathsForValuesAffectingValueForKey:,+ classFallbacksForKeyedArchiver,+ SFSQLiteClassName,+ _copyDescription,+ doesNotRecognizeSelector:,+ __allocWithZone_OA:,+ instanceMethodSignatureForSelector:,+ methodSignatureForSelector:,+ init,+ description,+ dealloc,+ supportsBSXPCSecureCoding,+ bs_isPlistableType,+ bs_dataFromObject:,+ bs_secureObjectFromData:ofClass:,+ bs_secureObjectFromData:ofClasses:,+ bs_objectFromData:,+ bs_decodedFromData:,+ bs_secureDataFromObject:,+ bs_secureDecodedFromData:,+ bs_secureDecodedFromData:withAdditionalClasses:,+ supportsRBSXPCSecureCoding,+ new,+ self,+ allocWithZone:,+ superclass,+ initialize,+ performSelector:withObject:,+ performSelector:withObject:withObject:,+ performSelector:,+ zone,+ autorelease,+ isSubclassOfClass:,+ _isDeallocating,+ isMemberOfClass:,+ allowsWeakReference,+ alloc,+ isEqual:,+ isKindOfClass:,+ retainWeakReference,+ retain,+ retainCount,+ instancesRespondToSelector:,+ hash,+ mutableCopyWithZone:,+ forwardingTargetForSelector:,+ resolveInstanceMethod:,+ mutableCopy,+ methodForSelector:,+ instanceMethodForSelector:,+ release,+ respondsToSelector:,+ isAncestorOfObject:,+ _tryRetain,+ debugDescription,+ conformsToProtocol:,+ class,+ resolveClassMethod:,+ copy,+ copyWithZone:,+ forwardInvocation:,+ isProxy,+ isFault,+ toPBCodable,+ _intents_compareValue:relation:,+ _intents_readableSubtitleWithLocalizer:metadata:,+ descriptionAtIndent:,+ _intents_enumerateObjectsOfClass:withBlock:,+ _intents_isValidKey:,+ _setterForProperty:,+ _intents_indexingRepresentation,+ _intents_readableTitleWithLocalizer:metadata:,+ _intents_readableTitleForLanguage:withMetadata:,+ _intents_readableSubtitleForLanguage:withMetadata:,+ _intents_displayImageWithLocalizer:,+ _intents_localizedCopyWithLocalizer:,+ _intents_readableTitleForLanguage:,+ _intents_readableTitleWithLocalizer:,+ _intents_readableSubtitleWithLocalizer:,+ _intents_readableSubtitleForLanguage:,+ _intents_displayImageForLanguage:,+ _intents_setterForPropertyWithName:,+ _intents_localizedCopyForLanguage:,+ _intents_readableDescriptionForLanguage:,+ _intents_readableDescriptionForLanguage:withMetadata:,+ _intents_readableDescriptionWithLocalizer:,+ _intents_readableDescriptionWithLocalizer:metadata:,+ if_setValueIfNonNil:forKey:,+ if_setValueIfYES:forKey:,+ stringValueSafe:,+ utf8ValueSafe:,+ boolValueSafe,+ boolValueSafe:,+ int64ValueSafe,+ int64ValueSafe:,+ doubleValueSafe,+ doubleValueSafe:,+ stringValueSafe,+ utf8ValueSafe,+ CKDescriptionPropertiesWithPublic:private:shouldExpand:,+ CKDescription,+ CKUnredactedDescription,+ CKRedactedDescription,+ CKDescriptionRedact:avoidShortDescription:,+ CKObjectDescriptionRedact:avoidShortDescription:,+ CKObjectDescriptionRedact:,+ ck_bindInStatement:atIndex:,+ CKAssignToContainerWithID:,+ CKSingleLineDescription,+ _CKDescriptionWithExpansion:,+ CKExpandedDescription,+ CKPropertiesDescriptionStringFromProperties:,+ CKHashedDescription,+ CKPropertiesDescription,+ CA_validateValue:forKey:,+ CA_copyRenderValue,+ CAMLTypeForKey:,+ encodeWithCAMLWriter:,+ CA_distanceToValue:,+ CA_interpolateValues:::interpolator:,+ CA_interpolateValue:byFraction:,+ CAMLType,+ CA_copyNumericValue:,+ CA_archivingValueForKey:,+ CA_copyRenderValueWithColorspace:,+ CA_addValue:multipliedBy:,+ CA_roundToIntegerFromValue:,+ CAMLTypeSupportedForKey:,+ CA_prepareRenderValue,+ carpf_registerObserverForKeyPath:options:handler:,+ _allowsDirectEncoding,+ awakeAfterUsingCoder:,+ classForCoder,+ replacementObjectForCoder:,+ removeObserver:forKeyPath:,+ removeObservation:,+ _isKVOA,+ _changeValueForKeys:count:maybeOldValuesDict:maybeNewValuesDict:usingBlock:,+ setObservationInfo:,+ willChangeValueForKey:,+ _didChangeValuesForKeys:,+ _willChangeValuesForKeys:,+ valueForUndefinedKey:,+ addObserver:forKeyPath:options:context:,+ setValue:forKey:,+ performSelectorOnMainThread:withObject:waitUntilDone:modes:,+ performSelector:onThread:withObject:waitUntilDone:modes:,+ dictionaryWithValuesForKeys:,+ didChangeValueForKey:withSetMutation:usingObjects:,+ valueForKeyPath:,+ performSelector:onThread:withObject:waitUntilDone:,+ classForKeyedArchiver,+ _notifyObserversOfChangeFromValuesForKeys:toValuesForKeys:,+ _observerStorageOfSize:,+ willChangeValueForKey:withSetMutation:usingObjects:,+ performSelectorOnMainThread:withObject:waitUntilDone:,+ didChangeValueForKey:,+ setNilValueForKey:,+ _pendingChangeNotificationsArrayForKey:create:,+ removeObservation:forObservableKeyPath:,+ performSelector:withObject:afterDelay:inModes:,+ classForPortCoder,+ performSelectorInBackground:withObject:,+ finishObserving,+ _addObserver:forProperty:options:context:,+ observationInfo,+ willChange:valuesAtIndexes:forKey:,+ classForArchiver,+ _notifyObserversForKeyPath:change:,+ _changeValueForKey:key:key:usingBlock:,+ addObserverBlock:,+ _observerStorage,+ setObservation:forObservingKeyPath:,+ mutableOrderedSetValueForKeyPath:,+ addObserver:forObservableKeyPath:,+ performSelector:object:afterDelay:,+ setValuesForKeysWithDictionary:,+ performSelector:withObject:afterDelay:,+ didChange:valuesAtIndexes:forKey:,+ _destroyObserverList,+ addChainedObservers:,+ valueForKey:,+ validateValue:forKeyPath:error:,+ mutableArrayValueForKeyPath:,+ _receiveBox:,+ receiveObservedError:,+ mutableSetValueForKeyPath:,+ _isToManyChangeInformation,+ replacementObjectForKeyedArchiver:,+ _overrideUseFastBlockObservers,+ addObservationTransformer:,+ addObserver:,+ mutableSetValueForKey:,+ _changeValueForKey:usingBlock:,+ mutableArrayValueForKey:,+ _didEndKeyValueObserving,+ _implicitObservationInfo,+ replacementObjectForArchiver:,+ _removeObserver:forProperty:,+ validateValue:forKey:error:,+ autoContentAccessingProxy,+ observeValueForKeyPath:ofObject:change:context:,+ _willBeginKeyValueObserving,+ mutableOrderedSetValueForKey:,+ removeObserver:forKeyPath:context:,+ setValue:forKeyPath:,+ receiveObservedValue:,+ setValue:forUndefinedKey:,+ isNSSet__,+ isNSString__,+ isNSDate__,+ isNSNumber__,+ isNSData__,+ isNSObject__,+ isNSArray__,+ isNSValue__,+ isNSTimeZone__,+ isNSOrderedSet__,+ isNSDictionary__,+ _cfTypeID,+ isNSCFConstantString__,+ __release_OA,+ __retain_OA,+ ___tryRetain_OA,+ __dealloc_zombie,+ __autorelease_OA,+ bs_secureEncoded,+ bs_encoded,+ NSRepresentation,+ RBSIsXPCObject,+ pep_onMainThread,+ pep_onThread:,+ pep_onThread:immediateForMatchingThread:,+ pep_onOperationQueue:priority:,+ pep_onMainThreadIfNecessary,+ pep_afterDelay:,+ pep_onOperationQueue:,+ pep_getInvocation:,+ isNull,+ performSelector:withObject:afterDelay:ignoreMenuTracking:,+ __im_afterDelay:,+ __im_afterDelay:modes:,+ __im_onThread:,+ __im_onMainThread,+ __im_onThread:immediateForMatchingThread:,+ __im_onMainThreadIfNecessary,+ __im_onDetachedThread,+ __im_getInvocation:,+ un_safeBoolValue,+ finalize,- cuValueForEntitlementNoCache:,- uniquify,- hasEntitlement:,- shouldHandleInvalidation,- setShouldHandleInvalidation:,- _EX_transaction,- _EX_setTransaction:,- _EX_extensionProcess,- _EX_setExtensionProcess:,- addBarrierBlock:,- serviceName,- initWithMachServiceName:options:,- _sendInvocation:orArguments:count:methodSignature:selector:withProxy:,- setUserInfo:,- setOptions:,- effectiveGroupIdentifier,- setExportedObject:,- remoteObjectProxy,- userInfo,- interruptionHandler,- start,- remoteObjectProxyWithTimeout:errorHandler:,- _setQueue:,- _decodeAndInvokeReplyBlockWithEvent:sequence:replyInfo:,- _setUUID:,- initWithMachServiceName:,- invalidate,- exportedObject,- initWithServiceName:options:,- exportedInterface,- activate,- _xpcConnection,- init,- invalidationHandler,- _queue,- setRemoteObjectInterface:,- _unboostingRemoteObjectProxy,- setExportedInterface:,- initWithEndpoint:,- synchronousRemoteObjectProxyWithErrorHandler:,- _sendSelector:withProxy:arg1:arg2:arg3:arg4:,- _decodeAndInvokeMessageWithEvent:flags:,- _sendSelector:withProxy:arg1:arg2:arg3:,- _initWithRemoteService:name:options:mode:,- delegate,- set_additionalInvalidationHandler:,- _initWithRemoteService:name:options:,- resume,- _setLanguages:,- _decodeProgressMessageWithData:flags:,- remoteObjectProxyWithUserInfo:errorHandler:,- valueForEntitlement:,- _pauseProgress:,- initWithServiceName:,- auditSessionIdentifier,- setInterruptionHandler:,- setInvalidationHandler:,- _cancelProgress:,- _resumeProgress:,- _killConnection:,- _sendSelector:withProxy:arg1:arg2:,- _errorDescription,- suspend,- stop,- replacementObjectForEncoder:object:,- _sendDesistForProxy:,- remoteObjectProxyWithErrorHandler:,- _initWithRemoteConnection:name:,- effectiveUserIdentifier,- _additionalInvalidationHandler,- _sendSelector:withProxy:arg1:,- _remoteObjectInterfaceClass,- _sendSelector:withProxy:,- _setBootstrapObject:forKey:,- scheduleSendBarrierBlock:,- _initWithCustomTransport:,- auditToken,- _setTargetUserIdentifier:,- remoteObjectInterface,- setDelegate:,- description,- initWithListenerEndpoint:,- dealloc,- processIdentifier,- endpoint,- toPBCodable,- _intents_compareValue:relation:,- _intents_readableSubtitleWithLocalizer:metadata:,- descriptionAtIndent:,- _intents_enumerateObjectsOfClass:withBlock:,- _intents_isValidKey:,- _setterForProperty:,- _intents_indexingRepresentation,- _intents_readableTitleWithLocalizer:metadata:,- _intents_readableTitleForLanguage:withMetadata:,- _intents_readableSubtitleForLanguage:withMetadata:,- _intents_displayImageWithLocalizer:,- _intents_localizedCopyWithLocalizer:,- _intents_readableTitleForLanguage:,- _intents_readableTitleWithLocalizer:,- _intents_readableSubtitleWithLocalizer:,- _intents_readableSubtitleForLanguage:,- _intents_displayImageForLanguage:,- _intents_setterForPropertyWithName:,- _intents_localizedCopyForLanguage:,- _intents_readableDescriptionForLanguage:,- _intents_readableDescriptionForLanguage:withMetadata:,- _intents_readableDescriptionWithLocalizer:,- _intents_readableDescriptionWithLocalizer:metadata:,- if_setValueIfNonNil:forKey:,- if_setValueIfYES:forKey:,- stringValueSafe:,- utf8ValueSafe:,- boolValueSafe,- boolValueSafe:,- int64ValueSafe,- int64ValueSafe:,- doubleValueSafe,- doubleValueSafe:,- stringValueSafe,- utf8ValueSafe,- CKDescriptionPropertiesWithPublic:private:shouldExpand:,- CKDescription,- CKUnredactedDescription,- CKRedactedDescription,- CKDescriptionRedact:avoidShortDescription:,- CKObjectDescriptionRedact:avoidShortDescription:,- CKObjectDescriptionRedact:,- ck_bindInStatement:atIndex:,- CKAssignToContainerWithID:,- CKSingleLineDescription,- _CKDescriptionWithExpansion:,- CKExpandedDescription,- CKPropertiesDescriptionStringFromProperties:,- CKHashedDescription,- CKPropertiesDescription,- CA_validateValue:forKey:,- CA_copyRenderValue,- CAMLTypeForKey:,- encodeWithCAMLWriter:,- CA_distanceToValue:,- CA_interpolateValues:::interpolator:,- CA_interpolateValue:byFraction:,- CAMLType,- CA_copyNumericValue:,- CA_archivingValueForKey:,- CA_copyRenderValueWithColorspace:,- CA_addValue:multipliedBy:,- CA_roundToIntegerFromValue:,- CAMLTypeSupportedForKey:,- CA_prepareRenderValue,- carpf_registerObserverForKeyPath:options:handler:,- _allowsDirectEncoding,- awakeAfterUsingCoder:,- classForCoder,- implementsSelector:,- replacementObjectForCoder:,- removeObserver:forKeyPath:,- removeObservation:,- _isKVOA,- _changeValueForKeys:count:maybeOldValuesDict:maybeNewValuesDict:usingBlock:,- setObservationInfo:,- willChangeValueForKey:,- _didChangeValuesForKeys:,- _willChangeValuesForKeys:,- valueForUndefinedKey:,- addObserver:forKeyPath:options:context:,- setValue:forKey:,- performSelectorOnMainThread:withObject:waitUntilDone:modes:,- performSelector:onThread:withObject:waitUntilDone:modes:,- dictionaryWithValuesForKeys:,- didChangeValueForKey:withSetMutation:usingObjects:,- valueForKeyPath:,- performSelector:onThread:withObject:waitUntilDone:,- classForKeyedArchiver,- _notifyObserversOfChangeFromValuesForKeys:toValuesForKeys:,- _observerStorageOfSize:,- willChangeValueForKey:withSetMutation:usingObjects:,- performSelectorOnMainThread:withObject:waitUntilDone:,- didChangeValueForKey:,- setNilValueForKey:,- replacementObjectForPortCoder:,- _pendingChangeNotificationsArrayForKey:create:,- removeObservation:forObservableKeyPath:,- performSelector:withObject:afterDelay:inModes:,- classForPortCoder,- performSelectorInBackground:withObject:,- finishObserving,- _addObserver:forProperty:options:context:,- observationInfo,- willChange:valuesAtIndexes:forKey:,- classForArchiver,- _notifyObserversForKeyPath:change:,- _changeValueForKey:key:key:usingBlock:,- addObserverBlock:,- _observerStorage,- setObservation:forObservingKeyPath:,- mutableOrderedSetValueForKeyPath:,- addObserver:forObservableKeyPath:,- performSelector:object:afterDelay:,- setValuesForKeysWithDictionary:,- performSelector:withObject:afterDelay:,- didChange:valuesAtIndexes:forKey:,- _destroyObserverList,- addChainedObservers:,- valueForKey:,- validateValue:forKeyPath:error:,- mutableArrayValueForKeyPath:,- _receiveBox:,- receiveObservedError:,- mutableSetValueForKeyPath:,- _isToManyChangeInformation,- replacementObjectForKeyedArchiver:,- _overrideUseFastBlockObservers,- addObservationTransformer:,- addObserver:,- mutableSetValueForKey:,- _changeValueForKey:usingBlock:,- mutableArrayValueForKey:,- _didEndKeyValueObserving,- _implicitObservationInfo,- replacementObjectForArchiver:,- _removeObserver:forProperty:,- validateValue:forKey:error:,- autoContentAccessingProxy,- observeValueForKeyPath:ofObject:change:context:,- _willBeginKeyValueObserving,- mutableOrderedSetValueForKey:,- removeObserver:forKeyPath:context:,- setValue:forKeyPath:,- receiveObservedValue:,- setValue:forUndefinedKey:,- isNSSet__,- isNSString__,- isNSDate__,- isNSNumber__,- isNSData__,- isNSObject__,- isNSArray__,- isNSValue__,- isNSTimeZone__,- isNSOrderedSet__,- isNSDictionary__,- _cfTypeID,- isNSCFConstantString__,- __release_OA,- __retain_OA,- _copyDescription,- ___tryRetain_OA,- doesNotRecognizeSelector:,- __dealloc_zombie,- __autorelease_OA,- methodSignatureForSelector:,- bs_secureEncoded,- bs_encoded,- supportsBSXPCSecureCoding,- bs_isPlistableType,- NSRepresentation,- RBSIsXPCObject,- supportsRBSXPCSecureCoding,- pep_onMainThread,- pep_onThread:,- pep_onThread:immediateForMatchingThread:,- pep_onOperationQueue:priority:,- pep_onMainThreadIfNecessary,- pep_afterDelay:,- pep_onOperationQueue:,- pep_getInvocation:,- isNull,- performSelector:withObject:afterDelay:ignoreMenuTracking:,- __im_afterDelay:,- __im_afterDelay:modes:,- __im_onThread:,- __im_onMainThread,- __im_onThread:immediateForMatchingThread:,- __im_onMainThreadIfNecessary,- __im_onDetachedThread,- __im_getInvocation:,- un_safeBoolValue,- self,- superclass,- performSelector:withObject:,- performSelector:withObject:withObject:,- performSelector:,- zone,- autorelease,- finalize,- _isDeallocating,- isMemberOfClass:,- allowsWeakReference,- isEqual:,- isKindOfClass:,- retainWeakReference,- retain,- retainCount,- hash,- forwardingTargetForSelector:,- mutableCopy,- methodForSelector:,- release,- respondsToSelector:,- _tryRetain,- debugDescription,- conformsToProtocol:,- class,- copy,- forwardInvocation:,- isProxy,- isFault
```

格式化后：

```bash
+ geo_createGEODaemonToMapsPushDaemonConnection
+ geo_withMapsPushDaemon:errorHandler:
+ beginTransaction
+ endTransaction
+ _handoffCurrentReplyToQueue:block:
+ currentConnection
+ fromPBCodable:
...
...
...
- respondsToSelector:
- _tryRetain
- debugDescription
- conformsToProtocol:
- class
- copy
- forwardInvocation:
- isProxy
- isFault
```

### `AKAppleIDAuthenticationService`的`$ownMethods`

代码：

```js
const connOwnMethods = argSelfObj.$ownMethods;
console.log("connOwnMethods=", connOwnMethods);
```

输出：

```bash
connOwnMethods= + geo_createGEODaemonToMapsPushDaemonConnection,+ geo_withMapsPushDaemon:errorHandler:,+ beginTransaction,+ endTransaction,+ _handoffCurrentReplyToQueue:block:,+ currentConnection,- cuValueForEntitlementNoCache:,- uniquify,- hasEntitlement:,- shouldHandleInvalidation,- setShouldHandleInvalidation:,- _EX_transaction,- _EX_setTransaction:,- _EX_extensionProcess,- _EX_setExtensionProcess:,- addBarrierBlock:,- serviceName,- initWithMachServiceName:options:,- _sendInvocation:orArguments:count:methodSignature:selector:withProxy:,- setUserInfo:,- setOptions:,- effectiveGroupIdentifier,- setExportedObject:,- remoteObjectProxy,- userInfo,- interruptionHandler,- start,- remoteObjectProxyWithTimeout:errorHandler:,- _setQueue:,- _decodeAndInvokeReplyBlockWithEvent:sequence:replyInfo:,- _setUUID:,- initWithMachServiceName:,- invalidate,- exportedObject,- initWithServiceName:options:,- exportedInterface,- activate,- _xpcConnection,- init,- invalidationHandler,- _queue,- setRemoteObjectInterface:,- _unboostingRemoteObjectProxy,- setExportedInterface:,- initWithEndpoint:,- synchronousRemoteObjectProxyWithErrorHandler:,- _sendSelector:withProxy:arg1:arg2:arg3:arg4:,- _decodeAndInvokeMessageWithEvent:flags:,- _sendSelector:withProxy:arg1:arg2:arg3:,- _initWithRemoteService:name:options:mode:,- delegate,- set_additionalInvalidationHandler:,- _initWithRemoteService:name:options:,- resume,- _setLanguages:,- _decodeProgressMessageWithData:flags:,- remoteObjectProxyWithUserInfo:errorHandler:,- valueForEntitlement:,- _pauseProgress:,- initWithServiceName:,- auditSessionIdentifier,- setInterruptionHandler:,- setInvalidationHandler:,- _cancelProgress:,- _resumeProgress:,- _killConnection:,- _sendSelector:withProxy:arg1:arg2:,- _errorDescription,- suspend,- stop,- replacementObjectForEncoder:object:,- _sendDesistForProxy:,- remoteObjectProxyWithErrorHandler:,- _initWithRemoteConnection:name:,- effectiveUserIdentifier,- _additionalInvalidationHandler,- _sendSelector:withProxy:arg1:,- _remoteObjectInterfaceClass,- _sendSelector:withProxy:,- _setBootstrapObject:forKey:,- scheduleSendBarrierBlock:,- _initWithCustomTransport:,- auditToken,- _setTargetUserIdentifier:,- remoteObjectInterface,- setDelegate:,- description,- initWithListenerEndpoint:,- dealloc,- processIdentifier,- endpoint
```

格式化后：

```bash
+ geo_createGEODaemonToMapsPushDaemonConnection
+ geo_withMapsPushDaemon:errorHandler:
+ beginTransaction
+ endTransaction
+ _handoffCurrentReplyToQueue:block:
+ currentConnection
- cuValueForEntitlementNoCache:
- uniquify
- hasEntitlement:
- shouldHandleInvalidation
- setShouldHandleInvalidation:
- _EX_transaction
- _EX_setTransaction:
- _EX_extensionProcess
- _EX_setExtensionProcess:
- addBarrierBlock:
- serviceName
- initWithMachServiceName:options:
- _sendInvocation:orArguments:count:methodSignature:selector:withProxy:
- setUserInfo:
- setOptions:
- effectiveGroupIdentifier
- setExportedObject:
- remoteObjectProxy
- userInfo
- interruptionHandler
- start
- remoteObjectProxyWithTimeout:errorHandler:
- _setQueue:
- _decodeAndInvokeReplyBlockWithEvent:sequence:replyInfo:
- _setUUID:
- initWithMachServiceName:
- invalidate
- exportedObject
- initWithServiceName:options:
- exportedInterface
- activate
- _xpcConnection
- init
- invalidationHandler
- _queue
- setRemoteObjectInterface:
- _unboostingRemoteObjectProxy
- setExportedInterface:
- initWithEndpoint:
- synchronousRemoteObjectProxyWithErrorHandler:
- _sendSelector:withProxy:arg1:arg2:arg3:arg4:
- _decodeAndInvokeMessageWithEvent:flags:
- _sendSelector:withProxy:arg1:arg2:arg3:
- _initWithRemoteService:name:options:mode:
- delegate
- set_additionalInvalidationHandler:
- _initWithRemoteService:name:options:
- resume
- _setLanguages:
- _decodeProgressMessageWithData:flags:
- remoteObjectProxyWithUserInfo:errorHandler:
- valueForEntitlement:
- _pauseProgress:
- initWithServiceName:
- auditSessionIdentifier
- setInterruptionHandler:
- setInvalidationHandler:
- _cancelProgress:
- _resumeProgress:
- _killConnection:
- _sendSelector:withProxy:arg1:arg2:
- _errorDescription
- suspend
- stop
- replacementObjectForEncoder:object:
- _sendDesistForProxy:
- remoteObjectProxyWithErrorHandler:
- _initWithRemoteConnection:name:
- effectiveUserIdentifier
- _additionalInvalidationHandler
- _sendSelector:withProxy:arg1:
- _remoteObjectInterfaceClass
- _sendSelector:withProxy:
- _setBootstrapObject:forKey:
- scheduleSendBarrierBlock:
- _initWithCustomTransport:
- auditToken
- _setTargetUserIdentifier:
- remoteObjectInterface
- setDelegate:
- description
- initWithListenerEndpoint:
- dealloc
- processIdentifier
- endpoint
```

其中：

* NSXPCConnection
  * 没有之前Xcode调试所看到的属性
    * processName
    * processBundleIdentifier
  * 不过有我们所重点关注的其他的属性
    * exportedInterface
    * exportedObject
    * remoteObjectInterface
    * remoteObjectProxy
    * auditSessionIdentifier
    * effectiveUserIdentifier
    * effectiveGroupIdentifier
    * userInfo

-》实现了我们通过`$methods`和`$ownMethods`去调试`NSXPCConnection`类的属性值，找到我们所关注的属性值
