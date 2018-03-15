---
title: "Android のアセットを使用してください。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: e1890575f5c3a5bd2e0c0de0712ba459607e6139
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="using-android-assets"></a>Android のアセットを使用してください。

_資産_アプリケーションにテキスト、xml、フォント、音楽、ビデオなどの任意のファイルをインクルードする方法を提供します。 「リソース」としてこれらのファイルを含めるしようとする場合は、Android がそのリソース システムに処理され、生データを取得することはできません。 データ変更を加えずにアクセスする場合は、資産、それを実行する 1 つの方法です。

使用して、アプリケーションによってから読み取ることができるファイル システムと同じように、プロジェクトに追加された資産が表示されます[AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)です。
この簡単なデモでは、プロジェクトにテキスト ファイルのアセットを追加しようとしての読み取りを使用して`AssetManager`の TextView に表示します。


## <a name="add-asset-to-project"></a>資産をプロジェクトに追加します。

資産の移動、`Assets`プロジェクトのフォルダーです。 呼ばれるこのフォルダーに新しいテキスト ファイルを追加`read_asset.txt`です。 「は元の資産!」と同じように、そこにいくつかのテキストを配置します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio を設定する必要がありますが、**ビルド アクション**にこのファイルの**AndroidAsset**:

![ビルド アクションを AndroidAsset に設定します。](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac を設定する必要がありますが、**ビルド アクション**にこのファイルの**AndroidAsset**:

[![ビルド アクションを AndroidAsset に設定します。](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

適切な選択**"ビルド アクション"**により、コンパイル時に、APK にファイルをパッケージ化されます。


## <a name="reading-assets"></a>資産の読み取り

使って資産が読み取られた、 [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)です。 インスタンス、`AssetManager`にアクセスして使用できますが、[資産](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/)プロパティを`Android.Content.Context`、アクティビティなどです。
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

アプリケーションを実行し、次が表示されます。

![サンプルのスクリーン ショット](android-assets-images/screenshot.png)


## <a name="related-links"></a>関連リンク

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [コンテキスト](https://developer.xamarin.com/api/type/Android.Content.Context/)
