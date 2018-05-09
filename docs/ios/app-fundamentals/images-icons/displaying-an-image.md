---
title: イメージを表示します。
description: この記事は、Xamarin.iOS アプリと c# コードを使用して、または iOS デザイナー内のコントロールに代入することで、そのイメージを表示するイメージ アセットなどについて説明します。
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: f1f733fa91be7bf76e19896e78809d18494891d3
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="displaying-an-image"></a>イメージを表示します。

_この記事は、Xamarin.iOS アプリと c# コードを使用して、または iOS デザイナー内のコントロールに代入することで、そのイメージを表示するイメージ アセットなどについて説明します。_

ここでは、次のトピックについて詳しく説明します。

- [Xamarin.iOS アプリでの整理のイメージの追加および](#adding-assets)- のイメージ アセットをについて説明し、含まれる可能性がある、どのように編成され、Xamarin.iOS プロジェクト内で管理します。
- [アセット カタログにイメージを追加する](#asset-catalogs)-資産カタログを含むイメージを管理します。
    - [資産のカタログにベクター イメージを使用して](#Using-Vector-Images-in-Asset-Catalogs)-すべてのイメージのサイズを 1 つのベクトルを提供します。
- [テンプレート イメージを操作](#Working-with-Template-Images)-テンプレート イメージのイメージ アセットを設定することにより、開発者できます簡単に色分けして表示、イメージを設定して、アプリの UI のテーマの変更に合わせてそれを`Tint`プロパティです。
- [コントロールでイメージを使用する](#controls)- などの UI コントロールを持つ Xamarin.iOS プロジェクトに含まれるイメージ アセットを使用して表紙`UIButton`と`UIImageView`c# を使用してイメージを操作する方法と、`UIImage`オブジェクトの[FromBundle](#frombundle)メソッドです。
- [ストーリー ボードにイメージを表示する](#Displaying-an-Image-in-a-Storyboards)-ストーリー ボードを使用してイメージを表示する例を示します。
- [コードでイメージを表示する](#Displaying-an-Image-in-Code)の c# コードを使用してイメージを表示する例を示します。

<a name="adding-assets" />

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>追加して、Xamarin.iOS アプリで画像の整理

Xamarin.iOS アプリで使用するイメージを追加、ときに、開発者が使用する_アセット カタログ_すべての iOS デバイスとアプリが必要な解像度をサポートするためにします。

IOS 7 で追加された**アセット カタログのイメージ セット**すべてのバージョンまたはさまざまなデバイスをサポートし、規模のアプリの要素に必要なイメージの表現が含まれています。 イメージの資産ファイル名ではなく (を参照してください[解像度の独立したイメージとイメージの命名規則](~/ios/app-fundamentals/images-icons/displaying-an-image.md))、**イメージ セット**Json ファイルを使用して、どのデバイスや解像度に属しているどのイメージを指定するには. これは、管理および iOS (iOS 9 以上) からのイメージをサポートすることをお勧めします。

<a name="asset-catalogs" />

## <a name="adding-images-to-an-asset-catalog-image-set"></a>資産カタログ イメージに追加するイメージを設定します。

、前述のよう、**アセット カタログのイメージ セット**すべてのバージョンまたはさまざまなデバイスをサポートし、規模のアプリの要素に必要なイメージの表現が含まれています。 イメージの資産ファイル名ではなく**イメージ セット**Json ファイルを使用して、どのデバイスや解像度に属しているどのイメージを指定します。

新しいイメージ セットを作成し、イメージを追加して、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **ソリューション エクスプ ローラー**をダブルクリックして、`Assets.xcassets`ファイルを開いて編集するファイル。

    ![](displaying-an-image-images/imageset01.png "ソリューション エクスプ ローラーで Assets.xcassets")
2. 右クリックし、**資産一覧**選択**イメージは新しい設定**:

    ![](displaying-an-image-images/imageset02.png "新しいイメージ セットを追加します。")
3. 新しいイメージ セットを選択し、エディターが表示されます。

    ![](displaying-an-image-images/imageset03.png "イメージの設定エディター")
4. ここでは、さまざまなデバイスの各イメージでドラッグおよびと解決策が必要です。 (注: これらの解決策がで指定した解像度に対応する、[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)ドキュメントです)。
5. ダブルクリックして、新しいイメージ セットの**名前**で、**資産一覧**編集: ![ ](displaying-an-image-images/imageset04.png "新しいイメージ セットの名前を編集")

使用する場合、**イメージ設定**デザイナー、iOS での単にプロパティ エディターでのドロップダウン リストから、セットの名前を選択します。

![](displaying-an-image-images/imageset06.png "ドロップダウン リストから、イメージ セットの名前を選択します。")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 資産カタログを開く、**ソリューション エクスプ ローラー**、左上隅のをクリックし、 **Plus**ボタン。

    ![](displaying-an-image-images/asset5.png "プラス記号をクリックしてボタン")

2. 選択**イメージ セットを追加**され、新しいイメージ セットのイメージの設定エディターが表示されます。 ここでは、さまざまなデバイスの各イメージでドラッグおよびと解決策が必要です。 (注: これらの解決策がで指定した解像度に対応する、[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)ドキュメント)。

    ![](displaying-an-image-images/asset7.png "イメージ エディターの設定")

### <a name="renaming-an-image-set"></a>イメージ セットの名前を変更します。

イメージの設定、名前を変更するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**をダブルクリックして、**アセット カタログ**ファイルを開いて編集するファイル。

    ![](displaying-an-image-images/rename01.png "ソリューション エクスプ ローラーで、資産カタログ")
2. 選択、**イメージ設定**名前を変更します。

    ![](displaying-an-image-images/rename02.png "名前を変更するイメージのセットを選択します。")
3. **プロパティ エクスプ ローラー**下部までスクロールし、選択、**名前**(下にある、**その他**セクション)。

    ![](displaying-an-image-images/rename03.png "[その他] セクションの名前を選択します。")
4. 新しい入力**名前**の**イメージ設定**し、変更を保存します。

-----

使用する場合、**イメージ設定**コードでは、名前で参照を呼び出して、`FromBundle`のメソッド、`UIImage`クラスです。 たとえば、次のように入力します。

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> イメージ セットに割り当てられているイメージが表示されない場合正常に正しいファイル名を使用していることを確認してください、`FromBundle`メソッド (、**イメージ設定**であり、親**アセット カタログ**名)。 PNG イメージは、`.png`拡張機能は省略できます。 他のイメージ形式を拡張機能が必要 (例です。 `PurpleMonkey.jpg`)

<a name="Using-Vector-Images-in-Asset-Catalogs" />

### <a name="using-vector-images-in-asset-catalogs"></a>資産のカタログでベクター イメージの使用

IOS 8、特別な時点で**ベクター**クラスに追加された**イメージ セット**、開発者に含めることができます、 **PDF**形式の代わりに含むカセットでベクター イメージ別の解像度で個々 のビットマップ ファイルです。 1 つのベクトル ファイルを指定してこのメソッドを使用して、 `@1x` (ベクター PDF ファイル形式) の解決と`@2x`と`@3x`のバージョンのファイルがコンパイル時に生成され、アプリケーションのバンドルに含まれます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "アセット カタログのエディターでベクター イメージ")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "アセット カタログのエディターでベクター イメージ")

-----

たとえば、開発者が含まれている場合、 `MonkeyIcon.pdf` 150px x 150px、コンパイルされたときに、最終的なアプリのバンドルに含まれます資産次ビットマップの解像度を持つアセット カタログのベクトルとしてファイル。

- `MonkeyIcon@1x.png` -150px x 150px 解決します。
- `MonkeyIcon@2x.png` -300 ピクセル x 300px 解決します。
- `MonkeyIcon@3x.png` -450px x 450px 解決します。

次を考慮する資産カタログで PDF ベクター イメージを使用する場合。

- PDF がコンパイル時と最終的なアプリケーションに付属してビットマップのビットマップにラスタライズになる完全ベクターのサポートはありません。
- アセット カタログに設定されると、イメージのサイズを調整できません。 開発者 (またはのいずれかのコードに自動レイアウトおよびサイズのクラスを使用して) イメージのサイズを変更しようとすると、イメージは他の任意のビットマップと同じようにゆがんで表示されます。
- 資産カタログは、のみ iOS 7 との互換性およびそれ以降、iOS 6 をサポートするために、アプリが必要か低いほど、資産カタログを使用できません。

<a name="Working-with-Template-Images" />

## <a name="working-with-template-images"></a>テンプレート イメージの操作

IOS アプリの設計に基づき、あります、開発者がアイコンまたは画面の配色 (たとえば、ユーザー設定に基づいた) の変更を一致するように、ユーザー インターフェイスの内部でイメージをカスタマイズする必要がある場合。

この効果を簡単に実現するには、スイッチ、_レンダリング モード_イメージ資産の**テンプレート イメージ**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "テンプレート イメージに設定されている表示モード")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "テンプレートに設定されている表示モード")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS デザイナーでは、UI コントロールにイメージ アセットを割り当てるし、設定、**濃淡**イメージの色分けに。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "イメージの色分けに濃淡を設定します。")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "イメージの色分けに濃淡を設定します。")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

必要に応じて、イメージの資産および濃淡は、コード内で直接設定できます。

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

コードから完全には、テンプレート イメージを使用するのには、次の操作を行います。

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

`RenderMode`のプロパティ、`UIImage`が読み取り専用を使用して、`ImageWithRenderingMode`メソッドを適切な表示モード設定とイメージの新しいインスタンスを作成します。

3 つの可能性のある設定`UIImage.RenderMode`を介して、`UIImageRenderingMode`列挙型。

* `AlwaysOriginal` -強制的に変更を行わず、元のソース イメージ ファイルとして描画する画像。
* `AlwaysTemplate` -強制的に指定されたピクセルを色分けして、テンプレート イメージとして描画する画像`Tint`色。
* `Automatic` の使用されている環境に基づいて、テンプレート、または元のイメージをレンダリングいずれかとします。 イメージを使用する場合など、 `UIToolBar`、 `UINavigationBar`、`UITabBar`または`UISegmentControl`をテンプレートとして扱われます。

<a name="Adding-new-Assets-Collections" />

## <a name="adding-new-assets-collections"></a>新しい資産のコレクションを追加します。

新しいコレクションことがありますすべてのアプリのイメージを追加する代わりに、必要がある可能性がありますアセット カタログ内のイメージを使用する場合、`Assets.xcassets`コレクション。 たとえば、オンデマンドでリソースを設計するときです。

新しい資産カタログをプロジェクトに追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 右クリックし、**プロジェクト名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**
2. 選択**iOS** > **アセット カタログ**、入力、**名前**をクリックして、コレクションの**新規**ボタン。

    ![](displaying-an-image-images/asset01.png "新しい資産カタログを作成します。")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 右クリックし、ソリューション エクスプ ローラーで、**資産カタログ**フォルダー、および選択**追加 > 新しい資産カタログ**です。
2. 名前を指定し、をクリックして**追加**:

    ![](displaying-an-image-images/asset1.png "新しい資産カタログを作成します。")

-----


ここでは、コレクションで使用できます、既定値と同じ方法で`Assets.xcassets`コレクション、プロジェクトに自動的に追加します。

<a name="controls" />

## <a name="using-images-with-controls"></a>コントロールでイメージを使用します。

イメージを使用してアプリをサポートするために、に加えて iOS はタブ バー、ツールバー、ナビゲーション バー、テーブル、およびボタンなどのアプリのコントロール型にもデータのイメージを使用します。 コントロールに表示されるイメージを作成する簡単な方法を割り当てるには、`UIImage`をコントロールのインスタンス`Image`プロパティです。

<a name="frombundle" />

### <a name="frombundle"></a>FromBundle

`FromBundle`メソッドの呼び出しは同期 (ブロック) をいくつかの組み込みサポートとさまざまな解像度用のイメージ ファイルの自動処理をキャッシュなど、イメージの読み込みと管理機能を持ちます。  

次の例は、のイメージを設定する方法を示します、`UITabBarItem`上、 `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

想定される`MyImage`上記アセット カタログに追加したイメージ資産の名前を指定します。 アセット カタログのイメージを使用する場合を指定するだけでは、イメージの設定の名前、`FromBundle`メソッドを**PNG**画像の書式を設定します。

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

その他のイメージ形式の名前を持つ拡張機能が含まれます。 例えば:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

アイコンとイメージの詳細については、Apple のドキュメントを参照して[カスタム アイコンをクリックしてイメージ作成のガイドライン](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)です。

<a name="Displaying-an-Image-in-a-Storyboards" />

## <a name="displaying-an-image-in-a-storyboards"></a>ストーリー ボードにイメージを表示します。

簡単にすることができます資産カタログを使用して、Xamarin.iOS プロジェクトにイメージを追加するを使用して、ストーリー ボードに表示される、 `UIImageView` ios デザイナー。 たとえば、次のイメージ アセットが追加されている場合。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "サンプル イメージ アセットが追加されました")

ストーリー ボード上に表示するには、次の操作を行います。

1. ダブルクリックして、`Main.storyboard`ファイルで、**ソリューション エクスプ ローラー** iOS デザイナーで編集用に開きます。
2. 選択、**ビューのイメージ**から、**ツールボックス**:

     ![](displaying-an-image-images/display02.png "ツールボックスからイメージのビューを選択します。")
3. 位置とデザイン画面にイメージのビューをドラッグし、必要に応じてサイズします。

    ![](displaying-an-image-images/display03.png "デザイン サーフェイスに新しいイメージのビュー")
4. **ウィジェット**のセクションで、**プロパティ エクスプ ローラー**目的**イメージ**資産を表示します。

    ![](displaying-an-image-images/display04.png "表示する目的のイメージ アセットを選択します。")
5. **ビュー**セクションで、使用して、**モード**イメージがある方法を制御するときにサイズ変更、**イメージ ビュー**のサイズを変更します。
6. **イメージ ビュー**選択すると、クリックしてもう一度追加**制約**:

    ![](displaying-an-image-images/display05.png "制約の追加")
7. 各辺の"T"の形をしたハンドルをドラッグ、**イメージ ビュー** 「ピン留め」両側へ画像を画面の対応する端にします。 この方法で、**イメージ ビュー**画面のサイズを拡大および縮小されます。
8. ストーリー ボードに、変更を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "サンプル イメージ アセットが追加されました")

ストーリー ボード上に表示するには、次の操作を行います。

1. ダブルクリックして、`Main.storyboard`ファイルで、**ソリューション エクスプ ローラー** iOS デザイナーで編集用に開きます。
2. 選択、**ビューのイメージ**から、**ツールボックス**:

     ![](displaying-an-image-images/display02vs.png "ツールボックスからイメージのビューを選択します。")
3. 位置とデザイン画面にイメージのビューをドラッグし、必要に応じてサイズします。

    ![](displaying-an-image-images/display03vs.png "デザイン サーフェイスに新しいイメージのビュー")
4. **ウィジェット**のセクションで、**プロパティ エクスプ ローラー**目的**イメージ**資産を表示します。

    ![](displaying-an-image-images/display04vs.png "表示する目的のイメージ アセットを選択します。")
5. **ビュー**セクションで、使用して、**モード**イメージがある方法を制御するときにサイズ変更、**イメージ ビュー**のサイズを変更します。
6. **イメージ ビュー**選択すると、クリックしてもう一度追加**制約**:

    ![](displaying-an-image-images/display05vs.png "制約の追加")
7. 各辺の"T"の形をしたハンドルをドラッグ、**イメージ ビュー** 「ピン留め」両側へ画像を画面の対応する端にします。 この方法で、**イメージ ビュー**画面のサイズを拡大および縮小されます。
8. ストーリー ボードに、変更を保存します。

-----


<a name="Displaying-an-Image-in-Code" />

## <a name="displaying-an-image-in-code"></a>コードでイメージを表示します。

ストーリー ボードにイメージを表示すると同じようにイメージが、資産カタログを使用して、Xamarin.iOS プロジェクトに追加されると簡単に表示できる c# コードを使用します。

次の例を参照してください。

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

このコードでは、新しい`UIImageView`し、初期サイズと位置が与えられます。 プロジェクトに追加のイメージ アセットからイメージを読み込みますそれを追加であり、`UIImageView`親に`UIView`を表示します。

## <a name="related-links"></a>関連リンク

- [イメージ (サンプル) の操作](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタム アイコンをクリックしてイメージ作成のガイドライン](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
