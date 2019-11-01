---
title: Android NUnit テスト プロジェクトを自動化する方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/29/2018
ms.openlocfilehash: 1246eeac63a0ae232396d4c2fd69d8bf516f5e3e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026997"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit テスト プロジェクトを自動化する方法を教えてください

> [!NOTE]
> このガイドでは、UITest プロジェクトではなく、Android NUnit テストプロジェクトを自動化する方法について説明します。 UITest のガイドについては、[こちら](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)を参照してください。

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

    このファイルでは、`Xamarin.Android.NUnitLite.TestSuiteInstrumentation` ( **Xamarin. Android. .dll**) をサブクラス化して `TestInstrumentation`を作成します。

2. `TestInstrumentation` コンストラクターと `AddTests` メソッドを実装します。 `AddTests` メソッドは、実際に実行されるテストを制御します。

3. `.csproj` ファイルを変更して**TestInstrumentation.cs**を追加します。 (例:

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

4. 次のコマンドを使用して、単体テストを実行します。 `PACKAGE_NAME` をアプリのパッケージ名に置き換えます (パッケージ名は、アプリの `/manifest/@package` 属性にあり、 **Androidmanifest .xml**にあります)。

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. 必要に応じて、`.csproj` ファイルを変更して `RunTests` MSBuild ターゲットを追加できます。 これにより、次のようなコマンドを使用して単体テストを呼び出すことができます。

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (この新しいターゲットの使用は必須ではないことに注意してください。以前の `adb` コマンドは `msbuild`の代わりに使用できます)。

`adb shell am instrument` コマンドを使用して単体テストを実行する方法の詳細については、「 [ADB でテストを実行](https://developer.android.com/studio/test/command-line.html#RunTestsDevice)する Android 開発者向けのトピック」を参照してください。

> [!NOTE]
> [Xamarin 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming)リリースでは、android 呼び出し可能ラッパーの既定のパッケージ名は、エクスポートされる型のアセンブリ修飾名の MD5SUM に基づきます。 これにより、2つの異なるアセンブリから同じ完全修飾名を指定できるようになり、パッケージ化エラーは発生しません。 したがって、`Instrumentation` 属性の `Name` プロパティを使用して、読み取り可能な ACW/クラス名を生成するようにしてください。

_ACW 名は、上記の `adb` コマンドで使用する必要があり_ます。
クラスのC#名前を変更したりリファクタリングしたりするには、正しい ACW 名を使用するように`RunTests`コマンドを変更する必要があります。
