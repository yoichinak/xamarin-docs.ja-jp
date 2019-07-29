---
title: Android アセットの使用
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: f8a542b58fa891b63f43d1c87dea911b83e01949
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509317"
---
# <a name="using-android-assets"></a>Android アセットの使用

_アセット_を使用すると、テキスト、xml、フォント、音楽、ビデオなどの任意のファイルをアプリケーションに含めることができます。 これらのファイルを "リソース" として追加しようとすると、Android によってそれらのファイルがリソースシステムに処理されるため、生データを取得できなくなります。 データにアクセスする場合は、資産を使用する方法があります。

プロジェクトに追加されたアセットは、 [AssetManager](xref:Android.Content.Res.AssetManager)を使用してアプリケーションで読み取ることができるファイルシステムと同様に表示されます。
この簡単なデモでは、プロジェクトにテキストファイル資産を追加し、を使用し`AssetManager`てそれを読み取って、TextView に表示します。


## <a name="add-asset-to-project"></a>プロジェクトへの資産の追加

アセットは、 `Assets`プロジェクトのフォルダーにあります。 という名前`read_asset.txt`の新しいテキストファイルをこのフォルダーに追加します。 "私は資産から来ました" のようなテキストを入力します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio では、このファイルの**ビルドアクション**を**Androidasset**に設定する必要があります。

![AndroidAsset にビルドアクションを設定しています](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac は、このファイルの**ビルドアクション**を**Androidasset**に設定する必要があります。

[![AndroidAsset にビルドアクションを設定しています](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

適切な**BuildAction**を選択すると、コンパイル時にファイルが apk にパッケージ化されるようになります。


## <a name="reading-assets"></a>アセットの読み取り

アセットは[AssetManager](xref:Android.Content.Res.AssetManager)を使用して読み取られます。 の`AssetManager`インスタンスは、アクティビティなどのの[Assets](xref:Android.Content.Context.Assets)プロパティ`Android.Content.Context`にアクセスすることによって使用できます。
次のコードでは、 **read_asset**アセットを開き、内容を読み取って、TextView を使用して表示します。

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

アプリケーションを実行すると、次のように表示されます。

![スクリーンショットの例](android-assets-images/screenshot.png)


## <a name="related-links"></a>関連リンク

- [AssetManager](xref:Android.Content.Res.AssetManager)
- [関連](xref:Android.Content.Context)
