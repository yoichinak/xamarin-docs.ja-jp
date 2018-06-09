---
title: Xamarin.Forms アプリのライフ サイクル
description: この記事では、ライフ サイクル メソッド、ページ ナビゲーション イベント、およびモーダル ナビゲーション イベントを含む、アプリケーションのライフ サイクルに応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: fb651494b63a77ede47dd246ee054b5c67af2a35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240270"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms アプリのライフ サイクル

[ `Application` ](xref:Xamarin.Forms.Application)基底クラスは、次の機能を提供します。

* [ライフ サイクル メソッド](#Lifecycle_Methods) `OnStart`、 `OnSleep`、および`OnResume`です。
* [ナビゲーション イベントをページ](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing)、 [ `PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing)です。
* [モーダル ナビゲーション イベント](#modal) `ModalPushing`、 `ModalPushed`、 `ModalPopping`、および`ModalPopped`です。

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>ライフ サイクル メソッド

[ `Application` ](xref:Xamarin.Forms.Application)クラスには、ライフ サイクル メソッドの処理をオーバーライドする 3 つの仮想メソッドが含まれています。

* **OnStart** -アプリケーションの起動時に呼び出されます。

* **OnSleep** -アプリケーションはバック グラウンドに移動するたびに呼び出されます。

* **OnResume** -アプリケーションは、後に再開バック グラウンドに送信されるときに呼び出されます。

あることに注意してください*ありません*アプリケーションの終了時のメソッドです。
通常の状況 (ie。 クラッシュではない) からアプリケーションの終了が発生する可能性、 *OnSleep*状態では、コードを追加、通知なし。

これらのメソッドが呼び出されるタイミングを確認するには、実装、 `WriteLine` (下図) の各呼び出しし、各プラットフォームでテストします。

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

更新するときに*古い*Xamarin.Forms アプリケーションなどです。 Xamarin.Forms 1.3 または旧) を作成、Android の主要なアクティビティが含まれていることを確認`ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation`で、`[Activity()]`属性。 観察するが存在しない場合、`OnStart`回転上だけでなく、アプリケーションの起動時にメソッドが呼び出されます。 この属性は、現在の Xamarin.Forms アプリ テンプレートに自動的に含まれます。

<a name="page" />

## <a name="page-navigation-events"></a>ページ ナビゲーション イベント

2 つのイベントには、 [ `Application` ](xref:Xamarin.Forms.Application)表示および消失しているページの通知を提供するクラス。

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) ページが画面に表示されるときに発生します。
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) ページが画面に表示されなくしようとしたときに発生します。

これらのイベントは、画面に表示されている場合、ページを追跡するシナリオで使用できます。

> [!NOTE]
> [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing)と[ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing)からのイベントが発生する、 [ `Page` ](xref:Xamarin.Forms.Page)基底クラスの直後に、 [ `Page.Appearing`](xref:Xamarin.Forms.Page.Appearing)と[ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing)イベント、それぞれします。

<a name="modal" />

## <a name="modal-navigation-events"></a>モーダル ナビゲーション イベント

4 つのイベントには、 [ `Application` ](xref:Xamarin.Forms.Application)クラス、それぞれが独自のイベント引数を使用することが応答するモーダル ページに表示され、破棄します。

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** -`ModalPoppingEventArgs`クラスに含まれる、`Cancel`プロパティです。 ときに`Cancel`に設定されている`true`モーダル pop が取り消されました。
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> アプリケーション ライフ サイクル メソッドとをすべての事前モーダル ナビゲーション イベントを実装する`Application`Xamarin.Forms アプリを作成する方法 (ie 1.2 またはそれ以前のバージョンで記述された静的なを使用するアプリケーション`GetMainPage`メソッド) を作成するが更新されました、。既定`Application`の親として設定される`MainPage`です。
>
> この従来の動作を使用する Xamarin.Forms アプリケーションを更新する必要があります、`Application`サブクラスの説明に従って、[アプリケーション クラス](~/xamarin-forms/app-fundamentals/application-class.md)ページ。
