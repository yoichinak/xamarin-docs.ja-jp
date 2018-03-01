---
title: "アプリケーションの基礎"
description: "Xamarin.Forms 開発の基礎を調べる"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: afa3bf25b1448d98c49c95a66bd0f4dc55bde39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>アプリケーションの基礎

## <a name="accessibilityaccessibilityindexmd"></a>[ユーザー補助](accessibility/index.md)

Xamarin.Forms を使用したアクセスなどの機能 (画面閲覧ツールのサポート) を組み込むためのヒントします。

## <a name="app-classapplication-classmd"></a>[App クラス](application-class.md)

`Application`クラスは、Xamarin.Forms の開始位置 – サブクラスを実装する必要があるすべてのアプリに`App`を最初のページを設定します。 用意されています、`Properties`単純なデータ記憶域のコレクション。 これは、C# コードか XAML のいずれかで定義できます。

## <a name="app-lifecycleapp-lifecyclemd"></a>[アプリのライフ サイクル](app-lifecycle.md)

`Application`クラス`OnStart`、 `OnSleep`、および`OnResume`モーダル ナビゲーション イベントと同様に、メソッドを使用するカスタム コードでのアプリケーション ライフ サイクル イベントを処理します。

## <a name="behaviorsbehaviorsindexmd"></a>[ビヘイビアー](behaviors/index.md)

ユーザー インターフェイス コントロールは、サブクラス化なしで簡単に機能を追加するビヘイビアーを使用して拡張できます。

## <a name="custom-rendererscustom-rendererindexmd"></a>[カスタム レンダラー](custom-renderer/index.md)

カスタム レンダラーとしては、開発者の外観と (必要な場合は、ネイティブの Sdk を使用して)、各プラットフォームで動作をカスタマイズする Xamarin.Forms コントロールの既定のレンダリングを ' override' を使用できます。

## <a name="data-bindingdata-bindingindexmd"></a>[データ バインディング](data-binding/index.md)

データ バインディングは、その他のプロパティで自動的に反映されるまでに 1 つのプロパティで変更を許可する 2 つのオブジェクトのプロパティをリンクします。 データ バインディングは、モデル ビュー ViewModel の不可欠な部分 ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) アプリケーションのアーキテクチャです。

## <a name="dependency-servicedependency-serviceindexmd"></a>[依存関係サービス](dependency-service/index.md)

`DependencyService`共有コードでコードのインターフェイスをしたり、Xamarin.Forms でプラットフォーム固有の機能を参照するは容易になりますが自動的に解決されるプラットフォーム固有の実装を提供できるように、単純なロケーターを提供します。

## <a name="effectseffectsindexmd"></a>[効果](effects/index.md)

エフェクトをカスタマイズするには、各プラットフォームでネイティブ コントロールは、小規模のスタイル設定の変更に通常使用されます。

## <a name="gesturesgesturesindexmd"></a>[ジェスチャ](gestures/index.md)

Xamarin.Forms [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)クラスがユーザー インターフェイス コントロールで tap、ピンチ、およびパン ジェスチャをサポートしています。

## <a name="localizationlocalizationmd"></a>[ローカリゼーション](localization.md)

組み込みの .NET のローカリゼーション フレームワークは、Xamarin.Forms を使用したクロスプラット フォームの多言語アプリケーションの構築に使用できます。

## <a name="local-databasesdatabasesmd"></a>[ローカル データベース](databases.md)

Xamarin.Forms では、読み込みと共有コードでオブジェクトを保存できるようになります、SQLite データベース エンジンを使用してデータベースに基づくアプリケーションをサポートします。

## <a name="messaging-centermessaging-centermd"></a>[メッセージ センター](messaging-center.md)

Xamarin.Forms`MessagingCenter`によりモデルと単純なメッセージ コントラクトだけでなく相互についての情報を知らなくてもと通信するには、その他のコンポーネントを表示します。

## <a name="navigationnavigationindexmd"></a>[ナビゲーション](navigation/index.md)

Xamarin.Forms 数によっては、別のページ ナビゲーションのエクスペリエンスを提供する、`Page`使用されているを入力します。

## <a name="templatestemplatesindexmd"></a>[テンプレート](templates/index.md)

コントロール テンプレートでは、データ テンプレートがサポートされているコントロールのデータの表示を定義する機能を提供中に、簡単にテーマをテーマ re する機能が実行時に、アプリケーション ページに提供します。

## <a name="triggerstriggersmd"></a>[トリガー](triggers.md)

プロパティの変更と XAML イベントに応答することにより、コントロールを更新します。


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
