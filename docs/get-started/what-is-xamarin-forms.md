---
title: Xamarin.Forms の概要
description: この記事では、Xamarin.Forms と関連ライブラリについて紹介します。
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 05/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eb76da9be7fcb227c465c0a046b967b2f70b1cfb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198304"
---
# <a name="what-is-xamarinforms"></a>Xamarin.Forms の概要

[![iOS および Android での Xamarin.Forms サンプル アプリケーションのスクリーンショット](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin.Forms は、オープンソースの UI フレームワークです。 Xamarin.Forms を使用すると、開発者は 1 つの共有コードベースから Xamarin.Android、Xamarin.iOS、Windows のアプリケーションを作成できます。

Xamarin.Forms を使用すると、開発者は C# でコードビハインドを使用して、ユーザー インターフェイスを XAML で作成できます。 これらのインターフェイスは、各プラットフォームでパフォーマンスの高いネイティブ コントロールとしてレンダリングされます。

## <a name="who-xamarinforms-is-for"></a>Xamarin.Forms の対象者

Xamarin.Forms は、次のような目標を持つ開発者を対象としています。

- プラットフォーム間で UI のレイアウトと設計を共有する。
- プラットフォーム間でコード、テスト、ビジネス ロジックを共有する。
- Visual Studio を使用して C# でクロスプラットフォーム アプリを作成する。

## <a name="how-xamarinforms-works"></a>Xamarin.Forms の動作のしくみ

![Xamarin.Forms アーキテクチャの図](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin.Forms では、複数のプラットフォーム間にわたる UI 要素を作成するための一貫した API が用意されています。 この API は、XAML または C# のいずれかで実装でき、Model-View-ViewModel (MVVM) などのパターンのデータバインドがサポートされています。

Xamarin.Forms ではプラットフォーム レンダラーを利用して、実行時に、クロスプラットフォームの UI 要素が Xamarin.Android、Xamarin.iOS、UWP のネイティブ コントロールに変換されます。 これにより開発者は、プラットフォーム間でのコード共有の恩恵を受けながら、ネイティブなルック アンド フィールとパフォーマンスを得ることができます。

Xamarin.Forms アプリケーションは、通常、.NET Standard の共有ライブラリと個々のプラットフォーム プロジェクトで構成されます。 共有ライブラリには、XAML また C# ビュー、およびサービス、モデル、またはその他のコードなどのビジネス ロジックが含まれています。 プラットフォーム プロジェクトには、アプリケーションが必要とするプラットフォーム固有のロジックまたはパッケージが含まれています。

Xamarin.Forms では Xamarin プラットフォームを使用して、複数のプラットフォームにわたってネイティブに .NET アプリケーションが実行されます。 Xamarin プラットフォームの詳細については、「[Xamarin とは](~/get-started/what-is-xamarin.md)」を参照してください。

## <a name="additional-functionality"></a>その他の機能

Xamarin.Forms には、さまざまな機能をアプリケーションに追加するライブラリの大規模なエコシステムがあります。 このセクションでは、この追加機能の一部を説明します。

### Xamarin.Essentials

Xamarin.Essentials は、ネイティブ デバイスの機能にクロスプラットフォーム API を提供するライブラリです。 Xamarin 自体と同様に、Xamarin.Essentials は、ネイティブ ユーティリティへのアクセス プロセスを簡略化する抽象化です。 以下に、Xamarin.Essentials で提供されているユーティリティの例をいくつか示します。

- デバイス情報
- ファイル システム
- 加速度計
- ダイヤラー
- 音声合成
- 画面のロック

詳細については、「[Xamarin.Essentials](~/essentials/index.md)」を参照してください。

### <a name="shell"></a>Shell

Xamarin.Forms シェルでは、ほとんどのアプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さが軽減されます。 シェルで提供される機能の例としては、次のようなものがあります。

- 共通のナビゲーション エクスペリエンス
- URI ベースのナビゲーション スキーム
- 統合された検索ハンドラー

詳細については、「[Xamarin.Forms のシェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

### <a name="platform-specifics"></a>プラットフォーム固有設定

Xamarin.Forms には、複数のプラットフォームにわたってネイティブ コントロールをレンダリングする共通 API が用意されていますが、特定のプラットフォームに、その他のプラットフォームには存在しない機能がある場合があります。 たとえば、Android プラットフォームには `ListView` で高速スクロール用のネイティブ機能がありますが、iOS にはありません。 Xamarin.Forms プラットフォーム固有設定により、カスタムのレンダラーやエフェクトを作成しなくても、特定のプラットフォームでのみ使用できる機能を利用できます。

Xamarin.Forms には、さまざまなプラットフォーム固有設定の機能に向けた、事前構築済みのソリューションが含まれています。 詳細については次を参照してください:

- [Xamarin.Forms プラットフォーム固有設定](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android プラットフォーム固有設定](~/xamarin-forms/platform/android/index.md)
- [iOS プラットフォーム固有設定](~/xamarin-forms/platform/ios/index.md)
- [Windows プラットフォーム固有設定](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>素材のビジュアル

Xamarin.Forms の素材のビジュアルは、素材のデザイン ルールを Xamarin.Forms アプリケーションに適用するために使用されます。 Xamarin.Forms の素材のビジュアルでは、ビジュアル プロパティを使用して、カスタム レンダラーを UI に選択的に適用します。これによりアプリケーションが、iOS と Android 間で一貫性のあるルック アンド フィールになります。

詳細については、「[Xamarin.Forms の素材のビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)」を参照してください

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/index.yml)
- [Xamarin.Essentials](~/essentials/index.md)
- [Xamarin.Forms のシェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms の素材のビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)
