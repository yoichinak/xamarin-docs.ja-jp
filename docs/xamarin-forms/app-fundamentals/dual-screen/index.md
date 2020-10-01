---
title: Xamarin.Forms のデュアル画面
description: このガイドでは、デュアル画面デバイス用の Xamarin.Forms アプリを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e1d2a443a6005050c518e21e4e0f2df64c2aab0c
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562627"
---
# <a name="no-locxamarinforms-dual-screen"></a>Xamarin.Forms のデュアル画面

Microsoft Surface Duo のようなデュアル画面デバイスでは、アプリケーションの新しいユーザー体験が促進されます。 Xamarin.Forms には `TwoPaneView` クラスと `DualScreenInfo` クラスが含まれているため、デュアル画面デバイス用のアプリを開発できます。

## <a name="get-started"></a>作業開始

以下の手順に従い、デュアル画面機能を Xamarin.Forms アプリに追加します。

1. ソリューションの **[NuGet パッケージ マネージャー]** ダイアログを開きます。
2. **[参照]** タブで `Xamarin.Forms.DualScreen` を検索します。
3. ソリューションに `Xamarin.Forms.DualScreen` パッケージをインストールします。
4. `OnCreate` イベントで Android プロジェクトの `MainActivity` クラスに次の初期化メソッド呼び出しを追加します。

    ```csharp
    Xamarin.Forms.DualScreen.DualScreenService.Init(this);
    ```

    2 つの画面にまたがるなど、アプリの状態変化をアプリが検出するにはこのメソッドが必要です。

5. これらの `ConfigurationChanges` オプションが "_すべて_" 含まれるよう、Android プロジェクトの `MainActivity` クラスで `Activity` 属性を更新します。

    ```@csharp
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation
        | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize | ConfigChanges.UiMode
    ```

    これらの値は、構成の変更や範囲の状態をより確実に報告できるようにするために必要です。 既定では、2 つだけが Xamarin.Forms プロジェクトに追加されます。そのため、信頼できるデュアル画面サポートのために残りを忘れずに追加してください。

## <a name="troubleshooting"></a>トラブルシューティング

`DualScreenInfo` クラスまたは `TwoPaneView` レイアウトが想定どおりに動作しない場合、このページのセットアップ手順を再確認してください。 `Init` メソッドまたは `ConfigurationChanges` 属性値を省略したり、間違って構成したりすることは、よくあるエラーの原因です。

追加のガイダンスと参照実装が必要であれば、[Xamarin.Forms デュアル画面サンプル](/dual-screen/xamarin/samples)をご覧ください。

## <a name="next-steps"></a>次の手順

NuGet を追加したら、次の手順でアプリにデュアル画面機能を追加します。

- [デュアル画面デザイン パターン](design-patterns.md) - デュアル画面のデバイスで複数の画面を最適に使用する方法を検討する場合は、このパターンに関するガイダンスを参照して、アプリケーション インターフェイスに最適なものを見つけてください。
- [TwoPaneView レイアウト](twopaneview.md) - 同じ名前の UWP コントロールがきっかけとなった Xamarin.Forms の `TwoPaneView` クラスは、デュアル画面デバイス用に最適化されたクロスプラットフォームのレイアウトです。
- [DualScreenInfo ヘルパー クラス](dual-screen-info.md) - `DualScreenInfo` で、表示されるペイン、その大きさ、デバイスがどのようなものか、ヒンジの角度などを決定できるようになります。
- [デュアル画面トリガー](triggers.md) - [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) 名前空間には、アタッチされたレイアウト (ウィンドウ) の表示モードが変更されたときに [`VisualState`](xref:Xamarin.Forms.VisualState) 変更をトリガーする 2 つの状態トリガーが含まれています。

詳細については、[デュアル画面開発者向けのドキュメント](/dual-screen/)を参照してください。