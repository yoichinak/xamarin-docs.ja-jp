---
title: Android の概要
description: このドキュメントでは、Android と .NET の埋め込みを使用して作業を開始する方法について説明します。 これは、.NET の埋め込み、Android ライブラリ プロジェクトの作成、Android Studio プロジェクトとその他の考慮事項で生成された出力を使用してインストールするについて説明します。
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 6fbd46578f07692f266d97279031f1893bb96a1f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793918"
---
# <a name="getting-started-with-android"></a>Android の概要

要件に加え、 [Java の概要](~/tools/dotnet-embedding/get-started/java/index.md)ガイドも必要です。

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/)以降
* [Android Studio 3.x](https://developer.android.com/studio/index.html) with Java 1.8

概要については、としては、次の操作を行います。

* C# での Android ライブラリ プロジェクトを作成します。
* .NET の埋め込み NuGet 経由でのインストールします。
* Android ライブラリ アセンブリ上で .NET の埋め込みを実行します。
* Android Studio での Java プロジェクトで生成された AAR ファイルを使用します。

## <a name="create-an-android-library-project"></a>Android ライブラリ プロジェクトを作成します。

Visual Studio for Windows または Mac を開き、新しい Android のクラス ライブラリ プロジェクトを作成、名前を付けます**csharp からのこんにちは**、しに保存 **~/Projects/hello-from-csharp**または **%USERPROFILE%\Csharp からの Projects\hello**です。

アクティビティを追加する新しい Android という**HelloActivity.cs**に Android のレイアウトと、その後**Resource/layout/hello.axml**です。

新しい`TextView`レイアウト、および楽しいものにテキストを変更します。

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

アクティビティを呼び出していることを確認`SetContentView`新しいレイアウトに。

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
> 必ず、`[Register]`属性。 詳細については、「[制限](#current-limitations-on-android)です。

プロジェクトをビルドします。 結果として得られるアセンブリを保存する`bin/Debug/hello-from-csharp.dll`です。

## <a name="installing-net-embedding-from-nuget"></a>.NET は NuGet から埋め込みをインストールします。

手順に従います。[指示](~/tools/dotnet-embedding/get-started/install/install.md)をインストールして、プロジェクトの .NET の埋め込みを構成します。

構成する必要がありますコマンドの呼び出しは、次のようになります。

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

1. Android Studio を開きで新しいプロジェクトを作成、**空のアクティビティ**です。
2. 右クリックし、**アプリ**モジュールを選択して**新規 > モジュール**です。
3. 選択**インポートします。JAR/です。[Aar] パッケージ**です。
4. 検索するディレクトリの参照を使用して **~/Projects/hello-from-csharp/output/hello_from_csharp.aar**  をクリック**完了**です。

![Android Studio へのインポート [aar]](android-images/androidstudioimport.png)

これには、という名前の新しいモジュールに AAR ファイルはコピー **hello_from_csharp**です。

![Android Studio プロジェクト](android-images/androidstudioproject.png)

新しいモジュールを使用する、**アプリ**を右クリックして**モジュール設定を開く**です。 **の依存関係** タブで、新しい**モジュールの依存関係**選択 **: hello_from_csharp**です。

![Android Studio 依存関係](android-images/androidstudiodependencies.png)

アクティビティ内の追加、新しい`onResume`メソッド、および使用する次のコードを c# アクティビティの起動。

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

さらに 1 つの変更は、Android Studio プロジェクトで .NET の埋め込みに必要です。

アプリの開いて**build.gradle**ファイルし、次の変更を追加します。

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin.Android は現在、APK から直接 .NET アセンブリを読み込みますが、圧縮されずにアセンブリが必要です。

このセットアップがあるない場合、アプリが起動時にクラッシュして、コンソールに出力する次のように。

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>アプリを実行する

アプリを起動するには。

![エミュレーターで実行されている c# のサンプルからこんにちは](android-images/hello-from-csharp-android.png)

ここでの変更点に注意してください。

* C# の場合、クラスがある`HelloActivity`、そのサブクラス Java
* Android のリソース ファイルがあります。
* Android Studio で Java からこれらを使用しました

このサンプルが動作するには、次のすべては、最終的な APK に設定されます。

* Xamarin.Android がアプリケーション開始時に構成されています。
* 含まれる .NET アセンブリ**資産/アセンブリ**
* **AndroidManifest.xml** c# のアクティビティなどの変更点です。
* Android のリソースと .NET のライブラリからの資産
* [Android の呼び出し可能ラッパー](~/android/platform/java-integration/android-callable-wrappers.md)のいずれか`Java.Lang.Object`サブクラス

その他のチュートリアルを探している場合はチェック アウト Charles Petzold を埋め込むことを示しています、次のビデオ[フィンガー ペイント デモ](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/)Android Studio プロジェクトで。

[![Android 用の Embeddinator 4000](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Java 1.8 を使用します。

これを記述する時点で、最適なオプションは、Android Studio 3.0 を使用する ([ここからダウンロード](https://developer.android.com/studio/index.html))。

アプリ モジュールの内 1.8 の Java を有効にする**build.gradle**ファイル。

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

見てもを行うには、 [Android Studio のテスト プロジェクト](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle)詳細についてはします。 

Android Studio 2.3.x 安定を使用しようとしている場合は、非推奨の回線のモジュラー ジャック ツール チェーンを有効にする必要があります。

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

場合今サブクラス`Java.Lang.Object`Xamarin.Android、Java スタブ (Android 呼び出し可能ラッパー) は、.NET 埋め込む代わりが生成されます。 このためには c# Xamarin.Android として Java へのエクスポートの同じ規則に従う必要があります。 例えば:

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

* `[Register]` 必要な Java のパッケージ名にマップするために必要な
* `[Export]` メソッドを Java に表示されるようにするために必要な

使用して`ViewSubclass`Java で次のようにします。

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

詳細について[Xamarin.Android と Java 統合](~/android/platform/java-integration/index.md)です。

## <a name="multiple-assemblies"></a>複数のアセンブリ

1 つのアセンブリの埋め込みは簡単です。ただし、1 つの c# アセンブリよりも多くする必要が可能性が高くなりますです。 何度も Android サポート ライブラリや処理がさらに複雑になる Google Play サービスなどの NuGet パッケージの依存関係があります。

これにより、.NET の埋め込みをなど、最終的な AAR にさまざまな種類のファイルを含める必要があるために、ジレンマ、します。

* Android の資産
* Android のリソース
* Android のネイティブ ライブラリ
* Android の java ソース

ほとんどの場合、[aar] に、Android のサポート ライブラリまたは Google Play サービスからこれらのファイルを含めるしないが、Android Studio で Google からの公式のバージョンを使用する方が。

推奨される方法を次に示します。

* 所有している任意のアセンブリを .NET の埋め込みを渡す (のソースがある) Java から呼び出そうとして
* Android の資産、ネイティブ ライブラリ、またはからリソースに必要な任意のアセンブリを .NET の埋め込みを渡す
* Android のような Java の依存関係 Android Studio でのライブラリまたは Google Play サービスのサポートを追加します。

したがって、コマンドがあります。

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

NuGet から何も除外する必要があります、することを調べる場合を除きが含まれている Android の資産、リソース、および Android Studio プロジェクトで必要とするなどです。 Java、およびリンカーからを呼び出す必要はありませんの依存関係を省略することも_必要があります_ライブラリのために必要なパーツが含まれています。

Android Studio で、必要な Java の依存関係を追加する、 **build.gradle**ファイルようになります。

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>関連項目

* [Android でのコールバック](~/tools/dotnet-embedding/android/callbacks.md)
* [Android の事前調査](~/tools/dotnet-embedding/android/index.md)
* [.NET の埋め込みの制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>関連リンク

- [気象サンプル (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
