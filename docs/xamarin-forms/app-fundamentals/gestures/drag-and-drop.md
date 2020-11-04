---
title: ドラッグ アンド ドロップ ジェスチャ認識エンジンを追加する
description: この記事では、Xamarin.Forms を使用してドラッグ アンド ドロップ ジェスチャを認識する方法について説明します。
ms.prod: xamarin
ms.assetid: 4CB2F270-908A-4A89-B852-70BC04066E8C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: af56e84598f73693a8cb0e93573b789a716c194a
ms.sourcegitcommit: 1550019cd1e858d4d13a4ae6dfb4a5947702f24b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/28/2020
ms.locfileid: "92897469"
---
# <a name="add-drag-and-drop-gesture-recognizers"></a>ドラッグ アンド ドロップ ジェスチャ認識エンジンを追加する

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/)

ドラッグ アンド ドロップ ジェスチャを使用すると、項目とそれに関連付けられているデータ パッケージを、連続するジェスチャを使用して、画面上のある位置から別の位置にドラッグできます。 ドラッグ アンド ドロップは 1 つのアプリケーション内で行うことも、あるアプリケーションで開始して別で終了することもできます。

> [!IMPORTANT]
> Xamarin.Forms のドラッグ アンド ドロップ ジェスチャ認識エンジンは現在試験段階であり、`DragAndDrop_Experimental` フラグを設定することによってのみ使用できます。 詳しくは、[試験的なフラグ](~/xamarin-forms/internals/experimental-flags.md)に関する記事を参照してください。
>
> ドラッグ アンド ドロップ ジェスチャの認識は、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) でサポートされています。 ただし、iOS では、iOS 11 以降のプラットフォームが必要です。

ドラッグ ジェスチャが開始される要素である " *ドラッグ ソース* " では、データ パッケージ オブジェクトを設定することにより、転送されるデータを提供できます。 ドラッグ ソースが解放されると、ドロップが発生します。 その後、ドラッグ ソースの下にある要素である " *ドロップ ターゲット* " により、データ パッケージが処理されます。

アプリケーションでドラッグ アンド ドロップを有効にするプロセスは次のとおりです。

1. `DragGestureRecognizer` オブジェクトを `GestureRecognizers` コレクションに追加し、`DragGestureRecognizer.CanDrag` プロパティを `true` に設定することによって、要素のドラッグを有効にします。 詳細については、「[ドラッグを有効にする](#enable-drag)」を参照してください。
1. (省略可能) データ パッケージを構築します。 画像コントロールとテキスト コントロールの場合は Xamarin.Forms によってデータ パッケージが自動的に設定されますが、他のコンテンツについては、ユーザー独自のデータ パッケージを作成する必要があります。 詳細については、「[データ パッケージを作成する](#build-a-data-package)」を参照してください。
1. `DropGestureRecognizer` オブジェクトを `GestureRecognizers` コレクションに追加し、`DropGestureRecognizer.AllowDrop` プロパティを `true` に設定することによって、要素のドロップを有効にします。 詳細については、「[ドロップを有効にする](#enable-drop)」を参照してください。
1. (省略可能) `DropGestureRecognizer.DragOver` イベントを処理することにより、ドロップ ターゲットによって許可される操作の種類を示します。 詳細については、「[DragOver イベントを処理する](#handle-the-dragover-event)」を参照してください。
1. (省略可能) データ パッケージを処理し、ドロップされたコンテンツを受け取ります。 画像データとテキスト データは Xamarin.Forms によってデータ パッケージから自動的に取得されますが、他のコンテンツについてはユーザーがデータ パッケージを処理する必要があります。 詳細については、「[データ パッケージを処理する](#process-the-data-package)」を参照してください。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) への、またはそこからの項目のドラッグは、現在サポートされていません。

## <a name="enable-drag"></a>ドラッグを有効にする

Xamarin.Forms では、ドラッグ ジェスチャの認識は `DragGestureRecognizer` クラスによって提供されます。 このクラスでは、次のプロパティが定義されています。

- `CanDrag`: `bool` 型、ジェスチャ認識エンジンがアタッチされている要素をドラッグ ソースにすることができるかどうかを示します。 このプロパティの既定値は `false` です。
- `DragStartingCommand`: `ICommand`型、ドラッグ ジェスチャが最初に認識されたときに実行されます。
- `DragStartingCommandParameter`: `object` 型、`DragStartingCommand` に渡されるパラメーター。
- `DropCompletedCommand`: `ICommand`型、ドラッグ ソースがドロップされたときに実行されます。
- `DropCompletedCommandParameter`: `object` 型、`DropCompletedCommand` に渡されるパラメーター。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

`DragGestureRecognizer` クラスでは、`DragStarting` イベントと `DropCompleted` イベントも定義されています。 `DragGestureRecognizer` オブジェクトによってドラッグ ジェスチャが検出されると、`DragStartingCommand` が実行されて、`DragStarting` イベントが呼び出されます。 その後、`DragGestureRecognizer` オブジェクトによってドロップ ジェスチャの完了が検出されると、`DropCompletedCommand` が実行されて、`DropCompleted` イベントが呼び出されます。

`DragStarting` イベントに付随する `DragStartingEventArgs` オブジェクトでは、次のプロパティが定義されています。

- `Handled`:`bool` 型、イベント ハンドラーによってイベントが処理されたかどうか、または Xamarin.Forms による独自の処理を続行する必要があるかどうかを示します。
- `Cancel`: `bool` 型、イベントを取り消す必要があるかどうかを示します。
- `Data`: `DataPackage` 型、ドラッグ ソースに付随するデータ パッケージを示します。 これは、読み取り専用プロパティです。

`DropCompleted` イベントに付随する `DropCompletedEventArgs` オブジェクトには、`DataPackageOperation` 型の読み取り専用プロパティ `DropResult` があります。 `DataPackageOperation` 列挙型の詳細については、「[DragOver イベントを処理する](#handle-the-dragover-event)」を参照してください。

次に示す XAML の例は、[`Image`](xref:Xamarin.Forms.Image) に関連付けられている `DragGestureRecognizer` です。

```xaml
<Image Source="monkeyface.png">
    <Image.GestureRecognizers>
        <DragGestureRecognizer CanDrag="True" />
    </Image.GestureRecognizers>
</Image>
```

この例では、[`Image`](xref:Xamarin.Forms.Image) でドラッグ ジェスチャを開始できます。

> [!TIP]
> iOS、Android、UWP では、ドラッグ ジェスチャは長押しの後でドラッグすることによって開始されます。

`DragGestureRecognizer` コマンドの使用例については、[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/)を参照してください。

## <a name="build-a-data-package"></a>データ パッケージを作成する

Xamarin.Forms を使用すると、次のコントロールでドラッグが開始された時点で、データ パッケージが自動的に作成されます。

- テキスト コントロール。 [`CheckBox`](xref:Xamarin.Forms.CheckBox)、[`DatePicker`](xref:Xamarin.Forms.DatePicker)、[`Editor`](xref:Xamarin.Forms.Editor)、[`Entry`](xref:Xamarin.Forms.Entry)、[`Label`](xref:Xamarin.Forms.Label)、[`RadioButton`](xref:Xamarin.Forms.RadioButton)、[`Switch`](xref:Xamarin.Forms.Switch)、[`TimePicker`](xref:Xamarin.Forms.TimePicker) の各オブジェクトから、テキスト値をドラッグできます。
- 画像コントロール。 [`Button`](xref:Xamarin.Forms.Button)、[`Image`](xref:Xamarin.Forms.Image)、[`ImageButton`](xref:Xamarin.Forms.ImageButton) の各コントロールから、画像をドラッグできます。

次の表では、テキスト コントロールでドラッグが開始されたときに読み取られるプロパティと、試みられる変換を示します。

| コントロール | プロパティ | 変換 |
| --- | --- | --- |
| `CheckBox` | `IsChecked` | `string` に変換された `bool`。 |
| `DatePicker` | `Date` | `string` に変換された `DateTime`。 |
| `Editor` | `Text` ||
| `Entry` | `Text` ||
| `Label` | `Text` ||
| `RadioButton` | `IsChecked` | `string` に変換された `bool`。 |
| `Switch` | `IsToggled` | `string` に変換された `bool`。 |
| `TimePicker` | `Time` | `string` に変換された `TimeSpan`。 |

テキストと画像以外のコンテンツの場合は、データ パッケージを自分で作成する必要があります。

データ パッケージは `DataPackage` クラスによって表され、次のプロパティが定義されています。

- `Properties`: `DataPackagePropertySet`型、`DataPackage` に含まれるデータを構成するプロパティのコレクションです。 このプロパティは、読み取り専用のプロパティです。
- `Image`: [`ImageSource`](xref:Xamarin.Forms.ImageSource) 型、`DataPackage` に含まれる画像です。
- `Text`: `string` 型、`DataPackage` に含まれるテキストです。
- `View`: `DataPackageView` 型、`DataPackage` の読み取り専用バージョンです。

`DataPackagePropertySet` クラスは、`Dictionary<string,object>` として格納されるプロパティ バッグを表します。 `DataPackageView` クラスの詳細については、「[データ パッケージを処理する](#process-the-data-package)」を参照してください。

### <a name="store-image-or-text-data"></a>画像データまたはテキスト データを格納する

画像データまたはテキスト データをドラッグ ソースと関連付けるには、`DataPackage.Image` または `DataPackage.Text` プロパティにデータを格納します。 これは、`DragStarting` イベントのハンドラー内で行うことができます。

次の XAML の例では、`DragStarting` イベントのハンドラーを登録する `DragGestureRecognizer` を示します。

```xaml
<Path Stroke="Black"
      StrokeThickness="4">
    <Path.GestureRecognizers>
        <DragGestureRecognizer CanDrag="True"
                               DragStarting="OnDragStarting" />
    </Path.GestureRecognizers>
    <Path.Data>
        <!-- PathGeometry goes here -->
    </Path.Data>
</Path>
```

この例では、`DragGestureRecognizer` が `Path` オブジェクトにアタッチされます。 `Path` でドラッグ ジェスチャが検出されると `DragStarting` イベントが生成されて、`OnDragStarting` イベント ハンドラーが実行されます。

```csharp
void OnDragStarting(object sender, DragStartingEventArgs e)
{
    e.Data.Text = "My text data goes here";
}
```

`DragStarting` イベントに付随する `DragStartingEventArgs` オブジェクトには、`DataPackage` 型の `Data` プロパティがあります。 この例では、`DataPackage` オブジェクトの `Text` プロパティが `string` に設定されます。 その後、ドロップ時に `DataPackage` にアクセスして、`string` を取得できます。

### <a name="store-data-in-the-property-bag"></a>プロパティ バッグにデータを格納する

画像やテキストを含むどのようなデータでも、`DataPackage.Properties` コレクションにデータを格納することにより、ドラッグ ソースと関連付けることができます。 これは、`DragStarting` イベントのハンドラー内で行うことができます。

次の XAML の例では、`DragStarting` イベントのハンドラーを登録する `DragGestureRecognizer` を示します。

```xaml
<Rectangle Stroke="Red"
           Fill="DarkBlue"
           StrokeThickness="4"
           HeightRequest="200"
           WidthRequest="200">
    <Rectangle.GestureRecognizers>
        <DragGestureRecognizer CanDrag="True"
                               DragStarting="OnDragStarting" />
    </Rectangle.GestureRecognizers>
</Rectangle>
```

この例では、`DragGestureRecognizer` が `Rectangle` オブジェクトにアタッチされます。 `Rectangle` でドラッグ ジェスチャが検出されると `DragStarting` イベントが生成されて、`OnDragStarting` イベント ハンドラーが実行されます。

```csharp
void OnDragStarting(object sender, DragStartingEventArgs e)
{
    Shape shape = (sender as Element).Parent as Shape;
    e.Data.Properties.Add("Square", new Square(shape.Width, shape.Height));
}
```

`DragStarting` イベントに付随する `DragStartingEventArgs` オブジェクトには、`DataPackage` 型の `Data` プロパティがあります。 `Dictionary<string, object>` コレクションである `DataPackage` オブジェクトの `Properties` コレクションを変更して、任意の必要なデータを格納することができます。 この例では、`Rectangle` のサイズを表す、"Square" キーに対する `Square` オブジェクトを格納するために、`Properties` ディクショナリが変更されています。

## <a name="enable-drop"></a>ドロップを有効にする

Xamarin.Forms では、ドロップ ジェスチャの認識は `DropGestureRecognizer` クラスによって提供されます。 このクラスでは、次のプロパティが定義されています。

- `AllowDrop`: `bool` 型、ジェスチャ認識エンジンがアタッチされている要素をドロップ ターゲットにすることができるかどうかを示します。 このプロパティの既定値は `false` です。
- `DragOverCommand`: `ICommand`型、ドラッグ ソースがドロップ ターゲット上にドラッグされたときに実行されます。
- `DragOverCommandParameter`: `object` 型、`DragOverCommand` に渡されるパラメーター。
- `DropCommand`: `ICommand`型、ドラッグ ソースがドロップ ターゲットにドロップされたときに実行されます。
- `DropCommandParameter`: `object` 型、`DropCommand` に渡されるパラメーター。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

`DropGestureRecognizer` クラスでは、`DragOver` イベントと `Drop` イベントも定義されています。 `DropGestureRecognizer` によってドロップ ターゲット上でドラッグ ソースが認識されると、`DragOverCommand` が実行されて、`DragOver` イベントが呼び出されます。 その後、`DropGestureRecognizer` によってドロップ ターゲット上でドロップ ジェスチャが認識されると、`DropCommand` が実行されて、`Drop` イベントが呼び出されます。

`DragOver` イベントに付随する `DragEventArgs` クラスでは、次のプロパティが定義されています。

- `Data`: `DataPackage` 型、ドラッグ ソースに関連付けられたデータが格納されます。 このプロパティは読み取り専用です。
- `AcceptedOperation`: `DataPackageOperation` 型、ドロップ ターゲットによって許可されている操作を指定します。

`DataPackageOperation` 列挙型の詳細については、「[DragOver イベントを処理する](#handle-the-dragover-event)」を参照してください。

`Drop` イベントに付随する `DropEventArgs` クラスでは、次のプロパティが定義されています。

- `Data`: `DataPackageView` 型、データ パッケージの読み取り専用バージョンです。
- `Handled`:`bool` 型、イベント ハンドラーによってイベントが処理されたかどうか、または Xamarin.Forms による独自の処理を続行する必要があるかどうかを示します。

次に示す XAML の例は、[`Image`](xref:Xamarin.Forms.Image) に関連付けられている `DropGestureRecognizer` です。

```xaml
<Image BackgroundColor="Silver"
       HeightRequest="300"
       WidthRequest="250">
    <Image.GestureRecognizers>
        <DropGestureRecognizer AllowDrop="True" />
    </Image.GestureRecognizers>
</Image>
```

この例では、ドラッグ ソースが [`Image`](xref:Xamarin.Forms.Image) ドロップ ターゲットにドロップされると、ドラッグ ソースが [`ImageSource`](xref:Xamarin.Forms.ImageSource) である場合は、ドラッグ ソースがドロップ ターゲットにコピーされます。 これは、Xamarin.Forms によって、ドラッグされた画像とテキストが互換性のあるドロップ ターゲットに自動的にコピーされるためです。

`DropGestureRecognizer` コマンドの使用例については、[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/)を参照してください。

## <a name="handle-the-dragover-event"></a>DragOver イベントを処理する

必要に応じて `DropGestureRecognizer.DragOver` イベントを処理し、ドロップ ターゲットによって許可されている操作の種類を示すことができます。 これを実現するには、`DragOver` イベントに付随する `DragEventArgs` オブジェクトの `DataPackageOperation` 型の `AcceptedOperation` プロパティを設定します。

`DataPackageOperation` 列挙体を使って、次のメンバーを定義できます。

- `None` は、何も実行されないことを示します。
- `Copy` は、ドラッグ ソースのコンテンツがドロップ ターゲットにコピーされることを示します。

> [!IMPORTANT]
> `DragEventArgs` オブジェクトが作成されるとき、`AcceptedOperation` プロパティの既定値は `DataPackageOperation.Copy` に設定されます。

次の XAML の例では、`DragOver` イベントのハンドラーを登録する `DropGestureRecognizer` を示します。

```xaml
<Image BackgroundColor="Silver"
       HeightRequest="300"
       WidthRequest="250">
    <Image.GestureRecognizers>
        <DropGestureRecognizer AllowDrop="True"
                               DragOver="OnDragOver" />
    </Image.GestureRecognizers>
</Image>
```

この例では、`DropGestureRecognizer` が [`Image`](xref:Xamarin.Forms.Image) オブジェクトにアタッチされます。 `DragOver` イベントは、ドラッグ ソースがドロップ ターゲットの上にドラッグされただけで、ドロップされていない場合に発生し、`OnDragOver` イベント ハンドラーが実行されます。

```csharp
void OnDragOver(object sender, DragEventArgs e)
{
    e.AcceptedOperation = DataPackageOperation.None;
}
```

この例では、`DragEventArgs` オブジェクトの `AcceptedOperation` プロパティが `DataPackageOperation.None` に設定されます。 これにより、ドラッグ ソースがドロップ ターゲットにドロップされても、アクションは何も実行されません。

## <a name="process-the-data-package"></a>データ パッケージを処理する

`Drop` イベントは、ドロップ ターゲットの上でドラッグ ソースが解放されたときに発生します。 この場合、ドラッグ ソースが次のコントロールにドロップされると、Xamarin.Forms によってデータ パッケージからのデータの取得が自動的に試みられます。

- テキスト コントロール。 [`CheckBox`](xref:Xamarin.Forms.CheckBox)、[`DatePicker`](xref:Xamarin.Forms.DatePicker)、[`Editor`](xref:Xamarin.Forms.Editor)、[`Entry`](xref:Xamarin.Forms.Entry)、[`Label`](xref:Xamarin.Forms.Label)、[`RadioButton`](xref:Xamarin.Forms.RadioButton)、[`Switch`](xref:Xamarin.Forms.Switch)、[`TimePicker`](xref:Xamarin.Forms.TimePicker) の各オブジェクトに、テキスト値をドロップできます。
- 画像コントロール。 [`Button`](xref:Xamarin.Forms.Button)、[`Image`](xref:Xamarin.Forms.Image)、[`ImageButton`](xref:Xamarin.Forms.ImageButton) の各コントロールに、画像をドロップできます。

次の表では、テキスト コントロールにテキスト ベースのドラッグ ソースがドロップされたときに設定されるプロパティと、試みられる変換を示します。

| コントロール | プロパティ | 変換 |
| --- | --- | --- |
| `CheckBox` | `IsChecked` | `string` は `bool` に変換されます。 |
| `DatePicker` | `Date` | `string` は `DateTime` に変換されます。 |
| `Editor` | `Text` ||
| `Entry` | `Text` ||
| `Label` | `Text` ||
| `RadioButton` | `IsChecked` | `string` は `bool` に変換されます。 |
| `Switch` | `IsToggled` | `string` は `bool` に変換されます。 |
| `TimePicker` | `Time` | `string` は `TimeSpan` に変換されます。 |

テキストと画像以外のコンテンツの場合は、データ パッケージを自分で処理する必要があります。

`Drop` イベントに付随する `DropEventArgs` クラスでは、`DataPackageView` 型の `Data` プロパティが定義されています。 このプロパティは、データ パッケージの読み取り専用バージョンを表します。

### <a name="retrieve-image-or-text-data"></a>画像データまたはテキスト データを取得する

`DataPackageView` クラスで定義されているメソッドを使用して、`Drop` イベントのハンドラー内でデータ パッケージから画像データまたはテキスト データを取得できます。

`DataPackageView` クラスには、`GetImageAsync` メソッドと `GetTextAsync` メソッドが含まれています。 `GetImageAsync` メソッドを使用すると、`DataPackage.Image` プロパティに格納されたデータ パッケージから画像が取得され、`Task<ImageSource>` が返されます。 同様に、`GetTextAsync` メソッドを使用すると、`DataPackage.Text` プロパティに格納されたデータ パッケージからテキストが取得され、`Task<string>` が返されます。

次の例では、`Path` に対するデータ パッケージからテキストを取得する `Drop` イベント ハンドラーが示されています。

```csharp
async void OnDrop(object sender, DropEventArgs e)
{
    string text = await e.Data.GetTextAsync();

    // Perform logic to take action based on the text value.
}
```

この例では、`GetTextAsync` メソッドを使用してデータ パッケージからテキスト データが取得されます。 その後、テキスト値に基づくアクションを実行できます。

### <a name="retrieve-data-from-the-property-bag"></a>プロパティ バッグからデータを取得する

データ パッケージの `Properties` コレクションにアクセスすることにより、`Drop` イベントのハンドラー内でデータ パッケージから任意のデータを取得できます。

`DataPackageView` クラスでは、`DataPackagePropertySetView` 型の `Properties` プロパティが定義されています。 `DataPackagePropertySetView` クラスは、`Dictionary<string, object>` として格納される読み取り専用のプロパティ バッグを表します。

次の例では、`Rectangle` に対するデータ パッケージのプロパティ バッグからデータを取得する `Drop` イベント ハンドラーが示されています。

```csharp
void OnDrop(object sender, DropEventArgs e)
{
    Square square = (Square)e.Data.Properties["Square"];

    // Perform logic to take action based on retrieved value.
}
```

この例では、"Square" ディクショナリ キーを指定することにより、データ パッケージのプロパティ バッグから `Square` オブジェクトを取得します。 その後、取得された値に基づくアクションを実行できます。

## <a name="related-links"></a>関連リンク

- [ドラッグ アンド ドロップ ジェスチャ (サンプル)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-draganddropgesture/)
