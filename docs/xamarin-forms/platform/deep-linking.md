---
title: アプリケーションのインデックス作成とディープ リンク
description: この記事では、アプリケーションのインデックスを使用する方法と、Xamarin.Forms アプリケーションのコンテンツを iOS および Android デバイスで検索できるようにするディープ リンクを示します。
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 7a102765a3633b8abaf01b3f090d8253230bc16b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996097"
---
# <a name="application-indexing-and-deep-linking"></a>アプリケーションのインデックス作成とディープ リンク

_アプリケーションがインデックス作成により、いくつかを使用して、検索結果に表示して、最新の後、忘れてそれ以外の場合はアプリケーションです。ディープ リンクから参照されているページに移動して、通常、アプリケーションのデータを含む検索結果に応答するアプリケーションは、ディープ リンクできます。この記事では、アプリケーションのインデックスを使用する方法と、Xamarin.Forms アプリケーションのコンテンツを iOS および Android デバイスで検索できるようにするディープ リンクを示します。_

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**詳細な Xamarin.Forms と Azure でリンク[Xamarin University](https://university.xamarin.com/)**


Xamarin.Forms アプリケーションのインデックス作成とディープ リンクは、ユーザー アプリケーション間を移動すると、アプリケーションがインデックス作成のメタデータの公開の API を提供します。 Spotlight 検索で、Google の検索、または web の検索のインデックス付けされたコンテンツを検索し、できます。 ディープ リンクを含む検索結果をタップして、アプリケーションによって処理できますし、ディープ リンクから参照されているページに移動するため、通常イベントが開始されます。

サンプル アプリケーションでは次のスクリーン ショットに示すように、ローカルの SQLite データベースにデータの格納、Todo リスト アプリケーションを示しています。

![](deep-linking-images/screenshots.png "TodoList アプリケーション")

各`TodoItem`ユーザーによって作成されたインスタンスのインデックスを作成します。 プラットフォーム固有の検索は、インデックス付きのデータ、アプリケーションを配置に使用できます。 ユーザーがアプリケーションの検索結果の項目をタップする、アプリケーションを起動する、`TodoItemPage`への移動が、`TodoItem`ディープから参照されているリンクが表示されます。

詳細については、SQLite データベースを使用して、次を参照してください。[ローカル Database](~/xamarin-forms/app-fundamentals/databases.md)。

> [!NOTE]
> Xamarin.Forms アプリケーションのインデックス作成しディープ リンクの機能は、iOS と Android のプラットフォームでのみ使用し、iOS 9 と API 23 をそれぞれが必要です。

## <a name="setup"></a>セットアップ

次のセクションでは、この機能を使用して iOS および Android プラットフォームの追加のセットアップ指示を提供します。

### <a name="ios"></a>iOS

IOS プラットフォームでは、この機能を使用するために必要な追加の設定はありません。

### <a name="android"></a>Android

Android のプラットフォームでは、さまざまなアプリケーションのインデックス作成とディープ リンクの機能を使用するために満たす必要がありますの前提条件があります。

1. アプリケーションのバージョンは、Google Play でライブである必要があります。
1. コンパニオン web サイトは、Google の開発者コンソールでアプリケーションに対して登録する必要があります。 Url には、アプリケーションが web サイトと関連付けられていると、その作業、web サイトと検索結果に処理できるし、アプリケーションの両方のインデックスを作成します。 詳細については、次を参照してください。[アプリが Google 検索のインデックス作成](https://support.google.com/googleplay/android-developer/answer/6041489)Google の web サイト。
1. アプリケーションの HTTP URL のインテントをサポートする必要があります、`MainActivity`に応答できるクラスは、アプリケーションのデータの URL スキームには、どのような種類のインデックス作成をアプリケーションに指示します。 詳細については、次を参照してください。[インテント フィルターを構成する](~/android/platform/app-linking.md#configure-intent-filter)します。

これらの前提条件が満たされると、Xamarin.Forms アプリケーションのインデックス作成と、Android プラットフォームでのディープ リンクを使用する次の追加のセットアップが必要です。

1. インストール、 [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) Android アプリケーション プロジェクトに NuGet パッケージ。
1. `MainActivity.cs`ファイルで、インポート、`Xamarin.Forms.Platform.Android.AppLinks`名前空間。
1. `MainActivity.OnCreate`下にあるコードの次の行を追加、オーバーライド`Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

詳細については、次を参照してください。[ディープ リンク コンテンツを Xamarin.Forms での URL ナビゲーション](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)Xamarin ブログ。

## <a name="indexing-a-page"></a>ページのインデックス作成

ページのインデックスを作成し、Google、スポット ライト検索を公開するプロセスは次のとおりです。

1. 作成、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)ディープ リンクで、ユーザーが検索結果にインデックス付けされたコンテンツを選択すると、ページに戻り、ページのインデックスに必要なメタデータを格納しています。
1. 登録、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)検索のインデックスを作成するインスタンス。

次のコード例は、作成する方法を示します、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)インスタンス。

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

[ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)インスタンスには、さまざまな値を持つが、ページのインデックスし、ディープ リンクの作成に必要なプロパティが含まれています。 [ `Title` ](xref:Xamarin.Forms.IAppLinkEntry.Title)、 [ `Description` ](xref:Xamarin.Forms.IAppLinkEntry.Description)、および[ `Thumbnail` ](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail)プロパティは、検索結果に表示されるときに、インデックス付きコンテンツを識別するために使用されます。 [ `IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)プロパティに設定されて`true`をインデックス付けされたコンテンツが現在表示されていることを示します。 [ `AppLinkUri` ](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri)プロパティは、 `Uri` 、現在のページに戻り、現在の表示に必要な情報を格納している`TodoItem`します。 次の例は、例を示しています。`Uri`サンプル アプリケーションについて。

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

これは、`Uri`起動に必要なすべての情報が含まれています、`deeplinking`アプリに移動します、 `DeepLinking.TodoItemPage`、し、表示、`TodoItem`を持つ、`ID`の`ec38ebd1-811e-4809-8a55-0d028fce7819`します。

## <a name="registering-content-for-indexing"></a>コンテンツをインデックス用の登録

1 回、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)インスタンスが作成されて、検索結果に表示するインデックス作成を登録する必要がある必要があります。 これを行うと、 [ `RegisterLink` ](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry))メソッドは、次のコード例で示した。

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

これを追加、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)をアプリケーションのインスタンス[ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks)コレクション。

> [!NOTE]
> `RegisterLink`メソッドがページのインデックスが作成されているコンテンツを更新することもできます。

1 回、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)検索結果に表示するに対して、インデックス作成、インスタンスに登録されています。 次のスクリーン ショットは、iOS プラットフォームでの検索結果に表示されているインデックス付きのコンテンツを示しています。

![](deep-linking-images/ios-search.png "IOS での検索結果にインデックス付けされたコンテンツ")

## <a name="de-registering-indexed-content"></a>コンテンツ インデックスが作成を登録解除します。

[ `DeregisterLink` ](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry))次のコード例に示すように、検索結果からインデックス付けされたコンテンツを削除するメソッドを使用します。

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

これにより、削除、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)インスタンスから、アプリケーションの[ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks)コレクション。

> [!NOTE]
> Android では、検索結果からインデックス付けされたコンテンツを削除することはできません。

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>ディープ リンクへの応答

インデックス付けされたコンテンツを選択し、検索結果に表示されますが、ユーザーが選択されているときに、`App`クラスは、処理するために、要求が受信するアプリケーション、`Uri`インデックス付けされたコンテンツに含まれています。 この要求で処理できる、 [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri))オーバーライドを次のコード例で示した。

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

[ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri))メソッドでは、ことを確認、受信した`Uri`解析前に、アプリケーションのためのものでは、`Uri`ページに移動して、ページに渡されるパラメーターにします。 作成されると、ナビゲートするページのインスタンスと`TodoItem`表されるページでパラメーターを取得します。 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 、ページにナビゲートするのに設定し、`TodoItem`します。 これにより、その場合に、`TodoItemPage`によって表示される、 [ `PushAsync` ](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))メソッドが表示されます、`TodoItem`が`ID`ディープ リンクが含まれています。

## <a name="making-content-available-for-search-indexing"></a>コンテンツを検索インデックス作成を利用できるように

ディープ リンクによって表されるページが表示されるたびに、 [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)にプロパティを設定することができます`true`します。 これにより、iOS と Android で、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)インスタンスの検索インデックスの作成、および iOS のみ利用可能なまた、`AppLinkEntry`ハンドオフの使用可能なインスタンス。 ハンドオフの詳細については、次を参照してください。[ハンドオフの概要](~/ios/platform/handoff.md)します。

次のコード例は、設定を示します、 [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)プロパティを`true`で、 [ `Page.OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)をオーバーライドします。

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

同様に、ときにディープ リンクによって表されるページから移動、 [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)にプロパティを設定することができます`false`します。 IOS と Android でこれを停止、 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry)インスタンスが提供されている検索インデックス作成、および iOS のみ、it も停止広告、`AppLinkEntry`ハンドオフのインスタンス。 これを実現できます、 [ `Page.OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)の次のコード例に示すをオーバーライドします。

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>ハンドオフにデータを提供します。

Ios では、ページのインデックスを作成するとき、アプリケーション固有のデータを格納できます。 これは、データを追加することによって実現されます、 [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues)は、コレクション、`Dictionary<string, string>`ハンドオフに使われるキーと値のペアを格納するためです。 ハンドオフは、ユーザーが自分のデバイスのいずれかでアクティビティを起動し、(ユーザーの iCloud アカウントによって識別される) とは、自分のデバイスのもう 1 つでそのアクティビティを継続するための方法です。 次のコードでは、アプリケーション固有のキー/値ペアを格納する例を示します。

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

格納された値、 [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues)コレクションのインデックス付きのページでは、メタデータに格納され、ディープ リンクを格納している (または別のコンテンツを表示するハンドオフを使用する場合は、検索結果で、ユーザーがタップしたときに復元サインイン済みのデバイス)。

さらに、次のキーの値を指定できます。

- `contentType` –、`string`インデックス付けされたコンテンツの統一された型の識別子を指定します。 この値を使用することをお勧めの規約は、インデックス付きコンテンツが含まれるページの型名です。
- `associatedWebPage` –、`string`を表す場合は、web でインデックス付けされたコンテンツを表示することも、またはアプリケーションは、Safari のディープ リンクをサポートしている場合にアクセスする web ページ。
- `shouldAddToPublicIndex` –、`string`いずれかの`true`または`false`iOS デバイスでアプリケーションをインストールしていないユーザーに提示することは、Apple のパブリック クラウドのインデックスにインデックス付けされたコンテンツを追加するかどうかを制御します。 ただし、パブリックなインデックス作成のためコンテンツが設定されている、いってを自動的に追加されますを Apple のパブリック クラウドのインデックス。 詳細については、次を参照してください。[パブリック検索インデックス作成](~/ios/platform/search/nsuseractivity.md)です。 このキーを設定する必要がありますので注意`false`する個人データを追加するときに、 [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues)コレクション。

> [!NOTE]
> `KeyValues` Android プラットフォームで使用されていないコレクション。

ハンドオフの詳細については、次を参照してください。[ハンドオフの概要](~/ios/platform/handoff.md)します。

## <a name="summary"></a>まとめ

この記事では、アプリケーションのインデックスを使用する方法および Xamarin.Forms アプリケーションのコンテンツを iOS および Android デバイスで検索できるようにするディープ リンクを紹介します。 アプリケーションがインデックス作成により、いくつかを使用した後、について忘れてそれ以外の場合は検索結果に表示して、最新のアプリケーションです。


## <a name="related-links"></a>関連リンク

- [ディープ リンク (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS Api の検索](~/ios/platform/search/index.md)
- [Android 6.0 でのアプリ リンク](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
