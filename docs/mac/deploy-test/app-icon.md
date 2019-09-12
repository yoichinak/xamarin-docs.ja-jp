---
title: Xamarin.Mac アプリのアプリケーション アイコン
description: この記事では、Xamarin.Mac アプリケーションのアイコンに必要な画像の作成、.icns ファイルへの画像のバンドル、および Xamarin.Mac プロジェクトへのアイコンの追加について説明します。
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 2a5f8f6f2feda1ab27c874d8281483e9e26f0855
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770137"
---
# <a name="application-icon-for-xamarinmac-apps"></a>Xamarin.Mac アプリのアプリケーション アイコン

_この記事では、Xamarin.Mac アプリケーションのアイコンに必要な画像の作成、.icns ファイルへの画像のバンドル、および Xamarin.Mac プロジェクトへのアイコンの追加について説明します。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションで C# と .NET を使用している場合、開発者は、*Objective-C* と *Xcode* で作業している開発者と同じ画像およびアイコンツールにアクセスできます。

優れたアイコンは、Xamarin.Mac アプリの主な目的を伝え、アプリを使用するときにユーザーが期待するエクスペリエンスのヒントになる必要があります。 この記事では、アイコンのイメージ アセットを作成して、それらのアセットを `AppIcon.appiconset` ファイルにパッケージ化し、そのファイルを Xamarin.Mac アプリで利用するために必要なすべての手順について説明します。

![AppIcon.appiconset エディター](app-icon-images/intro01.png "AppIcon.appiconset エディター")

## <a name="application-icon"></a>アプリケーション アイコン

優れたアイコンは、Xamarin.Mac アプリの主な目的を伝え、アプリを使用するときにユーザーが期待するエクスペリエンスのヒントになる必要があります。 すべての macOS アプリに、Finder、ドック、スタートパッド、およびコンピューター全体にわたって他の場所でアイコンを表示するためのアイコンのいくつかのサイズを含める必要があります。

## <a name="designing-the-icon"></a>アイコンを設計する

Apple では、アプリケーションのアイコンを設計するときの次のヒントを提案しています。

- 現実的で一意の図形をアイコンに指定することを検討してください。
- MacOS アプリに対応する iOS がある場合、iOS アプリのアイコンを再利用しないでください。
- ユーザーが容易に認識できる汎用の画像を使用します。
- 単純にするために努めてください。
- アイコンがアプリのストーリーを伝えるように、色と影を控え目に使用してください。
- 実際のテキストと_意味のわからない_テキストまたはテキストを示す線を混在させないでください。
- 実際の写真を使用せずに、アイコンのサブジェクトの理想的なバージョンを作成します。
- macOS UI 要素をアイコン内で使用しないでください。
- Apple のアイコンのレプリカをアイコン内で使用しないでください。

Xamarin.Mac アプリのアイコンをデザインする前に、[Apple アプリ アイコン ギャラリー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) の [OS X ヒューマン インターフェイス ガイドラインの](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)と「[Designing App Icons](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1)」(アプリ アイコンの設計) セクションを読んでください。

## <a name="required-image-sizes-and-filenames"></a>必要な画像のサイズとファイル名

開発者が Xamarin.Mac アプリで使用する他のイメージ リソースと同じように、アプリのアイコンも標準バージョンと Retina 解像度バージョンの両方提供する必要があります その他の画像と同じようにアイコン ファイルに名前を付けるときには `@2x` の形式を使用します。

- **標準解像度**  - _画像名_ **.** _ファイル名拡張子_ (例: **icon_512x512.png**)
- **高解像度**  - _画像名_ **@2x.** _ファイル名拡張子_ (例: **icon_512x512@2x.png** )

たとえば、512 x 512 バージョンのアプリのアイコンを指定するには、ファイルの名前は **icon_512x512.png** と **icon_512x512@2x.png** になります。

ユーザーに表示されるすべての場所で、アイコンがきれいに表示されるようにするために、下に表示されているサイズのリソースを提供します。

|ファイル名|ピクセル単位のサイズ|
|---|---|
|icon_512x512@2x.png|1024 x 1024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|128 x 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|16 x 16|

詳細については、Apple の「[Provide High-Resolution Versions of All App Graphics Resources](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3)」(すべてのアプリのグラフィック リソースの高解像度バージョンを提供する) ドキュメントを参照してください。

## <a name="packaging-the-icon-resources"></a>アイコン リソースをパッケージ化する

アイコンをデザインし、必要なファイル サイズと名前で保存したら、Visual Studio for Mac で、Xamarin.Mac で使用するためのイメージ アセットに簡単に割り当てることができます。

次の手順で行います。

1. **Solution Pad** で、**Assets.xcassets** > **AppIcons.appiconset** を開きます。 

    ![AppIcon.appiconset の編集](app-icon-images/intro01.png "AppIcon.appiconset の編集")
2. 必要な各アイコン サイズについて、アイコンをクリックし、上で作成した対応する画像ファイルを選択します。 

    [![アイコン画像の選択](app-icon-images/intro02.png "アイコン画像の選択")](app-icon-images/intro02-large.png#lightbox)
3. 変更内容を保存します。

## <a name="using-the-icon"></a>アイコンを使用する

`AppIcon.appiconset` ファイルがビルドされたら、Visual Studio for Mac で Xamarin.Mac プロジェクトに割り当てる必要があります。

次の手順で行います。

1. **Solution Pad** で **Info.plist** をダブルクリックし、 **[プロジェクト オプション]** を開きます。
2. **[Mac OS X アプリケーション ターゲット]** セクションで、をクリックして、**アプリ アイコン**をクリックし、`AppIcon.appiconset` ファイルを選択します。 

    [![アイコン セットの設定](app-icon-images/icon01.png "アイコン セットの設定")](app-icon-images/icon01-large.png#lightbox)
3. 変更を保存します。

アプリの実行時に、ドックに新しいアイコンが表示されます。

![macOS ドックのアプリ アイコンの例](app-icon-images/icon04.png "macOS ドックのアプリ アイコンの例")

## <a name="summary"></a>まとめ

この記事では、macOS アプリ アイコンを作成し、アイコンをパッケージ化して、アイコンを Xamarin.Mac プロジェクトに組み込むために必要な画像の操作について詳しく説明しました。

## <a name="related-links"></a>関連リンク

- [MacImages (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [イメージの処理](~/mac/app-fundamentals/image.md)
- [macOS ヒューマン インターフェイス ガイドライン - アイコンと画像](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [OS X 向けの高解像度について](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Icns Builder](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
