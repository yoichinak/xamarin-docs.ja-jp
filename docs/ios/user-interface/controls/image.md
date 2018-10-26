---
title: Xamarin.iOS でのイメージを表示します。
description: このドキュメントでは、Xamarin.iOS でイメージを表示する方法について説明します。 プログラムまたは iOS Designer によって、アプリに追加のイメージがについて説明します。
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/13/2018
ms.openlocfilehash: bb57dbdb87555fb0b3ab8941024e5a84e4723099
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112638"
---
# <a name="displaying-images-with-xamarinios"></a>Xamarin.iOS でのイメージを表示します。

アプリにイメージを追加するには、2 つの手順が必要です: 最初に、プロジェクトにイメージを追加次に、コントロールと、画面に表示するコードを追加します。 参照してください、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)Xamarin.iOS での画像処理のカバレッジの詳細情報の記事。

## <a name="adding-images-to-your-app"></a>アプリへの画像の追加

Mac のソリューションでは、Visual Studio での任意のフォルダーにイメージを追加できる場合に、**ビルド アクション**に設定されている**コンテンツ**ファイルがアプリに含められ、表示されることができます。

Visual Studio for Mac と呼ばれる特殊なディレクトリもサポートしています。**リソース**イメージ ファイルを含むことができます。 リソース フォルダー内のファイルが必要、**ビルド アクション**設定**BundleResource**します。

このスクリーン ショットは、**ビルド アクション**ときファイルに表示されるオプションを右クリックします。

 [![](image-images/image30a.png "[ビルド アクション] メニュー")](image-images/image30a.png#lightbox)

Visual Studio for Mac は通常、適切な選択**ビルド アクション**自動的がファイルをプロジェクトに移動する場合に特には、これらの設定では、注意する必要があります。

### <a name="adding-an-image-file"></a>イメージ ファイルを追加します。

プロジェクトにイメージ ファイルを追加するプロジェクトを右クリックして選択**ファイルを追加しています.**

 [![](image-images/image31a.png "ファイルを [追加] メニュー")](image-images/image31a.png#lightbox)

イメージ (またはイメージ) の選択を標準ファイル ダイアログに含めたいです。 イメージがあるために、既定のビルド アクション**BundleResource** – 特定の理由がない限り、この値をオーバーライドしません。

 [![](image-images/image32a.png "追加ファイル ダイアログ ボックス")](image-images/image32a.png#lightbox)

イメージとが読み込まれ、コードに表示される使用可能なプロジェクトに追加されます。 このスクリーン ショットは、iOS アプリケーション プロジェクトに追加されたイメージを示しています。

 [![](image-images/image33a.png "プロジェクト内のイメージ")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>リソース ディレクトリとは何ですか。

配置されたファイル、**リソース**ディレクトリは通常ファイルの内容を扱い、**リソース**フォルダーは、アプリケーションのルートにコピーされでそこから参照できますコード。 多くの理由で便利なこのすることができます。

-  既定のスタートアップのイメージとアプリケーションのアイコンなどのアプリケーションのプロパティで構成されたイメージを保存します。
-  その他のイメージと、コードから個別にファイルを格納するように容易になるよう管理 (サブディレクトリは、リソース ディレクトリの内容をコピーするときに保持されます)。


**リソース**ディレクトリは、コードでは、それらのイメージが容易に書き込み共有コード ライブラリを必要とする、コンシューマー側アプリケーションのルートにコピーされると想定できますので、ライブラリ プロジェクトで特に便利です画像、サウンド、ビデオ、XML、またはその他のファイル。

**リソース**ディレクトリへの名前は、そのためにする必要があります、あり、すべてのファイルがビルド アクションがあります。 **BundleResource**します。

## <a name="displaying-the-image"></a>イメージを表示します。

IOS Designer を使用して、 **Image View**イメージまたはアニメーション化された一連のイメージを表示します。 **Image View**ツールボックスからアイコンを次に示します。

 [![](image-images/image35a.png "ツールボックスに ImageView")](image-images/image35.png#lightbox)

ドラッグ、 **Image View**から、**ツールボックス**ビュー コント ローラーにします。 その後、 **Image View > イメージ**ドロップダウン リストで、プロジェクトのすべての使用可能なイメージ ファイルの一覧が表示されます。 イメージのビューに追加するこれらのいずれかを選択します。

 [![](image-images/image36a.png "ツールボックスに ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>プログラムでイメージを表示します。

**SF Monkey.jpg**のルートにある、**リソース**ディレクトリ アプリケーション バンドルのルートで実行時に利用可能になります。 イメージのビュー コントロールでは、このイメージを表示するには、次のコードを使用します。

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

内のイメージを配置しました。 いた場合**SF/リソース/Pics Monkey.jpg**、コードが含まれますし、 **Pics**フォルダーのパス。

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

リソース ファイルを参照を含める必要があることはありません、**リソース**フォルダー。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
