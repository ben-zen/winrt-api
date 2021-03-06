---
-api-id: T:Windows.ApplicationModel.Activation.CachedFileUpdaterActivatedEventArgs
-api-type: winrt class
---

<!-- Class syntax.
public class CachedFileUpdaterActivatedEventArgs : Windows.ApplicationModel.Activation.IActivatedEventArgs, Windows.ApplicationModel.Activation.IActivatedEventArgsWithUser, Windows.ApplicationModel.Activation.ICachedFileUpdaterActivatedEventArgs
-->

# Windows.ApplicationModel.Activation.CachedFileUpdaterActivatedEventArgs

## -description
Provides information about the activated event that fires when the user saves or opens a file that needs updates from the app.

> **JavaScript**
> This type appears as [WebUICachedFileUpdaterActivatedEventArgs](../windows.ui.webui/webuicachedfileupdateractivatedeventargs.md).

## -remarks
Learn more about providing updates for files that your app offers in the [Quickstart: Providing file services through ](http://msdn.microsoft.com/library/3a348fea-c4b3-4847-a350-a41a69441c00) and in the [Windows.Storage.Pickers.Provider](../windows.storage.pickers.provider/windows_storage_pickers_provider.md) namespace reference.

A [CachedFileUpdaterActivatedEventArgs](cachedfileupdateractivatedeventargs.md) object is passed to the app's activated event handler when the user saves a file that requires content management from the app. This type of activation is indicated by the [ActivationKind.CachedFileUpdater](activationkind.md) value returned by the [Kind](cachedfileupdateractivatedeventargs_kind.md) property.

Apps written in JavaScript must listen for and handle [Windows.UI.WebUI.WebUIApplication.activated](../windows.ui.webui/webuiapplication_activated.md) events.


<!--{annotation author="wolfs" time="10/23/2012 2:39:33 PM"}This isn't correct. The jupiter app model wires this up for you when they instantiate the Application object on the main STA. You handle activation scenarios by overriding the On* methods of Application. You don't ever have to use anything from the CoreApplication / Core namespace for all but the most fringe/advanced scenarios.-->
Windows Store app using C++, C#, or Visual Basic typically implement activation points by overriding methods of the [Application](../windows.ui.xaml/application.md) object. The default template app.xaml code-behind files always include an override for [OnLaunched](../windows.ui.xaml/application_onlaunched.md), but defining overrides for other activation points such as [OnCachedFileUpdaterActivated](../windows.ui.xaml/application_oncachedfileupdateractivated.md) is up to your app code.

All [Application](../windows.ui.xaml/application.md) overrides involved in an activation scenario should call [Window.Activate](../windows.ui.xaml/window_activate.md) in their implementations.

## -examples
The [File picker contracts sample](http://go.microsoft.com/fwlink/p/?linkid=231536) demonstrates how to respond to a **CachedFileUpdater** activation point.

```csharp

// CachedFileUpdater activated event handler
protected override void OnCachedFileUpdaterActivated(CachedFileUpdaterActivatedEventArgs args)
{
    var CachedFileUpdaterPage = new SDKTemplate.CachedFileUpdaterPage();
    CachedFileUpdaterPage.Activate(args);
}

// Overloaded method to respond to CachedFileUpdater events
public void Activate(CachedFileUpdaterActivatedEventArgs args)
{
            // Get file picker UI
            cachedFileUpdaterUI = args.CachedFileUpdaterUI;

            cachedFileUpdaterUI.FileUpdateRequested += CachedFileUpdaterUI_FileUpdateRequested;
            cachedFileUpdaterUI.UIRequested += CachedFileUpdaterUI_UIRequested;

            switch (cachedFileUpdaterUI.UpdateTarget)
            {
                case CachedFileTarget.Local:
                    scenarios = new List<Scenario> { new Scenario() { Title = "Get latest version", ClassType = typeof(FilePickerContracts.CachedFileUpdater_Local) } };
                    break;
                case CachedFileTarget.Remote:
                    scenarios = new List<Scenario> { new Scenario() { Title = "Remote file update", ClassType = typeof(FilePickerContracts.CachedFileUpdater_Remote) } };
                    break;
            }

            Window.Current.Activate();
        }
```

For C#, `args` for an [OnCachedFileUpdaterActivated](../windows.ui.xaml/application_oncachedfileupdateractivated.md) override on the [Application](../windows.ui.xaml/application.md) object references a [CachedFileUpdaterActivatedEventArgs](cachedfileupdateractivatedeventargs.md) object. The [OnCachedFileUpdaterActivated](../windows.ui.xaml/application_oncachedfileupdateractivated.md) override is in the App.xaml.cs file and the `Activate` method is in the CachedFileUpdaterPage.xaml.cs file of the [File picker contracts sample](http://go.microsoft.com/fwlink/p/?linkid=231536).

## -see-also
[ActivationKind enumeration](activationkind.md), [Windows.Storage.Provider namespace](../windows.storage.provider/windows_storage_provider.md), [File picker contracts sample](http://go.microsoft.com/fwlink/p/?linkid=231536), [Windows.UI.Core.CoreApplicationView.Activated event](../windows.applicationmodel.core/coreapplicationview_activated.md), [OnCachedFileUpdaterActivated](../windows.ui.xaml/application_oncachedfileupdateractivated.md), [Application](../windows.ui.xaml/application.md)