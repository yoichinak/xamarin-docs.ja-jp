---
title: "Android NUnit テスト プロジェクトを自動化する方法は?"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 11b693193b36a80b55a61308d98b76f4f6984e8a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit テスト プロジェクトを自動化する方法は?

> [!NOTE]
> このガイドでは、Android NUnit テスト プロジェクトを Xamarin.UITest プロジェクトではなくを設定するための手順について説明します。 Xamarin.UITest ガイドを参照して[ここ](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)です。

Android の単体テスト プロジェクト [Visual Studio for Mac] または単体テスト アプリ (Android) [Visual Studio] を作成するときに既定では、自動的にテスト実行しませんが、します。
Android 単体テストを自動化する: テストを実行する NUnit、ターゲット デバイスで、使用して、`Android.App.Instrumentation`サブクラスでは、作成を使用して実行できる、`adb shell am instrument`コマンド。

最初に、作成、 **TestInstrumentation.cs**のサブクラスを作成するファイル`Xamarin.Android.NUnitLite.TestSuiteInstrumentation`(で宣言されている`Xamarin.Android.NUnitLite.dll`)。 `TestInstrumentation(IntPtr, JniHandleOwnership)`コンス トラクター_必要があります_指定して、仮想`AddTests()`メソッドをオーバーライドする必要があります。
`AddTests()` どのテストが実際に実行を制御します。 このファイルは、大部分の定型句です。

次に、`.csproj`を追加するように変更する必要があります`TestInstrumentation.cs`です。

必要に応じて、`.csproj`を追加するように変更が、`RunTests`として単体テストを呼び出すことを許可する MSBuild ターゲット。

```shell
msbuild /t:RunTests Project.csproj
```

新しいターゲットを使用して必須ではありません。対応する adb コマンドは代わりに使用できます。

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

置き換える`@PACKAGE\_NAME@`適宜; にある値では、 **AndroidManifest.xml** `/manifest/@package`属性。

*重要な注意事項*: に、 [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming)リリースの場合は、Android の呼び出し可能ラッパーは、に基づいてエクスポートされる型のアセンブリ修飾名の md5 チェックサムの既定のパッケージ名。 これにより、2 つのアセンブリから指定して、パッケージのエラーを取得できませんに同じ完全修飾名です。 これを使用することを確認、\`名前\`プロパティを\`インストルメンテーション\`で属性を読み取り可能なについて/クラス名を生成します。

_について名を使用する必要があります、`adb`コマンド_です。 名前の変更リファクタリング (C#) クラス、したがって変更する必要が、`RunTests`正しいについて名前を使用するコマンド。

.Csproj ファイルへの追加:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

```cs 
using System;
using System.Reflection;
 
using Android.App;
using Android.Content;
using Android.Runtime;
 
using Xamarin.Android.NUnitLite;
 
namespace App.Tests {
 
    [Instrumentation(Name="app.tests.TestInstrumentation")]
    public class TestInstrumentation : TestSuiteInstrumentation {
 
        public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
        {
        }
 
        protected override void AddTests ()
        {
            AddTest (Assembly.GetExecutingAssembly ());
        }
    }
}
```

