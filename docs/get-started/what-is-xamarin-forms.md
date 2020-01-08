---
title: Xamarinとは何ですか。形態?
description: この記事では、Xamarinについて説明します。フォームと関連ライブラリ。
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 09/18/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: c217acdc3bd5c007822cc29fc730df99e373ba01
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607842"
---
# <a name="what-is-opno-locxamarinforms"></a>Xamarinとは何ですか。形態?

[![スクリーンショット (例) [!ファンド.なし (Xamarin)]。IOS と Android のフォームアプリケーション](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin。フォームは、オープンソースの UI フレームワークです。 Xamarin。フォームを使用すると、開発者は1つの共有コードベースから Android、iOS、および Windows アプリケーションを作成できます。

Xamarin。フォームを使用すると、開発者はのC#分離コードを使用して XAML でユーザーインターフェイスを作成できます。 これらのインターフェイスは、各プラットフォームでパフォーマンスの高いネイティブコントロールとしてレンダリングされます。

## <a name="who-opno-locxamarinforms-is-for"></a>Xamarinします。フォームの対象

Xamarin。フォームは、次のような目標を持つ開発者を対象としています。

- 複数のプラットフォームで UI レイアウトと設計を共有します。
- プラットフォーム間でコード、テスト、ビジネスロジックを共有します。
- Visual Studio でクロスプラットフォームアプリC#を作成します。

## <a name="how-opno-locxamarinforms-works"></a>Xamarin方法。フォームの動作

![[!ファンド.なし (Xamarin)]。フォームアーキテクチャの図](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin。フォームは、プラットフォーム間で UI 要素を作成するための一貫した API を提供します。 この API は、XAML またはC#のいずれかで実装でき、モデルビューモデル (MVVM) などのパターンのデータバインドをサポートします。

実行時に Xamarinます。フォームでは、プラットフォームレンダラーを利用して、クロスプラットフォームの UI 要素を Android、iOS、UWP のネイティブコントロールに変換します。 を使用すると、開発者は、プラットフォーム間でのコード共有の利点を実感しながら、ネイティブの外観、感覚、パフォーマンスを得ることができます。

Xamarin。通常、フォームアプリケーションは、共有 .NET Standard ライブラリと個々のプラットフォームプロジェクトで構成されます。 共有ライブラリには、XAML またC#はビュー、およびサービス、モデル、またはその他のコードなどのビジネスロジックが含まれています。 プラットフォームプロジェクトには、アプリケーションが必要とするプラットフォーム固有のロジックまたはパッケージが含まれています。

Xamarin。フォームは、Xamarin を使用して、プラットフォーム間でネイティブに .NET アプリケーションを実行します。 Xamarinの詳細については、「 [Xamarinとは](~/get-started/what-is-xamarin.md)」を参照してください。

## <a name="additional-tools"></a>その他のツール

Xamarin。フォームには、アプリケーションにさまざまな機能を追加する NuGet パッケージの大規模なエコシステムがあります。 このセクションでは、一般的に使用されるいくつかの NuGet パッケージについて説明します。

### <a name="opno-locxamarinessentials"></a>Xamarin。基本

Xamarin。Essentials は、ネイティブデバイス機能用のクロスプラットフォーム Api を提供するライブラリです。 Xamarin 自体と同様に、Xamarinます。Essentials は、ネイティブユーティリティにアクセスするプロセスを簡略化する抽象化です。 Xamarinによって提供されるユーティリティの例をいくつか紹介します。要点は次のとおりです。

- デバイス情報
- ファイル システム
- 加速度計
- ダイヤラー
- 音声合成
- 画面のロック

詳細については、「Xamarin」を参照してください[。要点](~/essentials/index.md)。

### <a name="shell"></a>Shell

Xamarin。フォームシェルは、ほとんどのアプリケーションで必要とされる基本的な機能を提供することで、モバイルアプリケーション開発の複雑さを軽減します。 シェルで提供される機能の例としては、次のようなものがあります。

- 一般的なナビゲーションエクスペリエンス
- URI ベースのナビゲーションスキーム
- 統合された検索ハンドラー

詳細については、「Xamarin」を参照してください[。フォームシェル](~/xamarin-forms/app-fundamentals/shell/index.md)

### <a name="platform-specifics"></a>プラットフォームの詳細

Xamarin。フォームには、プラットフォーム間でネイティブコントロールをレンダリングする共通の API が用意されていますが、特定のプラットフォームには、他のプラットフォームには存在しない機能がある場合があります。 たとえば、Android プラットフォームには `ListView` の高速スクロール用のネイティブな機能がありますが、iOS では使用できません。 Xamarin。フォームプラットフォームの詳細を使用すると、カスタムレンダラーや特殊効果を作成せずに、特定のプラットフォームでのみ使用できる機能を利用できます。

Xamarin。フォームには、プラットフォーム固有のさまざまな機能用に構築済みのソリューションが含まれています。 詳細については、次のトピックを参照してください。

- [Xamarin。フォームプラットフォーム-詳細](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android プラットフォーム-詳細](~/xamarin-forms/platform/android/index.md)
- [iOS プラットフォーム-詳細](~/xamarin-forms/platform/ios/index.md)
- [Windows プラットフォーム-詳細](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>素材のビジュアル

Xamarin。フォームマテリアルビジュアルは、Xamarinにマテリアルデザインルールを適用するために使用されます。フォームアプリケーション。 Xamarin。フォームマテリアルビジュアルでは、ビジュアルプロパティを使用して、カスタムレンダラーを UI に選択的に適用します。これにより、iOS と Android で一貫したルックアンドフィールを使用してアプリケーションを作成できます。

詳細については、「Xamarin」を参照してください[。フォーム素材ビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)

## <a name="related-links"></a>関連リンク

- [Xamarinの使用を開始します。形態](~/xamarin-forms/index.yml)
- [Xamarin。基本](~/essentials/index.md)
- [Xamarin。フォームシェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin。フォーム素材ビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)
