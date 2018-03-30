---
title: Android NUnit テスト プロジェクトを自動化する方法は?
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2018
ms.openlocfilehash: 4d8ea267ef3a6bdea2db805725d5dd4220ab73c4
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit テスト プロジェクトを自動化する方法は?

> [!NOTE]
> このガイドでは、Android NUnit テスト プロジェクトを Xamarin.UITest プロジェクトではなくを自動化する方法について説明します。 Xamarin.UITest ガイドを参照して[ここ](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)です。

作成するときに、**単体テスト アプリ (Android)** Visual Studio でプロジェクト (または**Android の単体テスト**Mac 用の Visual Studio でプロジェクト)、このプロジェクトは、自動的に既定では、テストを実行します。
テストを実行する NUnit、ターゲット デバイスで、作成することができます、 [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/)サブクラスは次のコマンドを使用して起動します。 

```shell
adb shell am instrument 
```

次の手順では、このプロセスについて説明します。

1.  という名前の新しいファイルを作成する**TestInstrumentation.cs**: 

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
    このファイルに[Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (から**Xamarin.Android.NUnitLite.dll**) を作成するサブクラス`TestInstrumentation`です。

2.  実装、 [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/)コンス トラクターと[AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29)メソッドです。 `AddTests`メソッド コントロールがどのテストが実際に実行します。

3.  変更、`.csproj`を追加するファイル**TestInstrumentation.cs**です。 例えば:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

3.  単体テストを実行するのにには、次のコマンドを使用します。 置き換える`PACKAGE_NAME`アプリのパッケージ名を持つ (アプリのパッケージ名が見つかりません`/manifest/@package`に属性がある**AndroidManifest.xml**)。

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  必要に応じて変更することができます、`.csproj`を追加するファイル、 `RunTests` MSBuild ターゲットです。 これによって、次のようなコマンドを使用して単体テストを呼び出します。

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (注この新しいターゲットを使用している必要はありません以外の場合は、以前`adb`の代わりにコマンドを使用できる`msbuild`)。

使用しての詳細については、`adb shell am instrument`単体テストを実行し、Android Developer を参照してくださいコマンド[ADB でテストを実行している](https://developer.android.com/studio/test/command-line.html#RunTestsDevice)トピックです。


> [!NOTE]
> [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming)リリースの場合は、Android の呼び出し可能ラッパーは、に基づいてエクスポートされる型のアセンブリ修飾名の md5 チェックサムの既定のパッケージ名。 これにより、2 つのアセンブリから指定して、パッケージのエラーを取得できませんに同じ完全修飾名です。 これを使用することを確認、`Name`プロパティを`Instrumentation`読み取り可能なについて/クラス名を生成する属性。

_について名を使用する必要があります、`adb`上記のコマンド_です。
名前の変更リファクタリング (C#) クラス、したがって変更する必要が、`RunTests`正しいについて名前を使用するコマンド。

