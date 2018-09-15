# ACR Speech Recognition Plugin for Xamarin

_Easy to use cross platform speech recognition (speech to text) plugin for Xamarin & UWP_

[![NuGet](https://img.shields.io/nuget/v/RememorateFork.Plugin.SpeechRecognition.svg?maxAge=2592000)](https://www.nuget.org/packages/RememorateFork.Plugin.SpeechRecognition)

This was forked from [https://github.com/aritchie/speechrecognition](https://github.com/aritchie/speechrecognition).
It now lives at [https://github.com/garie/speechrecognition](https://github.com/garie/speechrecognition).
This is unlikely to be merged back to aritchie since I haven't made an effort to preserve backwards compatibility.

## PLATFORMS

* iOS 10+
* Android
* .NET Standard

## SETUP

#### iOS
Add the following to your
```xml
<key>NSSpeechRecognitionUsageDescription</key>
<string>Say something useful here</string>
<key>NSMicrophoneUsageDescription</key>
<string>Say something useful here</string>
```

#### Android

Add the following to your AndroidManifest.xml
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

## HOW TO USE

### Request Permission

Follow the instructions for setting up permission requests [here](https://github.com/jamesmontemagno/PermissionsPlugin).

```csharp
var granted = await CrossSpeechRecognition.Current.RequestPermission();
if (granted == SpeechRecognizerStatus.Available)
{
    if (!(await Settings.CheckPermissions(Plugin.Permissions.Abstractions.Permission.Microphone, false)))
    {
        await App.Current.MainPage.DisplayAlert("No Recording", "Microphone is not available.", "OK");
        return;
    }

    if (!(await Settings.CheckPermissions(Plugin.Permissions.Abstractions.Permission.Speech, false)))
    {
        await App.Current.MainPage.DisplayAlert("No Recording", "Speech recognition is not available.", "OK");
        return;
    }

    // Permission granted, do stuff here!
}
else
{
    await App.Current.MainPage.DisplayAlert("No Recording", "Speech recognition is not available.", "OK");
    return;
}
```

### Continuous Dictation
```csharp
var listener = CrossSpeechRecognition
    .Current
    .ContinuousDictation()
    .Subscribe(phrase => {
        // will keep returning phrases as pause is observed
    });

// make sure to dispose to stop listening
listener.Dispose();

```


### Listen for a phrase (good for a web search)
```csharp
CrossSpeechRecognition
    .Current
    .ListenUntilPause()
    .Subscribe(phrase => {})
```

### Listen for keywords
```csharp
CrossSpeechRecognition
    .Current
    .ListenForKeywords("yes", "no")
    .Subscribe(firstKeywordHeard => {})
```


### When can I talk?
```csharp
CrossSpeechRecognition
    .Current
    .WhenListenStatusChanged()
    .Subscribe(isListening => { you can talk if this is true });
```

## FAQ

Q. Why use reactive extensions and not async?

A. Speech is very event stream oriented which fits well with RX


Q. Should I use CrossSpeechRecognition.Current?

A. Hell NO!  DI that sucker using the Instance


## Roadmap

* Multilingual
* Confidence Scoring
* Start and end of speech eventing
* RMS detection