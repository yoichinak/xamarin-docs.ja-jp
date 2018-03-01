---
title: "Xamarin.Forms マルチスクリーンの詳細"
ms.topic: article
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 91be10daaeb508ca193e734d49a21323d70e2055
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Xamarin.Forms マルチスクリーンの詳細

[Xamarin.Forms マルチスクリーン クイックスタート](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md)では、Phoneword アプリケーションを拡張し、アプリケーションの通話履歴を追跡記録する 2 つ目の画面を追加しました。 この記事では、Xamarin.Forms アプリケーションでのページ ナビゲーションとデータ バインドについて理解するために構築された新機能を確認します。

## <a name="navigation"></a>ナビゲーション

Xamarin.Forms にはナビゲーション モデルが内蔵されています。このモデルはページ スタックのナビゲーションとユーザー エクスペリエンスを管理します。 このモデルは、`Page` オブジェクトの後入れ先出し (LIFO) スタックを実行します。 ページを移動するとき、アプリケーションは新しいページをこのスタックにプッシュします。 前のページに戻るとき、アプリケーションは現在のページをスタックからポップします。

Xamarin.Forms には、[`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) オブジェクトのスタックを管理する [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) クラスがあります。 `NavigationPage` クラスはまた、ページの最上部にナビゲーション バーを追加します。このバーには、タイトルと、前にページに戻るための <span class="uiitem">[戻る]</span> ボタンが表示されます。このボタンはプラットフォーム固有です。 次のコード例では、アプリケーションで最初のページに `NavigationPage` をラップする方法を確認できます。

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

すべての [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) インスタンスに、ページ スタックを変更するメソッドを公開する [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) プロパティがあります。 このメソッドは、アプリケーションに [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) が含まれる場合にのみ呼び出します。 `CallHistoryPage` に移動するには、下のコード例のように、[`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) メソッドを呼び出す必要があります。

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

これでナビゲーション スタックに新しい `CallHistoryPage` オブジェクトがプッシュされます。 元のページにプログラムを使用して戻るには、`CallHistoryPage` オブジェクトが次のコード例のように [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) メソッドを呼び出す必要があります。

```csharp
await Navigation.PopAsync();
```

ただし、Phoneword アプリケーションでは、このコードは不要です。[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) クラスがナビゲーション バーをページの最上部に追加するからです。このバーには、前のページに戻るための <span class="uiitem">[戻る]</span> ボタンがあります (プラットフォーム固有)。

## <a name="data-binding"></a>データ バインディング

Xamarin.Forms アプリケーションがそのデータを表示し、相互作用するしくみを簡単にするためにデータ バインディングが使用されます。 データ バインディングはユーザー インターフェイスと基礎アプリケーションの間で接続を確立します。 [`BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) クラスには、データ バインディングをサポートするためのインフラストラクチャの大部分が含まれています。

データ バインディングは、2 つのオブジェクト間の関係を定義します。 *ソース* オブジェクトはデータを提供します。 *ターゲット* オブジェクトは、ソース オブジェクトのデータを使用します (また、しばしば表示します)。 Phoneword アプリケーションでは、バインディング ターゲットは電話番号を表示する [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) コントロールであり、バインディング ソースは `PhoneNumbers` コレクションです。

`PhoneNumbers` コレクションは `App` クラスで宣言され、初期化されます。次のコード例をご覧ください。

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) インスタンスは `CallHistoryPage` クラスで宣言され、初期化されます。次のコード例をご覧ください。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    ...
    <ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
    </ContentPage.Content>
</ContentPage>
```

この例では、[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) コントロールは、[`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) プロパティのバインド先となるデータの `IEnumerable` コレクションを表示します。 データのコレクションはどのような種類のオブジェクトにもなれますが、既定では、`ListView` は各項目の `ToString` メソッドを利用し、その項目を表示します。 [`x:Static`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) マークアップ拡張は、`local` 名前空間にある `App` クラスの静的 `PhoneNumbers` プロパティに `ItemsSource` プロパティをバインドすることを示すために利用されます。

データ バインディングの詳細については、「[Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」 (データ バインディングの基礎) を参照してください。 XAML マークアップ拡張機能の詳細については、「[XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)」を参照してください。

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword で導入されているその他の概念

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) は、画面に項目のコレクションを表示します。 `ListView` の各項目は 1 つのセルに含まれます。 `ListView` コントロールの詳細については、「[ListView](~/xamarin-forms/user-interface/listview/index.md)」を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションのページ ナビゲーションとデータ バインディングを紹介し、マルチスクリーンのプラットフォーム非依存アプリケーションでそれを利用する方法を示しました。
