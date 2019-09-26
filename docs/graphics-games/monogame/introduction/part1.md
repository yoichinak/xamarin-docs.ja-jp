---
title: パート 1-クロスプラットフォームのモノゲームの作成
description: このチュートリアルでは、モノゲームを使用して iOS および Android 用の新しいプロジェクトを作成する方法について説明します。 結果として、クロスプラットフォームの共有コードプロジェクトに加えて、プラットフォームごとに1つのプロジェクトを含む Visual Studio for Mac ソリューションが得られます。 このプロジェクトでは、実行時に空の青い画面が表示されます。
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: d72c428bb4b8c88365180c5c3c50b107eed2b21d
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "68978453"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>パート 1-クロスプラットフォームのモノゲームの作成

_このチュートリアルでは、モノゲームを使用して iOS および Android 用の新しいプロジェクトを作成する方法について説明します。結果として、クロスプラットフォームの共有コードプロジェクトに加えて、プラットフォームごとに1つのプロジェクトを含む Visual Studio for Mac ソリューションが得られます。このプロジェクトでは、実行時に空の青い画面が表示されます。_

モノゲームでは、コードを再利用する大部分でクロスプラットフォームのゲームを開発できます。 このチュートリアルでは、iOS および Android のプロジェクトと、クロスプラットフォームコード用の共有コードプロジェクトを含むソリューションの設定に焦点を当てます。

完了すると、プロジェクトには、ゲーム更新ロジックとゲーム描画ロジックを30フレーム/秒で実行するための適切な構造があります。 これは、任意のモノゲームプロジェクトの基本プロジェクトとして使用できます。 実行すると、プロジェクトは次のようになります。

![空のブルースクリーン](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio"></a>Visual Studio にモノゲームを追加する

> [!IMPORTANT]
> 既定では、Visual Studio 2019 または Visual Studio for Mac には、モノゲームがインストールされていません。
>
> から最新バージョンを手動でダウンロードしてインストール http://www.monogame.net/downloads/ してから、インストーラーを実行する必要があります。 テンプレートを表示するには、Visual Studio を再起動する必要がある場合があります。
>
> 次に、 **[ゲーム開発]** セクションが**アドインマネージャー**に表示されます。

Visual Studio for Mac 用のモノゲームアドインを有効にするには、[ **Visual Studio for Mac** > **アドインマネージャー** ] を選択します。 Windows 上の Visual Studio 2019 の場合は、[**ツール** > ] **[アドインマネージャー]** を選択します。 **[ギャラリー]** タブを選択し、 **[Game Development]** カテゴリを展開して **[モノ game Addin]** を選択し、 **[Install...]** をクリックします。

![Visual Studio for Mac 拡張機能ギャラリーモノゲームの選択](part1-images/image2.png)

インストールが完了すると、次のセクションで説明するように、モノゲームテンプレートが Visual Studio for Mac に表示されます。

## <a name="creating-a-new-solution"></a>新しいソリューションの作成

Visual Studio for Mac **ファイル > 新しいソリューション**を選択します。 **[新しいプロジェクト]** ダイアログボックスで、 **[その他**] をクリックし、 **[全般]** セクションまでスクロールして **[Universal モノ game Mobile application]** オプションを選択し、[次へ] をクリックします。

![新しいプロジェクトダイアログモノのゲームアプリケーションを作成する](part1-images/image3.png)

プロジェクトに "プレイ" という名前を指定し、[作成] をクリックします。

![[新しいプロジェクト] ダイアログの名前と場所の選択](part1-images/image4.png)

これで、プロジェクトは他の iOS または Android プロジェクトと同様に実行されるようになりました。 プロジェクトでは、コーンフラワー ブルー背景を表示するを実行する必要があります。

![空の blue アプリの背景](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android コンパイルエラーの修正

現在のバージョンのモノゲームテンプレートには、Android の`Activity1.cs`ファイルに構文エラーがいくつか含まれています。 これらの問題を解決するに`OnCreate`は、関数を次のように置き換えます。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>まとめ

このチュートリアルでは、Visual Studio for Mac を使用してクロスプラットフォームのモノゲームプロジェクトを作成する方法について説明します。 この結果は、空の青い画面になります。 このプロジェクトは、iOS および Android ゲームの開始点として使用できます。

## <a name="related-links"></a>関連リンク

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [モノゲーム iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [モノゲーム API ドキュメント](http://www.monogame.net/documentation/?page=main)
