---
title: Xamarin.Forms シェルのナビゲーション
description: Xamarin.Forms シェル アプリケーションでは、設定されたナビゲーション階層に従わなくても、アプリケーション内の任意のページへの移動を許可する URI ベースのナビゲーション操作を利用できます。
ms.prod: xamarin
ms.assetid: 57079D89-D1CB-48BD-9FEE-539CEC29EABB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f29bacf3546b2148a3d97c3c1ccaa44e02872be8
ms.sourcegitcommit: f2942b518f51317acbb263be5bc0c91e66239f50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94590312"
---
# <a name="no-locxamarinforms-shell-navigation"></a>Xamarin.Forms シェルのナビゲーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms シェルには、設定されたナビゲーション階層に従わなくても、アプリケーション内の任意のページに移動するルートを使用する URI ベースのナビゲーション操作が用意されています。 さらに、これにより、ナビゲーション スタックのすべてのページにアクセスすることなく、後方に移動する機能も提供されます。

`Shell` では、次のようなナビゲーション関連のプロパティを定義しています。

- `BackButtonBehavior`: `BackButtonBehavior`型、戻るボタンの動作を定義する添付プロパティ。
- `CurrentItem`: `FlyoutItem`型、現在選択されている `FlyoutItem`。
- `CurrentState`: `ShellNavigationState`型、`Shell` の現在のナビゲーションの状態。
- `Current`: `Shell` 型、`Application.Current.MainPage` の型キャストした別名。

`BackButtonBehavior`、`CurrentItem`、および `CurrentState` プロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、これらのプロパティは、データ バインディングの対象になる場合があります。

ナビゲーションは、`Shell` クラスから、`GoToAsync` メソッドを呼び出すことで実行されます。 ナビゲーションが実行されるときに `Navigating` イベントが発生し、ナビゲーションが完了するときに `Navigated` イベントが発生します。

> [!NOTE]
> Xamarin.Forms シェル アプリケーション内では、[Navigation](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティを使って引き続きナビゲーションを実行できます。 詳しくは、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」をご覧ください。

## <a name="routes"></a>ルート

ナビゲーションは、移動先の URI を指定して、シェル アプリケーション内で実行されます。 ナビゲーション URI には、3 つの要素を含めることができます。

- "*ルート*"。シェルの視覚階層の一部として存在するコンテンツへのパスを定義します。
- "*ページ*"。 シェルの視覚階層に存在しないページは、シェル アプリケーション内の任意の場所からナビゲーション スタック上にプッシュできます。 たとえば、項目の詳細ページは、シェルの視覚階層には定義されませんが、必要に応じてナビゲーション スタック上にプッシュできます。
- 1 つまたは複数の "*クエリ パラメーター*"。 クエリ パラメーターは、ナビゲーション中に移動先ページに渡すことができるパラメーターです。

ナビゲーション URI に 3 つすべての要素を含めると、構造は //route/page?queryParameters にようになります。

### <a name="register-routes"></a>ルートを登録する

ルートは、`Route` プロパティを利用して、`FlyoutItem`、`Tab`、および `ShellContent` オブジェクトに対して定義できます。

```xaml
<Shell ...>
    <FlyoutItem ...
                Route="animals">
        <Tab ...
             Route="domestic">
            <ShellContent ...
                          Route="cats" />
            <ShellContent ...
                          Route="dogs" />
        </Tab>
        <ShellContent ...
                      Route="monkeys" />
        <ShellContent ...
                      Route="elephants" />  
        <ShellContent ...
                      Route="bears" />
    </FlyoutItem>
    <ShellContent ...
                  Route="about" />                  
    ...
</Shell>
```

> [!NOTE]
> シェルの階層内にあるすべての項目には、関連付けられているルートがあります。 開発者によってルートが設定されない場合、ルートは実行時に生成されます。 ただし、異なるアプリケーション セッションにわたって、生成されたルートに一貫性があるという保証はありません。

この例では、サンプル アプリケーションから、プログラムによるナビゲーションに使用できる次のルート階層を作成します。

```
animals
  domestic
    cats
    dogs
  monkeys
  elephants
  bears
about
```

`dogs` ルートの `ShellContent` オブジェクトに移動するには、絶対ルート URI は `//animals/domestic/dogs` になります。 同様に、`about` ルートの `ShellContent` オブジェクトに移動するには、絶対ルート URI は `//about` になります。

> [!IMPORTANT]
> ルートの重複が検出された場合、アプリケーションの起動時に `ArgumentException` がスローされます。 この例外は、階層内の同じレベルにある 2 つ以上のルートが 1 つのルート名を共有している場合にもスローされます。

#### <a name="register-page-routes"></a>ページのルートを登録する

シェルのサブクラス コンストラクター内、またはルートが呼び出される前に実行される他の任意の場所には、シェルの視覚階層に表されない任意のページへの追加のルートを、明示的に登録できます。

```csharp
Routing.RegisterRoute("monkeydetails", typeof(MonkeyDetailPage));
Routing.RegisterRoute("beardetails", typeof(BearDetailPage));
Routing.RegisterRoute("catdetails", typeof(CatDetailPage));
Routing.RegisterRoute("dogdetails", typeof(DogDetailPage));
Routing.RegisterRoute("elephantdetails", typeof(ElephantDetailPage));
```

この例では、シェルのサブクラスに定義さていない項目の詳細ページを、ルートとして登録します。 これらのページには、URI ベースのナビゲーションを使用して、アプリケーション内の任意の場所から移動できます。 このようなページのルートは、"*グローバル ルート*" と呼ばれます。

> [!NOTE]
> `Routing.RegisterRoute` メソッドを使ってルートが登録されたページは、必要に応じて、`Routing.UnRegisterRoute` メソッドを使って登録解除できます。

または、必要に応じて、別のルート階層にページを登録できます。

```csharp
Routing.RegisterRoute("monkeys/details", typeof(MonkeyDetailPage));
Routing.RegisterRoute("bears/details", typeof(BearDetailPage));
Routing.RegisterRoute("cats/details", typeof(CatDetailPage));
Routing.RegisterRoute("dogs/details", typeof(DogDetailPage));
Routing.RegisterRoute("elephants/details", typeof(ElephantDetailPage));
```

この例では、`monkeys` ルートのページから `details`ルートに移動すると `MonkeyDetailPage` が表示される、コンテキストに応じたページ ナビゲーションを実現しています。 同様に、`elephants` ルートのページから `details` ルートに移動すると `ElephantDetailPage` が表示されます。

> [!IMPORTANT]
> `Routing.RegisterRoute` メソッドが 2 つ以上の異なる型に同じルートを登録しようとすると、`ArgumentException` がスローされます。

## <a name="perform-navigation"></a>ナビゲーションを実行する

ナビゲーションを実行するには、最初に `Shell` サブクラスへの参照を取得する必要があります。 この参照は、`App.Current.MainPage` プロパティを `Shell` オブジェクトにキャストするか、または `Shell.Current` プロパティを経由して取得できます。 その後、`Shell` オブジェクト上の `GoToAsync` メソッドを呼び出すことで、ナビゲーションを実行できます。 このメソッドでは `ShellNavigationState` へ移動して、ナビゲーションのアニメーションが終わったら完了する `Task` を返します。 `ShellNavigationState` オブジェクトは、`string` または `Uri` から、`GoToAsync` メソッドによって構成され、`string` または `Uri` 引数に設定された `Location` プロパティを備えています。

シェルの視覚階層のルートに移動する場合、ナビゲーション スタックは作成されません。 しかし、シェルの視覚階層にはないページに移動する場合は、ナビゲーション スタックが作成されます。

> [!NOTE]
> `Shell` の現在のナビゲーションの状態は、`Shell.Current.CurrentState` プロパティ経由で取得できます。`Location` プロパティに、表示されるルートの URI が含まれます。

### <a name="absolute-routes"></a>絶対ルート

有効な絶対 URI を `GoToAsync` メソッドへの引数として指定することで、ナビゲーションを実行できます。

```csharp
await Shell.Current.GoToAsync("//animals/monkeys");
```

この例では、`ShellContent` オブジェクトに対して定義されたルートを使って、`monkeys` ルートのページへ移動します。 `monkeys` ルートを表している `ShellContent` オブジェクトは `FlyoutItem` オブジェクトの子であり、そのルートは `animals` になっています。

### <a name="relative-routes"></a>相対ルート

有効な相対 URI を `GoToAsync` メソッドへの引数として指定することで、ナビゲーションを実行することもできます。 ルーティング システムでは、`ShellContent` オブジェクトの URI との一致を試みます。 そのため、アプリケーション内のすべてのルートが一意の場合は、相対 URI として一意のルート名を指定するだけで、ナビゲーションを実行できます。

次の相対ルート形式がサポートされます。

| Format | 説明 |
| --- | --- |
| *route* | 現在の位置から上位方向に、指定されたルートを求めてルート階層が検索されます。 一致するページがナビゲーション スタックにプッシュされます。 |
| /*route* | 現在の位置から下位方向に、指定されたルートを求めてルート階層が検索されます。 一致するページがナビゲーション スタックにプッシュされます。 |
| //*route* | 現在の位置から上位方向に、指定されたルートを求めてルート階層が検索されます。 一致するページによって、ナビゲーション スタックが置き換えられます。 |
| ///*route* | 現在の位置から下位方向に、指定されたルートを求めてルート階層が検索されます。 一致するページによって、ナビゲーション スタックが置き換えられます。 |

次の例では、`monkeydetails` ルートのページに移動します。

```csharp
await Shell.Current.GoToAsync("monkeydetails");
```

この例では、一致するページが見つかるまで、`monkeyDetails` ルートが階層の上位方向に検索されます。 該当するページが見つかったら、ナビゲーション スタックにプッシュされます。

#### <a name="contextual-navigation"></a>コンテキストに応じたナビゲーション

相対ルートでは、コンテキストに応じたナビゲーションが可能です。 たとえば、次のようなルート階層を検討します。

```
monkeys
  details
bears
  details
```

`monkeys` ルートに登録したページが表示されているときに、`details` ルートに移動すると、`monkeys/details` ルートに登録したページが表示されます。 同様に、`bears` ルートに登録したページが表示されているときに、`details` ルートに移動すると、`bears/details` ルートに登録したページが表示されます。 この例でのルートの登録方法については、「[ページのルートを登録する](#register-page-routes)」をご覧ください。

### <a name="backwards-navigation"></a>後方ナビゲーション

".." を `GotoAsync` メソッドへの引数として指定することで、後方ナビゲーションを実行できます。

```csharp
await Shell.Current.GoToAsync("..");
```

".." を使用した後方ナビゲーションは、ルートと組み合わせることもできます。

```csharp
await Shell.Current.GoToAsync("../route");
```

この例では、後方ナビゲーションが実行されてから、指定したルートへ移動します。

> [!IMPORTANT]
> 後方ナビゲーション後に指定したルートに移動するには、指定したルートに移動するように、後方ナビゲーションによってルート階層の現在の場所に移動する必要があります。

同様に、後方に複数回移動してから、指定したルートに移動することもできます。

```csharp
await Shell.Current.GoToAsync("../../route");
```

この例では、後方ナビゲーションが 2 回実行されてから、指定したルートに移動します。

さらに、後方に移動する場合は、クエリのプロパティを介してデータを渡すこともできます。

```csharp
await Shell.Current.GoToAsync($"..?parameterToPassBack={parameterValueToPassBack}");
```

この例では、後方ナビゲーションが実行されてから、クエリ パラメーターの値が前のページのクエリ パラメーターに渡されます。

> [!NOTE]
> クエリ パラメーターは、任意の後方ナビゲーション要求に追加することができます。

移動時にデータを渡す方法の詳細については、「[データを渡す](#pass-data)」を参照してください。

### <a name="invalid-routes"></a>無効なルート

次のルート形式は無効です。

| 形式 | 説明 |
| --- | --- |
| *route* または /*route* | 視覚階層にあるルートは、ナビゲーション スタック上にプッシュできません。 |
| //*page* または ///*page* | 現在、グローバル ルートをナビゲーション スタック上の唯一のページにはできません。 そのため、グローバル ルートへの絶対ルーティングはサポートされていません。 |

これらのルート形式のいずれかを使用すると、`Exception` がスローされます。

> [!IMPORTANT]
> 存在しないルートへのナビゲーションを試行すると、`ArgumentException` 例外がスローされます。

### <a name="debugging-navigation"></a>ナビゲーションをデバッグする

一部の Shell クラスは、クラスまたはフィールドがデバッガーによって表示される方法を指定する `DebuggerDisplayAttribute` によって修飾されています。 これにより、ナビゲーション要求に関連付けられたデータを表示して、ナビゲーション要求をデバッグできます。 たとえば、次のスクリーンショットには、`Shell.Current` オブジェクトの `CurrentItem` および `CurrentState` プロパティが表示されています。

![デバッガーのスクリーンショット](navigation-images/debugger.png "デバッガー")

この例では、`FlyoutItem` 型の `CurrentItem` プロパティによって、`FlyoutItem` オブジェクトのタイトルとルートを表示します。 同様に、`ShellNavigationState` 型の `CurrentState` プロパティによって、Shell アプリケーション内で表示されるルートの URI を表示します。

### <a name="tab-class"></a>Tab クラス

`Tab` クラスでは、`Tab` 内に現在プッシュされているナビゲーション スタックを表す `IReadOnlyList<Page>` 型の `Stack` プロパティを定義します。 また、クラスでは、オーバーライドできる次のようなナビゲーション メソッドを提供しています。

- `GetNavigationStack`: 現在のナビゲーション スタックを示す `IReadOnlyList<Page`> を返します。
- `OnInsertPageBefore`: `INavigation.InsertPageBefore` が呼び出されたときに、呼び出されます。
- `OnPopAsync`: `Task<Page>` を返し、`INavigation.PopAsync` が呼び出されたときに、呼び出されます。
- `OnPopToRootAsync`: `Task` を返し、`INavigation.OnPopToRootAsync` が呼び出されたときに、呼び出されます。
- `OnPushAsync`: `Task` を返し、`INavigation.PushAsync` が呼び出されたときに、呼び出されます。
- `OnRemovePage`: `INavigation.RemovePage` が呼び出されたときに、呼び出されます。

たとえば、次のコード例は、`OnRemovePage` メソッドをオーバーライドする方法を示しています。

```csharp
public class MyTab : Tab
{
    protected override void OnRemovePage(Page page)
    {
        base.OnRemovePage(page);

        // Custom logic
    }
}
```

次に、`MyTab` オブジェクトは、`Tab` オブジェクト内ではなく、シェルのビジュアル階層内で使用できます。

## <a name="navigation-events"></a>ナビゲーション イベント

`Shell` クラスでは、プログラムによるナビゲーションまたはユーザー操作のどちらかに起因して、ナビゲーションが実行されるときに発生する `Navigating` イベントを定義しています。 `Navigating` イベントに伴う `ShellNavigatingEventArgs` オブジェクトでは、次のプロパティを提供しています。

| プロパティ | Type | 説明 |
|---|---|---|
| `Current` | `ShellNavigationState` | 現在のページの URI。 |
| `Source` | `ShellNavigationSource` | 発生したナビゲーションの種類。 |
| `Target` | `ShellNavigationState`  | ナビゲーションの発信先を表す URI。 |
| `CanCancel`  | `bool` | ナビゲーションをキャンセルできるかどうかを示す値。 |
| `Cancelled`  | `bool` | ナビゲーションがキャンセルされたかどうかを示す値。 |

さらに、`ShellNavigatingEventArgs` クラスでは、ナビゲーションのキャンセルに使用できる `Cancel` メソッドを提供しています。

また、`Shell` クラスでは、ナビゲーションが完了したときに発生する `Navigated` イベントも定義しています。 `Navigating` イベントに伴う `ShellNavigatedEventArgs` オブジェクトでは、次のプロパティを提供しています。

| プロパティ | Type | 説明 |
|---|---|---|
| `Current` | `ShellNavigationState` | 現在のページの URI。 |
| `Previous`| `ShellNavigationState` | 前のページの URI。 |
| `Source`  | `ShellNavigationSource` | 発生したナビゲーションの種類。 |

`ShellNavigatedEventArgs` および `ShellNavigatingEventArgs` クラスの両方に、`ShellNavigationSource` 型の `Source` プロパティがあります。 この列挙体では、次の値を提供しています。

- `Unknown`
- `Push`
- `Pop`
- `PopToRoot`
- `Insert`
- `Remove`
- `ShellItemChanged`
- `ShellSectionChanged`
- `ShellContentChanged`

そのため、`Navigating` イベント用のハンドラーでは、ナビゲーションを遮断して、ナビゲーション ソースに基づくアクションを実行することが可能です。 たとえば、次のコードでは、ページ上のデータが未保存の場合に、前に戻るナビゲーションをキャンセルする方法を示しています。

```csharp
void OnNavigating(object sender, ShellNavigatingEventArgs e)
{
    // Cancel back navigation if data is unsaved
    if (e.Source == ShellNavigationSource.Pop && !dataSaved)
    {
        e.Cancel();
    }
}
```

## <a name="pass-data"></a>データを渡す

データは、プログラムによるナビゲーションを実行するときに、クエリ パラメーターとして渡すことができます。 たとえば、ユーザーが `ElephantsPage`上の象を選択した場合に、サンプル アプリケーションでは次のコードが実行されます。

```csharp
async void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string elephantName = (e.CurrentSelection.FirstOrDefault() as Animal).Name;
    await Shell.Current.GoToAsync($"//animals/elephants/elephantdetails?name={elephantName}");
}
```

このコード サンプルでは、[`CollectionView`](xref:Xamarin.Forms.CollectionView) 内の現在選択されているゾウを取得し、`elephantName` をクエリ パラメーターとして渡して `elephantdetails` ルートに移動します。 クエリ パラメーターはナビゲーション用にエンコードされた URL になるため、"Indian Elephant" は "Indian%20Elephant" になることに注意してください。

データを受信するには、移動先ページを表すクラスまたはページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に対応するクラスが、各クエリ パラメーターの `QueryPropertyAttribute` によって修飾されている必要があります。

```csharp
[QueryProperty("Name", "name")]
public partial class ElephantDetailPage : ContentPage
{
    public string Name
    {
        set
        {
            BindingContext = ElephantData.Elephants.FirstOrDefault(m => m.Name == Uri.UnescapeDataString(value));
        }
    }
    ...
}
```

`QueryPropertyAttribute` の最初の引数には、データを受信するプロパティの名前を指定し、2 番目の引数にはクエリ パラメータ― ID を指定します。そのため、上の例にある `QueryPropertyAttribute`では、`GoToAsync` メソッドの呼び出しにおいて URI から `name`クエリ パラメータ―に渡されたデータを `Name` プロパティが受信するように、指定しています。 その後、`Name` プロパティはクエリ パラメータ―値を URL にデコードし、それを使用してページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に表示されるオブジェクトを設定します。

> [!NOTE]
> クラスは、複数の `QueryPropertyAttribute` オブジェクトによって修飾できます。

## <a name="back-button-behavior"></a>[戻る] ボタンの動作

`BackButtonBehavior` クラスでは、戻るボタンの外観と動作を制御する次のプロパティを定義します。

- `Command`: `ICommand`型、戻るボタンが押されたときに実行されます。
- `CommandParameter`: `object` 型、`Command`に渡されるパラメーターです。
- `IconOverride`: [`ImageSource`](xref:Xamarin.Forms.ImageSource) 型、戻るボタンに使用されるアイコンです。
- `boolean` 型の `IsEnabled`は、戻るボタンが有効かどうかを示します。 既定値は `true` です。
- `TextOverride`: `string` 型、戻るボタンに使用されるテキスト。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトによって支持されています。つまり、プロパティがデータ バインディングの対象になる場合があります。

`BackButtonBehavior` クラスは、`Shell.BackButtonBehavior` 添付プロパティを `BackButtonBehavior` オブジェクトに設定することで使用できます。

```xaml
<ContentPage ...>    
    <Shell.BackButtonBehavior>
        <BackButtonBehavior Command="{Binding BackCommand}"
                            IconOverride="back.png" />   
    </Shell.BackButtonBehavior>
    ...
</ContentPage>
```

同等の C# コードを次に示します。

```csharp
Shell.SetBackButtonBehavior(this, new BackButtonBehavior
{
    Command = new Command(() =>
    {
        ...
    }),
    IconOverride = "back.png"
});
```

`Command` プロパティは、戻るボタンが押されたときに実行されるように `ICommand`に設定され、`IconOverride` プロパティは、戻るボタンに使用されるアイコンに設定されます。

[![iOS および Android 上でシェルの戻るボタン アイコンをオーバーライドするスクリーンショット](navigation-images/back-button.png "シェルの戻るボタン アイコンのオーバーライド")](navigation-images/back-button-large.png#lightbox "シェルの戻るボタン アイコンのオーバーライド")

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
