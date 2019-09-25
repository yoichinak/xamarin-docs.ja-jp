---
title: Xamarin.Forms とは
description: この記事では、Xamarin のフォームと関連ライブラリについて説明します。
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 09/18/2019
ms.openlocfilehash: 8bd530e743330ab8058f13cb7ba09f95a30ee886
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256607"
---
# <a name="what-is-xamarinforms"></a>Xamarin.Forms とは

[![IOS および Android での Xamarin. Forms アプリケーションの例のスクリーンショット](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin. Forms はオープンソースの UI フレームワークです。 Xamarin を使用すると、開発者は1つの共有コードベースから Android、iOS、および Windows アプリケーションを作成できます。

Xamarin を使用すると、開発者はのC#分離コードを使用して XAML でユーザーインターフェイスを作成できます。 これらのインターフェイスは、各プラットフォームでパフォーマンスの高いネイティブコントロールとしてレンダリングされます。

## <a name="who-xamarinforms-is-for"></a>Xamarin. Forms の対象

Xamarin. フォームは、次のような目標を持つ開発者を対象としています。

- 複数のプラットフォームで UI レイアウトと設計を共有します。
- プラットフォーム間でコード、テスト、ビジネスロジックを共有します。
- Visual Studio でクロスプラットフォームアプリC#を作成します。

## <a name="how-xamarinforms-works"></a>Xamarin. Forms のしくみ

![Xamarin. フォームアーキテクチャダイアグラム](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin. Forms は、プラットフォーム間で UI 要素を作成するための一貫した API を提供します。 この API は、XAML またはC#のいずれかで実装でき、モデルビューモデル (MVVM) などのパターンのデータバインドをサポートします。

実行時には、Xamarin. Forms はプラットフォームレンダラーを利用し、クロスプラットフォーム UI 要素を Android、iOS、UWP のネイティブコントロールに変換します。 を使用すると、開発者は、プラットフォーム間でのコード共有の利点を実感しながら、ネイティブの外観、感覚、パフォーマンスを得ることができます。

通常、Xamarin アプリケーションは、共有 .NET Standard ライブラリと個々のプラットフォームプロジェクトで構成されます。 共有ライブラリには、XAML またC#はビュー、およびサービス、モデル、またはその他のコードなどのビジネスロジックが含まれています。 プラットフォームプロジェクトには、アプリケーションが必要とするプラットフォーム固有のロジックまたはパッケージが含まれています。

Xamarin は Xamarin を使用して、プラットフォーム間でネイティブに .NET アプリケーションを実行します。 Xamarin の詳細については、「 [xamarin とは](~/get-started/what-is-xamarin.md)」を参照してください。

## <a name="additional-tools"></a>その他のツール

Xamarin. Forms には、さまざまな機能をアプリケーションに追加する NuGet パッケージの大規模なエコシステムがあります。 このセクションでは、一般的に使用されるいくつかの NuGet パッケージについて説明します。

### <a name="xamarinessentials"></a>Xamarin.Essentials

Xamarin. Essentials は、ネイティブデバイス機能用のクロスプラットフォーム Api を提供するライブラリです。 Xamarin 自体と同様に、Xamarin. Essentials は、ネイティブユーティリティへのアクセスプロセスを簡略化する抽象化です。 Xamarin. Essentials に用意されているユーティリティの例を次に示します。

- デバイス情報
- ファイル システム
- 加速度計
- ダイヤラー
- 音声合成
- 画面のロック

詳細については、「 [Xamarin. Essentials](~/essentials/index.md)」を参照してください。

### <a name="shell"></a>Shell

Xamarin では、ほとんどのアプリケーションで必要とされる基本的な機能を提供することで、モバイルアプリケーション開発の複雑さが軽減されます。 シェルで提供される機能の例としては、次のようなものがあります。

- 一般的なナビゲーションエクスペリエンス
- URI ベースのナビゲーションスキーム
- 統合された検索ハンドラー

詳細については、「 [Xamarin. フォームシェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

### <a name="platform-specifics"></a>プラットフォームの詳細

Xamarin には、プラットフォーム間でネイティブコントロールをレンダリングする共通の API が用意されていますが、特定のプラットフォームには、他のプラットフォームには存在しない機能がある場合があります。 たとえば、Android プラットフォームには、での高速スクロール用のネイティブ`ListView`な機能がありますが、iOS では使用できません。 Xamarin. Forms platform-詳細を使用すると、カスタムレンダラーや特殊効果を作成せずに、特定のプラットフォームでのみ使用できる機能を利用できます。

Xamarin. Forms には、プラットフォーム固有のさまざまな機能用に構築済みのソリューションが含まれています。 詳細については次を参照してください:

- [Xamarin. Forms プラットフォーム-詳細](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android プラットフォーム-詳細](~/xamarin-forms/platform/android/index.md)
- [iOS プラットフォーム-詳細](~/xamarin-forms/platform/ios/index.md)
- [Windows プラットフォーム-詳細](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>素材のビジュアル

Xamarin. Forms Material ビジュアルは、マテリアルデザインルールを Xamarin アプリケーションに適用するために使用されます。 Xamarin. Forms Material ビジュアルでは、ビジュアルプロパティを使用して、カスタムレンダラーを UI に選択的に適用します。これにより、iOS と Android 全体で一貫したルックアンドフィールを使用してアプリケーションを作成できます。

詳細については、「 [Xamarin. Forms Material ビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin を使ってみる](~/xamarin-forms/index.yml)
- [Xamarin.Essentials](~/essentials/index.md)
- [Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin. フォーム素材ビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)
