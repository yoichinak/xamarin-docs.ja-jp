---
title: Xamarin でのイメージの表示
description: この記事では、Xamarin の iOS アプリにイメージ資産を含め、コードを使用するかC# 、ios デザイナーのコントロールに割り当てることによって、イメージを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/24/2018
ms.openlocfilehash: cda45f01dae2dc17c2517a7f013acacde7906a4b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004479"
---
# <a name="displaying-an-image-in-xamarinios"></a>Xamarin でのイメージの表示

_この記事では、Xamarin の iOS アプリにイメージ資産を含め、コードを使用するかC# 、ios デザイナーのコントロールに割り当てることによって、イメージを表示する方法について説明します。_

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Xamarin iOS アプリでのイメージの追加と整理

Xamarin iOS アプリで使用するイメージを追加すると、開発者は_資産カタログ_を使用して、アプリに必要なすべての iOS デバイスと解像度をサポートします。

IOS 7 で追加された**資産カタログイメージセット**には、アプリのさまざまなデバイスとスケールファクターをサポートするために必要なイメージのすべてのバージョンまたは表現が含まれています。 イメージアセットファイル名を使用するのではなく、**イメージセット**は Json ファイルを使用して、どのイメージがどのデバイスまたはどの解像度に属しているかを指定します。 Ios (iOS 9 以降) でイメージを管理およびサポートする場合は、この方法をお勧めします。

## <a name="adding-images-to-an-asset-catalog-image-set"></a>アセットカタログイメージセットへのイメージの追加

既に説明したように、**資産カタログのイメージセット**には、アプリのさまざまなデバイスやスケールファクターをサポートするために必要なイメージのすべてのバージョンまたは表現が含まれています。 イメージアセットファイル名を使用するのではなく、**イメージセット**は Json ファイルを使用して、どのイメージがどのデバイスまたはどの解像度に属しているかを指定します。

新しいイメージセットを作成してイメージを追加するには、次の手順を実行します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **ソリューションエクスプローラー**で、`Assets.xcassets` ファイルをダブルクリックして編集用に開きます。

    ![](displaying-an-image-images/imageset01.png "The Assets.xcassets in the Solution Explorer")
2. [**アセット] ボックス**を右クリックし、 **[新しいイメージセット]** を選択します。

    ![](displaying-an-image-images/imageset02.png "Adding a New Image Set")
3. 新しいイメージセットを選択すると、エディターが表示されます。

    ![](displaying-an-image-images/imageset03.png "The Image Set editor")
4. ここでは、必要なさまざまなデバイスと解像度ごとにイメージをドラッグします。
5. [**アセット] ボックスの一覧**で、新しいイメージセットの**名前**をダブルクリックして編集します。![](displaying-an-image-images/imageset04.png "新しいイメージセットの名前を編集しています")

IOS デザイナーで**イメージセット**を使用する場合は、プロパティエディターのドロップダウンリストからセットの名前を選択するだけです。

![](displaying-an-image-images/imageset06.png "Select an image set's name from the dropdown list")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**からアセットカタログを開き、左上隅に**ある [+** ] ボタンをクリックします。

    ![](displaying-an-image-images/asset5.png "Click the Plus button")

2. **[イメージセットの追加]** を選択すると、新しいイメージセットのイメージセットエディターが表示されます。 ここでは、必要なさまざまなデバイスと解像度ごとにイメージをドラッグします。

    ![](displaying-an-image-images/asset7.png "The image set editor")

### <a name="renaming-an-image-set"></a>イメージセットの名前変更

イメージセットの名前を変更するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、**アセットカタログ**ファイルをダブルクリックして編集用に開きます。

    ![](displaying-an-image-images/rename01.png "The Asset Catalog in the Solution Explorer")
2. 名前を変更する**イメージセット**を選択します:

    ![](displaying-an-image-images/rename02.png "Select the Image Set to rename")
3. **プロパティエクスプローラー**で一番下までスクロールし、 **[名前]** (その **[他]** セクションの下) を選択します。

    ![](displaying-an-image-images/rename03.png "Select Name under the Misc section")
4. **イメージセット**の新しい**名前**を入力し、変更を保存します。

-----

コードで設定されたイメージを使用する場合は、`UIImage` クラスの `FromBundle` メソッドを呼び出すことによって、名前でその**イメージ**を参照します。 たとえば、次のように入力します。

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> イメージセットに割り当てられたイメージが正しく表示されない場合は、正しいファイル名が `FromBundle` メソッド (親**アセットカタログ**名ではなく、**イメージセット**) で使用されていることを確認します。 PNG 画像の場合、`.png` 拡張機能を省略できます。 その他のイメージ形式の場合は、拡張機能が必要です (例として、 `PurpleMonkey.jpg`)

### <a name="using-vector-images-in-asset-catalogs"></a>アセットカタログでのベクターイメージの使用

IOS 8 の時点では、特殊な**vector**クラスが**イメージセット**に追加されています。これにより、開発者は、さまざまな解像度で個別のビットマップファイルを使用する代わりに、 **PDF**形式のベクター画像をカセットに含めることができます。 このメソッドを使用して、`@1x` の解像度 (ベクター PDF ファイルとして書式設定) 用の単一のベクターファイルを指定します。また、ファイルの `@2x` と `@3x` のバージョンがコンパイル時に生成され、アプリケーションのバンドルに含まれます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](displaying-an-image-images/imageset05.png "Vector Images in the Asset Catalogs editor")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/asset8.png "Vector Images in the Asset Catalogs editor")

-----

たとえば、開発者が、150 px x 150 px の解決策を使用して、資産カタログのベクターとして `MonkeyIcon.pdf` ファイルを含む場合、コンパイル時に次のビットマップ資産が最終的なアプリバンドルに含まれます。

- `MonkeyIcon@1x.png`-150 px x 150 px 解像度。
- `MonkeyIcon@2x.png`-300 x 300 resolution。
- `MonkeyIcon@3x.png`-450px x 450px 解像度。

アセットカタログで PDF ベクターイメージを使用する場合は、次の点を考慮する必要があります。

- これは、コンパイル時に PDF がビットマップにラスタライズされ、最終的なアプリケーションに付属するビットマップになるため、完全なベクターサポートではありません。
- アセットカタログで設定されたイメージのサイズを調整することはできません。 開発者がイメージのサイズを変更しようとした場合 (コードまたは Auto Layout クラスと Size クラスを使用)、イメージは他のビットマップと同様に変形されます。
- 資産カタログは、iOS 7 以上とのみ互換性があります。アプリで iOS 6 以下をサポートする必要がある場合は、アセットカタログを使用できません。

## <a name="working-with-template-images"></a>テンプレートイメージの操作

IOS アプリの設計に基づいて、開発者がユーザーインターフェイス内のアイコンまたはイメージをカスタマイズして、配色の変化 (ユーザー設定に基づくなど) を一致させる必要がある場合があります。

この効果を簡単に実現するには、イメージ資産の_レンダリングモード_を **[テンプレートイメージ]** に切り替えます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](displaying-an-image-images/templateimage01.png "The Render Mode set to Template Image")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage01vs.png "The Render Mode set to Template")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS デザイナーから、イメージ資産を UI コントロールに割り当て、次にイメージを色分けするように**濃淡**を設定します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](displaying-an-image-images/templateimage03.png "Set the Tint to colorize the image")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage03vs.png "Set the Tint to colorize the image")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

必要に応じて、イメージのアセットと濃淡をコードで直接設定できます。

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

コードから完全にテンプレートイメージを使用するには、次の手順を実行します。

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

`UIImage` の `RenderMode` プロパティは読み取り専用なので、`ImageWithRenderingMode` メソッドを使用して、必要なレンダリングモード設定でイメージの新しいインスタンスを作成します。

`UIImageRenderingMode` 列挙型を使用して `UIImage.RenderMode` には、次の3つの設定があります。

- `AlwaysOriginal`-イメージを変更せずに元のソースイメージファイルとして強制的にレンダリングします。
- `AlwaysTemplate`-指定した `Tint` 色のピクセルを色分けすることによって、イメージをテンプレートイメージとして強制的にレンダリングします。
- `Automatic`-イメージをテンプレートとして表示するか、使用する環境に基づいて元にします。 たとえば、イメージが `UIToolBar`で使用されている場合、`UINavigationBar`、`UITabBar` または `UISegmentControl` テンプレートとして扱われます。

## <a name="adding-new-assets-collections"></a>新しいアセットコレクションの追加

Assets カタログ内のイメージを使用する場合、すべてのアプリのイメージを `Assets.xcassets` コレクションに追加するのではなく、新しいコレクションが必要になる場合があります。 たとえば、オンデマンドリソースを設計する場合などです。

新しい Assets カタログをプロジェクトに追加するには、次のようにします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **ソリューションエクスプローラー**で**プロジェクト名**を右クリックし、[ > **新しいファイル**の**追加**] を選択します。
2. [ **IOS** > **資産カタログ**] を選択し、コレクションの**名前**を入力して、 **[新規]** ボタンをクリックします。

    ![](displaying-an-image-images/asset01.png "Creating a new Asset Catalog")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. ソリューションエクスプローラーで、 **[資産カタログ]** フォルダーを右クリックし、 **[新しい資産カタログの追加 >]** を選択します。
2. 名前を付け、 **[追加]** をクリックします。

    ![](displaying-an-image-images/asset1.png "Creating a new Asset Catalog")

-----

ここからは、プロジェクトに自動的に含まれる既定の `Assets.xcassets` コレクションと同じように、コレクションを操作できます。

## <a name="using-images-with-controls"></a>コントロールでのイメージの使用

アプリをサポートするためにイメージを使用するだけでなく、iOS では、タブバー、ツールバー、ナビゲーションバー、テーブル、ボタンなどのアプリのコントロールの種類を含むイメージも使用します。 コントロールにイメージを表示する簡単な方法は、コントロールの `Image` プロパティに `UIImage` インスタンスを割り当てることです。

### <a name="frombundle"></a>FromBundle

`FromBundle` メソッド呼び出しは、さまざまな解像度のイメージファイルのキャッシュサポートや自動処理など、多数のイメージ読み込みおよび管理機能が組み込まれた同期 (ブロッキング) 呼び出しです。

次の例は、`UITabBar`上の `UITabBarItem` のイメージを設定する方法を示しています。

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

`MyImage` が、上のアセットカタログに追加されたイメージ資産の名前であることを前提としています。 アセットカタログイメージを操作するときは、 **PNG**形式の画像の `FromBundle` 方法で設定したイメージの名前を指定するだけです。

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

その他のイメージ形式には、という名前の拡張子を含めます。 (例:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

アイコンとイメージの詳細については、Apple のドキュメントの「[カスタムアイコンとイメージの作成](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)に関するガイドライン」を参照してください。

## <a name="displaying-an-image-in-a-storyboards"></a>ストーリーボードでのイメージの表示

アセットカタログを使用して Xamarin. iOS プロジェクトにイメージを追加すると、iOS Designer の `UIImageView` を使用してストーリーボードに簡単に表示できます。 たとえば、次のイメージアセットが追加されているとします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](displaying-an-image-images/display01.png "A sample Image Asset has been added")

ストーリーボードに表示するには、次の手順を実行します。

1. **ソリューションエクスプローラー**内の `Main.storyboard` ファイルをダブルクリックして、iOS Designer で編集するために開きます。
2. **[ツールボックス]** から**イメージビュー**を選択します。

     ![](displaying-an-image-images/display02.png "Select an Image View from the Toolbox")
3. イメージビューをデザイン画面にドラッグし、必要に応じて位置とサイズを変更します。

    ![](displaying-an-image-images/display03.png "A new Image View on the Design Surface")
4. **プロパティエクスプローラー**の **[ウィジェット]** セクションで、表示する**イメージ**アセットを選択します。

    ![](displaying-an-image-images/display04.png "Select the desired Image asset to be displayed")
5. **[ビュー]** セクションで、**イメージビュー**のサイズが変更されたときの画像のサイズ変更方法を制御するには、**モード**を使用します。
6. **イメージビュー**を選択した状態で、もう一度クリックして**制約**を追加します。

    ![](displaying-an-image-images/display05.png "Adding Constraints")
7. **イメージビュー**の各エッジにある "T" という形のハンドルを画面の対応する側にドラッグして、画像を "ピン留め" します。 このようにして、画面のサイズを変更すると、**画像の表示**が縮小され、拡大します。
8. 変更内容をストーリーボードに保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/display01vs.png "A sample Image Asset has been added")

ストーリーボードに表示するには、次の手順を実行します。

1. **ソリューションエクスプローラー**内の `Main.storyboard` ファイルをダブルクリックして、iOS Designer で編集するために開きます。
2. **[ツールボックス]** から**イメージビュー**を選択します。

     ![](displaying-an-image-images/display02vs.png "Select an Image View from the Toolbox")
3. イメージビューをデザイン画面にドラッグし、必要に応じて位置とサイズを変更します。

    ![](displaying-an-image-images/display03vs.png "A new Image View on the Design Surface")
4. **プロパティエクスプローラー**の **[ウィジェット]** セクションで、表示する**イメージ**アセットを選択します。

    ![](displaying-an-image-images/display04vs.png "Select the desired Image asset to be displayed")
5. **[ビュー]** セクションで、**イメージビュー**のサイズが変更されたときの画像のサイズ変更方法を制御するには、**モード**を使用します。
6. **イメージビュー**を選択した状態で、もう一度クリックして**制約**を追加します。

    ![](displaying-an-image-images/display05vs.png "Adding Constraints")
7. **イメージビュー**の各エッジにある "T" という形のハンドルを画面の対応する側にドラッグして、画像を "ピン留め" します。 このようにして、画面のサイズを変更すると、**画像の表示**が縮小され、拡大します。
8. 変更内容をストーリーボードに保存します。

-----

## <a name="displaying-an-image-in-code"></a>コードでのイメージの表示

ストーリーボードに画像を表示するのと同じように、アセットカタログを使用して Xamarin の iOS プロジェクトにイメージを追加すると、コードをC#使用して簡単に表示できるようになります。

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

このコードは、新しい `UIImageView` を作成し、初期サイズと位置を提供します。 次に、プロジェクトに追加されたイメージアセットからイメージを読み込み、`UIImageView` を親 `UIView` に追加して表示します。

## <a name="related-links"></a>関連リンク

- [イメージの操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [イメージのサイズと解像度 (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
