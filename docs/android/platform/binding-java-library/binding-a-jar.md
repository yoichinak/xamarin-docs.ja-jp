---
title: .JAR のバインド
description: このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成する手順が説明します。JAR ファイル。
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 3c84b29807fd4a181ed867095645005bf9ba4df0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957334"
---
# <a name="binding-a-jar"></a>.JAR のバインド

_このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成する手順が説明します。JAR ファイル。_

## <a name="overview"></a>概要

Android のコミュニティには、アプリで使用することがあります多くの Java ライブラリが用意されています。 これらの Java ライブラリで多くの場合、パッケージ化されます。JAR (Java アーカイブ) 形式では、パッケージ化します。JAR を*Java バインド ライブラリ*の機能が Xamarin.Android アプリで使用できるようにします。 Java バインド ライブラリの目的は、Api を作成する、します。JAR ファイルを使用可能なC#を通じて自動的に生成されたコード ラッパー コード。

Xamarin ツールは、1 つまたは複数の入力からバインド ライブラリを生成できます。JAR ファイル。 バインド ライブラリ (します。DLL アセンブリ) には、次のものが含まれています。 

-   元の内容。JAR ファイル。

-   マネージ呼び出し可能ラッパー (MCW)、あるC#型内の対応する Java 型をラップします。JAR ファイル。

MCW に生成されたコードでは、API の呼び出しを転送する JNI (Java ネイティブ インターフェイス) を使用して、基になります。JAR ファイル。 バインディング ライブラリは、いずれかを作成できます。Android (Xamarin ツールが非 Android Java ライブラリのバインドを現在サポートしていないことに注意してください) で使用する本来の目的を JAR ファイルです。 内容を含めずに、バインド ライブラリを構築することもできます、します。JAR ファイルを DLL に依存しているようにします。実行時に JAR です。

このガイドでは、1 つのバインド ライブラリの作成の基礎を手順説明します。JAR ファイル。 すべてが適切な例について説明します&ndash;は、カスタマイズなしまたはバインドのデバッグが必要な場合。 
[バインドを使用してメタデータを作成する](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)バインド プロセスが完全に自動化し一定の手動による介入が必要です。 より高度なシナリオの例を示します。 Java ライブラリによるバインディングの一般 (基本的なコード例では) の概要については、次を参照してください。 [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)します。 

 
## <a name="walkthrough"></a>チュートリアル

次のチュートリアルでは、バインド ライブラリを作成します[Picasso](http://square.github.io/picasso/)、人気のある Android。イメージの読み込みとキャッシュ機能を提供する JAR です。 次の手順を使用してバインドは**picasso-2.x.x.jar** Xamarin.Android プロジェクトで使用できる新しい .NET アセンブリを作成します。 

1. 新しい Java バインド ライブラリ プロジェクトを作成します。

2. 追加します。JAR ファイルをプロジェクトにします。

3. 適切なビルド アクションを設定します。JAR ファイル。

4. ターゲット フレームワークを選択します。JAR をサポートします。

5. バインド ライブラリをビルドします。

バインド ライブラリを作成したら、バインド ライブラリ API を呼び出すことを示す小規模な Android アプリを開発します。 この例でのメソッドにアクセスする**picasso-2.x.x.jar**:

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

バインド ライブラリを生成後**picasso-2.x.x.jar**からこれらのメソッドを呼び出しC#します。 例えば:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>バインド ライブラリを作成します。

次の手順を開始する前にダウンロード[picasso-2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)します。

最初に、新しいバインド ライブラリ プロジェクトを作成します。 Visual Studio for Mac または Visual Studio で新しいソリューションを作成し、選択、 *Android バインド ライブラリ*テンプレート。 (このチュートリアルのスクリーン ショットは、Visual Studio を使用して、Visual Studio for Mac は非常に似ています)。ソリューションの名前を**JarBinding**: 

[![JarBinding ライブラリ プロジェクトを作成します。](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

テンプレートに含まれる、 **Jars**フォルダーを追加する、します。バインド ライブラリ プロジェクトに JAR(s) します。 右クリックし、 **Jars**フォルダーと選択**追加 > 既存項目の**: 

[![既存の項目を追加する](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

移動し、**picasso-2.x.x.jar**ファイルが以前にダウンロードし、選択し、をクリックして**追加**: 

[![Jar ファイルを選択し、[追加] をクリックしてください](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

いることを確認、**picasso-2.x.x.jar**ファイルがプロジェクトに正常に追加します。 

[![Jar をプロジェクトに追加](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

指定する必要があります Java バインド ライブラリ プロジェクトを作成するときにするかどうか、です。JAR は、バインド ライブラリに埋め込むか、個別にパッケージ化することです。 次のいずれかには、指定した*ビルド アクション*: 

-   **EmbeddedJar** &ndash;します。JAR をバインド ライブラリで埋め込まれます。

-   **InputJar** &ndash;します。バインド ライブラリから個別 JAR が保持されます。

通常、使用して、 **EmbeddedJar**ビルド アクションようにします。JAR はバインド ライブラリに自動的にパッケージ化します。 これは、最も簡単なオプション&ndash;Java バイトコードでします。JAR は、Dex バイトコードに変換され、(管理されている呼び出し可能ラッパー) と共に、APK に埋め込まれます。 保持する場合、します。バインド ライブラリから個別の JAR を使用することができます、 **InputJar**オプション。 ただし、を確認する必要があります、します。JAR ファイルは、アプリを実行しているデバイスで使用できます。 

ビルド アクション設定**EmbeddedJar**: 

[![EmbeddedJar ビルド アクションを選択します。](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

次に、プロジェクトを構成するプロパティを開き、*ターゲット フレームワーク*します。 場合、します。JAR は、Android Api を使用して、API レベルにターゲット フレームワークを設定します。JAR が必要です。 開発者では通常、します。JAR ファイルはどの API レベル (レベル) を示しますが、します。JAR と互換性が。 (ターゲット フレームワークの設定と一般的な Android API レベルの詳細については、次を参照してください[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)。)。

バインド ライブラリのターゲット API レベルの設定 (この例で使用している API レベル 19)。 

[![ターゲット API レベル API 19 に設定](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


最後に、バインド ライブラリをビルドします。 一部の警告メッセージが表示されますが、バインド ライブラリ プロジェクトは正常にビルドされ、出力が生成する必要があります。次の場所に DLL:**JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>バインド ライブラリを使用します。

これを使用します。Xamarin.Android アプリで DLL を以下に示します。

1.  バインド ライブラリへの参照を追加します。

2.  呼び出しを作成します。マネージ呼び出し可能ラッパーを介して JAR です。 

次の手順では、ダウンロードにイメージを表示してバインド ライブラリを使用する最小のアプリを作成します、 `ImageView`;、「複雑」内にあるコードは。JAR ファイル。 

最初に、バインド ライブラリを利用する新しい Xamarin.Android アプリを作成します。 ソリューションを右クリックして**新しいプロジェクトの追加**; 新しいプロジェクトの名前**BindingTest**します。 このチュートリアルでは; を簡略化するためにこのアプリ バインド ライブラリと同じソリューションで作成していますただし、バインド ライブラリを使用するアプリは、別のソリューションで代わりに、存在します。 

[![新しい BindingTest プロジェクトを追加します。](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

右クリックし、**参照**のノード、 **BindingTest**順に選択して**参照の追加.** :

[![右の参照を追加します。](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

チェック、 **JarBinding**前に作成したプロジェクトをクリックします**OK**:

[![JarBinding プロジェクトを選択します。](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

開く、**参照**のノード、 **BindingTest**プロジェクトし、のことを確認、 **JarBinding**参照が存在します。 

[![参照 JarBinding が表示されます。](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

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

次の追加`using`ステートメント**MainActivity.cs** &ndash;に Java ベースのメソッドを簡単にアクセスが可能になります。`Picasso`バインド ライブラリ内にあるクラス。

```csharp
using Com.Squareup.Picasso;
```

変更、`OnCreate`メソッドを使用するように、 `Picasso` URL からイメージを読み込み、表示するにはクラス、 `ImageView`: 

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

コンパイルし、実行、 **BindingTest**プロジェクト。 アプリは起動時に、および (ネットワークの状態) に応じての短い遅延の後に、ダウンロードして次のスクリーン ショットのようなイメージを表示にする必要があります。

[![スクリーン ショットの BindingTest 実行](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

おめでとうございます! Java ライブラリを正常に連結されています。JAR し、Xamarin.Android アプリで使用します。
 
 
## <a name="summary"></a>まとめ

このチュートリアルでは、サード パーティのバインド ライブラリを作成しました。最小限のテストをアプリにバインド ライブラリを追加する JAR ファイル、されことを確認するアプリを実行し、C#コード内に存在する Java コードを呼び出すことができます、します。JAR ファイル。 



## <a name="related-links"></a>関連リンク

- [Java バインド ライブラリ (ビデオ) を構築](https://university.xamarin.com/classes#10090)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
