
---
title: Start a Voice Call
description: Describes how quickly implement a voice call.
platform: Cocos Creator
updatedAt: Fri Nov 06 2020 14:34:41 GMT+0800 (CST)
---
# Start a Voice Call
This article describes how to integrate the Agora Cocos Creator SDK and implement a basic voice call in your app.

## Prerequisites

- [Cocos Creator](https://docs.cocos.com/creator/manual/en/getting-started/install.html) 2.0.9 or later (The interface description in this article is based on Cocos Creator 2.4.3)
- [Cocoapods](https://guides.cocoapods.org/using/getting-started.html#getting-started)
- Operating system and IDE requirements:

  | Target Platform | Operating system version | IDE version                 |
  | :-------------- | :----------------------- | :-------------------------- |
  | Android         | Android 4.1 or later     | Android Studio 3.0 or later |
  | iOS             | iOS 8.0 or later         | Xcode 9.0 or later          |

- A valid [Cocos account](https://account.cocos.com/) and a [Cocos App ID](https://docs.cocos.com/creator/manual/en/cocos-service/#usage)

 <div class="alert note">If your network has a firewall, follow the instructions in <a href="https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms">Firewall Requirements</a > to access Agora's services.</div>

## Create a Cocos Creator project

Use the following steps to build a project from scratch. Skip to [Integrate the SDK](#integrate) if you have already created a Cocos Creator project.

1. Open Cocos Creator and go to **New Project**.
2. Choose an example (**Hello Typescript** is recommended) you need, choose a storage path, and input a project name.
3. Click **Create**.

## <a name="integrate"></a>Integrate the SDK

1. Open a Cocos Creator project, and click **Service** on the right to open the **Service** panel.
2. Click the ![](https://web-cdn.agora.io/docs-files/1603983326448) button following **AppID**, and click **Set Cocos AppID** to associate your game with your project.
  ![](https://web-cdn.agora.io/docs-files/1603983352672)
3. On the **Service** panel, scroll down to find and select **Agora Voice**.
4. Click the ![](https://web-cdn.agora.io/docs-files/1603983397604) button following **Agora Voice**, read the prompts carefully, and click **Open Service** to integrate the Agora Cocos Creator SDK.
5. After enabling the **Agora Voice** service, Cocos Creator automatically adds codes in your project for obtaining device permissions, so you do not need to write these codes by yourself.
   Cocos Creator also automatically registers an Agora account and creates an Agora project for you. You can click **Dashbord** to visit Agora Console; you can see the Agora project and [Agora App ID](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#getappid) on the **Project Management** page.

## Implementation

Once the project is set up, create a component script and a scene based on your requirements, and add the script into the scene node. See [Creating and using component script](https://docs.cocos.com/creator/manual/en/scripting/use-component.html).

You can use core APIs of the Agora Cocos Creator SDK in a component script to implement a basic voice call. The following figure shows the API call sequence:

![](https://web-cdn.agora.io/docs-files/1603983906888)

### 1. Create a user interface

Create a user interface (UI) for the live interactive audio streaming in your project. If you already have a UI in your project, skip to [Initialize the Agora engine](#initialize).

Agora recommends adding the following elements to the UI:

- A voice call window
- An end-call button

### <a name="initialize"></a>2. Initialize the Agora engine

Initialize the Agora engine before calling any other Agora APIs. You need to pass in the App ID of your Agora project in this step. See [Get the Agora App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#getappid).

Call `init` and pass in the App ID to initialize the Agora engine.

According to your requirements, you can also listen for callback events in this step, such as when the local user joins or leaves the channel.

```typescript
// Initialize the Agora engine. You need to use your Agora App ID to replace YOUR_APPID.
agora.init("YOUR_APPID");
// Listen for callback events.
initAgoraEvents: function () {
    if (agora) {
        agora.on('joinChannelSuccess', this.onJoinChannelSuccess, this);
        agora.on('leaveChannel', this.onLeaveChannel, this);
        agora.on('warning', this.onWarning, this);
        agora.on('error', this.onError, this);
        agora.on('userJoined', this.onUserJoined, this);
        agora.on('userOffline', this.onUserOffline, this);
    }
}
```

### 3. Join a channel

After initializing the Agora engine, you can call `joinChannel` to join a channel. You need to generate a token to replace `YOUR_TOKEN` in the code sample below.

- For testing, you can [generate a temporary token](https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token) in Agora Console.
- For production, Agora recommends using a token generated at your server. See [Generate a Token](../../en/Voice/token_server.md).

<div class="alert note">Once the user joins the channel, the user subscribes to the audio streams of all the other users in the channel by default, giving rise to usage and billing calculation. If you do not want to subscribe to a specified stream or all remote streams, call the <tt>mute</tt> methods accordingly.</div>

```typescript
// For SDKs earlier than v3.1.2, call this method to enable interoperability between the Agora Cocos Creator SDK and the Agora Web SDK if the Agora Web SDK is in the channel.
// As of v3.1.2, the Agora Cocos Creator SDK enables the interoperability with the Agora Web SDK by default, so you do not need to call this method.
agora.enableWebSdkInteroperability(true);
// Join a channel with a token.
agora.joinChannel("YOUR_TOKEN", “channel5”, "Extra Optional Data", 0);
```

### 4. Leave the channel

According to your scenario, such as when the voice call ends and when you need to close the app, call `leaveChannel` to leave the current channel.

```typescript
agora.leaveChannel();
```

### 5. Destroy the Agora engine

After leaving the channel, if you want to exit the app or release the memory of the Agora engine, call `release` to destroy the Agora engine.

```typescript
agora.release();
```

## Run the project

Agora recommends running and debugging your project through a real Android or iOS device. See [Compile and preview](https://docs.cocos.com/creator/manual/en/publish/publish-native.html#compile-and-preview).

When the voice call starts successfully, you can hear remote users in the app.

## Reference

Agora provide an open-source [Voice-Call-for-Mobile-Gaming](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-CocosCreator-Voice-Agora) sample project on GitHub that implements a basic one-to-one voice call. You can download the demo project as a code source reference.
