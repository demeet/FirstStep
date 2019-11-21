# Android SDK

## Table of content

1. [Prerequisites](#prerequisites)
2. [Installation](#installation) 
3. [Chat Frame Creation](#chat-frame-creation) 
     - [XML](#xml) 
     - [Programmatically](#programmatically) 
     - [Modal Screen](#modal-screen) 
     - [Navigation Screen](#navigation-screen) 
4. [Settings](#settings) 

## Prerequisites
This document is intended for bot specialists and developers with 
working knowledge of Android development. It also assumes you have a native 
Android app into which you plan to integrate the Ada Android SDK.

## Installation
[ ![Download](https://api.bintray.com/packages/demeetest/testrepo/android-sdk/images/download.svg?version=0.1.1) ](https://bintray.com/demeetest/testrepo/android-sdk/0.1.1/link)

https://bintray.com/beta/#/demeetest/testrepo/android-sdk/0.1.2/link


## Chat Frame Creation 
After installing SDK, you get the opportunity to show the chat window 
to your user in several ways.
#### XML 
In sdk there is an opportunity to initialize the view in the xml markup:
```xml
<support.ada.embed.widget.AdaEmbedView
        android:id="@+id/ada_chat_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:ada_handle="ada-example" />
```
To display the view, the `ada_handle` parameter is required. If you specify 
it as an attribute, the chat will be displayed immediately upon 
attachment to the parent view. If it is not specified, then you 
can do it later programmatically using the method: 
`initialize(settings: AdaEmbedView.Settings)`

```kotlin
val adaView = findViewById(R.id.ada_chat_frame)

adaSettings = AdaEmbedView.Settings.Builder("ada-example")
    .build()
adaView.initialize(adaSettings)
container.addView(adaView)
```

Please note "ada-example" is being used for demonstration purposes. 
Be sure to modify handle, as well as any other values as needed 
for your bot.

#### Programmatically 

To programmatically create the chat frame, you need to create a view 
object, pass context to the constructor. 

```kotlin
val adaView = AdaEmbedView(getContext())
```
After this, the view will be created, but will not be initialized. Call 
`initialize(settings: AdaEmbedView.Settings)` to do this and 
pass settings object as an argument.

```kotlin
val adaSettings = AdaEmbedView.Settings.Builder("ada-example")
    .cluster("ca")
    .language("en")
    .build()
adaView.initialize(adaSettings)
```

To display a chat frame on the screen, it remains only to add the 
previously created view to the container using the Android SDK.

#### Modal Screen 
TBD

#### Navigation Screen 
TBD

## Settings
Using SDK, you can configure the initial chat settings for greater 
customization.
#### Cluster
Specifies the Kubernetes cluster to be used. Unless directed by an Ada 
team member, you will not need to change this value.

```xml
app:ada_cluster="ca"
```


#### Greeting
This can be used to customize the greeting messages that new users see. This is useful for setting view-specific greetings across your app. The greeting should correspond to the ID of the Answer you would like to use. The ID can be found in the URL of the corresponding Answer in the dashboard.

```xml
app:ada_greetings="5c59aaabd8269e0339979014"
```

#### Handle
The handle for your bot. This is a required field.

```xml
app:ada_handle="ada-example"
```
#### Language
Takes in a language code to programmatically set the bot language. 
Languages must first be turned on in the Settings > Multilingual page 
of your Ada dashboard. Language codes follow the 
[ISO 639-1 language format](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes).

```xml
app:ada_language="en"
```

#### Metafields
Used to pass meta information about a Chatter. This can be useful for 
tracking information about your end users, as well as for personalizing 
their experience. For example, you may wish to track the phone_number and 
name for conversation attribution. Once set, this information can be 
accessed in the email attachment from Handoff Form submissions, or via 
the Chatter modal in the Conversations page of your Ada dashboard. 

You can also set "meta fields" using XML. For this, you need to create a JSON file 
in res/raw directory and set reference to view declaration:


***raw/metafields.json***
```json
{
  "id":12244,
  "name": "John",
  "authorized": true
}
```

```xml
app:ada_metaFields="@raw/metafields"
```

#### Styles
The `styles` setting can be used to override default styles inside the 
Chat bot. The value of the string should be the CSS rule-set you wish to 
apply inside the Chat UI. A list of CSS selectors available for 
targetting can be found in the table below.

| WARNING: We do not recommend assigning styles to classes you inspect 
in the DOM. Class naming is subject to change, and can cause your custom 
styles to break. |
| --- |

Selector | Description
--- | ---
`#message-container` | The outer wrapper, containing the top bar, message list, and input bar
`#ada-close-button` | The button used to close the Web Chat window
`#input-bar` | The bottom wrapper, containg the textarea element, send button, and bottom text
`#message-input` | The textarea inside the input bar, used for user input
`#clear-message` | The button used to clear text from the message input
`#send-button ` | The button for submitting the user input
`#status-bar` | The bottom text inside the input bar
`#close-info-button` | The button to close the settings modal
`#language-selector` | The language select container
`#language-picker` | The language select element
`#terms-of-service` | The terms of service link
`#privacy` | The privacy link
`#messages-list` | The messages container
`#topBar` | The top bar container above the message list
`#info-button` | The settings modal button
`.g-message` | The base message selector
`.g-message--is-owned-by-user` | The selector for messages from the end user

```xml
app:ada_styles="*{font-size: 14px !important;}"
```

#### Builder Configuration

You can also configure the Ada bot programmatically using the
`AdaEmbedView.Settings` class.

```kotlin
val adaSettings = AdaEmbedView.Settings.Builder("ada-example")
            .cluster("ca")
            .greetings("5c59aaabd8269e0339979014")
            .styles("*{font-size: 14px !important;}")
            .language("en")
            .metaFields(metaFieldsMap)
            .build()
```
