---
title: "アプリケーションがインデックス作成とディープ リンク"
description: "いくつかを使用して検索結果に表示される関連を把握して後忘れてそれ以外の場合はアプリケーションをアプリケーションがインデックス作成できます。 ディープ リンクをディープ リンクから参照されているページに移動して、通常、アプリケーションのデータを含む検索結果に応答するアプリケーションはできます。 この記事では、アプリケーションのインデックスを使用する方法、および Xamarin.Forms アプリケーション コンテンツを iOS および Android デバイスで検索できるようにするディープ リンクを示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: b2decf1331764ed6b1696126d8b23318e329e0c7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="application-indexing-and-deep-linking"></a>アプリケーションがインデックス作成とディープ リンク

_いくつかを使用して検索結果に表示される関連を把握して後忘れてそれ以外の場合はアプリケーションをアプリケーションがインデックス作成できます。ディープ リンクをディープ リンクから参照されているページに移動して、通常、アプリケーションのデータを含む検索結果に応答するアプリケーションはできます。この記事では、アプリケーションのインデックスを使用する方法、および Xamarin.Forms アプリケーション コンテンツを iOS および Android デバイスで検索できるようにするディープ リンクを示します。_

## <a name="overview"></a>概要

Xamarin.Forms アプリケーション インデックスとディープ リンクは、ユーザー アプリケーション間を移動すると、アプリケーションがインデックス作成のためのメタデータの公開の API を提供します。 Spotlight 検索で、Google 検索、または web の検索のインデックス付きコンテンツを検索し、できます。 ディープ リンクを含む検索結果をタップして、アプリケーションによって処理できるし、は、通常のディープ リンクから参照されているページへの移動に使用するイベントが開始されます。

次のスクリーン ショットに示すように、サンプル アプリケーションは作業一覧アプリケーションのローカルの SQLite データベースに、データを保存する場所を示します。

![](deep-linking-images/screenshots.png "TodoList アプリケーション")

各`TodoItem`ユーザーによって作成されたインスタンスのインデックスを作成します。 プラットフォーム固有の検索は、アプリケーションからのインデックス付きデータを検索し、使用できます。 ユーザーがアプリケーションについては、検索結果の項目をタップしたときに、アプリケーションを起動する、`TodoItemPage`に移動し、`TodoItem`詳細なから参照されているリンクが表示されます。

詳細については、SQLite データベースを使用して、次を参照してください。[ローカル データベースで作業](~/xamarin-forms/app-fundamentals/databases.md)です。

> [!NOTE]
> **注**: Xamarin.Forms アプリケーション インデックスの作成と機能の深いリンクは、iOS や Android プラットフォームでのみ使用し、iOS 9 と API 23 をそれぞれが必要です。

## <a name="setup"></a>セットアップ

次のセクションでは、この機能を使用して iOS および Android プラットフォームの任意の追加のセットアップ手順を提供します。

### <a name="ios"></a>iOS

IOS プラットフォームでは、この機能を使用するために必要な追加の設定はありません。

### <a name="android"></a>Android

プラットフォームでは、Android がいくつかの前提条件アプリケーションがインデックス作成とディープ リンクの機能を使用するために満たす必要があります。

1. アプリケーションのバージョンは、Google Play にライブである必要があります。
1. Google の開発者コンソールでアプリケーションに対してコンパニオン web サイトを登録する必要があります。 Url を指定できます、アプリケーションは、web サイトに関連付けられたは、web サイトと、アプリケーションでは、検索結果を処理し、その作業のインデックスを作成します。 詳細については、次を参照してください。[アプリが Google 検索のインデックス作成](https://support.google.com/googleplay/android-developer/answer/6041489)Google の web サイトです。
1. アプリケーションでの HTTP URL の目的をサポートする必要があります、`MainActivity`に応答できるクラスは、アプリケーション データの URL スキームの種類のインデックスを作成するアプリケーションに指示します。 詳細については、次を参照してください。[目的としたフィルターを構成する](~/android/platform/app-linking.md#configure-intent-filter)です。

これらの前提条件が満たされると、Xamarin.Forms アプリケーション インデックスの作成と Android プラットフォームではディープ リンクを使用する次の追加の設定が必要です。

1. インストール、 [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) Android アプリケーション プロジェクトに NuGet パッケージです。
1. `MainActivity.cs`ファイル、インポート、`Xamarin.Forms.Platform.Android.AppLinks`名前空間。
1. `MainActivity.OnCreate`下にあるコードの次の行を追加、オーバーライド`Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

詳細については、次を参照してください。[ディープ リンク コンテンツ Xamarin.Forms の URL ナビゲーションを使用して](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)Xamarin ブログ。

## <a name="indexing-a-page"></a>ページ インデックスの作成

ページのインデックスを作成し、Google とメディアの検索に公開するプロセスは次のとおりです。

1. 作成、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)検索結果にインデックス付きコンテンツを選択すると、ページに戻りますへのディープ リンクと共に、ページのインデックスに必要なメタデータを格納しています。
1. 登録、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)インスタンスに検索のインデックスを作成します。

次のコード例を作成する方法を示しています、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)インスタンス。

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

[ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)インスタンスには、いくつかの値を持つが、ページのインデックスおよびディープ リンクの作成に必要なプロパティが含まれています。 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/)、 [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/)、および[ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/)プロパティを使用して、検索結果に表示されるときに、インデックス付きコンテンツを識別します。 [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/)プロパティに設定されている`true`をインデックス付きコンテンツが現在表示されていることを示します。 [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/)プロパティは、 `Uri` 、現在のページに戻り、現在の表示に必要な情報を含む`TodoItem`です。 次の例は、例を示しています。`Uri`サンプル アプリケーションについては。

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

これは、`Uri`起動に必要なすべての情報を含む、`deeplinking`アプリに移動、 `DeepLinking.TodoItemPage`、し、表示、`TodoItem`を持つ、`ID`の`ec38ebd1-811e-4809-8a55-0d028fce7819`します。

## <a name="registering-content-for-indexing"></a>インデックス作成用コンテンツを登録します。

1 回、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)インスタンスが作成されて、検索結果に表示するインデックスを登録する必要がある必要があります。 これは、使用、 [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/)メソッドを次のコード例に示されています。

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

これを追加、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)をアプリケーションのインスタンス[ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/)コレクション。

> [!NOTE]
> **注**:`RegisterLink`メソッドは、ページのインデックスが設定されて、コンテンツを更新するも使用できます。

1 回、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)検索結果に表示するに対して、インデックス作成、インスタンスに登録されています。 次のスクリーン ショットは、iOS プラットフォームでは検索結果に表示されるインデックス付きコンテンツを示しています。

![](deep-linking-images/ios-search.png "IOS の検索結果にインデックス付きコンテンツ")

## <a name="de-registering-indexed-content"></a>インデックス付きコンテンツを登録解除

[ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/)次のコード例に示すように検索結果から、インデックス付きコンテンツを削除するメソッドを使用します。

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

これにより、削除、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)インスタンスから、アプリケーションの[ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/)コレクション。

> [!NOTE]
> **注**: Android での検索結果でインデックス付けされたコンテンツを削除することはできません。

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>ディープ リンクへの応答

インデックス付けされたコンテンツは検索結果に表示されが、ユーザーが選択されているときに、`App`処理の要求が受信するアプリケーション用のクラス、`Uri`インデックス付きコンテンツに含まれています。 この要求を処理することができます、 [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/)オーバーライドを次のコード例で示したようにします。

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

[ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/)メソッドでは、ことを確認、受け取った`Uri`解析前に、アプリケーションのためのものでは、`Uri`をページに移動して、ページに渡されるパラメーターにします。 作成されるに移動できない場合に、ページのインスタンスと`TodoItem`表されるページでパラメーターを取得します。 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)ページに移動するのには、セット`TodoItem`です。 これにより、そのとき、`TodoItemPage`によって表示される、 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)メソッドが表示されます、`TodoItem`が`ID`ディープ リンクに含まれます。

## <a name="making-content-available-for-search-indexing"></a>検索インデックス作成用のコンテンツを作成すること

ディープ リンクによって表されるページが表示されるたびに、 [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/)プロパティに設定することができます`true`です。 これにより、iOS および Android 上、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) search がインデックス作成、および iOS のみ利用可能なインスタンス、また、`AppLinkEntry`ハンドオフの使用可能なインスタンス。 ハンドオフの詳細については、次を参照してください。[ハンドオフの概要](~/ios/platform/handoff.md)です。

設定を次のコード例に示します、 [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/)プロパティを`true`で、 [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/)をオーバーライドします。

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

離れていても、ディープ リンクによって表されるページが移動したときに同様に、 [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/)プロパティに設定することができます`false`です。 IOS および Android では、これによって、停止、 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)アドバタイズされている検索インデックスを作成、および iOS のみ、it でもインスタンスの停止広告、`AppLinkEntry`ハンドオフ用のインスタンス。 これで、 [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/)オーバーライドを次のコード例で示したようにします。

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>ハンドオフにデータを提供します。

Ios の場合、ページのインデックスを作成するとき、アプリケーション固有のデータを格納できます。 これを実現するデータを追加することによって、 [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/)は、コレクション、`Dictionary<string, string>`ハンドオフで使用されるキーと値のペアを格納するためです。 ハンドオフは、ユーザーが自分のデバイスの 1 つのアクティビティを開始し、(ユーザーの iCloud アカウントによって識別される) と、自分のデバイスの他の場所でそのアクティビティを継続するための方法です。 次のコードは、アプリケーション固有のキー/値ペアを格納する例を示しています。

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

格納された値、 [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/)コレクションは、インデックス ページのメタデータに格納され、ディープ リンクを格納している (または別のコンテンツを表示するハンドオフを使用する場合は、検索結果で、ユーザーがタップしたときに復元されますサインインしているデバイス)。

さらに、次のキー値を指定することができます。

- `contentType` –、`string`インデックス付きコンテンツの統一された型の識別子を指定します。 この値を使用することをお勧めの規則は、インデックス付きコンテンツを含むページの種類名です。
- `associatedWebPage` –、`string`を表す場合は、web でインデックス付きコンテンツを表示することも、またはアプリケーションは、Safari のディープ リンクをサポートしている場合にアクセスする web ページ。
- `shouldAddToPublicIndex` –、`string`いずれかの`true`または`false`を提示することにより、iOS デバイスにアプリケーションをインストールしていないユーザーに、Apple のパブリック クラウド インデックスにインデックス付きコンテンツを追加するかどうかを制御します。 ただし、パブリックのインデックス作成のためコンテンツが設定されているからといってとは限りませんを自動的に追加されますを Apple のパブリック クラウドのインデックス。 詳細については、次を参照してください。[パブリック検索インデックス](~/ios/platform/search/nsuseractivity.md)です。 このキーに設定する必要がありますを`false`個人データを追加するときに、 [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/)コレクション。

> [!NOTE]
> **注**:`KeyValues`コレクションは、Android プラットフォームでは使用されません。

ハンドオフの詳細については、次を参照してください。[ハンドオフの概要](~/ios/platform/handoff.md)です。

## <a name="summary"></a>まとめ

この記事には、アプリケーションのインデックスを使用する方法、および Xamarin.Forms アプリケーション コンテンツを iOS および Android デバイスで検索できるようにするディープ リンクが示されています。 状態を維持していくつかを使用した後、に関する忘れてそれ以外の場合は検索結果に表示される関連アプリケーションをアプリケーションのインデックスを作成できます。


## <a name="related-links"></a>関連リンク

- [ディープ リンク (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS Search Api](~/ios/platform/search/index.md)
- [Android 6.0 でアプリ リンク](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
