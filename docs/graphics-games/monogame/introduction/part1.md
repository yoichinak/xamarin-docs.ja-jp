---
title: パート 1-クロスプラット フォーム MonoGame の作成
description: このチュートリアルでは、iOS と Android の MonoGame を使用して新しいプロジェクトを作成する方法を示します。 Visual Studio for Mac ソリューション、プラットフォームごとに 1 つのプロジェクトと同様に、クロス プラットフォームの共有コード プロジェクトになります。 このプロジェクトの実行時に空の青い画面が表示されます。
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: c8ef426c742f875e26fc0fcf88a9468e1618e30f
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832525"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>パート 1-クロスプラット フォーム MonoGame の作成

_このチュートリアルでは、iOS と Android の MonoGame を使用して新しいプロジェクトを作成する方法を示します。Visual Studio for Mac ソリューション、プラットフォームごとに 1 つのプロジェクトと同様に、クロス プラットフォームの共有コード プロジェクトになります。このプロジェクトの実行時に空の青い画面が表示されます。_

MonoGame では、コードの再利用の大部分を使用したクロス プラットフォーム ゲームの開発を使用できます。 このチュートリアルに焦点を iOS と Android でのプロジェクトとクロスプラット フォーム コードの共有コード プロジェクトを含むソリューションを設定します。

終わったら、ゲームの更新ロジックを実行するための適切な構造を持つプロジェクトし、ゲームのロジックを 1 秒あたり 30 フレームで描画なります。 任意の MonoGame プロジェクト ベースのプロジェクトとして使用できます。 実行時に、プロジェクトは次のようになります。

![空のブルー スクリーン](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Visual Studio for Mac を追加する MonoGame

MonoGame として追加できる追加の Visual studio for mac。 Mac で次のように選択します**Visual Studio for Mac** > **アドイン マネージャー...** . Windows では、ツールを選択 * * * * >**アドイン マネージャー...** . 選択、**ギャラリー** タブで、展開、**ゲーム開発**カテゴリを選択**MonoGame Addin**をクリックし、**インストール**:

![MonoGame を選択すると、Mac の拡張機能ギャラリーの visual Studio](part1-images/image2.png)

> [!IMPORTANT]
> 場合、**ゲーム開発**アドイン マネージャーにセクションがない場合、手動でダウンロードし、ここから最新のバージョンをインストールすることができます: http://www.monogame.net/downloads/ します。 Visual Studio に表示されるテンプレート用の Mac を再起動する必要があります。

インストールされると、次のセクションで紹介するよう MonoGame テンプレートは for Mac、Visual Studio で表示します。

## <a name="creating-a-new-solution"></a>新しいソリューションを作成します。

Visual studio for Mac 選択**ファイル > 新しいソリューション**します。 **新しいプロジェクト**ダイアログ ボックスで、をクリック **[その他]** 、までスクロール、**全般** セクションで、選択、* * ユニバーサル MonoGame モバイル アプリケーション * * オンにして、[次へ] をクリックします。

![MonoGame アプリケーションを作成する新しいプロジェクト ダイアログ ボックス](part1-images/image3.png)

プロジェクト WalkingGame の名前し、作成 をクリックします。

![名前と場所を選択する新しいプロジェクト ダイアログ ボックス](part1-images/image4.png)

今すぐプロジェクトは、他の iOS または Android のプロジェクトと同じよう実行されます。 プロジェクトでは、コーンフラワー ブルー背景を表示するを実行する必要があります。

![青色空のアプリのバック グラウンド](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android のコンパイル エラーを修正

MonoGame のテンプレートの現在のバージョンでは、いくつかの構文エラーを含む、android の`Activity1.cs`ファイル。 これらの問題を解決するには、置換、`OnCreate`を次の関数。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>Summary

このチュートリアルは、Visual Studio for mac を使用、クロスプラット フォーム MonoGame プロジェクトを作成する方法を説明しました この結果は、空のブルー スクリーンです。 このプロジェクトは、iOS または Android ゲームの開始点として使用できます。

## <a name="related-links"></a>関連リンク

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API ドキュメント](http://www.monogame.net/documentation/?page=main)
