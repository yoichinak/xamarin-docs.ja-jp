---
title: Xamarin.Forms のアプリケーションの基礎
description: すべての必須なコア概念を含む Xamarin.Forms アプリケーション開発の基礎について、アクセシビリティやローカライズなどの最後のしあげまで説明する。
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
ms.openlocfilehash: 61d9c5b1c7b81bfc8a20932ce1d188cd0cb0f980
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650438"
---
# <a name="xamarinforms-application-fundamentals"></a>Xamarin.Forms のアプリケーションの基礎

## <a name="accessibilityaccessibilityindexmd"></a>[ユーザー補助](accessibility/index.md)

アクセシビリティに配慮した機能 (画面読み上げツールのサポートなど) と Xamarin.Forms を組み合わせるためのヒントです。

## <a name="app-classapplication-classmd"></a>[App クラス](application-class.md)

`Application` クラスは Xamarin.Forms の開始点です – すべてのアプリでサブクラス `App` を実装して初期ページを設定する必要があります。 これは、単純なデータ ストレージ用の `Properties` コレクションも備えています。 これは C# または XAML のどちらでも定義できます。

## <a name="app-lifecycleapp-lifecyclemd"></a>[アプリのライフサイクル](app-lifecycle.md)

モーダル ナビゲーション イベント、および `Application` クラスの `OnStart`、`OnSleep`、`OnResume` メソッドを使うと、カスタム コードを使ってアプリケーションのライフサイクル イベントを処理できます。

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[アプリケーション インデックス作成とディープ リンク作成](deep-linking.md)

アプリケーション インデックス作成を使用すると、何度か使用した後に忘れてしまいそうなアプリケーションを検索結果に表示することで、関連性を保つことができます。 ディープ リンクの設定を使用すると、アプリケーションのデータが含まれている検索結果にアプリケーションが応答するようにできます。通常の応答では、ディープ リンクから参照されているページに移動します。

## <a name="behaviorsbehaviorsindexmd"></a>[ビヘイビアー](behaviors/index.md)

ビヘイビアーを使って機能を追加することで、ユーザー インターフェイスのコントロールをサブクラス化なしで簡単に拡張できます。

## <a name="custom-rendererscustom-rendererindexmd"></a>[カスタム レンダラー](custom-renderer/index.md)

カスタム レンダリングを使うと、開発者は Xamarin.Forms コントロールの既定のレンダリングを 'オーバーライド' し、各プラットフォーム上でそれぞれの外観とビヘイビアーを (必要な場合はネイティブ SDK を使用して) カスタマイズできます。

## <a name="data-bindingdata-bindingindexmd"></a>[データ バインディング](data-binding/index.md)

データ バインディングは、2 つのオブジェクトのプロパティをリンクして、片方のプロパティへの変更が自動的にもう片方のプロパティに反映されるようにします。 データ バインディングは、Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) アプリケーション アーキテクチャにとって不可欠の部分です。

## <a name="dependency-servicedependency-serviceindexmd"></a>[依存関係サービス](dependency-service/index.md)

`DependencyService` には、ご自身の共有コードのインターフェイスにコードを加え、自動的に解決されるプラットフォーム固有の実装を提供するための単純なロケーターが用意されていて、Xamarin.Forms でプラットフォーム固有の機能を参照しやすくなります。

## <a name="effectseffectsindexmd"></a>[エフェクト](effects/index.md)

効果を使用すると、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。通常は、小規模なスタイル変更のために使用します。

## <a name="gesturesgesturesindexmd"></a>[ジェスチャ](gestures/index.md)

Xamarin.Forms の [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) クラスでは、ユーザー インターフェイス コントロール上でのタップ、ピンチ、およびパンのジェスチャがサポートされています。

## <a name="localizationlocalizationindexmd"></a>[ローカリゼーション](localization/index.md)

組み込みの .NET ローカライズ フレームワークを使って、Xamarin.Forms を使ったクロスプラットフォームの多言語アプリケーションを構築できます。

## <a name="messaging-centermessaging-centermd"></a>[メッセージング センター](messaging-center.md)

Xamarin.Forms の `MessagingCenter` を使うと、ビュー モデルとその他のコンポーネントの通信を、シンプルなメッセージ コントラクト以外は、相互について何も知らなくても実現できます。

## <a name="navigationnavigationindexmd"></a>[ナビゲーション](navigation/index.md)

Xamarin.Forms は、使用している `Page` 型に応じて多数のページ ナビゲーション エクスペリエンスを提供します。

## <a name="shellshellindexmd"></a>[Shell](shell/index.md)

Xamarin.Forms シェルでは、ほとんどのモバイル アプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さを軽減します。 これには、一般的なナビゲーション ユーザー エクスペリエンス、URI ベースのナビゲーション体系、および統合された検索ハンドラーが含まれます。

## <a name="templatestemplatesindexmd"></a>[テンプレート](templates/index.md)

コントロール テンプレートを使うと、実行時にアプリケーション ページを簡単にテーマ設定および再テーマ設定できます。一方、データ テンプレートを使うと、サポートされているコントロール上のデータの表現方法を定義できます。

## <a name="triggerstriggersmd"></a>[トリガー](triggers.md)

XAML でのプロパティの変更とイベントに応答して、コントロールを更新します。
