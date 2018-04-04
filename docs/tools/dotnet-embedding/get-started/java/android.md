---
title: Android の概要
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: ed3d6ae3f8537bfae456c6919743e8c9fbfb2009
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-android"></a>Android の概要


要件に加え、 [Java の概要](~/tools/dotnet-embedding/get-started/java/index.md)ガイドも必要です。

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/)以降
* [Android Studio 3.x](https://developer.android.com/studio/index.html) with Java 1.8

概要については、としては、次の操作を行います。

* C# での Android ライブラリ プロジェクトを作成します。
* .NET の埋め込み NuGet 経由でのインストールします。
* Embeddinator を Android ライブラリ アセンブリを実行します。
* Android Studio での Java プロジェクトで生成された AAR ファイルを使用します。

## <a name="create-an-android-library-project"></a>Android ライブラリ プロジェクトを作成します。

Visual Studio for Windows または Mac を開き、新しい Android のクラス ライブラリ プロジェクトを作成、名前を付けます`hello-from-csharp`、しに保存`~/Projects/hello-from-csharp`または`%USERPROFILE%\Projects\hello-from-csharp`です。

アクティビティを追加する新しい Android という`HelloActivity.cs`に Android のレイアウトと、その後`Resource/layout/hello.axml`です。

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
*注: ことを忘れないでください、`[Register]`属性制限の下に詳細を参照してください*

プロジェクトをビルド、結果として得られるアセンブリに保存する`bin/Debug/hello-from-csharp.dll`です。

## <a name="installing-net-embedding-from-nuget"></a>.NET は NuGet から埋め込みをインストールします。

選択**追加 > NuGet パッケージを追加しています.**およびインストール`Embeddinator-4000`NuGet パッケージ マネージャーから: ![NuGet Package Manager](android-images/visualstudionuget.png)

これがインストールされます`Embeddinator-4000.exe`に、`packages/Embeddinator-4000/tools`ディレクトリ。

## <a name="run-embeddinator-4000"></a>Embeddinator 4000 を実行します。

Embeddinator を実行し、Android ライブラリ プロジェクト アセンブリのネイティブ AAR ファイルを作成するビルド後のステップを追加します。

Mac 用 Visual Studio でに移動_プロジェクトのオプション > ビルド > カスタム コマンド_を追加し、_後にビルド_手順です。

次の commnd をセットアップするには。
```
mono '${SolutionDir}/packages/Embeddinator-4000.0.2.0.80/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

> [!NOTE]
> 注: は NuGet からインストールしたバージョン番号を使用するを確認してください。

C# プロジェクトの開発を継続的な操作を実行する場合は、追加することも、カスタム コマンドをクリーンアップするのには、 `output` Embeddinator を実行する前にディレクトリ。
```
rm -Rf '${SolutionDir}/output/'
```

![カスタム ビルド アクション](android-images/visualstudiocustombuild.png)

Android AAR ファイルが配置される`~/Projects/hello-from-csharp/output/hello_from_csharp.aar`です。 _注: ハイフンは Java はサポートされていないためパッケージ名に置き換えられます。_

### <a name="running-net-embedding-on-windows"></a>実行中の .NET Windows 上の埋め込み

同じで、本質的に設定されますが、Windows で Visual Studio のメニューはわずかに異なります。 シェル コマンドも若干異なります。

移動して**プロジェクトのオプション > ビルド イベント**に次を入力し、**ビルド後に実行するコマンドライン**ボックス。
```
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

次のスクリーン ショット: など

![Windows 上の Embeddinator](android-images/visualstudiowindows.png)

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Android Studio プロジェクトで生成された出力を使用します。

1. Android Studio を開きで新しいプロジェクトを作成、**空のアクティビティ**です。
2. 右クリックし、`app`モジュールを選択して**新規 > モジュール**です。
3. 選択**インポートします。JAR/です。[Aar] パッケージ**です。
4. 検索するディレクトリの参照を使用して`~/Projects/hello-from-csharp/output/hello_from_csharp.aar` をクリック**完了**です。

![Android Studio へのインポート [aar]](android-images/androidstudioimport.png)

これには、という名前の新しいモジュールに AAR ファイルはコピー`hello_from_csharp`です。

![Android Studio Project](android-images/androidstudioproject.png)

新しいモジュールを使用する、`app`を右クリックして**モジュール設定を開く**です。 **の依存関係** タブで、新しい**モジュールの依存関係**選択`:hello_from_csharp`です。

![Android Studio 依存関係](android-images/androidstudiodependencies.png)

アクティビティ内の追加、新しい`onResume`メソッド、および C# の場合、アクティビティを起動する次のコードを使用します。

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

### <a name="assembly-compression-important"></a>アセンブリの圧縮*重要*

さらに 1 つの変更は、Android Studio プロジェクトで .NET の埋め込みに必要です。

アプリの開いて`build.gradle`ファイルし、次の変更を追加します。
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

```csharp
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>アプリを実行する

アプリを起動するには。

![エミュレーターで実行されている c# のサンプルからこんにちは](android-images/hello-from-csharp-android.png)

ここでの変更点に注意してください。

* C# の場合、クラスがある`HelloActivity`、そのサブクラス Java
* Android のリソース ファイルがあります。
* Android Studio で Java からこれらを使用しました

そのためこのサンプルが動作するには、次のすべてでは、最終的な APK のセットアップには。

* Xamarin.Android がアプリケーション開始時に構成されています。
* 含まれる .NET アセンブリ `assets/assemblies`
* `AndroidManifest.xml` c# アクティビティなどの変更点です。
* Android のリソースと .NET のライブラリからの資産
* [Android の呼び出し可能ラッパー](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/android_callable_wrappers/)のいずれか`Java.Lang.Object`サブクラス

その他のチュートリアルを探している場合をチェック アウトこのビデオを埋め込む Charles Petzold[フィンガー ペイント デモ](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/)Android Studio でプロジェクトの次に。

[![Android 用の Embeddinator 4000](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Java 1.8 を使用します。

これを記述する時点で、最適なオプションは、Android Studio 3.0 を使用する ([ここからダウンロード](https://developer.android.com/studio/index.html))。

アプリ モジュールの内 1.8 の Java を有効にする`build.gradle`ファイル。
```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
Android Studio のテスト プロジェクトを見ても行える[ここ](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle)詳細についてはします。

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

今すぐ場合サブクラス`Java.Lang.Object`、Xamarin.Android Embeddinator の代わりに、Java スタブ (Android 呼び出し可能ラッパー) が生成されます。

したがって、c# Xamarin.Android として Java へのエクスポートと同じ規則に従ってください。

したがって、たとえば C# の場合。
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

これにより、Embeddinator が最終的な AAR になどさまざまな種類のファイルを含める必要があるために、ジレンマ、します。
* Android の資産
* Android のリソース
* Android のネイティブ ライブラリ
* Android の java ソース

ほとんどの場合、[aar] に、Android のサポート ライブラリまたは Google Play サービスからこれらのファイルを含めるしないが、Android Studio で Google からの公式のバージョンを使用する方が。

推奨される方法を次に示します。
* Embeddinator を所有している任意のアセンブリに渡す (のソースがある) Java から呼び出そうとして
* Embeddinator Android 資産、ネイティブ ライブラリ、またはからリソースに必要な任意のアセンブリを渡す
* Android のような Java の依存関係 Android Studio でのライブラリまたは Google Play サービスのサポートを追加します。

したがって、コマンドがあります。
```
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```
NuGet から何も除外する必要があります、することを調べる場合を除きが含まれている Android の資産、リソース、および Android Studio プロジェクトで必要とするなどです。 Java、およびリンカーからを呼び出す必要はありませんの依存関係を省略することも_必要があります_ライブラリのために必要なパーツが含まれています。

Android Studio で、必要な Java の依存関係を追加する、`build.gradle`ファイルようになります。
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
* [Embeddinator の制限事項](~/tools/dotnet-embedding/limitations.md)
* [オープン ソース プロジェクトに貢献しています。](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [エラー コードと説明](~/tools/dotnet-embedding/errors.md)


## <a name="related-links"></a>関連リンク

- [気象サンプル (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
