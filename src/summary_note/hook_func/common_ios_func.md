# 常用iOS函数

Frida中逆向iOS的app期间，常会涉及到，一些相对通用的，常用的函数，整理如下：

* 网络相关
  * 请求=request
    * `NSURLRequest`
      ```objc
      +[NSURLRequest requestWithURL:]
      +[NSURLRequest requestWithURL:cachePolicy:timeoutInterval:]
      -[NSURLRequest requestWithURL:cachePolicy:timeoutInterval:]
      -[NSURLRequest initWithURL:]
      -[NSURLRequest initWithURL:cachePolicy:timeoutInterval:]
      ```
    * `NSMutableURLRequest`
      ```objc
      -[NSMutableURLRequest initWithURL:]
      -[NSMutableURLRequest setHTTPBody:]
      ```
    * `NSURL`
      ```objc
      +[NSURL URLWithString:]
      +[NSURL URLWithString:relativeToURL:]
      -[NSURL initWithScheme:host:path:]
      -[NSURL initWithString:]
      -[NSURL initWithString:relativeToURL:]
      ```
    * `NSURLConnection`
      ```objc
      +[NSURLConnection sendSynchronousRequest:returningResponse:error:]
      +[NSURLConnection sendAsynchronousRequest:queue:completionHandler:]
      +[NSURLConnection connectionWithRequest:delegate:]
      -[NSURLConnection initWithRequest:delegate:]
      -[NSURLConnection start]
      -[NSURLConnection cancel]
      -[NSURLConnection connection:didReceiveResponse:]
      -[NSURLConnection connection:didReceiveData:]
      -[NSURLConnection connectionDidFinishLoading:]
      -[NSURLConnection connection:didFailWithError:]
      ```
  * 响应=response
    * `NSHTTPURLResponse`
      ```objc
      -[NSHTTPURLResponse initWithURL:statusCode:HTTPVersion:headerFields:]
      -[NSHTTPURLResponse allHeaderFields]
      -[NSHTTPURLResponse statusCode]
      ```
    * `NSURLResponse`
      ```objc
      -[NSURLResponse initWithURL:MIMEType:expectedContentLength:textEncodingName]
      ```
