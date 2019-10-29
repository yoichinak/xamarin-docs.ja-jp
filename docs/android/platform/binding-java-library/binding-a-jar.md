---
title: .JAR のバインド
description: このチュートリアルでは、Android から Xamarin Android Java バインドライブラリを作成する手順について説明します。JAR ファイル。
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 59969abae739db1d9035ec31738c39a3912f47ae
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027768"
---
# <a name="binding-a-jar"></a>.JAR のバインド

_このチュートリアルでは、Android から Xamarin Android Java バインドライブラリを作成する手順について説明します。JAR ファイル。_

## <a name="overview"></a>概要

Android コミュニティでは、アプリで使用できる多くの Java ライブラリが提供されています。 多くの場合、これらの Java ライブラリはにパッケージ化されています。JAR (Java アーカイブ) 形式ですが、をパッケージ化することができます。その機能を Xamarin Android アプリで使用できるように、 *Java バインドライブラリ*に JAR を提供します。 Java バインドライブラリの目的は、で Api を作成することです。JAR ファイルは、 C#自動的に生成されたコードラッパーを使用してコードで使用できます。

Xamarin ツールは、1つ以上の入力からバインドライブラリを生成できます。JAR ファイル。 バインドライブラリ (.DLL アセンブリ) には次のものが含まれます。 

- 元のの内容。JAR ファイル。

- マネージ呼び出し可能ラッパー (MCW)。これC#は、内で対応する Java 型をラップする型です。JAR ファイル。

生成された MCW コードは、JNI (Java Native Interface) を使用して、API 呼び出しを基になるに転送します。JAR ファイル。 任意ののバインドライブラリを作成できます。Android と共に使用することを目的とした JAR ファイル (Xamarin ツールは、現在 Android 以外の Java ライブラリのバインドをサポートしていないことに注意してください)。 の内容を含めずに、バインドライブラリを構築することもできます。DLL がに依存していることを示す JAR ファイル。実行時の JAR。

このガイドでは、1つのにバインドライブラリを作成する方法の基本について説明します。JAR ファイル。 ここでは、すべてが &ndash; 適切に動作する例を示します。この例では、バインドのカスタマイズやデバッグは必要ありません。 
[メタデータを使用してバインディングを作成](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)すると、バインディングプロセスが完全に自動ではなく、一部の手動操作が必要になる、より高度なシナリオの例が提供されます。 一般的な Java ライブラリバインディングの概要 (基本的なコード例を含む) については、「 [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)」を参照してください。 

## <a name="walkthrough"></a>チュートリアル

次のチュートリアルでは、人気のある Android である[ピカソ](https://square.github.io/picasso/)のバインドライブラリを作成します。イメージの読み込みとキャッシュ機能を提供する JAR。 **Picasso-2**をバインドするには、次の手順を使用して、Xamarin. Android プロジェクトで使用できる新しい .net アセンブリを作成します。 

1. 新しい Java バインドライブラリプロジェクトを作成します。

2. を追加します。JAR ファイルをプロジェクトにします。

3. に適切なビルドアクションを設定します。JAR ファイル。

4. をインストールするターゲットフレームワークを選択します。JAR はをサポートします。

5. バインドライブラリをビルドします。

バインドライブラリを作成したら、バインドライブラリで Api を呼び出す機能を示す小さな Android アプリを開発します。 この例では、 **picasso-2**のメソッドにアクセスします。

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

**Picasso-2**のバインドライブラリを生成した後は、からC#これらのメソッドを呼び出すことができます。 (例:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>バインドライブラリの作成

以下の手順を実行する前に、 [picasso-2](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)をダウンロードしてください。

最初に、新しいバインドライブラリプロジェクトを作成します。 Visual Studio for Mac または Visual Studio で、新しいソリューションを作成し、[ *Android バインドライブラリ*] テンプレートを選択します。 (このチュートリアルのスクリーンショットでは Visual Studio を使用しますが、Visual Studio for Mac はよく似ています)。ソリューションに**JarBinding**という名前を指定します。 

[JarBinding ライブラリプロジェクトを作成![には](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

テンプレートには、を追加する**jar**フォルダーが含まれています。バインドライブラリプロジェクトへの JAR。 **[Jar]** フォルダーを右クリックし、 **[既存の項目の追加 >]** を選択します。 

[![既存の項目を追加する](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

先ほどダウンロードした**picasso-2**ファイルに移動し、それを選択して **[追加]** をクリックします。 

[jar ファイルを選択![、[追加] をクリックします。](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

**Picasso-2**ファイルがプロジェクトに正常に追加されたことを確認します。 

[プロジェクトに追加された![Jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Java バインドライブラリプロジェクトを作成する場合は、を指定する必要があります。JAR は、バインドライブラリに埋め込まれるか、個別にパッケージ化されます。 これを行うには、次のいずれかの*ビルドアクション*を指定します。 

- **EmbeddedJar** &ndash; ます。JAR はバインドライブラリに埋め込まれます。

- **Inputjar**は &ndash; ます。JAR はバインドライブラリとは別に保持されます。

通常は、 **EmbeddedJar**ビルドアクションを使用して、をにします。JAR は自動的にバインドライブラリにパッケージ化されます。 これは、の Java バイトコード &ndash; 最も簡単なオプションです。JAR は、Dex バイトコードに変換され、APK に (マネージ呼び出し可能ラッパーと共に) 埋め込まれます。 を保持する場合は。JAR はバインドライブラリとは別に、 **Inputjar**オプションを使用することができます。ただし、であることを確認する必要があります。JAR ファイルは、アプリを実行するデバイスで使用できます。 

ビルドアクションを**EmbeddedJar**に設定します。 

[![EmbeddedJar ビルドアクションの選択](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

次に、プロジェクトのプロパティを開いて、*ターゲットフレームワーク*を構成します。 の場合は。JAR は任意の Android Api を使用し、ターゲットフレームワークをに設定されている API レベルに設定します。JAR が必要です。 通常、の開発者です。JAR ファイルは、の API レベル (またはレベル) を示します。JAR はと互換性があります。 (ターゲットフレームワークの設定と、一般的な Android API レベルの詳細については、「 [ANDROID Api レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください)。

バインドライブラリのターゲット API レベルを設定します (この例では API レベル19を使用しています)。 

[API 19 に設定さ![ターゲット API レベル](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

最後に、バインドライブラリをビルドします。 いくつかの警告メッセージが表示される場合がありますが、バインドライブラリプロジェクトは正常にビルドされ、出力が生成されます。次の場所にある DLL: **JarBinding/bin/Debug/JarBinding**

### <a name="using-the-bindings-library"></a>バインドライブラリの使用

これを使用する場合は。DLL: Xamarin Android アプリで、次の操作を行います。

1. バインドライブラリへの参照を追加します。

2. を呼び出します。マネージド呼び出し可能ラッパーを使用した JAR。 

次の手順では、バインドライブラリを使用して、`ImageView`のイメージをダウンロードして表示する最小限のアプリを作成します。"重い処理" は、に存在するコードによって行われます。JAR ファイル。 

まず、バインドライブラリを使用する新しい Xamarin Android アプリを作成します。 ソリューションを右クリックし、 **[新しいプロジェクトの追加]** を選択します。新しいプロジェクトに**Bindingtest**という名前を指定します。 このチュートリアルを簡略化するために、このアプリをバインドライブラリと同じソリューションに作成しています。ただし、バインドライブラリを使用するアプリケーションは、別のソリューションに存在する可能性があります。 

[新しい BindingTest プロジェクトの追加![](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

**Bindingtest**プロジェクトの **[参照]** ノードを右クリックし、 **[参照の追加]** を選択します。

[![右の参照の追加](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

前の手順で作成した**JarBinding**プロジェクトを確認し、[ **OK]** をクリックします。

[JarBinding プロジェクトの選択![](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

**Bindingtest**プロジェクトの **[参照設定]** ノードを開き、 **JarBinding**参照が存在することを確認します。 

[![JarBinding が [参照] の下に表示される](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

**Bindingtest**レイアウト (メインの**axml**) を次のように変更します。これにより、1つの `ImageView`が含まれるようになります。

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

次の `using` ステートメントを**MainActivity.cs**に追加すると、バインドライブラリに存在する Java ベースの `Picasso` クラスのメソッドに簡単にアクセスできるようになり &ndash; ます。

```csharp
using Com.Squareup.Picasso;
```

`Picasso` クラスを使用して URL からイメージを読み込み、`ImageView`に表示するように `OnCreate` メソッドを変更します。 

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

**Bindingtest**プロジェクトをコンパイルして実行します。 アプリが起動し、短い遅延 (ネットワークの状態によって異なります) が発生すると、次のスクリーンショットのようなイメージがダウンロードされて表示されます。

[BindingTest が実行されている![スクリーンショット](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

おめでとうございます! Java ライブラリが正常にバインドされました。JAR を作成し、Xamarin. Android アプリで使用します。

## <a name="summary"></a>まとめ

このチュートリアルでは、サードパーティのバインドライブラリを作成しました。JAR ファイルは、最小限のテストアプリにバインドライブラリを追加し、アプリを実行して、 C#コードが内に存在する Java コードを呼び出すことができることを確認します。JAR ファイル。 

## <a name="related-links"></a>関連リンク

- [Java バインドライブラリの構築 (ビデオ)](https://university.xamarin.com/classes#10090)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
