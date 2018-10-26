---
title: Android の概要
description: このドキュメントでは、Android での .NET の埋め込みの使用を開始する方法について説明します。 これは、.NET の埋め込み、Android ライブラリ プロジェクトを作成する、Android Studio プロジェクトとその他の考慮事項で生成された出力を使用してインストールするについて説明します。
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: lobrien
ms.author: laobri
ms.date: 03/28/2018
ms.openlocfilehash: e60892edfcf73f3e7cada8923e16bcc1be2c203e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121387"
---
# <a name="getting-started-with-android"></a>Android の概要

要件に加え、 [Java の概要](~/tools/dotnet-embedding/get-started/java/index.md)ガイドも必要になります。

* [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/)またはそれ以降
* [Android Studio 3.x](https://developer.android.com/studio/index.html) Java 1.8 を使用

概要については、としてご紹介します。

* 作成、 C# Android ライブラリ プロジェクト
* NuGet を使用して .NET の埋め込みをインストールします。
* Android ライブラリ アセンブリでの .NET の埋め込みの実行します。
* Android Studio での Java プロジェクトで生成された AAR ファイルを使用して、

## <a name="create-an-android-library-project"></a>Android ライブラリ プロジェクトを作成します。

Visual Studio の Windows または Mac を開き、新しい Android クラス ライブラリ プロジェクトを作成、名前を付けます**csharp からのこんにちは**、保存して **~/Projects/hello-from-csharp**または **%USERPROFILE%\Csharp からの Projects\hello**します。

という名前の新しい Android アクティビティを追加**HelloActivity.cs**に Android のレイアウトと、その後**Resource/layout/hello.axml**します。

新しい追加`TextView`レイアウト、および何か楽しいものにテキストを変更します。

レイアウト、ソースは、次のようになります。

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

アクティビティで呼び出していることを確認`SetContentView`と、新しいレイアウト。

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
> 忘れず、`[Register]`属性。 詳細については、次を参照してください。[制限](#current-limitations-on-android)します。

プロジェクトをビルドします。 結果として得られるアセンブリを保存する`bin/Debug/hello-from-csharp.dll`します。

## <a name="installing-net-embedding-from-nuget"></a>.NET の埋め込み NuGet からインストールします。

手順に従います。[指示](~/tools/dotnet-embedding/get-started/install/install.md)をインストールして、プロジェクトの .NET の埋め込みを構成します。

このようなコマンドの呼び出しを構成する必要がありますがなります。

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

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Android Studio プロジェクトで生成された出力を使用します。

1. Android Studio を開き、新しいプロジェクトを作成、**空のアクティビティ**します。
2. 右クリックし、**アプリ**モジュール選択**新規 > モジュール**します。
3. 選択**インポートします。JAR/。AAR パッケージ**します。
4. 検索するディレクトリ ブラウザーを使用して **~/Projects/hello-from-csharp/output/hello_from_csharp.aar**  をクリック**完了**します。

![Android Studio にインポート AAR](android-images/androidstudioimport.png)

これは、AAR ファイル コピーという名前の新しいモジュールに**hello_from_csharp**します。

![Android Studio プロジェクト](android-images/androidstudioproject.png)

新しいモジュールを使用する、**アプリ**を右クリックして**モジュール設定を開く**します。 **依存関係** タブで、新しい追加**モジュールの依存関係**選択 **: hello_from_csharp**します。

![Android Studio の依存関係](android-images/androidstudiodependencies.png)

アクティビティを追加、新しい`onResume`メソッド、および起動する次のコードを使用して、C#アクティビティ。

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

さらに 1 つの変更は、Android Studio プロジェクトで .NET を埋め込む必要があります。

アプリの開く**build.gradle**ファイルを開き、次の変更を追加します。

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

現在、Xamarin.Android は、APK から直接 .NET アセンブリを読み込み、アセンブリを圧縮しない必要しますが、あります。

このセットアップではありません、アプリが起動時にクラッシュし、コンソールに出力するようになります。

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>アプリを実行する

アプリを起動するには。

![こんにちはC#エミュレーターで実行されているサンプル](android-images/hello-from-csharp-android.png)

ここでの変更点に注意してください。

* ある、C#クラス、 `HelloActivity`、そのサブクラスの Java
* Android のリソース ファイルがあります。
* Android Studio で Java からこれらを使用しました

このサンプルを動作させるには、次のすべては、最終的な APK に設定されます。

* Xamarin.Android がアプリケーションの起動時に構成されています。
* 含まれる .NET アセンブリ**資産/アセンブリ**
* **AndroidManifest.xml**の変更、C#アクティビティなどです。
* Android のリソースやアセットの .NET ライブラリから
* [Android 呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)は`Java.Lang.Object`サブクラス

追加のチュートリアルを探している場合は、Charles Petzold の埋め込みについては、次のビデオをチェック_アウト[FingerPaint デモ](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/)Android Studio プロジェクトで。

[![Android 用 Embeddinator-4000](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Java 1.8 を使用します。

これを作成、時点で最善の方法は Android Studio 3.0 を使用する ([ここからダウンロード](https://developer.android.com/studio/index.html))。

アプリ モジュールのでは、Java 1.8 を有効にする**build.gradle**ファイル。

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

見てもを行うには、[テスト プロジェクトを Android Studio](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle)の詳細。 

Android Studio 2.3.x 安定版を使用する場合、非推奨のジャック ツール チェーンを有効にする必要があります。

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Android での現在の制限事項

現在のところ場合、サブクラス`Java.Lang.Object`、Xamarin.Android .NET の埋め込みではなく、Java スタブ (Android 呼び出し可能ラッパー) が生成されます。 このため、する必要があります、エクスポートするための同じ規則に従ってくださいC#Xamarin.Android として Java にします。 例えば:

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

* `[Register]` 必要な Java パッケージ名にマップするために必要な
* `[Export]` メソッドを Java に表示されるようにするが必要です。

使用して`ViewSubclass`Java で次のようにします。

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

詳細をご覧ください[Xamarin.Android での Java 統合](~/android/platform/java-integration/index.md)します。

## <a name="multiple-assemblies"></a>複数のアセンブリ

1 つのアセンブリの埋め込みは、単純です。ただし、1 つ以上が可能性が高くなりますがC#アセンブリ。 何度も Android サポート ライブラリやさらに複雑になる Google play 開発者サービスなどの NuGet パッケージの依存関係があります。

これにより、.NET の埋め込みなどの最終的な AAR にさまざまな種類のファイルを含める必要があるために、ジレンマをします。

* Android アセット
* Android のリソース
* Android のネイティブ ライブラリ
* Android の java ソース

ほとんどの場合の AAR に Android サポート ライブラリまたは Google play 開発者サービスからこれらのファイルを追加したくないが、Google からの公式バージョン Android Studio で使用したいです。

推奨される方法を次に示します。

* 自分が所有する任意のアセンブリを .NET の埋め込みを渡す (のソースがある) Java からを呼び出すと
* Android アセットやネイティブ ライブラリは、リソースから必要な任意のアセンブリを .NET の埋め込みを渡す
* ライブラリまたは Google play 開発者サービスを Android Studio でサポートして、Android などの Java 依存関係の追加します。

したがって、コマンドは次のようになります。

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

NuGet から何も除外する必要があります、Android アセット、リソース、および Android Studio プロジェクトで必要とするなどがあることを確認しない限り、します。 Java、およびリンカーを呼び出す必要はありませんの依存関係を省略することもできます。_する必要があります_ために必要なライブラリのパーツが含まれています。

Android Studio で、必要な Java 依存関係を追加する、 **build.gradle**ファイルようになります。

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>関連項目

* [Android コールバック](~/tools/dotnet-embedding/android/callbacks.md)
* [Android の予備調査](~/tools/dotnet-embedding/android/index.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献します。](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>関連リンク

- [天気の例 (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
