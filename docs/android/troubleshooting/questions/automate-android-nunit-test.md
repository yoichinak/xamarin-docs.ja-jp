---
title: Android NUnit テスト プロジェクトを自動化する方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: e96f9a0ce4d1eec9bf853faceeb85a2acb4840af
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761019"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit テスト プロジェクトを自動化する方法を教えてください

> [!NOTE]
> このガイドでは、UITest プロジェクトではなく、Android NUnit テストプロジェクトを自動化する方法について説明します。 UITest のガイドについては、[こちら](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)を参照してください。

Visual Studio (または Visual Studio for Mac の**Android 単体テスト**プロジェクト) で**単体テストアプリ (android)** プロジェクトを作成すると、既定では、このプロジェクトによってテストが自動的に実行されません。
ターゲットデバイスで NUnit テストを実行するには、次のコマンドを使用して起動される[app.xaml](xref:Android.App.Instrumentation)サブクラスを作成します。 

```shell
adb shell am instrument 
```

次の手順では、このプロセスについて説明します。

1. **TestInstrumentation.cs**という名前の新しいファイルを作成します。 

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

    このファイルでは`Xamarin.Android.NUnitLite.TestSuiteInstrumentation` 、( **Xamarin. Android. .dll**から) がサブクラスとして作成`TestInstrumentation`されます。

2. `TestInstrumentation` コンストラクター`AddTests`とメソッドを実装します。 メソッド`AddTests`は、実際に実行されるテストを制御します。

3. TestInstrumentation.cs を追加するようにファイルを変更します。`.csproj` 例えば:

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

4. 次のコマンドを使用して、単体テストを実行します。 を`PACKAGE_NAME`アプリのパッケージ名に置き換えます (パッケージ名は、 **androidmanifest .xml**にあるアプリの`/manifest/@package`属性にあります)。

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. 必要に応じて`.csproj` 、 `RunTests` MSBuild ターゲットを追加するようにファイルを変更できます。 これにより、次のようなコマンドを使用して単体テストを呼び出すことができます。

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (この新しいターゲットの使用は必須ではないこと`adb`に注意してください。 `msbuild`の代わりに前のコマンドを使用できます)。

`adb shell am instrument`コマンドを使用して単体テストを実行する方法の詳細については、「 [ADB でテストを実行](https://developer.android.com/studio/test/command-line.html#RunTestsDevice)する Android 開発者向けのトピック」を参照してください。

> [!NOTE]
> [Xamarin 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming)リリースでは、android 呼び出し可能ラッパーの既定のパッケージ名は、エクスポートされる型のアセンブリ修飾名の MD5SUM に基づきます。 これにより、2つの異なるアセンブリから同じ完全修飾名を指定できるようになり、パッケージ化エラーは発生しません。 そのため、 `Name` `Instrumentation`属性のプロパティを使用して、読み取り可能な ACW/クラス名を生成するようにしてください。

_ACW 名は、上記の`adb`コマンドで使用する必要があり_ます。
クラスのC#名前を変更したりリファクタリングしたり`RunTests`するには、正しい ACW 名を使用するようにコマンドを変更する必要があります。
