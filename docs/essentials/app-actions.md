---
title: 'Xamarin.Essentials: アプリの操作'
description: Xamarin.Essentials の加速度計クラスを使用すると、アプリ アイコンからアプリのショートカットを作成して応答できます。
ms.assetid: 5edf9bc5-b721-448c-a8a2-0a9d4d0c792c
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8b1da7d07c042ff10b48948c4f411d65c6c05002
ms.sourcegitcommit: d4d293174a8324ce82b8f961ae6eadce294cafd7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "98194888"
---
# <a name="no-locxamarinessentials-app-actions"></a>Xamarin.Essentials: アプリの操作

**AppActions** クラスを使用すると、アプリ アイコンからアプリのショートカットを作成して応答できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**AppActions** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

インテント フィルターを `MainActivity` クラスに追加します。

```csharp
[IntentFilter(
        new[] { Xamarin.Essentials.Platform.Intent.ActionAppAction },
        Categories = new[] { Android.Content.Intent.CategoryDefault })]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    ...
```

次に、次のロジックを追加して、アクションを処理します。

```csharp
protected override void OnResume()
{
    base.OnResume();

    Xamarin.Essentials.Platform.OnResume(this);
}

protected override void OnNewIntent(Android.Content.Intent intent)
{
    base.OnNewIntent(intent);

    Xamarin.Essentials.Platform.OnNewIntent(intent);
}
```

# <a name="ios"></a>[iOS](#tab/ios)

`AppDelegate.cs` では、次のロジックを追加して、アクションを処理します。

```csharp
public override void PerformActionForShortcutItem(UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
    => Xamarin.Essentials.Platform.PerformActionForShortcutItem(application, shortcutItem, completionHandler);
```

# <a name="uwp"></a>[UWP](#tab/uwp)

`OnLaunched` メソッドの `App.xaml.cs` ファイルでは、メソッドの下部に次のロジックを追加します。

```csharp
Xamarin.Essentials.Platform.OnLaunched(e);
```

-----

## <a name="create-actions"></a>アクションを作成する

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```
アプリ アクションはいつでも作成可能ですが、多くの場合、アプリケーションの起動時に作成されます。 `SetAsync` メソッドを呼び出して、アプリのアクション一覧を作成します。


```csharp
try
{
    await AppActions.SetAsync(
        new AppAction("app_info", "App Info", icon: "app_info_action_icon"),
        new AppAction("battery_info", "Battery Info"));
}
catch (FeatureNotSupportedException ex)
{
    Debug.WriteLine("App Actions not supported");
}
```

アプリ アクションが特定のバージョンのオペレーティング システムでサポートされていない場合は、`FeatureNotSupportedException` がスローされます。 

`AppAction` には、次のプロパティを設定できます。

* ID: アクションのタップに応答するために使用される一意の識別子。
* タイトル: 表示する表示タイトル。
* サブタイトル: タイトルの下に表示するサブタイトル (サポートされている場合)。
* アイコン: 各プラットフォームで対応するリソース ディレクトリのアイコンと一致している必要があります。

![ホーム画面でのアプリ アクション](images/appactions.png)

## <a name="responding-to-actions"></a>アクションへの応答

アプリケーションが起動するときに、`OnAppAction` イベントが登録されます。 アプリ アクションが選択されるときに、選択されたアクションに関する情報と共にイベントが送信されます。

```csharp
public App()
{
    //...
    AppActions.OnAppAction += AppActions_OnAppAction;
}

void AppActions_OnAppAction(object sender, AppActionEventArgs e)
{
    // Don't handle events fired for old application instances
    // and cleanup the old instance's event handler
    if (Application.Current != this && Application.Current is App app)
    {
        AppActions.OnAppAction -= app.AppActions_OnAppAction;
        return;
    }
    MainThread.BeginInvokeOnMainThread(async () =>
    {
        await Shell.Current.GoToAsync($"//{e.AppAction.Id}");
    });
}
```

## <a name="getactions"></a>GetActions
`AppActions.GetAsync()` を呼び出すことによって、アプリ アクションの現在の一覧を取得できます。

## <a name="api"></a>API

- [AppActions ソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/AppActions)
- [AppActions API のドキュメント](xref:Xamarin.Essentials.AppActions)
