---
title: アプリケーション インデックス作成とディープ リンクの設定
description: この記事では、アプリケーション インデックス作成とディープ リンクの設定を使用して、Xamarin.Forms アプリケーションのコンテンツを iOS および Android のデバイス上で検索できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/28/2018
ms.openlocfilehash: fcd8333a0623058fceb486183ddb995e85eaf18a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76940321"
---
# <a name="application-indexing-and-deep-linking"></a>アプリケーション インデックス作成とディープ リンクの設定

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/deeplinking)

_アプリケーション インデックス作成を使用すると、何度か使用した後に忘れてしまいそうなアプリケーションを検索結果に表示することで、関連性を保つことができます。ディープ リンクの設定を使用すると、アプリケーションのデータが含まれている検索結果にアプリケーションが応答するようにできます。通常の応答では、ディープ リンクから参照されているページに移動します。この記事では、アプリケーション インデックス作成とディープ リンクの設定を使用して、Xamarin.Forms アプリケーションのコンテンツを iOS および Android のデバイス上で検索できるようにする方法について説明します。_

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Xamarin.Forms と Azure でのディープ リンク設定のビデオ**

Xamarin.Forms アプリケーション インデックス作成とディープ リンクの設定では、ユーザーがアプリケーション間を移動するとき、アプリケーション インデックス作成用のメタデータを発行する API が提供されます。 インデックスが付けられたコンテンツは、Spotlight 検索、Google 検索、または Web 検索で検索することができます。 ディープ リンクが含まれている検索結果をタップすると、アプリケーションによって処理できるイベントが開始されます。これは、通常、ディープ リンクから参照されているページに移動するために使用されます。

サンプル アプリケーションは、次のスクリーン ショットに示すように、データがローカルの SQLite データベースに格納される Todo リスト アプリケーションの例です。

![](deep-linking-images/screenshots.png "TodoList Application")

ユーザーによって作成された各 `TodoItem` インスタンスにインデックスが付けられます。 これにより、プラットフォーム固有の検索を使用して、アプリケーションからインデックス付きのデータを特定できるようになります。 アプリケーションに関する検索結果の項目をユーザーがタップすると、そのアプリケーションが起動し、`TodoItemPage` に移動し、ディープ リンクから参照された `TodoItem` が表示されます。

SQLite データベースの使用の詳細については、「[Xamarin.Forms ローカル データベース](~/xamarin-forms/data-cloud/data/databases.md)」を参照してください。

> [!NOTE]
> Xamarin.Forms アプリケーション インデックス作成およびディープ リンクの設定の機能は、iOS および Android プラットフォーム上でのみ使用することができ、そのためにはそれぞれ iOS 9 以上および API 23 以上が必要です。

## <a name="setup"></a>セットアップ

次のセクションでは、iOS および Android プラットフォーム上でこの機能を使用する上で必要な追加のセットアップ手順を説明します。

### <a name="ios"></a>iOS

iOS プラットフォーム上では、バンドルに署名するためのカスタムのエンタイトルメント ファイルとして **Entitlements.plist** ファイルがご利用の iOS プラットフォーム プロジェクトによって設定されていることを確認します。

iOS ユニバーサル リンクを使用するには:

1. `applinks` キーを使用して関連付けられているドメインのエンタイトルメントをご利用のアプリに追加します。そのアプリでサポートされるすべてのドメインが対象となります。
1. ご利用の Web サイトに Apple アプリ サイトの関連ファイルを追加します。
1. その Apple アプリ サイトの関連ファイルに `applinks` キーを追加します。

詳細については、developer.apple.com に掲載されている「[Allowing Apps and Websites to Link to Your Content](https://developer.apple.com/documentation/uikit/core_app/allowing_apps_and_websites_to_link_to_your_content)」(アプリおよび Web サイトから自分のコンテンツへのリンクを許可) を参照してください。

### <a name="android"></a>Android

Android プラットフォーム上でアプリケーション インデックス作成およびディープ リンクの設定の機能を使用するためには、複数の前提条件を満たす必要があります。

1. ご利用のアプリケーションのバージョンが Google Play で動作状態にある必要があります。
1. Google の開発者コンソール内で、アプリケーションに対してコンパニオン Web サイトを登録する必要があります。 アプリケーションが Web サイトに関連付けられたら、URL にインデックスを付けて Web サイトとアプリケーションの両方で使用できます。これにより、検索結果に提供されるようになります。 詳細については、Google の Web サイト上の「[Google 検索での App Indexing](https://support.google.com/googleplay/android-developer/answer/6041489)」を参照してください。
1. ご利用のアプリケーションでは、`MainActivity` クラス上の HTTP URL インテントがサポートされている必要があります。これにより、アプリケーションが応答可能な URL データ スキームの種類がアプリケーション インデックス作成に指示されます。 詳細については、「[Configuring the Intent Filter](~/android/platform/app-linking.md#configure-intent-filter)」(インテント フィルタの構成) を参照してください。

これらの前提条件が満たされたら、Android プラットフォーム上で Xamarin.Forms アプリケーション インデックス作成およびディープ リンクの設定を使用するために次に示すような追加のセットアップが必要です。

1. [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) NuGet パッケージを Android アプリケーション プロジェクトにインストールします。
1. **MainActivity.cs** ファイル内に、`Xamarin.Forms.Platform.Android.AppLinks` 名前空間を使用するための宣言を追加します。
1. **MainActivity.cs** ファイル内に、`Firebase` 名前空間を使用するための宣言を追加します。
1. Web ブラウザーで、[Firebase Console](https://console.firebase.google.com/) を介して新しいプロジェクトを作成します。
1. Firebase Console で、ご利用の Android アプリに Firebase を追加し、必要なデータを入力します。
1. 結果として生成された **google-services.json** ファイルをダウンロードします。
1. **google-services.json** ファイルを、Android プロジェクトのルート ディレクトリに追加し、その**ビルド アクション**を **GoogleServicesJson** に設定します。
1. `MainActivity.OnCreate` オーバーライド内で、`Forms.Init(this, bundle)` の下に次のコード行を追加します。

```csharp
FirebaseApp.InitializeApp(this);
AndroidAppLinks.Init(this);
```

**google-services.json** をプロジェクトに追加 (および *GoogleServicesJson** ビルド アクションを設定) すると、ビルド プロセスによって、クライアント ID と API キーが抽出され、さらにこれらの資格情報が、生成されたマニフェスト ファイルに追加されます。

> [!NOTE]
> この記事では、多くの場合、アプリケーション リンクとディープ リンクという用語が同じ意味で使用されます。 しかし、Android に関しては、これらの用語は異なる意味を持ちます。 Android の場合、ディープ リンクとは、ユーザーがそのアプリで特定のアクティビティに直接移行できるようにするためのインテント フィルターのことです。 ディープ リンクをクリックすると、あいまいさ排除のためのダイアログが表示されることがあります。これにより、ユーザーはその URL を処理できる複数のアプリのいずれかを選択できます。 Android のアプリ リンクは、Web サイトの URL に基づくディープ リンクです。これは Web サイトに属することが検証済みです。 アプリ リンクをクリックすると、アプリがインストールされている場合はアプリが開き、あいまいさ排除のダイアログが開くことはありません。

詳細については、Xamarin ブログでの「[Deep Link Content with Xamarin.Forms URL Navigation](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)」 (Xamarin.Forms URL ナビゲーションを使用したディープ リンク コンテンツ) を参照してください。

## <a name="indexing-a-page"></a>ページのインデックス作成

ページのインデックスを作成し、それを Google および Spotlight 検索に公開するプロセスは次のとおりです。

1. ページのインデックス作成に必要なメタデータを含む [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) と、ユーザーが検索結果内のインデックス付けされたコンテンツを選択したときにページに戻るためのディープ リンクを作成します。
1. [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスを登録し、検索できるようにそれにインデックスを付けます。

次のコード例では、[`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスを作成する方法について示します。

```csharp
AppLinkEntry GetAppLink(TodoItem item)
{
    var pageType = GetType().ToString();
    var pageLink = new AppLinkEntry
    {
        Title = item.Name,
        Description = item.Notes,
        AppLinkUri = new Uri($"http://{App.AppName}/{pageType}?id={item.ID}", UriKind.RelativeOrAbsolute),
        IsLinkActive = true,
        Thumbnail = ImageSource.FromFile("monkey.png")
    };

    pageLink.KeyValues.Add("contentType", "TodoItemPage");
    pageLink.KeyValues.Add("appName", App.AppName);
    pageLink.KeyValues.Add("companyName", "Xamarin");

    return pageLink;
}
```

[`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスには、ページのインデックス作成およびディープ リンクの作成に必要な値が格納された複数のプロパティが含まれています。 [`Title`](xref:Xamarin.Forms.IAppLinkEntry.Title)、[`Description`](xref:Xamarin.Forms.IAppLinkEntry.Description)、[`Thumbnail`](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) の各プロパティは、検索結果内にインデックス付きのコンテンツが表示された場合にそれを識別するために使用されます。 [`IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) プロパティは `true` に設定されます。これは、インデックス付きのコンテンツが現在表示されていることを示します。 [`AppLinkUri`](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) プロパティは、現在のページに戻るため、および現在の `TodoItem` を表示するために必要な情報が含まれる `Uri` です。 次の例には、サンプル アプリケーションの `Uri` 例が示されています。

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=2
```

この `Uri` には、`deeplinking` アプリを起動し、`DeepLinking.TodoItemPage` に移動し、`ID` が 2 である `TodoItem` を表示するために必要な情報がすべて含まれています。

## <a name="registering-content-for-indexing"></a>インデックス作成のためのコンテンツの登録

[`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスが作成されたら、検索結果に表示されるようにするためのインデックス作成に備えてそれを登録する必要があります。 これは、次のコード例に示すように、[`RegisterLink`](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) メソッドを使用して行われます。

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

これにより、[`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスがアプリケーションの [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) コレクションに追加されます。

> [!NOTE]
> `RegisterLink` メソッドを使用することで、インデックスが付けられたページのコンテンツを更新することもできます。

インデックス作成のために [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスが登録されると、それを検索結果に表示できるようになります。 次のスクリーン ショットは、iOS プラットフォームでの検索結果に表示されているインデックス付きコンテンツの例です。

![](deep-linking-images/ios-search.png "Indexed Content in Search Results on iOS")

## <a name="de-registering-indexed-content"></a>インデックス付きコンテンツの登録を解除する

[`DeregisterLink`](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) メソッドは、次のコード例に示すように、検索結果からインデックス付けされたコンテンツを削除するために使用されます。

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

これにより、アプリケーションの [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) コレクションから [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスが削除されます。

> [!NOTE]
> Android 上では、インデックス付けされたコンテンツを検索結果から削除することはできません。

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>ディープ リンクへの応答

インデックス付きコンテンツが検索結果に表示され、それをユーザーが選択すると、インデックス付きコンテンツに含まれている `Uri` を処理するための要求がアプリケーションの `App` クラスによって受信されます。 この要求は、次のコード例に示すように、[`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) オーバーライド内で処理できます。

```csharp
public class App : Application
{
    ...
    protected override async void OnAppLinkRequestReceived(Uri uri)
    {
        string appDomain = "http://" + App.AppName.ToLowerInvariant() + "/";
        if (!uri.ToString().ToLowerInvariant().StartsWith(appDomain, StringComparison.Ordinal))
            return;

        string pageUrl = uri.ToString().Replace(appDomain, string.Empty).Trim();
        var parts = pageUrl.Split('?');
        string page = parts[0];
        string pageParameter = parts[1].Replace("id=", string.Empty);

        var formsPage = Activator.CreateInstance(Type.GetType(page));
        var todoItemPage = formsPage as TodoItemPage;
        if (todoItemPage != null)
        {
            var todoItem = await App.Database.GetItemAsync(int.Parse(pageParameter));
            todoItemPage.BindingContext = todoItem;
            await MainPage.Navigation.PushAsync(formsPage as Page);
        }
        base.OnAppLinkRequestReceived(uri);
    }
}
```

受信した `Uri` を解析して移動先のページおよびページに渡すパラメーターを特定する前に、その `Uri` がアプリケーションを対象とするものかどうかが、[`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) メソッドによって確認されます。 移動先のページのインスタンスが作成され、ページ パラメーターによって表現される `TodoItem` が取得されます。 次に、移動先であるページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) が `TodoItem` に設定されます。 これにより、[`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) メソッドによって `TodoItemPage` が表示されると、ディープ リンクに含まれている `ID` を持つ `TodoItem` が確実に表示されます。

## <a name="making-content-available-for-search-indexing"></a>Search インデックス処理でコンテンツを利用できるようにする

ディープ リンクによって表されるページが表示されるたびに、[`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) プロパティを `true` に設定することができます。 iOS と Android では、これによって、Search インデックス処理で [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスが有効になります。また、iOS の場合のみ、ハンドオフに対しても `AppLinkEntry` インスタンスが有効になります。 ハンドオフの詳細については、[ハンドオフの概要](~/ios/platform/handoff.md)に関するページを参照してください。

[`Page.OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) オーバーライド内で、[`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) プロパティを `true` に設定するコード例を次に示します。

```csharp
protected override void OnAppearing()
{
    appLink = GetAppLink(BindingContext as TodoItem);
    if (appLink != null)
    {
        appLink.IsLinkActive = true;
    }
}
```

同様に、ディープ リンクによって表されるページから離れるときに、[`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) プロパティを `false` に設定することができます。 iOS と Android では、これによって、Search インデックス処理に対する [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) インスタンスの公開が停止されます。また、iOS の場合のみ、ハンドオフに対する `AppLinkEntry` インスタンスの公開が停止されます。 これは、次のコード例に示すように、[`Page.OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) オーバーライド内で達成できます。

```csharp
protected override void OnDisappearing()
{
    if (appLink != null)
    {
        appLink.IsLinkActive = false;
    }
}
```

## <a name="providing-data-to-handoff"></a>ハンドオフにデータを提供する

iOS 上では、ページのインデックスを作成するとき、アプリケーション固有のデータを格納することができます。 これを実現するには、[`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) コレクションにデータを追加します。これは、ハンドオフに使用されるキーと値のペアを格納するための `Dictionary<string, string>` です。 ハンドオフは、ユーザーが自分のデバイスのいずれかでアクティビティを開始し、自分の別のデバイス (ユーザーの iCloud アカウントによって識別される) でそのアクティビティを継続するための方法です。 次のコードは、アプリケーション固有のキーと値のペアを格納する例です。

```csharp
var pageLink = new AppLinkEntry
{
    ...
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

[`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) コレクションに格納された値は、インデックス付きページのメタデータに格納され、ディープ リンクが含まれている検索結果をユーザーがタップしたとき (または、別のサインイン デバイス上のコンテンツを表示するためにハンドオフが使用されたとき) に復元されます。

さらに、次のキーの値を指定できます。

- `contentType` – インデックス付きのコンテンツの uniform 型識別子を指定する `string`。 この値に対して使用が推奨される規約は、インデックス付きコンテンツが含まれるページの型名です。
- `associatedWebPage` – Web 上でインデックス付きコンテンツも表示できる場合、またはアプリケーションによって Safari のディープ リンクがサポートされている場合にアクセスする Web ページを表す `string`。
- `shouldAddToPublicIndex` – `true` または `false` のいずれか `string`。これにより、インデックス付きコンテンツを Apple のパブリック クラウド インデックスに追加するかどうかが制御されます。それは自分の iOS デバイス上にアプリケーションをインストールしていないユーザーに提供することができます。 ただし、パブリック インデックス作成に対してコンテンツが設定されているからといって、Apple のパブリック クラウド インデックスにそれが自動的に追加されることを意味するものではありません。 詳細については、[パブリック Search インデックス処理](~/ios/platform/search/nsuseractivity.md)に関するページを参照してください。 個人データを [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) コレクションに追加する場合は、このキーを `false` に設定する必要があることに注意してください。

> [!NOTE]
> `KeyValues` コレクションは、Android プラットフォームでは使用されません。

ハンドオフの詳細については、[ハンドオフの概要](~/ios/platform/handoff.md)に関するページを参照してください。

## <a name="summary"></a>まとめ

この記事では、アプリケーション インデックス作成とディープ リンクの設定を使用して、Xamarin.Forms アプリケーションのコンテンツを iOS および Android のデバイス上で検索できるようにする方法について説明しました。 アプリケーション インデックス作成を使用すると、何度か使用した後に忘れてしまいそうなアプリケーションを検索結果に表示することで、関連性を保つことができます。

## <a name="related-links"></a>関連リンク

- [ディープ リンクの設定 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/deeplinking)
- [iOS 検索 API](~/ios/platform/search/index.md)
- [Android 6.0 でのアプリ リンク作成](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
