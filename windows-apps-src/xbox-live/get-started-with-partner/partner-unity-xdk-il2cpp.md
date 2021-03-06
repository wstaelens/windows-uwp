---
title: Unity for XDK with IL2CPP backend
author: KevinAsgari
description: Add Xbox Live support to Unity for XDK with IL2CPP scripting backend for ID@Xbox and managed partners
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one, Unity
ms.localizationpriority: medium
---

# Add Xbox Live support to Unity for XDK with IL2CPP scripting backend for ID@Xbox and managed partners

## Overview

Windows Runtime Support for IL2CPP in Unity

With the release of Unity 5.6f3 the engine has included a new feature that enables developers to use Windows Runtime (WinRT) components directly in script by including them in the game project directly. Until 5.6 developers have needed a plugin, or dll to support any platform feature (including Xbox Live SDK) from game script. This new projection layer removes the plugin requirement, and introduces a new and simplified workflow supported only with games that choose the IL2CPP scripting backend.

For more information on how to get started, see the Unity documentation: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## Steps

**1) Install Unity**

Install Unity 5.6 or higher, and ensure you have the Xbox One editor extension installed.

**2) Install Visual Studio Tools for Unity version 3.1 and above for IntelliSense support when using WinMDs**
For Visual Studio 2015, this can be found at https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  For Visual Studio 2017, the component can be added inside the Visual Studio 2017 installer.

**3) Open a new or existing Unity project**

**4) Switch the platform to Xbox One in the Unity Build Settings menu**

**5) Enable IL2CPP scripting backend in the Unity player settings, and set API compatibility to .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**6) Switch the Script Compiler to Roslyn**

**7) The Xbox One appropriate system libraries will all be added automatically to your project, and no extra steps are needed to include the platform binaries.**

**8) Add and attach a new C\# script to a Unity object.**

For example, click on a Unity object such as the "Main Camera", and click "Add Component" \| "New Script" \| C\# Script \| and name it "XboxLiveScript". Any game object will do.

**9) Open the script in Visual Studio (with VSTU 3.1+ installed)**

You will notice two projects, open your game script XboxLiveTest.cs in the "Player" project generated by VSTU

![](../images/unity/unity-il2cpp-2.png)

This is a special project generated for XDK, and includes references for the winmd files you have placed in your assets.
It will also define the "#if ENABLE_WINMD_SUPPORT" define for you so IntelliSense and syntax highlighting will work properly.

**10) Add the following Xbox Live code to the XboxLiveTest.cs source file**

```cpp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**11)	Make sure you have 'InternetClient' capability selected in the publishing settings found in player settings**

![](../images/unity/unity-il2cpp-3.png)

**12) Build the project in Unity.**
