---
title: "イメージを表示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 716189fbf1518e9100a78cc5ae64e9e63a24c949
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="displaying-images"></a>イメージを表示します。

2 つの手順をアプリにイメージを追加することが必要です最初に、イメージをプロジェクトに追加。次に、コントロールと、画面に表示するコードを追加します。 参照してください、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)カバレッジ イメージ Xamarin.iOS で処理の詳細な記事。

## <a name="adding-images-to-your-app"></a>アプリへのイメージの追加

Mac ソリューションでは、Visual Studio での任意のフォルダーにイメージを追加できる場合は、**ビルド アクション**に設定されている**コンテンツ**し、ファイルは、アプリに含まれて表示されることができます。

Mac 用の visual Studio には、イメージ ファイルを含めることができますもリソースと呼ばれる特殊なディレクトリもサポートしています。 [リソース] フォルダー内のファイルが必要、**ビルド アクション**'éý' **BundleResource**です。

このスクリーン ショットを示しています、**ビルド アクション**ときファイルに表示されるオプションは、右クリックします。

 [![](image-images/image30a.png "[ビルド アクション] メニュー")](image-images/image30a.png#lightbox)

Visual Studio for Mac は通常、正しい選択**ビルド アクション**自動的に注意してください、これらの設定ファイルをプロジェクトに移動する場合に特にがします。

### <a name="adding-an-image-file"></a>イメージ ファイルを追加します。

プロジェクトにイメージ ファイルを追加するにまずプロジェクトを右クリックして、**ファイルを追加しています.**

 [![](image-images/image31a.png "ファイルを [追加] メニュー")](image-images/image31a.png#lightbox)

選択 (イメージ) の標準ファイル ダイアログに含めるしたいです。 イメージの既定のビルド アクション**BundleResource** – 特定の理由がない限り、この値を上書きしません。

 [![](image-images/image32a.png "追加ファイル ダイアログ ボックス")](image-images/image32a.png#lightbox)

イメージとが読み込まれ、コードに表示される使用可能なプロジェクトに追加されます。 このスクリーン ショットは、iOS アプリケーション プロジェクトに追加されたイメージを示しています。

 [![](image-images/image33a.png "プロジェクト内のイメージ")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>リソース ディレクトリとは何ですか。

リソース ディレクトリに配置されたファイルは通常のファイルから扱い: リソース フォルダーの内容が、アプリケーションのルートにコピーされ、コード内から参照できます。 これは、役に立ちますさまざまな理由。

-  起動時の既定のイメージとアプリケーションのアイコンなどのアプリケーションのプロパティで構成されているイメージを保存します。
-  その他のイメージと、コードから個別にファイルを格納するためが容易になる管理 (サブディレクトリは、リソース ディレクトリの内容がコピーされるときに保持されます)。


コードでは、それらのイメージは、イメージ、サウンド、ビデオ、XML、またはその他の必要な共有コード ライブラリを記述しやすく、処理を行うアプリケーションのルートにコピーすると想定できますので、リソース ディレクトリは、ライブラリ プロジェクトで特に便利ファイル。



リソース ディレクトリ必要がありますので名前を付けるし、すべてのファイルがビルド アクションに設定を持つ必要があります**BundleResource**

## <a name="displaying-the-image"></a>イメージを表示します。

デザイナーを使用してイメージを表示するには、イメージ ビューはコンテナーとして使用する必要があり、1 つのイメージまたはイメージのアニメーションを表示できます。 **イメージ ビュー**ツールボックスからアイコンを次に示します。

 [![](image-images/image35a.png "ツールボックスに ImageView")](image-images/image35.png#lightbox)

ドラッグ、**ビューのイメージ**から、 **Toobox**ビュー コント ローラーにします。 * * イメージ ビュー > イメージ * *、ドロップダウン リストは、プロジェクト内のすべての使用可能なイメージ ファイルの一覧を提供します。 イメージのビューに追加する次のいずれかを選択します。

 [![](image-images/image36a.png "ツールボックスに ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>イメージをプログラムで表示します。

Blocks.jpg リソース ディレクトリのルートにあるために、アプリケーション バンドルのルートで実行時に使用がなります。 ImageView でこのイメージを表示するのには、コントロールは、次のコードを使用します。

```csharp
imageview1.Image = UIImage.FromBundle ("SF Monkey.png");
```

内のイメージを配置お場合`/Resources/Pics/blocks.jpg`コードは、パスに写真のフォルダーを追加してから。

```csharp
imageview1.Image = UIImage.FromBundle ("Pics/SF Monkey.png");
```

リソース ファイルを参照しないを含める必要、`Resources`フォルダーです。


## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
