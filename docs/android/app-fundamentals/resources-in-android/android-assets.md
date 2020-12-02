---
title: Android アセットの使用
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 795f80f69294abdfd7bf412225ab77cbbe5cb5b1
ms.sourcegitcommit: d2daaa6ca5fe630f80d5a8151985d9f96a2fc93b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2020
ms.locfileid: "96513001"
---
# <a name="using-android-assets"></a>Android アセットの使用

_アセット_ を使用すると、テキスト、xml、フォント、音楽、ビデオなどの任意のファイルをアプリケーションに含めることができます。 これらのファイルを "リソース" として追加しようとすると、Android によってそれらのファイルがリソースシステムに処理されるため、生データを取得できなくなります。 データにアクセスする場合は、資産を使用する方法があります。

プロジェクトに追加されたアセットは、 [AssetManager](xref:Android.Content.Res.AssetManager)を使用してアプリケーションで読み取ることができるファイルシステムと同様に表示されます。
この簡単なデモでは、プロジェクトにテキストファイル資産を追加し、を使用してそれを読み取って、 `AssetManager` TextView に表示します。

## <a name="add-asset-to-project"></a>プロジェクトへの資産の追加

アセットは、 `Assets` プロジェクトのフォルダーにあります。 という名前の新しいテキストファイルをこのフォルダーに追加 `read_asset.txt` します。 "私は資産から来ました" のようなテキストを入力します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio では、このファイルの **ビルドアクション** を **Androidasset** に設定する必要があります。

![AndroidAsset にビルドアクションを設定しています](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac は、このファイルの **ビルドアクション** を **Androidasset** に設定する必要があります。

[![AndroidAsset にビルドアクションを設定しています](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

適切な **BuildAction** を選択すると、コンパイル時にファイルが apk にパッケージ化されるようになります。

## <a name="reading-assets"></a>アセットの読み取り

アセットは [AssetManager](xref:Android.Content.Res.AssetManager)を使用して読み取られます。 のインスタンスは、アクティビティなどのの `AssetManager` [Assets](xref:Android.Content.Context.Assets) プロパティにアクセスすることによって使用でき `Android.Content.Context` ます。
次のコードでは、 **read_asset.txt** アセットを開き、内容を読み取って、TextView を使用して表示します。

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

### <a name="reading-binary-assets"></a>バイナリアセットの読み取り

上の例でを使用すること `StreamReader` は、テキスト資産に最適です。 バイナリアセットの場合は、次のコードを使用します。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Read the contents of our asset
    const int maxReadSize = 256 * 1024;
    byte[] content;
    AssetManager assets = this.Assets;
    using (BinaryReader br = new BinaryReader (assets.Open ("mydatabase.db")))
    {
        content = br.ReadBytes (maxReadSize);
    }

    // Do something with it...

}
```

## <a name="running-the-application"></a>アプリケーションの実行

アプリケーションを実行すると、次のように表示されます。

![スクリーンショットの例](android-assets-images/screenshot.png)

## <a name="related-links"></a>関連リンク

- [AssetManager](xref:Android.Content.Res.AssetManager)
- [コンテキスト](xref:Android.Content.Context)
