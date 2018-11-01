---
title: Xamarin.Forms アプリのライフ サイクル
description: この記事では、ライフ サイクル メソッド、ページ ナビゲーション イベントでは、モーダル ナビゲーション イベントなど、アプリケーションのライフ サイクルに応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 5bdce8d1752b3d7273ffec233b120266909999c4
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675121"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms アプリのライフ サイクル

[ `Application` ](xref:Xamarin.Forms.Application)基底クラスは、次の機能を提供します。

* [ライフ サイクル メソッド](#Lifecycle_Methods) `OnStart`、 `OnSleep`、および`OnResume`します。
* [ナビゲーション イベントをページ](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing)、 [ `PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing)します。
* [モーダル ナビゲーション イベント](#modal) `ModalPushing`、 `ModalPushed`、 `ModalPopping`、および`ModalPopped`します。

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>ライフ サイクル メソッド

[ `Application` ](xref:Xamarin.Forms.Application)クラスには、ライフ サイクル メソッドを処理するためにオーバーライド可能な 3 つの仮想メソッドが含まれています。

* **OnStart** -アプリケーションの起動時に呼び出されます。

* **OnSleep** -アプリケーションが、バック グラウンドに移動するたびに呼び出されます。

* **OnResume** -アプリケーションが再開され、バック グラウンドに送信したときに呼び出されます。

あることに注意してください*ありません*のアプリケーションの終了メソッド。
通常の状況 (つまり、クラッシュしない) では、アプリケーションの終了がから発生する、 *OnSleep*せず、コードの追加の通知の状態。

これらのメソッドが呼び出されるタイミングを確認するには、実装、 `WriteLine` (次に示す) の各呼び出しし、各プラットフォームでテストします。

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

更新するときに*古い*Xamarin.Forms アプリケーション (例。 Xamarin.Forms 1.3 またはそれ以前) を作成、Android のメイン アクティビティが含まれるように`ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation`で、`[Activity()]`属性。 観察するが存在しない場合、`OnStart`と、アプリケーションが初めて起動したときの回転でメソッドが呼び出されます。 この属性は、現在の Xamarin.Forms アプリ テンプレートで自動的に含まれます。

<a name="page" />

## <a name="page-navigation-events"></a>ページ ナビゲーション イベント

上にある 2 つのイベント、 [ `Application` ](xref:Xamarin.Forms.Application)ページ消えていることの通知を提供するクラス。

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -ページが画面に表示するときに発生します。
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -ページが画面から消えるときに発生します。

これらのイベントは、画面に表示されているページを追跡するシナリオで使用できます。

> [!NOTE]
> [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing)と[ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing)からのイベントが発生、 [ `Page` ](xref:Xamarin.Forms.Page)基底クラスの直後に、 [ `Page.Appearing`](xref:Xamarin.Forms.Page.Appearing)と[ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing)イベント、それぞれします。

<a name="modal" />

## <a name="modal-navigation-events"></a>モーダル ナビゲーション イベント

上にある 4 つのイベント、 [ `Application` ](xref:Xamarin.Forms.Application)クラス、それぞれに独自のイベント引数ができる、モーダル ページ表示され、破棄されているに応答します。

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** -`ModalPoppingEventArgs`クラスが含まれています、`Cancel`プロパティ。 ときに`Cancel`に設定されている`true`モーダル ポップアップが取り消されました。
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> モーダル ナビゲーション イベントをすべて前とアプリケーション ライフ サイクル メソッドを実装する`Application`の Xamarin.Forms アプリを作成する方法 (つまりで記述されたアプリケーション バージョン 1.2 以前を使用して、静的な`GetMainPage`メソッド) を作成するが更新されました、既定`Application`の親として設定されています`MainPage`します。
>
> この従来の動作を使用して、Xamarin.Forms アプリケーションを更新する必要があります、`Application`サブクラスの説明に従って、[アプリケーション クラス](~/xamarin-forms/app-fundamentals/application-class.md)ページ。
