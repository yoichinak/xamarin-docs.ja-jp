---
title: "クロス プラットフォーム MonoGame を作成する – 第 1 部"
description: "このチュートリアルでは、iOS および Android MonoGame を使用して新しいプロジェクトを作成する方法を示します。 結果は、Mac ソリューション、プラットフォームごとに 1 つのプロジェクトと同様に、プラットフォーム間の共有コード プロジェクトの Visual Studio です。 このプロジェクトの実行時に空の青い画面が表示されます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 0d1352b4129dc1cf8be42e813787b9b73f80cd3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>クロス プラットフォーム MonoGame を作成する – 第 1 部

_このチュートリアルでは、iOS および Android MonoGame を使用して新しいプロジェクトを作成する方法を示します。結果は、Mac ソリューション、プラットフォームごとに 1 つのプロジェクトと同様に、プラットフォーム間の共有コード プロジェクトの Visual Studio です。このプロジェクトの実行時に空の青い画面が表示されます。_

MonoGame は、コードの再利用の大部分を使用したクロスプラット フォームのゲームの開発を使用できます。 このチュートリアルは、iOS と Android 用のプロジェクトだけでなくクロスプラット フォーム コードの共有コード プロジェクトを含むソリューションを設定する方法は説明します。

完了したら、ゲームの更新ロジックを実行するための適切な構造を持つプロジェクトがおされゲームのロジックを 1 秒あたり 30 フレームで描画します。 これは、すべて MonoGame プロジェクトの基本プロジェクトとして使用できます。 実行時に、プロジェクトは次のようになります。

![](part1-images/image1.png "プロジェクトは実行されたときに次のようになります")


# <a name="adding-monogame-to-visual-studio-for-mac"></a>Visual studio for Mac MonoGame を追加します。

MonoGame として追加できる追加の Visual studio for mac Mac で次のように選択します**Visual Studio for Mac** > **アドイン マネージャー.。** . Windows では、ツールを選択 * * * * >**アドイン マネージャー.** . 選択、**ギャラリー** タブで、展開、**ゲーム開発**カテゴリを選択**MonoGame Addin**をクリックし、**インストール**:

![](part1-images/image2.png "ギャラリータブを選択、ゲーム開発のカテゴリを展開し、MonoGame アドインを選択し、[インストール] をクリックしてください")

> [!IMPORTANT]
> **注**: 場合、**ゲーム開発**セクションがない、アドイン マネージャーから、手動でダウンロードし、ここから最新バージョンをインストールできます: http://www.monogame.net/downloads/ です。 表示されるテンプレート用に Mac を Visual Studio を再起動する必要があります。



インストールされると、MonoGame テンプレートに表示されます Visual Studio for Mac, ように、次のセクションが表示されます。


# <a name="creating-a-new-solution"></a>新しいソリューションの作成

Mac を Visual Studio で**ファイル > 新しいソリューション**です。 **新しいプロジェクト**ダイアログ ボックスで、をクリック**[その他]**、までスクロール、**全般** セクションで、選択、* * ユニバーサル MonoGame モバイル アプリケーション * * オンにして、[次へ] をクリックします。

![](part1-images/image3.png "新しいプロジェクトダイアログ ボックスで [その他] をクリックしてください、ユニバーサル MonoGame モバイル アプリケーション オプションを [全般] セクションまでスクロールして [次へ] をクリックしてください")

WalkingGame プロジェクトの名前し、[作成] をクリックします。

![](part1-images/image4.png "WalkingGame プロジェクトの名前を指定し、[作成] をクリックしてください")

これで、プロジェクトは、他の iOS または Android のプロジェクトと同じように実行されます。 プロジェクトでは、コーンフラワー ブルー背景を表示するを実行する必要があります。

![](part1-images/image5.png "コーンフラワー ブルー背景を表示するプロジェクトを実行する必要があります。")


# <a name="fixing-android-compile-errors"></a>Android のコンパイル エラーを修正します。

MonoGame のテンプレートの現在のバージョンには、android のいくつかの構文エラーが含まれています`Activity1.cs`ファイル。 これらの問題を修正するのには、置換、`OnCreate`を次の関数。


```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```


# <a name="summary"></a>まとめ

このチュートリアルは、for mac Visual Studio を使用してクロスプラット フォーム MonoGame プロジェクトを作成する方法を説明しました この結果は、空のブルー スクリーンです。 このプロジェクトは、iOS または Android ゲームの開始点として使用できます。

## <a name="related-links"></a>関連リンク

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API ドキュメント](http://www.monogame.net/documentation/?page=main)
