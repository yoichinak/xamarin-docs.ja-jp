---
title: "バインドします。JAR"
description: "このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成するための手順を説明します。JAR ファイルです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 011b6d184e55c9054a845d4922687b4565221859
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="binding-a-jar"></a>バインドします。JAR

_このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成するための手順を説明します。JAR ファイルです。_

## <a name="overview"></a>概要

Android のコミュニティには、アプリで使用することがある多数の Java ライブラリが用意されています。 これらの Java ライブラリは、多くの場合にパッケージ化します。JAR (Java アーカイブ) 形式では、パッケージ化します。JAR、 *Java バインド ライブラリ*その機能を Xamarin.Android アプリに使用できるようにします。 Java バインド ライブラリの目的は、Api を実行する、します。JAR ファイルが c# コードを自動的に生成されたラッパーを使用してコードを使用できます。

Xamarin ツールでは、1 つまたは複数の入力からバインド ライブラリを生成できます。JAR ファイル。 バインドのライブラリ (です。DLL アセンブリ) には、次のものが含まれています。 

-   元の内容。JAR ファイル。

-   マネージ呼び出し可能ラッパー (MCW) は c# 型内で対応する Java の型をラップします。JAR ファイル。

MCW に生成されたコードでは、API 呼び出しを転送する JNI (Java ネイティブ インターフェイス) を使用して、基になります。JAR ファイルです。 任意のバインドのライブラリを作成できます。Android (Xamarin ツールが Android Java 以外のライブラリのバインドを現在サポートしていないことに注意してください) と共に使用する本来の目的 JAR ファイルです。 内容を含めずにバインディング ライブラリをビルドすることもできます、します。JAR ファイルが DLL に依存しているようにします。実行時に JAR します。

このガイドでは 1 つのバインドのライブラリの作成の基本おステップをします。JAR ファイルです。 どこすべて移動右例と共にについて説明します&ndash;は、カスタマイズはまたはのバインディングのデバッグが必要な場所です。 
[バインディングを使用してメタデータを作成する](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)をバインディング プロセスは完全自動ではないと、ある程度の手動による介入が必要より高度なシナリオの例を提供します。 Java ライブラリ バインドの一般 (基本的なコード例では) の概要については、次を参照してください。[バインド Java ライブラリ](~/android/platform/binding-java-library/index.md)です。 

 
## <a name="walkthrough"></a>チュートリアル

次のチュートリアルを作成してバインド ライブラリ[ピカソ](http://square.github.io/picasso/)、人気のある Android。イメージの読み込みとキャッシュ機能を提供する JAR です。 次の手順を使ってバインドを**ピカソ 2.x.x.jar** Xamarin.Android プロジェクトで使用できる新しい .NET アセンブリを作成します。 

1. 新しい Java バインド ライブラリ プロジェクトを作成します。

2. 追加します。JAR ファイルをプロジェクト。

3. 適切なビルド アクションを設定します。JAR ファイルです。

4. ターゲット フレームワークを選択します。JAR をサポートします。

5. バインドのライブラリをビルドします。

バインドのライブラリで作成した後は、バインドのライブラリの Api を呼び出すために機能を示す小さな Android アプリを作成おします。 この例でのメソッドにアクセスする必要が**ピカソ 2.x.x.jar**:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}

```csharp
After we generate a Bindings Library for **picasso-2.x.x.jar**,
we can call these methods from C#. For example:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

<a name="creating" />

### <a name="creating-the-bindings-library"></a>バインドのライブラリを作成します。

下記の手順を開始する前にダウンロード[ピカソ 2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)です。

最初に、新しいバインドのライブラリ プロジェクトを作成します。 Mac または Visual Studio の Visual Studio で新しいソリューションを作成し、選択、 *Android バインド ライブラリ*テンプレート。 (このチュートリアルのスクリーン ショットは、Visual Studio を使用して、Visual Studio for Mac は非常に似ています)。ソリューションの名前を付けます**JarBinding**: 

[ ![JarBinding ライブラリ プロジェクトを作成します。](binding-a-jar-images/01-new-bindings-library-sml.png)](binding-a-jar-images/01-new-bindings-library.png)

テンプレートに含まれる、 **Jar**フォルダーを追加する場所、します。バインドのライブラリ プロジェクトへ JAR(s) です。 右クリックし、 **Jar**フォルダーと選択**追加 > 既存の項目**: 

[ ![既存項目を追加します。](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png)

移動し、**ピカソ 2.x.x.jar**ファイルは事前にダウンロードして選択し、をクリックして**追加**: 

[ ![Jar ファイルを選択し、[追加] をクリックしてください](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png)

いることを確認、**ピカソ 2.x.x.jar**プロジェクトにファイルを正常に追加します。 

[ ![プロジェクトに追加 jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png)

Java バインド ライブラリ プロジェクトを作成するときにする必要がありますを指定するかどうか、します。JAR は、バインド ライブラリに埋め込まれているか、個別にパッケージ化することです。 次のいずれかの手順を実行する指定した*ビルド アクション*: 

-   **EmbeddedJar** &ndash;します。バインド ライブラリ JAR が埋め込まれます。

-   **InputJar** &ndash;します。バインドのライブラリから個別 JAR が保持されます。

通常は、使用、 **EmbeddedJar**ビルド アクションできるようにします。JAR はバインドのライブラリに自動的にパッケージ化します。 これは、最も簡単なオプション&ndash;で Java バイトコード、します。JAR では、Dex バイトコードに変換され、(管理された呼び出し可能ラッパー) と共に、APK に埋め込まれます。 保持する場合、します。バインド ライブラリから個別の JAR、使用することができます、 **InputJar**オプションです。 ただし、ことを確認します。JAR ファイルは、アプリを実行しているデバイスで使用できます。 

ビルド アクション設定**EmbeddedJar**: 

[ ![EmbeddedJar ビルド アクションを選択します。](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png)

次に、プロジェクトを構成するプロパティを開き、*ターゲット フレームワーク*です。 場合、します。JAR、Android Api を使用して、API レベルにターゲット フレームワークを設定します。JAR が必要です。 通常、開発者のします。どの API レベル (レベル) は、JAR ファイルをします。JAR と互換性が。 (ターゲット フレームワークの設定と全般の Android API レベルの詳細については、次を参照してください[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)。)。

バインド ライブラリのターゲット API レベルの設定 (この例ではを使用している API レベル 19)。 

[ ![ターゲット API レベル 19 の API に設定](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png)


最後に、バインド ライブラリをビルドします。 いくつかの警告メッセージが表示されますが、バインドのライブラリ プロジェクトは正常にビルドされ、出力が生成する必要があります。次の場所に DLL: **JarBinding/bin/Debug/JarBinding.dll**
    

<a name="using" />

### <a name="using-the-bindings-library"></a>バインドのライブラリを使用します。

これを使用します。Xamarin.Android アプリ内の DLL は、次を操作します。

1.  バインドのライブラリへの参照を追加します。

2.  呼び出しを行う、します。マネージ呼び出し可能ラッパーを介して JAR します。 

次の手順でアプリケーションを作成した、最小限の画像を表示しダウンロード バインド ライブラリを使用する、 `ImageView`;「たいへん」内にあるコードは、します。JAR ファイルです。 

最初に、バインドのライブラリを使用する新しい Xamarin.Android アプリを作成します。 ソリューションを右クリックし **新しいプロジェクトの追加**; 新しいプロジェクトの名前**BindingTest**です。 このアプリです。 このチュートリアルを簡略化するために、バインドのライブラリと同じソリューションで作成していますただし、バインド ライブラリを使用するアプリはでした、別のソリューションに代わりに、存在します。 

[ ![新しい BindingTest プロジェクトを追加します。](binding-a-jar-images/07-add-new-project-sml.png)](binding-a-jar-images/07-add-new-project.png)

右クリックし、**参照**のノード、 **BindingTest**プロジェクトし、選択**の参照を追加しています.**:

[ ![右の参照を追加します。](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png)

チェック、 **JarBinding**先ほど作成したプロジェクトとクリック**OK**:

[ ![JarBinding プロジェクトを選択します。](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png)

開く、**参照**のノード、 **BindingTest**プロジェクトし、のことを確認、 **JarBinding**参照が存在します。 

[ ![参照 JarBinding が表示されます。](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png)

変更、 **BindingTest**レイアウト (**Main.axml**) を 1 つを持つよう`ImageView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

次の追加`using`ステートメントを**MainActivity.cs** &ndash; Java ベースのメソッドを簡単にアクセスできるようになります`Picasso`バインド ライブラリに存在するクラス。

```csharp
using Com.Squareup.Picasso;
```

変更、`OnCreate`メソッドを使用するように、 `Picasso` URL からイメージを読み込み、表示するクラス、 `ImageView`: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

コンパイルし、実行、 **BindingTest**プロジェクト。 アプリが起動し、(ネットワークの状態) に応じて短い遅延の後に、ダウンロードして次のスクリーン ショットのようなイメージを表示にする必要があります。

[ ![スクリーン ショットの BindingTest 実行](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png)

おめでとうございます!  Java ライブラリを正常に連結されています。JAR し、Xamarin.Android アプリで使用します。
 
<a name="summary" />
 
## <a name="summary"></a>まとめ

このチュートリアルでは、サード パーティのバインドのライブラリを作成しました。JAR ファイル、バインド ライブラリ アプリに追加して、最小限のテストと、c# コードが内に存在する Java コードを呼び出すことを確認するアプリを実行します。JAR ファイルです。 



## <a name="related-links"></a>関連リンク

- [Java バインド ライブラリ (ビデオ) のビルド](https://university.xamarin.com/classes#10090)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
