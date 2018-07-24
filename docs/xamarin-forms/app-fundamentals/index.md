---
title: Xamarin.Forms アプリケーションの基礎
description: アクセシビリティとローカライズなどの最後の仕上げをすべての必要なコア概念をなど、Xamarin.Forms アプリケーションの開発の基礎を調べる。
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 515dbd2683619cfcfb7a6c8ecac6bc147265ef7d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995613"
---
# <a name="xamarinforms-application-fundamentals"></a>Xamarin.Forms アプリケーションの基礎

## <a name="accessibilityaccessibilityindexmd"></a>[ユーザー補助](accessibility/index.md)

Xamarin.Forms でアクセス可能な機能 (画面読み上げツールのサポート) などを組み込むためのヒントします。

## <a name="app-classapplication-classmd"></a>[App クラス](application-class.md)

`Application`クラスは、Xamarin.Forms の開始点、サブクラスを実装する必要があるすべてのアプリ –`App`最初のページを設定します。 用意されています、`Properties`単純なデータ記憶域のコレクション。 これは、c# または XAML で定義できます。

## <a name="app-lifecycleapp-lifecyclemd"></a>[アプリのライフサイクル](app-lifecycle.md)

`Application`クラス`OnStart`、 `OnSleep`、および`OnResume`メソッドと、モーダル ナビゲーション イベントのカスタム コードでのアプリケーション ライフ サイクル イベントを処理できるようにします。

## <a name="behaviorsbehaviorsindexmd"></a>[ビヘイビアー](behaviors/index.md)

ユーザー インターフェイス コントロールは、サブクラス化なしで簡単に機能を追加するビヘイビアーを使用して拡張できます。

## <a name="custom-rendererscustom-rendererindexmd"></a>[カスタム レンダラー](custom-renderer/index.md)

カスタム レンダリングは、開発者の外観と (必要な場合は、ネイティブ Sdk を使用して) 各プラットフォームで動作をカスタマイズする Xamarin.Forms コントロールの既定のレンダリングを ' override' を使用できます。

## <a name="data-bindingdata-bindingindexmd"></a>[データ バインディング](data-binding/index.md)

データ バインディングは、その他のプロパティに自動的に反映されることの 1 つのプロパティに対する変更は許可を 2 つのオブジェクトのプロパティにリンクします。 データ バインディングは、モデル-ビュー-ビューモデルの不可欠な部分 ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) アプリケーションのアーキテクチャ。

## <a name="dependency-servicedependency-serviceindexmd"></a>[依存関係サービス](dependency-service/index.md)

`DependencyService`インターフェイスにコードの共有コードでして、簡単に Xamarin.Forms でプラットフォーム固有の機能を参照することが自動的に解決するプラットフォームに固有の実装を提供できるように、単純なロケーターを提供します。

## <a name="effectseffectsindexmd"></a>[エフェクト](effects/index.md)

カスタマイズするには、各プラットフォームのネイティブ コントロールを許可する効果は通常小さなスタイルの変更の使用とします。

## <a name="filesfilesmd"></a>[ファイル](files.md)

ファイルの処理を Xamarin.Forms では、.NET Standard ライブラリ、または埋め込みリソースを使用してコードを使用して実現できます。

## <a name="gesturesgesturesindexmd"></a>[ジェスチャ](gestures/index.md)

Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer)クラスがユーザー インターフェイス コントロールで tap、ピンチ、およびパン ジェスチャをサポートしています。

## <a name="localizationlocalizationindexmd"></a>[ローカリゼーション](localization/index.md)

Xamarin.Forms によるクロスプラット フォームで多言語アプリケーションを構築する、組み込みの .NET ローカライズ フレームワークを使用できます。

## <a name="local-databasesdatabasesmd"></a>[ローカル データベース](databases.md)

Xamarin.Forms は、負荷を共有コードでオブジェクトを保存することができます、SQLite データベース エンジンを使用してデータベース駆動型アプリケーションをサポートします。

## <a name="messaging-centermessaging-centermd"></a>[メッセージ センター](messaging-center.md)

Xamarin.Forms`MessagingCenter`により表示モデルとその他のコンポーネントを簡単なメッセージ コントラクトだけでなく相互について何も知らなくても通信します。

## <a name="navigationnavigationindexmd"></a>[ナビゲーション](navigation/index.md)

Xamarin.Forms 数に応じて、別のページ ナビゲーション エクスペリエンスを提供する、`Page`使用されているかを入力します。

## <a name="templatestemplatesindexmd"></a>[テンプレート](templates/index.md)

コントロール テンプレートでは、データ テンプレートがサポートされているコントロールのデータのプレゼンテーションを定義する機能を提供中に、簡単にテーマおよび再テーマ設定する機能が、実行時にアプリケーション ページに提供します。

## <a name="triggerstriggersmd"></a>[トリガー](triggers.md)

プロパティの変更と XAML 内のイベントに応答してコントロールを更新します。


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
