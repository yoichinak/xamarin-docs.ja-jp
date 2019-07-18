---
title: Xamarin.iOS でのイメージの表示
description: この記事では、Xamarin.iOS アプリと c# コードを使用するか、iOS デザイナーのコントロールに割り当てることで、そのイメージを表示する内のイメージ アセットなどについて説明します。
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/24/2018
ms.openlocfilehash: 6d584c45288d7e7341a4423c32a6966c6c7f21f3
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827905"
---
# <a name="displaying-an-image-in-xamarinios"></a>Xamarin.iOS でのイメージの表示

_この記事では、Xamarin.iOS アプリと c# コードを使用するか、iOS デザイナーのコントロールに割り当てることで、そのイメージを表示する内のイメージ アセットなどについて説明します。_

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>追加して、Xamarin.iOS アプリで画像を整理します。

開発者が使用して Xamarin.iOS アプリで使用するイメージを追加するときに、_資産カタログ_すべての iOS デバイスとアプリで必要な解像度をサポートするためにします。

IOS 7 で追加された**資産カタログの画像セット**すべてのバージョンまたはさまざまなデバイスをサポートし、スケール ファクターをアプリに必要なイメージの表現が含まれています。 イメージの資産ファイル名ではなく**画像セット**Json ファイルを使用して、どのデバイスと解像度にどのイメージが属しているを指定します。 これは、管理および iOS (iOS 9 以降) からのイメージをサポートすることをお勧めします。

## <a name="adding-images-to-an-asset-catalog-image-set"></a>資産カタログ イメージへのイメージの追加設定

、前述のよう、**資産カタログの画像セット**すべてのバージョンまたはさまざまなデバイスをサポートし、スケール ファクターをアプリに必要なイメージの表現が含まれています。 イメージの資産ファイル名ではなく**画像セット**Json ファイルを使用して、どのデバイスと解像度にどのイメージが属しているを指定します。

新しいイメージ セットを作成し、イメージを追加して、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Assets.xcassets`ファイルを開き、編集します。

    ![](displaying-an-image-images/imageset01.png "ソリューション エクスプ ローラーで Assets.xcassets")
2. 右クリックし、 **Assets リスト**選択**新しいイメージ セット**:

    ![](displaying-an-image-images/imageset02.png "新しいイメージ セットの追加")
3. 新しいイメージ セットを選択し、エディターが表示されます。

    ![](displaying-an-image-images/imageset03.png "セットのイメージ エディター")
4. ここでは、さまざまなデバイスと必要な解像度の各イメージにドラッグします。 
5. 新しいイメージ セットをダブルクリックして**名前**で、 **Assets リスト**編集します。![](displaying-an-image-images/imageset04.png "新しいイメージ セットの名前を編集")

使用する場合、**セット イメージ**で iOS Designer では、プロパティ エディターでのドロップダウン リストから単純にセットの名前を選択します。

![](displaying-an-image-images/imageset06.png "ドロップダウン リストからイメージ セットの名前を選択します。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 資産カタログを開く、**ソリューション エクスプ ローラー**、左上隅でをクリックし、 **Plus**ボタン。

    ![](displaying-an-image-images/asset5.png "プラス記号をクリックします ボタン")

2. 選択**イメージ セットの追加**と新しいイメージ セットのセットのイメージ エディターが表示されます。 ここでは、さまざまなデバイスと必要な解像度の各イメージにドラッグします。 

    ![](displaying-an-image-images/asset7.png "イメージ エディターの設定")

### <a name="renaming-an-image-set"></a>イメージ設定の名前を変更します。

イメージのセットを名前を変更するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ダブルクリックして、**資産カタログ**ファイルを開き、編集します。

    ![](displaying-an-image-images/rename01.png "ソリューション エクスプ ローラーで、資産カタログ")
2. 選択、**セット イメージ**名前を変更します。

    ![](displaying-an-image-images/rename02.png "名前を変更するイメージのセットを選択します")
3. **プロパティ エクスプ ローラー**、一番下までスクロールし、選択、**名前**(下、**その他**セクション)。

    ![](displaying-an-image-images/rename03.png "その他 セクションで名前を選択します。")
4. 新しい入力**名前**の**セット イメージ**変更を保存します。

-----

使用する場合、**セット イメージ**コードでは、名前で参照を呼び出して、`FromBundle`のメソッド、`UIImage`クラス。 たとえば、次のように入力します。

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> 場合は、イメージのセットに割り当てられているイメージが正しく表示されない、正しいファイル名で使用されていることを確認、`FromBundle`メソッド (、**セット イメージ**であり、親**資産カタログ**名前)。 PNG イメージの場合、`.png`拡張機能は省略できます。 他のイメージ形式の拡張機能が必要です (例。 `PurpleMonkey.jpg`)

### <a name="using-vector-images-in-asset-catalogs"></a>資産カタログでベクター イメージの使用

IOS 8、特別な時点で**ベクター**クラスに追加された**画像セット**、開発者ができる、 **PDF**形式の代わりに含むカセットでベクター イメージさまざまな解像度で個々 のビットマップ ファイル。 1 つのベクトル ファイルを指定して、このメソッドを使用して、 `@1x` (ベクターの PDF ファイル形式) の解決と`@2x`と`@3x`ファイルのバージョンがコンパイル時に生成され、アプリケーションのバンドルに含まれます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](displaying-an-image-images/imageset05.png "資産カタログ エディターでベクター イメージ")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/asset8.png "資産カタログ エディターでベクター イメージ")

-----

たとえば、開発者が含まれている場合、 `MonkeyIcon.pdf` 150px x 150px、資産は、コンパイルされたときに、最終的なアプリ バンドルに含めると、次のビットマップの解像度のアセット カタログのベクターとしてファイル。

- `MonkeyIcon@1x.png` -150px x 150px 解決します。
- `MonkeyIcon@2x.png` -300 ピクセル x 300px 解決します。
- `MonkeyIcon@3x.png` -450px x 450px 解決します。

次を考慮する資産カタログを PDF ベクター イメージを使用する場合。

- PDF がコンパイル時と、最終的なアプリケーションに付属してビットマップ ビットマップにラスタライズになるベクトルの完全なサポートはありません。
- アセット カタログに設定されていると、イメージのサイズを調整できません。 開発者 (またはコードに自動レイアウトとサイズ クラスを使用して) イメージのサイズを変更しようとすると、イメージは他の任意のビットマップと同じようにゆがんで表示されます。
- 資産カタログ、iOS 7 と互換性があり、大きい場合にのみ、アプリの iOS 6 をサポートするために必要または下に、資産カタログは使用できません。

## <a name="working-with-template-images"></a>テンプレート イメージの操作

IOS アプリの設計に基づきがあります、開発者がアイコンまたはイメージ内の (たとえば、ユーザー設定に基づく) の配色の変更に合わせてユーザー インターフェイスをカスタマイズする必要がある場合。

この効果を簡単に実現するには、スイッチ、_レンダリング モード_イメージ資産の**テンプレート イメージ**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](displaying-an-image-images/templateimage01.png "テンプレート イメージに設定されている表示モード")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage01vs.png "表示モード テンプレートに設定")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS Designer では、UI コントロールにイメージの資産を割り当てるし、設定、**濃淡**イメージを色分けして表示します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](displaying-an-image-images/templateimage03.png "イメージの色分けに濃淡を設定します。")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage03vs.png "イメージの色分けに濃淡を設定します。")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

必要に応じて、イメージの資産や濃淡は、コード内で直接設定できます。

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

コードから完全には、テンプレート イメージを使用するには、次の操作を行います。

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

`RenderMode`のプロパティを`UIImage`は読み取り専用を使用して、`ImageWithRenderingMode`目的のレンダリング モード設定で、イメージの新しいインスタンスを作成するメソッド。

3 つの可能性がある設定`UIImage.RenderMode`を使用して、`UIImageRenderingMode`列挙型。

* `AlwaysOriginal` -何も変更せず、元のソース イメージ ファイルとして表示される画像を強制します。
* `AlwaysTemplate` -テンプレート イメージとして、指定したピクセルを色分けして表示するイメージの強制`Tint`色。
* `Automatic` の使用されている環境に基づいて、テンプレート、または元か、イメージを表示します。 イメージを使用する場合など、 `UIToolBar`、 `UINavigationBar`、`UITabBar`または`UISegmentControl`をテンプレートとして扱われます。

## <a name="adding-new-assets-collections"></a>新しい資産のコレクションを追加します。

新しいコレクションはすべてのアプリのイメージを追加する代わりに、必要になりますと時間がある可能性があります資産カタログ内のイメージを使用する場合、`Assets.xcassets`コレクション。 たとえば、次のように、オンデマンド リソースを設計するときです。

プロジェクトに新しいアセット カタログを追加するには

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 右クリックし、**プロジェクト名**で、**ソリューション エクスプ ローラー**選択**追加** > **新しいファイル.**
2. 選択**iOS** > **資産カタログ**、入力、**名前**コレクションをクリックして、**新規**ボタン。

    ![](displaying-an-image-images/asset01.png "新しいアセット カタログを作成します。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. ソリューション エクスプ ローラーで右クリック**資産カタログ**フォルダー、および選択**追加 > 新しいアセット カタログ**します。
2. 名前を付け、**追加**:

    ![](displaying-an-image-images/asset1.png "新しいアセット カタログを作成します。")

-----

ここでは、コレクションで使用できます、既定値と同じ方法で`Assets.xcassets`プロジェクトで自動的に含まれているコレクション。

## <a name="using-images-with-controls"></a>コントロールでイメージを使用します。

IOS はイメージを使用してアプリをサポートするために、だけでなく、タブ バー、ツールバー、ナビゲーション バー、テーブル、およびボタンなどのコントロール型のアプリでイメージを使用します。 コントロールに表示されるイメージを作成する簡単な方法を割り当てるには、`UIImage`をコントロールのインスタンス`Image`プロパティ。

### <a name="frombundle"></a>FromBundle

`FromBundle`メソッドの呼び出しは、多くの組み込みサポートとさまざまな解像度のイメージ ファイルの自動処理のキャッシュなど、イメージの読み込みと管理機能を持つ同期 (ブロックする) 呼び出しをします。  

次の例のイメージを設定する方法を示しています、`UITabBarItem`上、 `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

仮定`MyImage`上のアセット カタログに追加のイメージ アセットの名前を指定します。 資産カタログのイメージを使用する場合、イメージ セットの名前を指定だけ、`FromBundle`メソッド**PNG**イメージの形式します。

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

その他のイメージ形式用の拡張機能は名前が含まれます。 例えば:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

アイコンとイメージの詳細については、Apple のドキュメントを参照してください。[カスタム アイコンとイメージの作成ガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)します。

## <a name="displaying-an-image-in-a-storyboards"></a>ストーリー ボードにイメージを表示

簡単にすることができます、イメージは、資産カタログを使用して Xamarin.iOS プロジェクトに追加されると、ストーリー ボードを使用して、表示される、 `UIImageView` iOS Designer でします。 たとえば、次のイメージ アセットが追加されている場合。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](displaying-an-image-images/display01.png "サンプル イメージ アセットが追加されました")

ストーリー ボードに表示するには、次の操作を行います。

1. ダブルクリックして、`Main.storyboard`ファイル、**ソリューション エクスプ ローラー** iOS Designer での編集用に開きます。
2. 選択、**イメージ ビュー**から、**ツールボックス**:

     ![](displaying-an-image-images/display02.png "ツールボックスからイメージのビューを選択します。")
3. デザイン画面と位置にイメージのビューをドラッグし、必要に応じてサイズします。

    ![](displaying-an-image-images/display03.png "デザイン画面で新しいイメージ ビュー")
4. **ウィジェット**のセクション、**プロパティ エクスプ ローラー**目的**イメージ**資産を表示します。

    ![](displaying-an-image-images/display04.png "表示するのには、目的のイメージ アセットを選択します。")
5. **ビュー**セクションで、使用、**モード**イメージがある方法を制御するときにサイズ変更、 **Image View**のサイズを変更します。
6. **Image View**を追加するには、もう一度クリックして選択すると、**制約**:

    ![](displaying-an-image-images/display05.png "制約の追加")
7. 各辺の"T"の形をしたハンドルをドラッグ、 **Image View**両側に画像を「ピン留め」画面の対応する側にします。 この方法で、 **Image View**画面のサイズを拡大および縮小されます。
8. ストーリー ボードに変更を保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/display01vs.png "サンプル イメージ アセットが追加されました")

ストーリー ボードに表示するには、次の操作を行います。

1. ダブルクリックして、`Main.storyboard`ファイル、**ソリューション エクスプ ローラー** iOS Designer での編集用に開きます。
2. 選択、**イメージ ビュー**から、**ツールボックス**:

     ![](displaying-an-image-images/display02vs.png "ツールボックスからイメージのビューを選択します。")
3. デザイン画面と位置にイメージのビューをドラッグし、必要に応じてサイズします。

    ![](displaying-an-image-images/display03vs.png "デザイン画面で新しいイメージ ビュー")
4. **ウィジェット**のセクション、**プロパティ エクスプ ローラー**目的**イメージ**資産を表示します。

    ![](displaying-an-image-images/display04vs.png "表示するのには、目的のイメージ アセットを選択します。")
5. **ビュー**セクションで、使用、**モード**イメージがある方法を制御するときにサイズ変更、 **Image View**のサイズを変更します。
6. **Image View**を追加するには、もう一度クリックして選択すると、**制約**:

    ![](displaying-an-image-images/display05vs.png "制約の追加")
7. 各辺の"T"の形をしたハンドルをドラッグ、 **Image View**両側に画像を「ピン留め」画面の対応する側にします。 この方法で、 **Image View**画面のサイズを拡大および縮小されます。
8. ストーリー ボードに変更を保存します。

-----

## <a name="displaying-an-image-in-code"></a>コードでイメージを表示します。

画像をストーリー ボードを表示と同じようにイメージが、資産カタログを使用して Xamarin.iOS プロジェクトに追加されると簡単に表示できる c# コードを使用しています。

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

このコードを作成する新しい`UIImageView`を初期サイズと位置を付けます。 次に、プロジェクトに追加のイメージ アセットからイメージを読み込み、追加します、`UIImageView`親`UIView`表示します。

## <a name="related-links"></a>関連リンク

- [画像 (サンプル) の操作](https://developer.xamarin.com/samples/monotouch/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [イメージのサイズと解像度 (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
