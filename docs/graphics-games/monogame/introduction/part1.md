---
title: クロス プラットフォーム MonoGame を作成する – 第 1 部
description: このチュートリアルでは、iOS および Android MonoGame を使用して新しいプロジェクトを作成する方法を示します。 結果は、Mac ソリューション、プラットフォームごとに 1 つのプロジェクトと同様に、プラットフォーム間の共有コード プロジェクトの Visual Studio です。 このプロジェクトの実行時に空の青い画面が表示されます。
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 1c859c5a8d8c5d8b0539d4158895e816d47d3d5e
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>クロス プラットフォーム MonoGame を作成する – 第 1 部

_このチュートリアルでは、iOS および Android MonoGame を使用して新しいプロジェクトを作成する方法を示します。結果は、Mac ソリューション、プラットフォームごとに 1 つのプロジェクトと同様に、プラットフォーム間の共有コード プロジェクトの Visual Studio です。このプロジェクトの実行時に空の青い画面が表示されます。_

MonoGame は、コードの再利用の大部分を使用したクロスプラット フォームのゲームの開発を使用できます。 このチュートリアルは、iOS と Android 用のプロジェクトだけでなくクロスプラット フォーム コードの共有コード プロジェクトを含むソリューションを設定する方法は説明します。

完了したら、ゲームの更新ロジックを実行するための適切な構造を持つプロジェクトがおされゲームのロジックを 1 秒あたり 30 フレームで描画します。 これは、すべて MonoGame プロジェクトの基本プロジェクトとして使用できます。 実行時に、プロジェクトは次のようになります。

![空白のブルー スクリーン](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Visual studio for Mac MonoGame を追加します。

MonoGame として追加できる追加の Visual studio for mac Mac で次のように選択します**Visual Studio for Mac** > **アドイン マネージャー...** . Windows では、ツールを選択 * * * * >**アドイン マネージャー...** . 選択、**ギャラリー** タブで、展開、**ゲーム開発**カテゴリを選択**MonoGame Addin**をクリックし、**インストール**:

![MonoGame を選択すると、Mac の拡張機能ギャラリー用の visual Studio](part1-images/image2.png)

> [!IMPORTANT]
> **注**: 場合、**ゲーム開発**セクションがない、アドイン マネージャーから、手動でダウンロードし、ここから最新バージョンをインストールできます:http://www.monogame.net/downloads/です。 表示されるテンプレート用に Mac を Visual Studio を再起動する必要があります。

インストールされると、MonoGame テンプレートに表示されます Visual Studio for Mac, ように、次のセクションが表示されます。

## <a name="creating-a-new-solution"></a>新しいソリューションの作成

Mac を Visual Studio で**ファイル > 新しいソリューション**です。 **新しいプロジェクト**ダイアログ ボックスで、をクリック**[その他]**、までスクロール、**全般** セクションで、選択、* * ユニバーサル MonoGame モバイル アプリケーション * * オンにして、[次へ] をクリックします。

![MonoGame アプリケーションを作成する新しいプロジェクト ダイアログ ボックス](part1-images/image3.png)

WalkingGame プロジェクトの名前し、[作成] をクリックします。

![新しいプロジェクト ダイアログ ボックスの名前と場所を選択](part1-images/image4.png)

これで、プロジェクトは、他の iOS または Android のプロジェクトと同じように実行されます。 プロジェクトでは、コーンフラワー ブルー背景を表示するを実行する必要があります。

![青の空のアプリケーションの背景](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android のコンパイル エラーを修正します。

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

## <a name="summary"></a>まとめ

このチュートリアルは、for mac Visual Studio を使用してクロスプラット フォーム MonoGame プロジェクトを作成する方法を説明しました この結果は、空のブルー スクリーンです。 このプロジェクトは、iOS または Android ゲームの開始点として使用できます。

## <a name="related-links"></a>関連リンク

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API ドキュメント](http://www.monogame.net/documentation/?page=main)
