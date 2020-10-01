---
title: Android NUnit テスト プロジェクトを自動化する方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/29/2018
ms.openlocfilehash: c8c9e721bc46d9071bb2af479a5e1d37b93fce27
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91458200"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit テスト プロジェクトを自動化する方法を教えてください

> [!NOTE]
> このガイドでは、Xamarin.UITest プロジェクトではなく、Android NUnit テスト プロジェクトを自動化する方法について説明します。 Xamarin.UITest のガイドについては、[こちら](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)をご覧ください。

Visual Studio で**単体テスト アプリ (Android)** プロジェクト (または、Visual Studio for Mac で **Android 単体テスト** プロジェクト) を作成すると、既定では、このプロジェクトによってテストは自動的に実行されません。
ターゲット デバイスで NUnit テストを実行するには、次のコマンドを使用して開始される [Android.App.Instrumentation](xref:Android.App.Instrumentation) サブクラスを作成します。 

```shell
adb shell am instrument 
```

次の手順では、このプロセスについて説明します。

1. **TestInstrumentation.cs** という名前の新しいファイルを作成します。 

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

    このファイルでは、`TestInstrumentation` を作成するために (**Xamarin.Android.NUnitLite.dll** から) `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` をサブクラス化します。

2. `TestInstrumentation` コンストラクターと `AddTests` メソッドを実装します。 `AddTests` メソッドでは、実際に実行されるテストを制御します。

3. `.csproj` ファイルを変更して、**TestInstrumentation.cs** を追加します。 次に例を示します。

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

4. 単体テストを実行するには、次のコマンドを使用します。 `PACKAGE_NAME` をアプリのパッケージ名に置き換えます (パッケージ名は、**AndroidManifest.xml** にあるアプリの `/manifest/@package` 属性で確認できます)。

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. 必要に応じて、`.csproj` ファイルを変更して `RunTests` MSBuild ターゲットを追加できます。 これにより、次のようなコマンドを使用して単体テストを呼び出すことができます。

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (この新しいターゲットの使用は必須ではないことに注意してください。前の `adb` コマンドを `msbuild` の代わりに使用できます)。

`adb shell am instrument` コマンドを使用して単体テストを実行する方法について詳しくは、Android デベロッパーの「[ADB を使用したテストの実行](https://developer.android.com/studio/test/command-line.html#RunTestsDevice)」トピックをご覧ください。

> [!NOTE]
> [Xamarin.Android 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) リリースでは、Android 呼び出し可能ラッパーの既定のパッケージ名は、エクスポートされる型のアセンブリ修飾名の MD5SUM に基づきます。 これにより、2 つの異なるアセンブリから同じ完全修飾名を指定できるようになり、パッケージ化エラーは発生しません。 したがって、`Instrumentation` 属性の `Name` プロパティを使用して、読み取り可能な ACW/クラス名を生成するようにしてください。

"_ACW 名は、上の `adb` コマンドで使用する必要があります_"。
したがって、C# クラスの名前を変更したりリファクタリングしたりするには、正しい ACW 名を使用するように `RunTests` コマンドを変更する必要があります。