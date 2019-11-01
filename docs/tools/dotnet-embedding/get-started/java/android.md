---
title: Android の概要
description: このドキュメントでは、Android での .NET 埋め込みの使用を開始する方法について説明します。 .NET 埋め込みのインストール、Android ライブラリプロジェクトの作成、Android Studio プロジェクトで生成された出力の使用、およびその他の考慮事項について説明します。
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: davidortinau
ms.author: daortin
ms.date: 03/28/2018
ms.openlocfilehash: bcda03d41cb3bafcfb3ee4b92046014cc5b0c119
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029771"
---
# <a name="getting-started-with-android"></a>Android の概要

[Java](~/tools/dotnet-embedding/get-started/java/index.md)の概要に関するガイドの要件に加えて、次のものも必要です。

- [Xamarin Android 7.5](https://visualstudio.microsoft.com/xamarin/)以降
- Java 1.8 での[Android Studio](https://developer.android.com/studio/index.html) 3.x

概要として、次のことを行います。

- Android ライブラリC#プロジェクトを作成する
- NuGet を使用して .NET 埋め込みをインストールする
- Android ライブラリアセンブリでの .NET 埋め込みの実行
- Android Studio の Java プロジェクトで生成された AAR ファイルを使用する

## <a name="create-an-android-library-project"></a>Android ライブラリプロジェクトを作成する

Visual Studio for Windows または Mac を開き、新しい Android クラスライブラリプロジェクトを作成します。このプロジェクトには、 **csharp**という名前を付け、 **~/Projects/hello-from-csharp**または **%USERPROFILE%\Projects\hello-from-csharp**に保存します。

**HelloActivity.cs**という名前の新しい android アクティビティを追加し、その後に**リソース/レイアウト/こんにちは. Axml**で android レイアウトを追加します。

レイアウトに新しい `TextView` を追加し、テキストを楽しいものに変更します。

レイアウトソースは次のようになります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

アクティビティで、新しいレイアウトで `SetContentView` を呼び出すようにします。

```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```

> [!NOTE]
> `[Register]` 属性を忘れないでください。 詳細については、「[制限事項](#current-limitations-on-android)」を参照してください。

プロジェクトをビルドします。 結果として得られるアセンブリは `bin/Debug/hello-from-csharp.dll`に保存されます。

## <a name="installing-net-embedding-from-nuget"></a>NuGet からの .NET 埋め込みのインストール

次の[手順](~/tools/dotnet-embedding/get-started/install/install.md)に従って、プロジェクトの .Net 埋め込みをインストールして構成します。

構成する必要があるコマンドの呼び出しは次のようになります。

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>生成された出力を Android Studio プロジェクトで使用する

1. Android Studio を開き、**空のアクティビティ**を含む新しいプロジェクトを作成します。
2. **アプリ**モジュールを右クリックし、 **[新しい > モジュール]** を選択します。
3. [インポート] を選択し**ます。JAR/。AAR パッケージ**。
4. ディレクトリブラウザーを使用して **~/Projects/hello-from-csharp/output/hello_from_csharp.aar**を検索し、 **[完了]** をクリックします。

![AAR を Android Studio にインポートする](android-images/androidstudioimport.png)

これにより、AAR ファイルが**hello_from_csharp**という名前の新しいモジュールにコピーされます。

![Android Studio プロジェクト](android-images/androidstudioproject.png)

**アプリ**から新しいモジュールを使用するには、右クリックし、 **[モジュール設定を開く]** を選択します。 **[依存関係]** タブで、新しい**モジュールの依存関係**を追加し、 **: hello_from_csharp**を選択します。

![Android Studio の依存関係](android-images/androidstudiodependencies.png)

アクティビティで、新しい `onResume` メソッドを追加し、次のコードを使用してC#アクティビティを起動します。

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>アセンブリの圧縮 (*重要*)

また、Android Studio プロジェクトに .NET を埋め込むには、さらに変更が必要になります。

アプリの**gradle**ファイルを開き、次の変更を追加します。

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin Android では、現在、APK から直接 .NET アセンブリを読み込んでいますが、アセンブリを圧縮しない必要があります。

この設定がない場合は、起動時にアプリがクラッシュし、次のような内容がコンソールに出力されます。

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>アプリを実行する

アプリを起動すると、次のようになります。

![エミュレーターでC#実行されているサンプルからの Hello](android-images/hello-from-csharp-android.png)

次の点に注意してください。

- Java のサブC#クラスである`HelloActivity`クラスがあります。
- Android リソースファイルがあります
- これらは、の Java から使用されてい Android Studio

このサンプルが機能するためには、次のすべてが最終的な APK で設定されています。

- Xamarin. Android はアプリケーションの開始時に構成されます。
- **アセット/アセンブリ**に含まれる .net アセンブリ
- アクティビティのための xml の変更 (など) C#
- Android のリソースと .NET ライブラリからのアセット
- 任意の `Java.Lang.Object` サブクラスの[Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)

追加のチュートリアルをお探しの場合は、次のビデオを参照してください。これは、Android Studio プロジェクトでのチャールズ Petzold 著) の[FingerPaint demo](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)の埋め込みを示しています。

[Android 用の![Embeddinator-4000](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Java 1.8 の使用

この記事の執筆時点では、Android Studio 3.0 ([ここからダウンロード](https://developer.android.com/studio/index.html)) を使用することをお勧めします。

アプリモジュールの**gradle**ファイルで Java 1.8 を有効にするには、次のようにします。

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

詳細については、 [Android Studio テストプロジェクト](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle)を参照することもできます。 

Android Studio 2.3. x stable を使用する場合は、非推奨のジャックツールチェーンを有効にする必要があります。

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Android に関する現在の制限事項

現時点では、`Java.Lang.Object`をサブクラス化すると、.NET 埋め込みではなく、Xamarin Android によって Java スタブ (Android 呼び出し可能ラッパー) が生成されます。 このため、Java を Xamarin Android にエクスポートC#する場合は、同じ規則に従う必要があります。 (例:

```csharp
[Register("mono.embeddinator.android.ViewSubclass")]
public class ViewSubclass : TextView
{
    public ViewSubclass(Context context) : base(context) { }

    [Export("apply")]
    public void Apply(string text)
    {
        Text = text;
    }
}
```

- 必要な Java パッケージ名にマップするために `[Register]` が必要です
- メソッドを Java から参照できるようにするには `[Export]` が必要です

Java では、次のように `ViewSubclass` を使用できます。

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

詳細については[、「Xamarin Android と Java の統合」を](~/android/platform/java-integration/index.md)参照してください。

## <a name="multiple-assemblies"></a>複数のアセンブリ

単一のアセンブリの埋め込みは簡単です。ただし、多くの場合、複数のC#アセンブリが存在する可能性が高くなります。 Android サポートライブラリや Google Play 開発者サービスなど、さらに複雑になる NuGet パッケージに対する依存関係が多数あります。

.NET の埋め込みには、次のような最終的な AAR に多くの種類のファイルを含める必要があるため、このようなジレンマが発生します。

- Android アセット
- Android のリソース
- Android ネイティブライブラリ
- Android java ソース

多くの場合、これらのファイルを Android サポートライブラリから含めたり、AAR に Google Play 開発者サービスしたりする必要はありませんが、Android Studio では Google の公式バージョンを使用します。

推奨される方法を次に示します。

- 自分が所有している (ソースがある) アセンブリを .NET に渡し、Java から呼び出す
- Android アセット、ネイティブライブラリ、またはリソースが必要なすべてのアセンブリを .NET に渡す
- Android サポートライブラリや Google Play 開発者サービスのような Java の依存関係を Android Studio に追加します。

コマンドは次のようになります。

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Android Studio プロジェクトで必要になる Android 資産やリソースなどが含まれていることがわかっている場合を除き、NuGet からは何も除外する必要があります。 Java から呼び出す必要のない依存関係を省略したり、必要なライブラリの部分_をリンカーに_含めたりすることもできます。

Android Studio に必要な Java 依存関係を追加するために、 **gradle**ファイルは次のようになります。

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>関連項目

- [Android でのコールバック](~/tools/dotnet-embedding/android/callbacks.md)
- [Android の暫定版の研究](~/tools/dotnet-embedding/android/index.md)
- [.NET 埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
- [オープンソースプロジェクトへの貢献](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [エラーコードと説明](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>関連リンク

- [Weather サンプル (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
