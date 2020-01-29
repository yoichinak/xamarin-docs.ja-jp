---
title: Xamarin.Forms とは
description: この記事では、Xamarin.Forms と関連ライブラリについて説明します。
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 09/18/2019
ms.openlocfilehash: 8bd530e743330ab8058f13cb7ba09f95a30ee886
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "75607842"
---
# <a name="what-is-xamarinforms"></a>Xamarin.Forms とは

[![iOS および Android での Xamarin.Forms のサンプル アプリケーションのスクリーンショット](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin.Forms はオープンソースの UI フレームワークです。 Xamarin.Forms を使用すると、開発者は 1 つの共有コードベースから Android、iOS、Windows のアプリケーションを作成できます。

Xamarin.Forms を使用すると、開発者は C# でコードビハインドを使用して XAML でユーザー インターフェイスを作成できます。 これらのインターフェイスは、各プラットフォームでパフォーマンスの高いネイティブ コントロールとしてレンダリングされます。

## <a name="who-xamarinforms-is-for"></a>Xamarin.Forms の対象者

Xamarin.Forms は、次のような目標を持つ開発者を対象としています。

- プラットフォーム間で UI のレイアウトと設計を共有する。
- プラットフォーム間でコード、テスト、ビジネス ロジックを共有する。
- Visual Studio を使用して C# でクロスプラットフォーム アプリを作成する。

## <a name="how-xamarinforms-works"></a>Xamarin.Forms のしくみ

![Xamarin.Forms アーキテクチャ ダイアグラム](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin.Forms では、プラットフォームを超えて UI 要素を作成するための一貫した API が用意されています。 この API は、XAML または C# のいずれかで実装でき、Model-View-ViewModel (MVVM) などのパターンのデータバインドがサポートされています。

Xamarin.Forms ではプラットフォーム レンダラーを利用して、実行時に、クロスプラットフォーム UI 要素が Android、iOS、UWP のネイティブ コントロールに変換されます。 これにより開発者は、プラットフォーム間でのコード共有の恩恵を受けながら、ネイティブなルック アンド フィールとパフォーマンスを得ることができます。

通常、Xamarin.Forms アプリケーションは、.NET Standard の共有ライブラリと個々のプラットフォーム プロジェクトで構成されます。 共有ライブラリには、XAML また C# ビュー、およびサービス、モデル、またはその他のコードなどのビジネス ロジックが含まれています。 プラットフォーム プロジェクトには、アプリケーションが必要とするプラットフォーム固有のロジックまたはパッケージが含まれています。

Xamarin.Forms では Xamarin を使用して、プラットフォームを超えてネイティブに .NET アプリケーションが実行されます。 Xamarin の詳細については、「[Xamarin とは](~/get-started/what-is-xamarin.md)」を参照してください。

## <a name="additional-tools"></a>その他のツール

Xamarin.Forms には、さまざまな機能をアプリケーションに追加する NuGet パッケージの大規模なエコシステムがあります。 このセクションでは、一般的に使用されるいくつかの NuGet パッケージについて説明します。

### <a name="xamarinessentials"></a>Xamarin.Essentials

Xamarin.Essentials は、ネイティブ デバイスの機能にクロスプラットフォーム API を提供するライブラリです。 Xamarin 自体と同様に、Xamarin.Essentials は、ネイティブ ユーティリティへのアクセス プロセスを簡略化する抽象化です。 Xamarin.Essentials で提供されているユーティリティの例をいくつか次に示します。

- デバイス情報
- ファイル システム
- 加速度計
- ダイヤラー
- 音声合成
- 画面のロック

詳細については、「[Xamarin.Essentials](~/essentials/index.md)」を参照してください。

### <a name="shell"></a>Shell

Xamarin.Forms シェルでは、ほとんどのアプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さを軽減します。 シェルで提供される機能の例としては、次のようなものがあります。

- 共通のナビゲーション エクスペリエンス
- URI ベースのナビゲーション スキーム
- 統合された検索ハンドラー

詳細については、「[Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

### <a name="platform-specifics"></a>プラットフォーム固有設定

Xamarin.Forms には、プラットフォームを超えてネイティブ コントロールをレンダリングする共通の API が用意されていますが、特定のプラットフォームには、他のプラットフォームには存在しない機能がある場合があります。 たとえば、Android プラットフォームには `ListView` で高速スクロール用のネイティブ機能がありますが、iOS にはありません。 Xamarin.Forms プラットフォーム固有設定により、カスタム レンダラーや効果を作成しなくても、特定のプラットフォームでのみ使用可能な機能を利用できます。

Xamarin.Forms には、さまざまなプラットフォーム固有設定の機能向けに、事前に構築されたソリューションが含まれています。 詳細については次を参照してください:

- [Xamarin.Forms プラットフォーム固有設定](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android プラットフォーム固有設定](~/xamarin-forms/platform/android/index.md)
- [iOS プラットフォーム固有設定](~/xamarin-forms/platform/ios/index.md)
- [Windows プラットフォーム固有設定](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>素材のビジュアル

Xamarin.Forms の素材のビジュアルは、素材のデザイン ルールを Xamarin.Forms アプリケーションに適用するために使用されます。 Xamarin.Forms の素材のビジュアルでは、ビジュアル プロパティを使用して、カスタム レンダラーを UI に選択的に適用します。これによりアプリケーションが、iOS と Android 間で一貫性のあるルック アンド フィールになります。

詳細については、「[Xamarin.Forms の素材のビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/index.yml)
- [Xamarin.Essentials](~/essentials/index.md)
- [Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms の素材のビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)
