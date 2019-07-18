---
title: Android アセットの使用
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 1e9a71de7725c8382e133d85977407bcc859fc58
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013719"
---
# <a name="using-android-assets"></a>Android アセットの使用

_資産_アプリケーションでテキスト、xml、フォント、音楽、ビデオなどの任意のファイルを追加する方法を提供します。 「リソース」としてこれらのファイルを含めると、Android は、リソース システムにそれらを処理し、生データを取得することはできません。 データの変更を加えずにアクセスする場合は、資産は、これを行う方法の 1 つです。

使用して、アプリケーションから読み取り可能なファイル システムと同様、プロジェクトに追加された資産が表示されます[AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)します。
この簡単なデモで、プロジェクトにテキスト ファイルの資産を追加しようとして読み取りを使用して`AssetManager`、し、[textview] に表示します。


## <a name="add-asset-to-project"></a>資産をプロジェクトに追加します。

資産の移動、`Assets`プロジェクトのフォルダー。 という名前のこのフォルダーに新しいテキスト ファイルを追加`read_asset.txt`します。 「用件資産!」のようにいくつかのテキストを配置します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio を設定する必要がありますが、**ビルド アクション**にこのファイルの**AndroidAsset**:

![ビルド アクションを AndroidAsset に設定します。](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac を設定する必要がありますが、**ビルド アクション**にこのファイルの**AndroidAsset**:

[![ビルド アクションを AndroidAsset に設定します。](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

適切な選択 **"ビルド アクション"** により、ファイルは、APK にコンパイル時にパッケージ化するようになります。


## <a name="reading-assets"></a>資産の読み取り

使用して資産が読み取られる、 [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)します。 インスタンス、`AssetManager`にアクセスすることがある、[資産](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/)プロパティを`Android.Content.Context`、アクティビティなどです。
次のコードを開いて、 **read_asset.txt**資産の内容の読み取りし、TextView を使用して表示します。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```


## <a name="running-the-application"></a>アプリケーションの実行

アプリケーションを実行し、次に表示する必要があります。

![例のスクリーン ショット](android-assets-images/screenshot.png)


## <a name="related-links"></a>関連リンク

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [コンテキスト](https://developer.xamarin.com/api/type/Android.Content.Context/)
