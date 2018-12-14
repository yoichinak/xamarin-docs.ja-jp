---
title: Xamarin.Forms アプリのライフサイクル
description: この記事では、ライフサイクル メソッド、ページ ナビゲーション イベント、モーダル ナビゲーション イベントなど、アプリケーションのライフサイクルに応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 5bdce8d1752b3d7273ffec233b120266909999c4
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675121"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms アプリのライフサイクル

[`Application`](xref:Xamarin.Forms.Application) の基底クラスでは、次の機能が提供されています。

* [ライフサイクル メソッド](#Lifecycle_Methods): `OnStart`、`OnSleep`、`OnResume`。
* [ページ ナビゲーション イベント](#page): [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing)、[`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing)。
* [モーダル ナビゲーション イベント](#modal): `ModalPushing`、`ModalPushed`、`ModalPopping`、`ModalPopped`。

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>ライフサイクル メソッド

[`Application`](xref:Xamarin.Forms.Application) クラスには 3 つの仮想メソッドが含まれており、それをオーバーライドしてライフサイクル メソッドを処理できます。

* **OnStart** - アプリケーションの起動時に呼び出されます。

* **OnSleep** - アプリケーションがバックグラウンドに移行するたびに呼び出されます。

* **OnResume** - バックグラウンドに送られたアプリケーションが再開するたびに呼び出されます。

アプリケーション終了時のメソッドは "*ない*" ことに注意してください。
通常の (つまり、クラッシュではない) 状況では、アプリケーションの終了は *OnSleep* 状態から行われ、コードへの追加の通知はありません。

これらのメソッドが呼び出されるタイミングを確認するには、各メソッドに `WriteLine` の呼び出しを実装し (次の例を参照)、各プラットフォームでテストします。

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

"*以前の*" Xamarin.Forms アプリケーション (たとえば、 Xamarin.Forms 1.3 またはそれ以前で作成されたもの) を更新するときは、Android のメイン アクティビティの `[Activity()]` 属性に `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` が含まれていることを確認してください。 これが存在しない場合は、ローテーションのときと、アプリケーションが最初に起動するときに、`OnStart` メソッドが呼び出されます。 この属性は、現在の Xamarin.Forms アプリ テンプレートには自動的に含まれています。

<a name="page" />

## <a name="page-navigation-events"></a>ページ ナビゲーション イベント

[`Application`](xref:Xamarin.Forms.Application) クラスには、ページの表示と消去の通知を提供する 2 つのイベントがあります。

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) - ページが画面に表示されようとしているときに発生します。
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) - ページが画面から消去されようとしているときに発生します。

画面に表示されるページを追跡する必要があるシナリオで、これらのイベントを使用できます。

> [!NOTE]
> [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) イベントと [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) イベントは、それぞれ [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) イベントと [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) イベントの直後に、[`Page`](xref:Xamarin.Forms.Page) 基底クラスから生成されます。

<a name="modal" />

## <a name="modal-navigation-events"></a>モーダル ナビゲーション イベント

[`Application`](xref:Xamarin.Forms.Application) クラスには、それぞれが独自のイベント引数を持つ 4 つのイベントがあり、それを使用してモーダル ページの表示と消去に応答できます。

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` クラスには `Cancel` プロパティが含まれます。 `Cancel` が `true` に設定されると、モーダル ポップアップは取り消されます。
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> アプリケーション ライフサイクル メソッドとモーダル ナビゲーション イベントを実装するため、Xamarin.Forms アプリの作成で使用されている `Application` より前のすべてのメソッドは (つまり、静的な `GetMainPage` メソッドを使用しているバージョン 1.2 以前で記述されたアプリケーション)、`MainPage` の親として設定される既定の `Application` を作成するように更新されています。
>
> この従来の動作を使用する Xamarin.Forms アプリケーションは、[Application クラス](~/xamarin-forms/app-fundamentals/application-class.md)のページで説明されているように、`Application` サブクラスに更新する必要があります。
