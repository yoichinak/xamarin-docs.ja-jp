---
title: Xamarin を使用したイメージの表示
description: このドキュメントでは、Xamarin. iOS でイメージを表示する方法について説明します。 プログラムまたは iOS Designer を使用して、アプリにイメージを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 191d037d12d27e2c6ab475467853023f440c71d4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291362"
---
# <a name="displaying-images-with-xamarinios"></a>Xamarin を使用したイメージの表示

アプリにイメージを追加するには、次の2つの手順を実行する必要があります。まず、イメージをプロジェクトに追加します。次に、コントロールとコードを追加して、画面に表示します。 Xamarin. iOS のイメージ処理の詳細については、[イメージの操作](~/ios/app-fundamentals/images-icons/index.md)に関する記事を参照してください。

## <a name="adding-images-to-your-app"></a>アプリへのイメージの追加

イメージは Visual Studio for Mac ソリューション内の任意のフォルダーに追加できます。また、[**ビルド] アクション**が **[コンテンツ]** に設定されている場合、そのファイルはアプリに含まれ、表示できます。

Visual Studio for Mac では、イメージファイルも格納できる**リソース**と呼ばれる特別なディレクトリもサポートされています。 Resources フォルダー内のファイルには、**ビルドアクション**が**BundleResource**に設定されている必要があります。

このスクリーンショットは、ファイルが右クリックされたときに表示される**ビルドアクション**オプションを示しています。

 [![](image-images/image30a.png "ビルドアクションメニュー")](image-images/image30a.png#lightbox)

Visual Studio for Mac は通常、適切な**ビルドアクション**を自動的に選択しますが、プロジェクト内でファイルを移動する場合は特に、これらの設定に注意する必要があります。

### <a name="adding-an-image-file"></a>イメージファイルの追加

プロジェクトにイメージファイルを追加するには、まずプロジェクトを右クリックし、 **[ファイルの追加...]** を選択します。

 [![](image-images/image31a.png "ファイルの追加...メニュー")](image-images/image31a.png#lightbox)

標準ファイルダイアログに含めるイメージ (またはイメージ) を選択します。 イメージの既定のビルドアクションは**BundleResource**になります。具体的な理由がない限り、この値をオーバーライドしないでください。

 [![](image-images/image32a.png "[ファイルの追加] ダイアログ")](image-images/image32a.png#lightbox)

イメージがプロジェクトに追加され、コードで読み込んで表示できるようになります。 このスクリーンショットは、iOS アプリケーションプロジェクトに追加されたイメージを示しています。

 [![](image-images/image33a.png "プロジェクト内の画像")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Resources ディレクトリとは何ですか。

**Resources**ディレクトリに配置されたファイルは、通常のファイルとは異なる方法で扱われます。 **resources**フォルダーの内容はアプリケーションのルートにコピーされ、コード内でそこから参照できます。 これは、さまざまな理由で役に立ちます。

- 既定のスタートアップイメージやアプリケーションアイコンなど、アプリケーションのプロパティで構成されたイメージを格納する。
- 他のイメージやファイルをコードとは別に格納すると、管理が容易になります (リソースディレクトリの内容がコピーされるときにサブディレクトリが保持されます)。


**リソース**ディレクトリは、ライブラリプロジェクトで特に便利です。コードでは、それらのイメージが使用中のアプリケーションのルートにコピーされることを想定して、イメージ、サウンド、ビデオ、XML を必要とする共有コードライブラリを作成しやすくするためです。その他のファイル。

**Resources**ディレクトリには名前を付ける必要があります。また、すべてのファイルにはビルドアクションを**BundleResource**に設定する必要があります。

## <a name="displaying-the-image"></a>画像を表示する

IOS デザイナーでイメージ**ビュー**を使用して、イメージまたはアニメーション化された一連のイメージを表示します。 ツールボックスの **[イメージビュー]** アイコンを次に示します。

 [![](image-images/image35a.png "ツールボックスの ImageView")](image-images/image35.png#lightbox)

**[ツールボックス]** から**イメージビュー**をビューコントローラーにドラッグします。 次に、 **[イメージビュー > イメージ]** で、プロジェクト内の使用可能なすべてのイメージファイルの一覧がドロップダウンリストに表示されます。 これらのいずれかを選択して、イメージビューに追加します。

 [![](image-images/image36a.png "ツールボックスの ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>プログラムによるイメージの表示

**SF**は**リソース**ディレクトリのルートに配置されているため、アプリケーションバンドルのルートで実行時に使用できるようになります。 このイメージをイメージビューコントロールに表示するには、次のコードを使用します。

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

イメージを作成した場合は、次のコードではパスに**Pics**フォルダーが含まれている**ことになり**ます。

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

リソースファイル参照に**リソース**フォルダーを含める必要はありません。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
