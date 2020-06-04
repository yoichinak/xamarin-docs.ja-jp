---
title: Xamarin.Forms アプリのライフサイクル
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3793a54f04b2c028752e18e2a5a238c275c2958a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129676"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms アプリのライフサイクル

[`Application`](xref:Xamarin.Forms.Application) の基底クラスでは、次の機能が提供されています。

- [ライフサイクル メソッド](#Lifecycle_Methods) `OnStart`、`OnSleep`、`OnResume`。
- [ページ ナビゲーション イベント](#page) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing)、[`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing)。
- [モーダル ナビゲーション イベント](#modal) `ModalPushing`、`ModalPushed`、`ModalPopping`、`ModalPopped`。

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>ライフサイクル メソッド

[`Application`](xref:Xamarin.Forms.Application) クラスには 3 つの仮想メソッドが含まれており、それをオーバーライドしてライフサイクルの変化に対応できます。

- `OnStart` - アプリケーションの起動時に呼び出されます。
- `OnSleep` - アプリケーションがバックグラウンドに移行するたびに呼び出されます。
- `OnResume` - バックグラウンドに送られたアプリケーションが再開するときに呼び出されます。

> [!NOTE]
> アプリケーションの終了のメソッドは*ありません*。 通常の (つまり、クラッシュではない) 状況では、アプリケーションの終了は *OnSleep* 状態から行われ、コードへの追加の通知はありません。

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

> [!IMPORTANT]
> Android では、`OnStart` メソッドは順番に呼び出されるだけでなく、メイン アクティビティの `[Activity()]` 属性に `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` がない場合のアプリケーションの初回起動時にも呼び出されます。

<a name="page" />

## <a name="page-notification-events"></a>ページ通知イベント

[`Application`](xref:Xamarin.Forms.Application) クラスには、ページの表示と消去の通知を提供する 2 つのイベントがあります。

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) - ページが画面に表示されようとしているときに発生します。
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) - ページが画面から消去されようとしているときに発生します。

画面に表示されるページを追跡する必要があるシナリオで、これらのイベントを使用できます。

> [!NOTE]
> [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) イベントと [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) イベントは、それぞれ [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) イベントと [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) イベントの直後に、[`Page`](xref:Xamarin.Forms.Page) 基底クラスから生成されます。

<a name="modal" />

## <a name="modal-navigation-events"></a>モーダル ナビゲーション イベント

[`Application`](xref:Xamarin.Forms.Application) クラスには、それぞれが独自のイベント引数を持つ 4 つのイベントがあり、それを使用してモーダル ページの表示と消去に応答できます。

- `ModalPushing` - ページがモーダルでプッシュされるときに発生します。
- `ModalPushed` - ページがモーダルでプッシュされた後に発生します。
- `ModalPopping` - ページがモーダルでポップされるときに発生します。
- `ModalPopped` - ページがモーダルでポップされた後に発生します。

> [!NOTE]
> 型が `ModalPoppingEventArgs` である `ModalPopping` のイベント引数には、`Cancel` プロパティが含まれています。 `Cancel` が `true` に設定されると、モーダル ポップアップは取り消されます。
