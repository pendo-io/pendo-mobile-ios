# SwiftUI Integration 
Currently SwiftUI support is provided as beta and is available via cocoapods and SPM:<br>

## Cocoapods
```
    #Place it at the top of your Podfile
    source 'https://github.com/pendo-io/specs-beta.git'
    source 'https://github.com/CocoaPods/Specs.git'
```

Add Pendo pod with all rest of the pods:
`pod 'Pendo'`

## SPM
In the SPM search for _pendo_ and use `swiftui_beta` branch:<br>
<img width="700" alt="SPM" src="https://user-images.githubusercontent.com/56674958/180163385-59639b68-df10-4d85-bd72-08dca771bd51.png">
 
Pure swiftUI apps don't include `AppDelegate` file by default. Please create one and follow the instructions in step: **2 Integration**.([Native IOS](pnddocs/native.md)) <br>
SwiftUI apps **don't respond** to: <br>
 `application(_ app: UIApplication,open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool` <br>
  if the app entry point is struct attributed with `@main`.<br>
In that case please add `.onOpenURL(perform:)` to your main view, for instance:
```swift
@main
struct YourApp: App {
    let persistenceController = PersistenceController.shared
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    var body: some Scene {
        return WindowGroup {
            TabView {
                YourView().tabItem {
                    Image("Icon")
                    Text("Text")
                }
            }
            .onOpenURL(perform: handleURL)
        }
    }
    
    func handleURL(_ url: URL) {
        _ = appDelegate.application(UIApplication.shared, open: url, options: [:])

    }
``` 
