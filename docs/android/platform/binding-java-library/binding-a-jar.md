---
title: .JAR のバインド
description: このチュートリアルでは、Android .JAR ファイルから Xamarin.Android Java バインド ライブラリを作成する詳しい手順について説明します。
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 2b8641a3b484da643457366605e9a5f33c97c3b5
ms.sourcegitcommit: d1980b2251999224e71c1289e4b4097595b7e261
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2020
ms.locfileid: "92928582"
---
# <a name="binding-a-jar"></a>.JAR のバインド

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタム バインディングの使用を調査しています。 今後の開発作業の発展のために、この [**アンケート**](https://www.surveymonkey.com/r/KKBHNLT)にご回答ください。

_このチュートリアルでは、Android .JAR ファイルから Xamarin.Android Java バインド ライブラリを作成する詳しい手順について説明します。_

## <a name="overview"></a>概要

Android コミュニティでは、アプリで使用できる多くの Java ライブラリが提供されています。 多くの場合、これらの Java ライブラリは .JAR (Java Archive) 形式にパッケージ化されていますが、その機能を Xamarin Android アプリで使用できるように、.JAR を *Java バインド ライブラリ* にパッケージ化できます。 Java バインド ライブラリの目的は、.JAR ファイル内の API を、自動的に生成されたコード ラッパーを使用して C# コードで使用できるようにすることです。

Xamarin ツールでは、1 つ以上の入力 .JAR ファイルからバインド ライブラリを生成できます。 バインド ライブラリ (.DLL アセンブリ) には次のものが含まれます。

- 元の .JAR ファイルの内容。

- マネージド呼び出し可能ラッパー (MCW)。これは、.JAR ファイル内の対応する Java 型をラップする C# 型です。

生成された MCW コードは、JNI (Java Native Interface) を使用して、API 呼び出しを基になる .JAR ファイルに転送します。 当初は Android で使用することを目的としていた任意の JAR ファイルのバインド ライブラリを作成できます (Xamarin ツールは、現在 Android 以外の Java ライブラリのバインドをサポートしていないことに注意してください)。 .JAR ファイルの内容を含めずにバインド ライブラリをビルドして、実行時に DLL が .JAR に依存するように選択することもできます。

このガイドでは、1 つの .JAR ファイルに対するバインド ライブラリを作成するための基本について説明します。 バインドのカスタマイズやデバッグの必要がない、すべてが適切に動作する例を示します。
[メタデータを使用したバインディングの作成](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)では、バインド プロセスが完全に自動化されておらず、一部の手動介入が必要になる、より高度なシナリオ例について説明しています。 一般的な Java ライブラリのバインドの概要 (基本的なコード例を含む) については、「[Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)」を参照してください。

## <a name="walkthrough"></a>チュートリアル

次のチュートリアルでは、イメージの読み込みとキャッシュ機能を提供する一般的な Android .JAR である [Picasso](https://square.github.io/picasso/) のバインド ライブラリを作成します。 次の手順を使用して **picasso-2.x.x.jar** をバインドし、Xamarin.Android プロジェクトで使用できる新しい .NET アセンブリを作成します。

1. 新しい Java バインド ライブラリ プロジェクトを作成します。

2. .JAR ファイルをプロジェクトに追加します。

3. .JAR ファイルに適切なビルド アクションを設定します。

4. .JAR でサポートされるターゲット フレームワークを選択します。

5. バインド ライブラリをビルドします。

バインド ライブラリを作成したら、バインド ライブラリで API を呼び出す機能を示す小さな Android アプリを開発します。
この例では、 **picasso-2.x.x.jar** のメソッドにアクセスします。

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
```

**picasso-2.x.x.jar** のバインド ライブラリを生成した後、これらのメソッドを C# から呼び出すことができます。 次に例を示します。

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("https://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>バインド ライブラリの作成

以下の手順を実行する前に、[picasso-2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar) をダウンロードしてください。

最初に、新しいバインド ライブラリ プロジェクトを作成します。 Visual Studio for Mac または Visual Studio で、新しいソリューションを作成し、" *Android バインド ライブラリ* " テンプレートを選択します。 (このチュートリアルのスクリーンショットでは Visual Studio が使用されていますが、Visual Studio for Mac もよく似ています)。ソリューションに **JarBinding** と名前を付けます。

[![JarBinding ライブラリ プロジェクトの作成](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

テンプレートには、 **Jars** フォルダーが含まれています。ここで、バインド ライブラリ プロジェクトに対して .JAR を追加します。 **Jars** フォルダーを右クリックし、 **[追加] > [既存の項目]** の順に選択します。

[![既存の項目を追加する](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

前にダウンロードした **picasso-2.x.x.jar** に移動し、それを選択して **[追加]** をクリックします。

[![jar ファイルを選択して、[追加] をクリック](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

**picasso-2.x.x.jar** ファイルがプロジェクトに正常に追加されたことを確認します。

[![プロジェクトに追加された Jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Java バインド ライブラリ プロジェクトを作成する場合は、.JAR をバインド ライブラリに埋め込むか、個別にパッケージ化するかを指定する必要があります。 これを行うには、次のいずれかの " *ビルド アクション* " を指定します。

- **EmbeddedJar** &ndash; .JAR はバインド ライブラリに埋め込まれます。

- **InputJar** &ndash; .JAR はバインド ライブラリとは別に保持されます。

通常は、 **EmbeddedJar** ビルド アクションを使用して、.JAR が自動的にバインド ライブラリにパッケージ化されるようにします。 これは、最も簡単なオプションです .JAR 内の Java バイトコードは Dex バイトコードに変換され、APK に (マネージド呼び出し可能ラッパーと共に) 埋め込まれます。 .JAR をバインド ライブラリとは別に保持する場合は、 **InputJar** オプションを使用できます。ただし、アプリを実行するデバイスで確実に .JAR ファイルが使用できるようにする必要があります。

ビルド アクションを **EmbeddedJar** に設定します。

[![EmbeddedJar ビルド アクションの選択](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

次に、プロジェクトのプロパティを開いて、" *ターゲット フレームワーク* " を構成します。 .JAR で Android API が使用される場合は、ターゲット フレームワークを .JAR が期待する API レベルに設定します。 通常、.JAR ファイルの開発者は、.JAR と互換性のある API レベルを示します。
(一般的なターゲット フレームワークの設定および Android API のレベルの詳細については、「[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)」を参照してください。)

バインド ライブラリのターゲット API レベルを設定します (この例では API レベル 19 を使用しています)。

[![API 19 に設定されたターゲット API レベル](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

最後に、バインド ライブラリをビルドします。 いくつかの警告メッセージが表示される場合がありますが、バインド ライブラリ プロジェクトが正常にビルドされ、次の場所に出力 .DLL が生成されます。 **JarBinding/bin/Debug/JarBinding.dll**

### <a name="using-the-bindings-library"></a>バインド ライブラリの使用

この .DLL を Xamarin.Android アプリで使用するには、次のようにします。

1. バインド ライブラリへの参照を追加します。

2. マネージド呼び出し可能ラッパーを使用して .JAR への呼び出しを行います。

次の手順では、バインド ライブラリを使用して、`ImageView` のイメージをダウンロードして表示する最小限のアプリを作成します。"複雑な処理" は、.JAR ファイル内のコードによって行われます。

最初に、バインド ライブラリを使用する新しい Xamarin.Android アプリを作成します。 ソリューションを右クリックして **[新しいプロジェクトの追加]** を選択し、新しいプロジェクトに「 **BindingTest** 」という名前を付けます。 このチュートリアルを簡略化するために、このアプリをバインド ライブラリと同じソリューションに作成しています。ただし、バインド ライブラリを使用するアプリは、別のソリューションに存在させることもできます。

[![新しい BindingTest プロジェクトを追加する](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

**BindingTest** プロジェクトの **[参照]** ノードを右クリックし、 **[参照の追加...]** を選択します。

[![参照の追加](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

先ほど作成した **JarBinding** プロジェクトをチェックし、 **[OK]** をクリックします。

[![JarBinding プロジェクトの選択](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

**BindingTest** プロジェクトの **[参照]** ノードを開き、 **JarBinding** 参照が存在することを確認します。

[![[参照] の下に JarBinding が表示される](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

`ImageView` が 1 つになるように、 **BindingTest** のレイアウト ( **Main.axml** ) を変更します。

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

次の `using` ステートメントを **MainActivity.cs** に追加します。これにより、バインド ライブラリに存在する Java ベースの `Picasso` クラスのメソッドに簡単にアクセスできるようになります。

```csharp
using Com.Squareup.Picasso;
```

`OnCreate` メソッドを変更し、`Picasso` クラスを使用して URL からイメージを読み込み、`ImageView` に表示するようにします。

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
            .Load ("https://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

**BindingTest** プロジェクトをコンパイルして実行します。 アプリが起動し、短い遅延 (ネットワークの状態によって異なります) の後に、次のスクリーンショットのようなイメージがダウンロードされて表示されます。

[![BindingTest が実行されているスクリーンショット](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

おめでとうございます! Java ライブラリ .JAR を正しくバインドし、Xamarin.Android アプリで使用しました。

## <a name="summary"></a>まとめ

このチュートリアルでは、サードパーティー .JAR ファイルのバインド ライブラリを作成して、最小限のテスト アプリにバインド ライブラリを追加し、アプリを実行して C# コードが .JAR ファイル内にある Java コードを呼び出せることを確認しました。

## <a name="related-links"></a>関連リンク

- [Java バインド ライブラリのビルド (ビデオ)](https://university.xamarin.com/classes#10090)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
