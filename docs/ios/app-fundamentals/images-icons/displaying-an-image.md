---
title: Xamarin でのイメージの表示
description: この記事では、Xamarin の iOS アプリにイメージ資産を含め、C# コードを使用するか、iOS デザイナーのコントロールに割り当ててイメージを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/24/2018
ms.openlocfilehash: f9de09065d7c26c9ae98ef664be63599becb4da5
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997450"
---
# <a name="displaying-an-image-in-xamarinios"></a>Xamarin でのイメージの表示

_この記事では、Xamarin の iOS アプリにイメージ資産を含め、C# コードを使用するか、iOS デザイナーのコントロールに割り当ててイメージを表示する方法について説明します。_

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Xamarin iOS アプリでのイメージの追加と整理

Xamarin iOS アプリで使用するイメージを追加すると、開発者は_資産カタログ_を使用して、アプリに必要なすべての iOS デバイスと解像度をサポートします。

IOS 7 で追加された**資産カタログイメージセット**には、アプリのさまざまなデバイスとスケールファクターをサポートするために必要なイメージのすべてのバージョンまたは表現が含まれています。 イメージアセットファイル名を使用するのではなく、**イメージセット**は Json ファイルを使用して、どのイメージがどのデバイスまたはどの解像度に属しているかを指定します。 Ios (iOS 9 以降) でイメージを管理およびサポートする場合は、この方法をお勧めします。

## <a name="adding-images-to-an-asset-catalog-image-set"></a>アセットカタログイメージセットへのイメージの追加

既に説明したように、**資産カタログのイメージセット**には、アプリのさまざまなデバイスやスケールファクターをサポートするために必要なイメージのすべてのバージョンまたは表現が含まれています。 イメージアセットファイル名を使用するのではなく、**イメージセット**は Json ファイルを使用して、どのイメージがどのデバイスまたはどの解像度に属しているかを指定します。

新しいイメージセットを作成してイメージを追加するには、次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **ソリューションエクスプローラー**で、ファイルをダブルクリックし `Assets.xcassets` て開き、編集します。

    ![ソリューションエクスプローラー内の asset. xcassets](displaying-an-image-images/imageset01.png)
2. [**アセット] ボックス**を右クリックし、[**新しいイメージセット**] を選択します。

    ![新しいイメージセットの追加](displaying-an-image-images/imageset02.png)
3. 新しいイメージセットを選択すると、エディターが表示されます。

    ![イメージセットエディター](displaying-an-image-images/imageset03.png)
4. ここでは、必要なさまざまなデバイスと解像度ごとにイメージをドラッグします。
5. [**アセット] 一覧**で新しいイメージセットの**名前**をダブルクリックして編集します。 ![ 新しいイメージセットの名前を編集します。](displaying-an-image-images/imageset04.png)

IOS デザイナーで**イメージセット**を使用する場合は、プロパティエディターのドロップダウンリストからセットの名前を選択するだけです。

![ドロップダウンリストからイメージセットの名前を選択します](displaying-an-image-images/imageset06.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**からアセットカタログを開き、左上隅に**ある [+** ] ボタンをクリックします。

    ![プラスボタンをクリックします。](displaying-an-image-images/asset5.png)

2. [**イメージセットの追加**] を選択すると、新しいイメージセットのイメージセットエディターが表示されます。 ここでは、必要なさまざまなデバイスと解像度ごとにイメージをドラッグします。

    ![イメージセットエディター](displaying-an-image-images/asset7.png)

### <a name="renaming-an-image-set"></a>イメージセットの名前変更

イメージセットの名前を変更するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、**アセットカタログ**ファイルをダブルクリックして編集用に開きます。

    ![ソリューションエクスプローラーの資産カタログ](displaying-an-image-images/rename01.png)
2. 名前を変更する**イメージセット**を選択します:

    ![名前を変更するイメージセットを選択します](displaying-an-image-images/rename02.png)
3. **プロパティエクスプローラー**で一番下までスクロールし、[**名前**] ([その**他**] セクションの下) を選択します。

    ![[その他] セクションで [名前] を選択します。](displaying-an-image-images/rename03.png)
4. **イメージセット**の新しい**名前**を入力し、変更を保存します。

-----

コードで設定された**イメージ**を使用する場合は、クラスのメソッドを呼び出すことによって、名前でそれを参照し `FromBundle` `UIImage` ます。 たとえば次のようになります。

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> イメージセットに割り当てられたイメージが正しく表示されない場合は、正しいファイル名が `FromBundle` メソッド (親**アセットカタログ**名ではなく、**イメージセット**) で使用されていることを確認します。 PNG イメージの場合は、 `.png` 拡張を省略できます。 その他のイメージ形式の場合は、拡張機能が必要です (例として、 `PurpleMonkey.jpg`).

### <a name="using-vector-images-in-asset-catalogs"></a>アセットカタログでのベクターイメージの使用

IOS 8 の時点では、特殊な**vector**クラスが**イメージセット**に追加されています。これにより、開発者は、さまざまな解像度で個別のビットマップファイルを使用する代わりに、 **PDF**形式のベクター画像をカセットに含めることができます。 このメソッドを使用して、(ベクター PDF ファイルとして書式設定された) 解像度に対して1つのベクターファイルを指定 `@1x` し `@2x` `@3x` ます。また、ファイルのバージョンとバージョンがコンパイル時に生成され、アプリケーションのバンドルに含まれます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![アセットカタログエディターのベクターイメージ](displaying-an-image-images/imageset05.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![アセットカタログエディターのベクターイメージ](displaying-an-image-images/asset8.png)

-----

たとえば、開発者が、 `MonkeyIcon.pdf` 150 px x 150 px の解決策を使用して資産カタログのベクターとしてファイルを含む場合、コンパイル時に次のビットマップアセットが最終的なアプリバンドルに含まれます。

- `MonkeyIcon@1x.png`-150 px x 150 px 解像度。
- `MonkeyIcon@2x.png`-300 x 300 resolution。
- `MonkeyIcon@3x.png`-450px x 450px 解像度。

アセットカタログで PDF ベクターイメージを使用する場合は、次の点を考慮する必要があります。

- これは、コンパイル時に PDF がビットマップにラスタライズされ、最終的なアプリケーションに付属するビットマップになるため、完全なベクターサポートではありません。
- アセットカタログで設定されたイメージのサイズを調整することはできません。 開発者がイメージのサイズを変更しようとした場合 (コードまたは Auto Layout クラスと Size クラスを使用)、イメージは他のビットマップと同様に変形されます。
- 資産カタログは、iOS 7 以上とのみ互換性があります。アプリで iOS 6 以下をサポートする必要がある場合は、アセットカタログを使用できません。

## <a name="working-with-template-images"></a>テンプレートイメージの操作

IOS アプリの設計に基づいて、開発者がユーザーインターフェイス内のアイコンまたはイメージをカスタマイズして、配色の変化 (ユーザー設定に基づくなど) を一致させる必要がある場合があります。

この効果を簡単に実現するには、イメージ資産の_レンダリングモード_を [**テンプレートイメージ**] に切り替えます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![テンプレートイメージに設定されたレンダリングモード](displaying-an-image-images/templateimage01.png)](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![テンプレートに設定されたレンダリングモード](displaying-an-image-images/templateimage01vs.png)](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS デザイナーから、イメージ資産を UI コントロールに割り当て、次にイメージを色分けするように**濃淡**を設定します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![濃淡を設定してイメージを色分けする](displaying-an-image-images/templateimage03.png)](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![濃淡を設定してイメージを色分けする](displaying-an-image-images/templateimage03vs.png)](displaying-an-image-images/templateimage03vs.png#lightbox)

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

`RenderMode`のプロパティは読み取り専用であるため `UIImage` 、必要なレンダリングモード設定を使用して、メソッドを使用し `ImageWithRenderingMode` てイメージの新しいインスタンスを作成します。

`UIImage.RenderMode`列挙型を使用すると、次の3つの設定が可能です `UIImageRenderingMode` 。

- `AlwaysOriginal`-イメージを変更せずに元のソースイメージファイルとして強制的にレンダリングします。
- `AlwaysTemplate`-指定された色のピクセルを色分けすることによって、イメージをテンプレートイメージとして強制的にレンダリングし `Tint` ます。
- `Automatic`-イメージをテンプレートとして、または使用されている環境に基づいて元のものとしてレンダリングします。 たとえば、イメージが、、またはで使用されている場合 `UIToolBar` `UINavigationBar` は、 `UITabBar` `UISegmentControl` テンプレートとして扱われます。

## <a name="adding-new-assets-collections"></a>新しいアセットコレクションの追加

Assets カタログ内のイメージを使用する場合、すべてのアプリのイメージをコレクションに追加するのではなく、新しいコレクションが必要になる場合があり `Assets.xcassets` ます。 たとえば、オンデマンドリソースを設計する場合などです。

新しい Assets カタログをプロジェクトに追加するには、次のようにします。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **ソリューションエクスプローラー**で**プロジェクト名**を右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。
2. [ **IOS**  >  **資産カタログ**] を選択し、コレクションの**名前**を入力して、[**新規**] ボタンをクリックします。

    ![新しいアセットカタログの作成](displaying-an-image-images/asset01.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. ソリューションエクスプローラーで、[**資産カタログ**] フォルダーを右クリックし、[**新しい資産カタログの追加 >**] を選択します。
2. 名前を付け、[**追加**] をクリックします。

    ![新しいアセットカタログの作成](displaying-an-image-images/asset1.png)

-----

ここから、コレクションは、 `Assets.xcassets` プロジェクトに自動的に含まれる既定のコレクションと同じ方法で操作できます。

## <a name="using-images-with-controls"></a>コントロールでのイメージの使用

アプリをサポートするためにイメージを使用するだけでなく、iOS では、タブバー、ツールバー、ナビゲーションバー、テーブル、ボタンなどのアプリのコントロールの種類を含むイメージも使用します。 コントロールにイメージを表示する簡単な方法は、 `UIImage` コントロールのプロパティにインスタンスを割り当てることです `Image` 。

### <a name="frombundle"></a>FromBundle

`FromBundle`メソッド呼び出しは、さまざまな解像度のイメージファイルのキャッシュサポートや自動処理など、多数のイメージ読み込みおよび管理機能が組み込まれた同期 (ブロッキング) 呼び出しです。

次の例は、ののイメージを設定する方法を示してい `UITabBarItem` `UITabBar` ます。

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

これ `MyImage` が、上のアセットカタログに追加されたイメージ資産の名前であることを前提としています。 アセットカタログイメージを操作するときは、 `FromBundle` **PNG**形式の画像用にメソッドで設定されたイメージの名前を指定するだけです。

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

その他のイメージ形式には、という名前の拡張子を含めます。 次に例を示します。

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

アイコンとイメージの詳細については、Apple のドキュメントの「[カスタムアイコンとイメージの作成](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)に関するガイドライン」を参照してください。

## <a name="displaying-an-image-in-a-storyboards"></a>ストーリーボードでのイメージの表示

アセットカタログを使用して Xamarin. iOS プロジェクトにイメージを追加すると、iOS Designer のを使用してストーリーボードに簡単に表示できます `UIImageView` 。 たとえば、次のイメージアセットが追加されているとします。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![サンプルイメージアセットが追加されました](displaying-an-image-images/display01.png)

ストーリーボードに表示するには、次の手順を実行します。

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Main.storyboard` て、IOS Designer で編集するために開きます。 **Solution Explorer**
2. [**ツールボックス**] から**イメージビュー**を選択します。

     ![ツールボックスからイメージビューを選択する](displaying-an-image-images/display02.png)
3. イメージビューをデザイン画面にドラッグし、必要に応じて位置とサイズを変更します。

    ![デザインサーフェイスの新しいイメージビュー](displaying-an-image-images/display03.png)
4. **プロパティエクスプローラー**の [**ウィジェット**] セクションで、表示する**イメージ**アセットを選択します。

    ![表示するイメージアセットを選択します](displaying-an-image-images/display04.png)
5. [**ビュー** ] セクションで、**イメージビュー**のサイズが変更されたときの画像のサイズ変更方法を制御するには、**モード**を使用します。
6. **イメージビュー**を選択した状態で、もう一度クリックして**制約**を追加します。

    ![制約の追加](displaying-an-image-images/display05.png)
7. **イメージビュー**の各エッジにある "T" という形のハンドルを画面の対応する側にドラッグして、画像を "ピン留め" します。 このようにして、画面のサイズを変更すると、**画像の表示**が縮小され、拡大します。
8. 変更内容をストーリーボードに保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![サンプルイメージアセットが追加されました](displaying-an-image-images/display01vs.png)

ストーリーボードに表示するには、次の手順を実行します。

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Main.storyboard` て、IOS Designer で編集するために開きます。 **Solution Explorer**
2. [**ツールボックス**] から**イメージビュー**を選択します。

     ![ツールボックスからイメージビューを選択する](displaying-an-image-images/display02vs.png)
3. イメージビューをデザイン画面にドラッグし、必要に応じて位置とサイズを変更します。

    ![デザインサーフェイスの新しいイメージビュー](displaying-an-image-images/display03vs.png)
4. **プロパティエクスプローラー**の [**ウィジェット**] セクションで、表示する**イメージ**アセットを選択します。

    ![表示するイメージアセットを選択します](displaying-an-image-images/display04vs.png)
5. [**ビュー** ] セクションで、**イメージビュー**のサイズが変更されたときの画像のサイズ変更方法を制御するには、**モード**を使用します。
6. **イメージビュー**を選択した状態で、もう一度クリックして**制約**を追加します。

    ![制約の追加](displaying-an-image-images/display05vs.png)
7. **イメージビュー**の各エッジにある "T" という形のハンドルを画面の対応する側にドラッグして、画像を "ピン留め" します。 このようにして、画面のサイズを変更すると、**画像の表示**が縮小され、拡大します。
8. 変更内容をストーリーボードに保存します。

-----

## <a name="displaying-an-image-in-code"></a>コードでのイメージの表示

ストーリーボードに画像を表示するのと同じように、アセットカタログを使用して Xamarin の iOS プロジェクトにイメージを追加すると、C# コードを使用して簡単に表示できるようになります。

次に例を示します。

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

このコードは、新しいを作成 `UIImageView` し、初期サイズと位置を提供します。 次に、プロジェクトに追加されたイメージアセットからイメージを読み込み、 `UIImageView` それを表示するためのを親に追加し `UIView` ます。

## <a name="related-links"></a>関連リンク

- [イメージの操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [イメージのサイズと解像度 (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
