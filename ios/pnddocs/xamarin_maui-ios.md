# MAUI iOS


>[!NOTE]
>The following integration instructions are relevant for SDK 3.0 or higher. <br> Follow our migration instructions to [upgrade from SDK 2.x to 3.0](/migration-docs/README.md) or refer to our [2.x integration instruction](https://github.com/pendo-io/pendo-mobile-sdk/blob/2.22.5/README.md).

>[!IMPORTANT]
>Requirements:
>- .NET 7

## Step 1. Install the Pendo SDK

1. In **Visual Studio** Solution Explorer, right-click on your project, then select "Add" - > "Add NuGet Packages…".
2. Search for: **PendoMAUIPlugin** with latest version.<br/>
3. Press **Add Package**.


## Step 2. Pendo SDK Integration

>[!NOTE]
>The `API Key` can be found in your Pendo Subscription Settings under the App Details Section.

1. Open the shared application **App.xaml.cs**:

    Add the following under 'using'

    ```c#
    using PendoMAUIPlugin;
    ``` 


    In the **protected override void OnStart()** method, add the following code:

    ```c#
    protected override void OnStart()
    {
        IPendoService pendo = PendoServiceFactory.CreatePendoService();

        /** if your app supports additional Platforms other than iOS and Android
        verify the Pendo instance is not null */
        if (pendo != null) { 
            string apiKey = "YOUR_API_KEY_HERE";
            pendo.Setup(apiKey);
        }

        ...
        
    }
    ```

2. Start the visitor's Session in the page where your visitor is being identified (e.g. login, register, etc.).

    ```c#
    using PendoMAUIPlugin;

    namespace ExampleApp
    {
        class ExampleLoginClass
        {

        public void MethodExample()
        {
            IPendoService pendo = PendoServiceFactory.CreatePendoService();

            ...

            /** if your app supports additional Platforms other than iOS and Android
            verify the Pendo instance is not null */
            if (pendo != null) { 

                var visitorId = "VISITOR-UNIQUE-ID";
                var accountId = "ACCOUNT-UNIQUE-ID";

                var visitorData = new Dictionary<string, object>
                {
                    { "age", 27 },
                    { "country", "USA" }
                };

                var accountData = new Dictionary<string, object>
                {
                    { "Tier", 1 },
                    { "Size", "Enterprise" }
                };

                pendo.StartSession(visitorId, accountId, visitorData, accountData);

            }

            ...

        }
    }
    ```

    **visitorId**: a user identifier (e.g. John Smith)  
    **visitorData**: the user metadata (e.g. email, phone, country, etc.)  
    **accountId**: an affiliation of the user to a specific company or group (e.g. Acme inc.)  
    **accountData** : the account metadata (e.g. tier, level, ARR, etc.)

    This code ends the previous mobile session (if applicable), starts a new mobile session and retrieves all guides based on the provided information.

>[!TIP]
>Passing `null` or `""` as the visitorId will generate <a href="https://help.pendo.io/resources/support-library/analytics/anonymous-visitors.html" target="_blank">anonymous visitor id</a>.

## Step 3. Mobile device connectivity for tagging and testing

>[!NOTE]
>The `Scheme ID` can be found in your Pendo Subscription Settings under the App Details Section.

These steps allow page <a href="https://support.pendo.io/hc/en-us/articles/360033609651-Tagging-Mobile-Pages#HowtoTagaPage" target="_blank">tagging</a>
and <a href="https://support.pendo.io/hc/en-us/articles/360033487792-Creating-a-Mobile-Guide#test-guide-on-device-0-6" target="_blank">guide</a> testing capabilities.

1. Add Pendo URL Scheme to **info.plist** file

   Under the iOS App Target > open info.plist > if URL Types doesn't exist, click on 'Add new entry' and name it 'URL types', for the type choose 'Array'.
   Create a new URL by clicking the + button.
   Under the new created Dictionary, change 'Document role' to 'URL Schemes' with type 'Array'.
   Expand 'URL Schemes' and add the `YOUR_SCHEME_ID` under the 'Value'.
   Under the created Dictionary in the previous step, add new entry with the name 'URL Identifier', type 'string' and set a name of your preference under its 'Value'.

2. Add or modify the function **OpenURL**

   Open ***AppDelegate.cs*** file and add ***using PendoMaui;*** 
   add the following code under ***OpenUrl*** method:

    ```c#
    using PendoMAUIPlugin;

    ...

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            if (url.Scheme.Contains("pendo"))
            {
                IPendoService pendo = PendoServiceFactory.CreatePendoService();

                /** if your app supports additional Platforms other than iOS and Android
                verify the Pendo instance is not null */
                if (pendo != null) { 
                    pendo.InitWithUrl(url.AbsoluteString);
                }

                return true;
            }
            return base.OpenUrl(app, url, options);
        }
    ```

## Step 4. Verify installation

1. Test using Visual Studio:  
Run the app.  
Review the device log and look for the following message:  
`Pendo SDK was successfully integrated and connected to the server.`
2. In the Pendo UI, go to Settings>Subscription Settings.
3. Hover over your app and select View app details.
4. Select the Install Settings tab and follow the instructions under Verify Your Installation to ensure you have successfully integrated the Pendo SDK.
5. Confirm that you can see your app as Integrated under <a href="https://app.pendo.io/admin" target="_blank">subscription settings</a>.

## Developer documentation

- API documentation available [here](/api-documentation/xamarin-maui-apis.md)

## Troubleshooting
- For technical issues please [review open issues](https://github.com/pendo-io/pendo-mobile-sdk/issues) or [submit a new issue](https://github.com/pendo-io/pendo-mobile-sdk/issues).
- Release notes can be found [here](https://developers.pendo.io/category/mobile-sdk/).
- For additional documentation visit our [Help Center Mobile Section](https://support.pendo.io/hc/en-us/categories/4403654621851-Mobile).