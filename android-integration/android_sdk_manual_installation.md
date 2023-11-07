# Pendo SDK Android Manual Installation

If you are having problems installing the SDK, please follow these steps - 

1. Download the Pendo SDK (we always recommend taking the latest) <a href="https://pendo.jfrog.io/ui/native/androidx-release/manual/" target="_blank">here.</a>

2. Unzip the downloaded file.

3. Add the local directory to the gradle file.

example:
```java
    repositories {
      maven {
         url "/path/to/local/file"
      }
    } 
```

4. Specify the version number for the Pendo dependency in the gradle file:

example, for version 2.22.0.5604 :

```java
    implementation (group:'sdk.pendo.io' , name:'pendoIO', version:'2.22.0.5604', changing:true)
```

## Developer Documentation

* API documentation available [here](TODO:missing-link)


## Troubleshooting

- For technical issues please [review open issues](https://github.com/pendo-io/pendo-mobile-sdk/issues) or [submit a new issue](https://github.com/pendo-io/pendo-mobile-sdk/issues).
- Release notes can be found [here](https://developers.pendo.io/category/mobile-sdk/).
- For Dex issues with Android applications refer to this [resource](https://developer.android.com/studio/build/multidex).
- For additional documentation visit our [Help Center Mobile Section](https://support.pendo.io/hc/en-us/categories/4403654621851-Mobile).