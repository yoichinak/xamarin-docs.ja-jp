---
title: Xamarin.Forms C# マークアップ
description: C# マークアップは、c# で宣言型のユーザーインターフェイスを構築するプロセスを簡略化するための、fluent ヘルパーメソッドとクラスのオプトインセットです Xamarin.Forms 。
ms.prod: xamarin
ms.assetid: D41B9DCD-5C34-4C2F-B177-FC082AB2E9E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a07931bfa53a5e4d77c2755b08745b8dd962b695
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557154"
---
# <a name="no-locxamarinforms-c-markup"></a>Xamarin.Forms C# マークアップ

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)

C# マークアップは、c# で宣言型のユーザーインターフェイスを構築するプロセスを簡略化するための、fluent ヘルパーメソッドとクラスのオプトインセットです Xamarin.Forms 。 C# マークアップによって提供される fluent API は、名前空間で使用でき `Xamarin.Forms.Markup` ます。

XAML の場合と同様に、C# のマークアップでは、UI マークアップと UI ロジックを明確に分離できます。 これは、UI マークアップと UI ロジックを個別の部分クラスファイルに分割することで実現できます。 たとえば、ログインページの場合、UI マークアップは *LoginPage.cs*という名前のファイルにありますが、ui ロジックは *LoginPage.logic.cs*という名前のファイルにあります。

C# マークアップは4.6 から入手でき Xamarin.Forms ます。 ただし、現在は実験的であり、 *App.cs* ファイルに次のコード行を追加することによってのみ使用できます。

```csharp
Device.SetFlags(new string[]{ "Markup_Experimental" });
```

> [!NOTE]
> C# マークアップは、でサポートされているすべてのプラットフォームで使用でき Xamarin.Forms ます。

## <a name="basic-example"></a>基本的な例

次の例では、C# でとを含む新しいにページコンテンツを設定してい [`Grid`](xref:Xamarin.Forms.Grid) [`Label`](xref:Xamarin.Forms.Label) [`Entry`](xref:Xamarin.Forms.Entry) ます。

```csharp
Grid grid = new Grid();

Label label = new Label { Text = "Code: " };
grid.Children.Add(label, 0, 1);

Entry entry = new Entry
{
    Placeholder = "Enter number",
    Keyboard = Keyboard.Numeric,
    BackgroundColor = Color.AliceBlue,
    TextColor = Color.Black,
    FontSize = 15,
    HeightRequest = 44,
    Margin = fieldMargin
};
grid.Children.Add(entry, 0, 2);
Grid.SetColumnSpan(entry, 2);
entry.SetBinding(Entry.TextProperty, new Binding("RegistrationCode"));

Content = grid;
```

この例では [`Grid`](xref:Xamarin.Forms.Grid) 、子オブジェクトとオブジェクトを使用してオブジェクトを作成し [`Label`](xref:Xamarin.Forms.Label) [`Entry`](xref:Xamarin.Forms.Entry) ます。 は `Label` テキストを表示し、 `Entry` データは `RegistrationCode` ビューモデルのプロパティにバインドされます。 各子ビューは、の特定の行に表示されるように設定され、はの `Grid` `Entry` すべての列にまたがり `Grid` ます。 さらに、の高さは、 `Entry` キーボード、色、テキストのフォントサイズ、およびの高さと共に設定され `Margin` ます。 最後に、 `Page.Content` プロパティがオブジェクトに設定され `Grid` ます。

C# マークアップでは、fluent API を使用してこのコードを再記述できます。

```csharp
using Xamarin.Forms.Markup;
using static Xamarin.Forms.Markup.GridRowsColumns;

Content = new Grid
{
  Children =
  {
    new Label { Text = "Code:" }
               .Row (BodyRow.CodeHeader) .Column (BodyCol.Header),

    new Entry { Placeholder = "Enter number", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
               .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
               .Bind (nameof(vm.RegistrationCode))
  }
}};
```

この例は前の例と同じですが、C# のマークアップ fluent API を使用すると、c# で UI を構築するプロセスが単純化されます。

> [!NOTE]
> C# マークアップには、特定のビュープロパティを設定する拡張メソッドが含まれています。 これらの拡張メソッドは、すべてのプロパティ setter を置き換えることを意図したものではありません。 代わりに、コードの読みやすさを向上させるように設計されており、プロパティセッターと組み合わせて使用できます。 プロパティには常に拡張メソッドを使用することをお勧めしますが、適切なバランスを選択できます。

## <a name="data-binding"></a>データ バインディング

C# マークアップには、ビューのバインド可能な `Bind` プロパティと指定されたプロパティの間にデータバインディングを作成する拡張メソッドとオーバーロードが含まれています。 メソッドは、 `Bind` に含まれるほとんどのコントロールの既定のバインド可能プロパティを認識し Xamarin.Forms ます。 このため、このメソッドを使用する場合は、通常、ターゲットプロパティを指定する必要はありません。 ただし、追加のコントロールの既定のバインド可能なプロパティを登録することもできます。

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.Register(HoverButton.CommandProperty, RadialGauge.ValueProperty);
```

メソッドは、任意のバインド `Bind` 可能なプロパティにバインドするために使用できます。

```csharp
using Xamarin.Forms.Markup;
// ...

new Label { Text = "No data available" }
           .Bind (Label.IsVisibleProperty, nameof(vm.Empty))
```

さらに、この `BindCommand` 拡張メソッドは、 `Command` 1 つのメソッド呼び出しでコントロールの既定のプロパティとプロパティにバインドでき `CommandParameter` ます。

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap me" }
              .BindCommand (nameof(vm.TapCommand))
```

既定では、は `CommandParameter` バインドコンテキストにバインドされています。 バインドパスとバインドのソースを指定することもでき `Command` `CommandParameter` ます。

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap Me" }
              .BindCommand (nameof(vm.TapCommand), vm, nameof(Item.Id))
```

この例では、バインディングコンテキストはインスタンスなので、 `Item` バインディングのソースを指定する必要はありません `Id` `CommandParameter` 。

にバインドするだけの場合は、 `Command` `null` メソッドの引数にを渡すことができ `parameterPath` `BindCommand` ます。 または、メソッドを使用し `Bind` ます。

`Command`追加のコントロールの既定のプロパティとプロパティを登録することもでき `CommandParameter` ます。

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.RegisterCommand(
    (CustomViewA.CommandProperty, CustomViewA.CommandParameterProperty),
    (CustomViewB.CommandProperty, CustomViewB.CommandParameterProperty)
);
```

インラインコンバーターコードは、 `Bind` パラメーターとパラメーターを使用してメソッドに渡すことができ `convert` `convertBack` ます。

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth),
                  convert: (int depth) => new Thickness(depth * 20, 0, 0, 0))
```

タイプセーフなコンバーターパラメーターもサポートされています。

```csharp
using Xamarin.Forms.Markup;
//...

new Label { }
           .Bind (nameof(viewModel.Text),
                  convert: (string text, int repeat) => string.Concat(Enumerable.Repeat(text, repeat)))
```

さらに、コンバーターコードとインスタンスをクラスで再利用でき `FuncConverter` ます。

```csharp
using Xamarin.Forms.Markup;
//...

FuncConverter<int, Thickness> treeMarginConverter = new FuncConverter<int, Thickness>(depth => new Thickness(depth * 20, 0, 0, 0));
new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth), converter: treeMarginConverter),
```

クラスは、 `FuncConverter` 次のオブジェクトもサポートしてい `CultureInfo` ます。

```csharp
using Xamarin.Forms.Markup;
//...

cultureAwareConverter = new FuncConverter<DateTimeOffset, string, int>(
    (date, daysToAdd, culture) => date.AddDays(daysToAdd).ToString(culture)
);
```

プロパティで指定されたオブジェクトにデータをバインドすることもでき `Span` `FormattedText` ます。

```csharp
using Xamarin.Forms.Markup;
//...

new Label { } .FormattedText (
    new Span { Text = "Built with " },
    new Span { TextColor = Color.Blue, TextDecorations = TextDecorations.Underline }
              .BindTapGesture (nameof(vm.ContinueToCSharpForMarkupCommand))
              .Bind (nameof(vm.Title))
)
```

### <a name="gesture-recognizers"></a>ジェスチャ認識エンジン

`Command` および `CommandParameter` プロパティは `GestureElement` `View` `BindClickGesture` 、、、およびの各拡張メソッドを使用して、型および型にデータをバインドでき `BindSwipeGesture` `BindTapGesture` ます。

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .BindTapGesture (nameof(vm.TapCommand))
```

この例では、指定された型のジェスチャ認識エンジンを作成し、に追加し [`Label`](xref:Xamarin.Forms.Label) ます。 拡張メソッドには、 `Bind*Gesture` 拡張メソッドと同じパラメーターが用意されて `BindCommand` います。 ただし、では、既定で `Bind*Gesture` はバインドされません `CommandParameter` `BindCommand` 。

パラメーターを使用してジェスチャ認識エンジンを初期化するには、 `ClickGesture` 、、 `PanGesture` `PinchGesture` 、、およびの各拡張メソッドを使用し `SwipeGesture` `TapGesture` ます。

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .TapGesture (g => g.Bind(nameof(vm.DoubleTapCommand)).NumberOfTapsRequired = 2)
```

ジェスチャ認識エンジンはであるため `BindableObject` 、 `Bind` `BindCommand` 初期化時におよび拡張メソッドを使用できます。 拡張メソッドを使用してカスタムジェスチャ認識エンジンの種類を初期化することもでき `Gesture<TGestureElement, TGestureRecognizer>` ます。

## <a name="layout"></a>レイアウト

C# マークアップには、レイアウト内のビューの配置をサポートする一連のレイアウト拡張メソッドと、ビューのコンテンツが含まれています。

| Type | 拡張メソッド |
|---|---|
| `FlexLayout` | `AlignSelf`, `Basis`, `Grow`, `Menu`, `Order`, `Shrink` |
| `Grid` | `Row`, `Column`, `RowSpan`, `ColumnSpan` |
| `Label` | `TextLeft`, `TextCenterHorizontal`, `TextRight` <br/> `TextTop`, `TextCenterVertical`, `TextBottom` <br/> `TextCenter` |
| `Layout` | `Padding`, `Paddings` |
| `LayoutOptions` | `Left`, `CenterHorizontal`, `FillHorizontal`, `Right` <br/> `LeftExpand`, `CenterExpandHorizontal`, `FillExpandHorizontal`, `RightExpand` <br /> `Top`, `Bottom`, `CenterVertical`, `FillVertical` <br /> `TopExpand`, `BottomExpand`, `CenterExpandVertical`, `FillExpandVertical` <br /> `Center`, `Fill`, `CenterExpand`, `FillExpand` |
| `View` | `Margin`, `Margins` |
| `VisualElement` | `Height`, `Width`, `MinHeight`, `MinWidth`, `Size`, `MinSize` |

### <a name="left-to-right-and-right-to-left-support"></a>左から右、右から左へのサポート

左から右 (LTR) または右から左 (RTL) のフロー方向をサポートするように設計された C# マークアップでは、上に示した拡張メソッドによって、最も直感的な名前のセット `Left` (、、) が提供されます `Right` `Top` `Bottom` 。

左および右の拡張メソッドの適切なセットを使用できるようにするには、プロセス内で、マークアップが設計されているフロー方向を明確にするには、次の2つの `using` ディレクティブ `using Xamarin.Forms.Markup.LeftToRight;` (、または) のいずれかを指定し `using Xamarin.Forms.Markup.RightToLeft;` ます。

左から右方向と右から左方向のフロー方向をサポートするように設計された C# マークアップでは、上記の名前空間のいずれかではなく、次の表に示す拡張メソッドを使用することをお勧めします。

| Type | 拡張メソッド |
|---|---|
| `Label` | `TextStart`, `TextEnd` |
| `LayoutOptions` | `Start`, `End` <br/> `StartExpand`, `EndExpand` |

### <a name="layout-line-convention"></a>レイアウト行の規則

次の順序で、ビューのすべてのレイアウト拡張メソッドを1行に配置することをお勧めします。

1. ビューを含む行と列。
1. 行と列内のアラインメント。
1. ビューの周囲の余白。
1. ビューのサイズ。
1. ビュー内の埋め込み。
1. 埋め込み内でのコンテンツの配置。

次のコードは、この規則の例を示しています。

```csharp
new Label { }
           .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal () // Layout line
```

規則に従って一貫していることで、C# マークアップを迅速に読み取り、ビューコンテンツが UI に配置されている場所のメンタルマップを作成できます。

## <a name="grid-rows-and-columns"></a>Grid の行と列

列挙は [`Grid`](xref:Xamarin.Forms.Grid) 、数値を使用する代わりに、行と列を定義するために使用できます。 これには、行または列を追加または削除するときに、番号を振り直す必要がないという利点があります。

> [!IMPORTANT]
> [`Grid`](xref:Xamarin.Forms.Grid)列挙を使用して行と列を定義するには、次のディレクティブが必要です `using` 。`using static Xamarin.Forms.Markup.GridRowsColumns;`

次のコードは、 [`Grid`](xref:Xamarin.Forms.Grid) 列挙型を使用して行と列を定義および使用する方法の例を示しています。

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms.Markup.LeftToRight;
using static Xamarin.Forms.Markup.GridRowsColumns;
// ...

enum BodyRow
{
    Prompt,
    CodeHeader,
    CodeEntry,
    Button
}

enum BodyCol
{
    FieldLabel,
    FieldValidation
}

View Build() => new Grid
{
    RowDefinitions = Rows.Define(
        (BodyRow.Prompt    , 170 ),
        (BodyRow.CodeHeader, 75  ),
        (BodyRow.CodeEntry , Auto),
        (BodyRow.Button    , Auto)
    ),

    ColumnDefinitions = Columns.Define(
        (BodyCol.FieldLabel     , 160 ),
        (BodyCol.FieldValidation, Star)
    ),

    Children =
    {
        new Label { LineBreakMode = LineBreakMode.WordWrap } .Font (15) .Bold ()
                   .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal ()
                   .Bind (nameof(vm.RegistrationPrompt)),

        new Label { Text = "Registration code" } .Bold ()
                   .Row (BodyRow.CodeHeader) .Column(BodyCol.FieldLabel) .Bottom () .Margin (fieldNameMargin),

        new Label { } .Italic ()
                   .Row (BodyRow.CodeHeader) .Column (BodyCol.FieldValidation) .Right () .Bottom () .Margin (fieldNameMargin)
                   .Bind (nameof(vm.RegistrationCodeValidationMessage)),

        new Entry { Placeholder = "E.g. 123456", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
                   .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
                   .Bind (nameof(vm.RegistrationCode), BindingMode.TwoWay),

        new Button { Text = "Verify" } .Style (FilledButton)
                    .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (PageMarginSize)
                    .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode))
                    .Bind (nameof(vm.VerifyRegistrationCodeCommand)),
    }
};
```

さらに、列挙なしで行と列を簡潔に定義できます。

```csharp
new Grid
{
    RowDefinitions = Rows.Define (Auto, Star, 20),
    ColumnDefinitions = Columns.Define (Auto, Star, 20, 40)
    // ...
}
```

## <a name="fonts"></a>フォント

次の一覧のコントロールでは `FontSize` 、、、 `Bold` `Italic` 、およびの各拡張メソッドを呼び出し `Font` て、コントロールによって表示されるテキストの外観を設定できます。

- `Button`
- `DatePicker`
- `Editor`
- `Entry`
- `Label`
- `Picker`
- `SearchBar`
- `Span`
- `TimePicker`

## <a name="effects"></a>エフェクト

拡張メソッドを使用して、コントロールに効果を適用でき `Effect` ます。

```csharp
using Xamarin.Forms.Markup;
// ...

new Button { Text = "Tap Me" }
            .Effects (new ButtonMixedCaps())
```

## <a name="logic-integration"></a>ロジックの統合

`Invoke`拡張メソッドを使用して、C# マークアップでコードをインラインで実行できます。

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Invoke (l => l.ItemTapped += OnListViewItemTapped)
```

さらに、拡張メソッドを使用して、 `Assign` ui マークアップ (ui ロジックファイル内) の外部からコントロールにアクセスすることもできます。

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Assign (out MyListView)
```

## <a name="styles"></a>スタイル

次の例は、C# マークアップを使用して暗黙的および明示的なスタイルを作成する方法を示しています。

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms;

namespace CSharpForMarkupDemos
{
    public static class Styles
    {
        static Style<Button> buttons, filledButton;
        static Style<Label> labels;
        static Style<Span> link;

        #region Implicit styles

        public static ResourceDictionary Implicit => new ResourceDictionary { Buttons, Labels };

        public static Style<Button> Buttons => buttons ?? (buttons = new Style<Button>(
            (Button.HeightRequestProperty, 44),
            (Button.FontSizeProperty, 13),
            (Button.HorizontalOptionsProperty, LayoutOptions.Center),
            (Button.VerticalOptionsProperty, LayoutOptions.Center)
        ));

        public static Style<Label> Labels => labels ?? (labels = new Style<Label>(
            (Label.FontSizeProperty, 13),
            (Label.TextColorProperty, Color.Black)
        ));

        #endregion Implicit styles

        #region Explicit styles

        public static Style<Button> FilledButton => filledButton ?? (filledButton = new Style<Button>(
            (Button.TextColorProperty, Color.White),
            (Button.BackgroundColorProperty, Color.FromHex("#1976D2")),
            (Button.CornerRadiusProperty, 5)
        )).BasedOn(Buttons);

        public static Style<Span> Link => link ?? (link = new Style<Span>(
            (Span.TextColorProperty, Color.Blue),
            (Span.TextDecorationsProperty, TextDecorations.Underline)
        ));

        #endregion Explicit styles
    }
}
```

暗黙的なスタイルは、アプリケーションリソースディクショナリに読み込むことによって使用できます。

```csharp
public App()
{
    Resources = Styles.Implicit;
    // ...
}
```

明示的なスタイルは、拡張メソッドで使用でき `Style` ます。

```csharp
using static CSharpForMarkupExample.Styles;
// ...

new Button { Text = "Tap Me" } .Style (FilledButton),
```

> [!NOTE]
> 拡張メソッドに加えて、、、 `Style` 、およびの各拡張メソッドもあり `ApplyToDerivedTypes` `BasedOn` `Add` `CanCascade` ます。

または、独自のスタイル拡張メソッドを作成することもできます。

```csharp
public static TButton Filled<TButton>(this TButton button) where TButton : Button
{
    button.Buttons(); // Equivalent to Style .BasedOn (Buttons)
    button.TextColor = Color.White;
    button.BackgroundColor = Color.Red;
    return button;
}
```

拡張メソッドは次のように `Filled` 使用できます。

```csharp
new Button { Text = "Tap Me" } .Filled ()
```

## <a name="platform-specifics"></a>プラットフォーム固有設定

拡張メソッドを使用して、 `Invoke` プラットフォーム固有のを適用できます。 ただし、あいまいさのエラーを回避するには、 `using` 名前空間のディレクティブを直接含めないで `Xamarin.Forms.PlatformConfiguration.*Specific` ください。 代わりに、名前空間エイリアスを作成し、エイリアスを使用してプラットフォーム固有のを使用します。

```csharp
using Xamarin.Forms.Markup;
using PciOS = Xamarin.Forms.PlatformConfiguration.iOSSpecific;
// ...

new ListView { } .Invoke (l => PciOS.ListView.SetGroupHeaderStyle(l, PciOS.GroupHeaderStyle.Grouped))
```

さらに、特定のプラットフォームの詳細を頻繁に使用する場合は、独自の extensions クラスに対して fluent 拡張メソッドを作成できます。

```csharp
public static T iOSGroupHeaderStyle<T>(this T listView, PciOS.GroupHeaderStyle style) where T : Forms.ListView
{
  PciOS.ListView.SetGroupHeaderStyle(listView, style);
  return listView;
}
```

拡張メソッドは次のように使用できます。

```csharp
new ListView { } .iOSGroupHeaderStyle(PciOS.GroupHeaderStyle.Grouped)
```

プラットフォーム固有の詳細については、「 [Android プラットフォーム](~/xamarin-forms/platform/android/index.md)の機能」、「 [iOS プラットフォームの機能](~/xamarin-forms/platform/ios/index.md)」、および「 [Windows プラットフォームの機能](~/xamarin-forms/platform/windows/index.md)」を参照してください。

## <a name="recommended-convention"></a>推奨される規則

プロパティとヘルパーメソッドの推奨される順序とグループは次のとおりです。

- **目的**: コントロールの目的を識別する値を持つプロパティまたはヘルパーメソッド (、など `Text` `Placeholder` `Assign` )。
- [**その他**]: レイアウトまたはバインドではない、同じ行または複数の行にあるすべてのプロパティまたはヘルパーメソッド。
- **Layout**: レイアウトは、行と列、レイアウトオプション、余白、サイズ、埋め込み、およびコンテンツの配置の内側に並べられています。
- **Bind**: データバインディングは、1行に1つのバインドプロパティを持つメソッドチェーンの最後に実行されます。 バインド可能な *既定* のプロパティがバインドされている場合は、メソッドチェーンの最後に配置する必要があります。

次のコードは、この規則に従う例を示しています。

```csharp
new Button { Text = "Verify" /* purpose */ } .Style (FilledButton) // other
            .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (10) // layout
            .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode)) // bind
            .Bind (nameof(vm.VerifyRegistrationCodeCommand)), // bind default

new Label { }
           .Assign (out animatedMessageLabel) // purpose
           .Invoke (label => label.SizeChanged += MessageLabel_SizeChanged) // other
           .Row (BodyRow.Message) .ColumnSpan (All<BodyCol>()) // layout
           .Bind (nameof(vm.Message)), // bind default
```

この規則を一貫して適用することで、C# のマークアップをすばやくスキャンし、UI レイアウトのメンタルイメージを作成することができます。

## <a name="related-links"></a>関連リンク

- [CSharpForMarkupDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)
- [Android プラットフォームの機能](~/xamarin-forms/platform/android/index.md)
- [iOS プラットフォームの機能](~/xamarin-forms/platform/ios/index.md)
- [Windows プラットフォームの機能](~/xamarin-forms/platform/windows/index.md)