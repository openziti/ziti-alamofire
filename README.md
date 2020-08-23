# Using Ziti with Alamofire

Example project for runing [Alamorefire](https://github.com/Alamofire/Alamofire) over a Ziti connection using the `ZitiUrlProtocol` of the [Ziti Swift SDK](https://github.com/openziti/ziti-sdk-swift).

# Key Pieces of Code

Register `ZitiUrlProtocol`:
```swift
ziti.runAsync { zErr in
    guard zErr == nil else {
        // Handle error
        return
    }
    ZitiUrlProtocol.register(ziti)
}
```

Convince `Alamofire` to use `ZitiUrlProtocol`:
```swift
let configuration = URLSessionConfiguration.default
configuration.protocolClasses?.insert(ZitiUrlProtocol.self, at: 0)
sessionManager = Alamofire.Session(configuration: configuration)
```

Make `Alamofire` requests:
```swift
sessionManager?.request(urlReq).response { response in
    print(String(reflecting: response))
}
```
URLs that are configured to be intercepted by `Ziti` will be routed over a `Ziti` network. Non-intercepted requests will be processed as they would have been before `Ziti` integration.

# Building
Both `Alamofire` and `Ziti` are available via CocoaPods.  After cloning this repo, run `pod install` from the command-line and open the resulting `.xcworkspace` 
