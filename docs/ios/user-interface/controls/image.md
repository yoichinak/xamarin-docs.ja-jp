---
title: Xamarin を使用したイメージの表示
description: このドキュメントでは、Xamarin. iOS でイメージを表示する方法について説明します。 プログラムまたは iOS Designer を使用して、アプリにイメージを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: 2350c068b0a719e8d633c0f96712aec40e7865aa
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697476"
---
# <a name="displaying-images-with-xamarinios"></a>Xamarin を使用したイメージの表示

アプリにイメージを追加するには、次の2つの手順を実行する必要があります。まず、イメージをプロジェクトに追加します。次に、コントロールとコードを追加して、画面に表示します。 Xamarin. iOS のイメージ処理の詳細については、 [イメージの操作](~/ios/app-fundamentals/images-icons/index.md) に関する記事を参照してください。

## <a name="adding-images-to-your-app"></a>アプリへのイメージの追加

イメージは Visual Studio for Mac ソリューション内の任意のフォルダーに追加できます。また、[ **ビルド] アクション** が [ **コンテンツ** ] に設定されている場合、そのファイルはアプリに含まれ、表示できます。

Visual Studio for Mac では、イメージファイルも格納できる **リソース** と呼ばれる特別なディレクトリもサポートされています。 Resources フォルダー内のファイルには、 **ビルドアクション** が **BundleResource** に設定されている必要があります。

このスクリーンショットは、ファイルが右クリックされたときに表示される **ビルドアクション** オプションを示しています。

 [![ビルドアクションメニュー](image-images/image30a.png)](image-images/image30a.png#lightbox)

Visual Studio for Mac は通常、適切な **ビルドアクション** を自動的に選択しますが、プロジェクト内でファイルを移動する場合は特に、これらの設定に注意する必要があります。

### <a name="adding-an-image-file"></a>イメージファイルの追加

プロジェクトにイメージファイルを追加するには、まずプロジェクトを右クリックし、[**ファイルの追加...** ] を選択します。

 [![ファイルの追加...メニュー](image-images/image31a.png)](image-images/image31a.png#lightbox)

標準ファイルダイアログに含めるイメージ (またはイメージ) を選択します。 イメージの既定のビルドアクションは **BundleResource** になります。具体的な理由がない限り、この値をオーバーライドしないでください。

 [![[ファイルの追加] ダイアログ](image-images/image32a.png)](image-images/image32a.png#lightbox)

イメージがプロジェクトに追加され、コードで読み込んで表示できるようになります。 このスクリーンショットは、iOS アプリケーションプロジェクトに追加されたイメージを示しています。

 [![プロジェクト内の画像](image-images/image33a.png)](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Resources ディレクトリとは何ですか。

**Resources** ディレクトリに配置されたファイルは、通常のファイルとは異なる方法で扱われます。 **resources** フォルダーの内容はアプリケーションのルートにコピーされ、コード内でそこから参照できます。 これは、さまざまな理由で役に立ちます。

- 既定のスタートアップイメージやアプリケーションアイコンなど、アプリケーションのプロパティで構成されたイメージを格納する。
- 他のイメージやファイルをコードとは別に格納すると、管理が容易になります (リソースディレクトリの内容がコピーされるときにサブディレクトリが保持されます)。

**リソース** ディレクトリは、ライブラリプロジェクトで特に便利です。コードでは、それらのイメージが使用中のアプリケーションのルートにコピーされることを想定でき、イメージ、サウンド、ビデオ、XML などのファイルを必要とする共有コードライブラリを簡単に記述できるためです。

**Resources** ディレクトリには名前を付ける必要があります。また、すべてのファイルにはビルドアクションを **BundleResource** に設定する必要があります。

## <a name="displaying-the-image"></a>画像を表示する

IOS デザイナーでイメージ **ビュー** を使用して、イメージまたはアニメーション化された一連のイメージを表示します。 ツールボックスの [ **イメージビュー** ] アイコンを次に示します。

 [![[ツールボックス] の [ImageView] アイコン。](image-images/image35a.png)](image-images/image35.png#lightbox)

[**ツールボックス**] から **イメージビュー** をビューコントローラーにドラッグします。 次に、[ **イメージビュー > イメージ** ] で、プロジェクト内の使用可能なすべてのイメージファイルの一覧がドロップダウンリストに表示されます。 これらのいずれかを選択して、イメージビューに追加します。

 [![ツールボックスの ImageView](image-images/image36a.png)](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>プログラムによるイメージの表示

**SF Monkey.jpg** は **Resources** ディレクトリのルートにあるため、アプリケーションバンドルのルートで実行時に使用できるようになります。 このイメージをイメージビューコントロールに表示するには、次のコードを使用します。

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

イメージを **/resources/Monkey.jpg** に配置した場合、コードにはパスに **Pics** フォルダーが含まれます。

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

リソースファイル参照に **リソース** フォルダーを含める必要はありません。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](/samples/xamarin/ios-samples/controls)