---
title: Xamarin.Forms ImageButton
description: ImageButton がイメージを表示し、タップまたは特定のタスクを実行するためにアプリケーションに指示するクリックに応答します。
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: 26f69f9ac2d315e1076cab6f7c1534cbdd39a9b3
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52173896"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

_ImageButton がイメージを表示し、タップまたは特定のタスクを実行するためにアプリケーションに指示するクリックに応答します。_

`ImageButton`結合の表示、 [ `Button` ](xref:Xamarin.Forms.Button)ビューと[ `Image` ](xref:Xamarin.Forms.Image)コンテンツを持つボタンを作成するビューは、イメージです。 ユーザーが、`ImageButton`を指で、または特定のタスクを実行するためにアプリケーションに指示するためにマウスをクリックします。 ただしとは異なり、`Button`ビュー、`ImageButton`ビューにテキストとテキストの外観の概念がありません。

> [!NOTE]
> 中に、 [ `Button` ](xref:Xamarin.Forms.Button)ビュー定義、 [ `Image` ](xref:Xamarin.Forms.Button.Image)にイメージを表示することができるプロパティ、 `Button`、小さいアイコンを表示するときに使用するこのプロパティが対象としています次に、`Button`テキスト。

このガイドのコード例がから取得した、 [FormsGallery サンプル](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)します。

## <a name="setting-the-image-source"></a>イメージ ソースを設定します。

`ImageButton` 定義、`Source`プロパティをイメージ ソース ファイルを URI、リソース、またはストリームに、ボタンに表示するイメージに設定する必要があります。 さまざまなソースからのイメージの読み込みの詳細については、次を参照してください。 [Xamarin.Forms でイメージ](images.md)します。

次の例では、インスタンス化する方法を示しています、 `ImageButton` XAML で。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Source`プロパティに表示されるイメージを指定します、`ImageButton`します。 この例では、次のスクリーン ショットは、その結果、各プラットフォーム プロジェクトから読み込まれるローカル ファイルに設定には。

[![基本的な ImageButton](imagebutton-images/BasicImageButton.png "基本的な ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "基本的な ImageButton")

既定で、`ImageButton`は、四角形を使用して、it が丸められますの角を与えることができますが、`CornerRadius`プロパティ。 詳細については`ImageButton`の外観を参照してください[ImageButton 外観](#imagebutton-appearance)します。

次の例は、XAML の前の例では完全に機能的に同等であるページを作成する方法を示しますC#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children = { header, imageButton }
        };
    }
}
```

## <a name="handling-imagebutton-clicks"></a>処理の ImageButton をクリックします。

`ImageButton` 定義、 `Clicked` 、ユーザーがタップしたときに発生するイベント、`ImageButton`指やマウス ポインターを使用します。 画面から指やマウス ボタンが離されたときに、イベントが発生した、`ImageButton`します。 `ImageButton`必要があります、`IsEnabled`プロパティに設定`true`タップに応答します。

次の例では、インスタンス化する方法を示しています、 `ImageButton` XAML とハンドルでその`Clicked`イベント。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand"
                    Clicked="OnImageButtonClicked" />

        <Label x:Name="label"
               Text="0 ImageButton clicks"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Clicked`という名前のイベント ハンドラーにイベントが設定されて`OnImageButtonClicked`分離コード ファイル内にあります。

```csharp
public partial class ImageButtonDemoPage : ContentPage
{
    int clickTotal;

    public ImageButtonDemoPage()
    {
        InitializeComponent();
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

ときに、`ImageButton`がタップされた、`OnImageButtonClicked`メソッドを実行します。 `sender`引数は、`ImageButton`このイベントを担当します。 これを使用して、アクセスすることができます、`ImageButton`オブジェクト、または複数を区別する`ImageButton`オブジェクトが同じ共有`Clicked`イベント。

この特定の`Clicked`ハンドラーは、カウンターをインクリメントして、内のカウンター値が表示されます、 [ `Label` ](xref:Xamarin.Forms.Label):

[![基本的な ImageButton をクリックして](imagebutton-images/ImageButton.png "基本的な ImageButton をクリックして")](imagebutton-images/ImageButton-Large.png#lightbox "基本的な ImageButton をクリックします")

次の例は、XAML の前の例では完全に機能的に同等であるページを作成する方法を示しますC#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    Label label;
    int clickTotal = 0;

    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };
        imageButton.Clicked += OnImageButtonClicked;

        label = new Label
        {
            Text = "0 ImageButton clicks",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children =
            {
                header,
                imageButton,
                label
            }
        };
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

## <a name="disabling-the-imagebutton"></a>ImageButton を無効にします。

特定の場所の特定の状態でアプリケーションが場合がありますが`ImageButton`クリックは、有効な操作ではありません。 その場合、`ImageButton`設定して無効にする必要があります、`IsEnabled`プロパティを`false`します。

## <a name="using-the-command-interface"></a>コマンド インターフェイスを使用します。

アプリケーションに応答することは`ImageButton`タップ処理せず、`Clicked`イベント。 `ImageButton`と呼ばれる別の通知メカニズムを実装して、_コマンド_または_コマンド実行_インターフェイス。 これは、2 つのプロパティで構成されます。

- `Command` 型の[ `ICommand` ](xref:System.Windows.Input.ICommand)、インターフェイスで定義されている、 [ `System.Windows.Input` ](xref:System.Windows.Input)名前空間。
- `CommandParameter` 型のプロパティ[ `Object`](xref:System.Object)します。

この方法は、モデル-ビュー-ビューモデル (MVVM) アーキテクチャを実装する場合に特に関連データ バインディング、およびに適しています。

詳細については、コマンド インターフェイスを使用して、次を参照してください。[コマンド インターフェイスを使用して](button.md#using-the-command-interface)で、[ボタン](button.md)ガイド。

## <a name="pressing-and-releasing-the-imagebutton"></a>キーを押すと、ImageButton を解放します。

それに、`Clicked`イベント、`ImageButton`も定義`Pressed`と`Released`イベント。 `Pressed`の指を押したときに発生するイベントを`ImageButton`、またはポインターの上に置かれたマウス ボタンが押された、`ImageButton`します。 `Released`イベントは、指やマウス ボタンが離されたときに発生します。 一般に、`Clicked`と同時にイベントが発生したも、`Released`の画面から離れた場所スライド指やマウスのポインターの場合は、イベント、`ImageButton`リリースされる前に、`Clicked`イベントが発生しないことができます。

これらのイベントに関する詳細については、次を参照してください。[キーを押すと、ボタンを放す](button.md#pressing-and-releasing-the-button)で、[ボタン](button.md)ガイド。

## <a name="imagebutton-appearance"></a>ImageButton 外観

プロパティに加えを`ImageButton`から継承、 [ `View` ](xref:Xamarin.Forms.View)クラス、`ImageButton`も外観に影響を与えるいくつかのプロパティを定義します。

- `Aspect` 表示領域に合わせて、イメージをスケーリングする方法です。
- `BorderColor` 領域の周囲の色、`ImageButton`します。
- `BorderWidth` 罫線の幅。
- `CornerRadius` 角の半径は、`ImageButton`します。

`Aspect`プロパティは、のメンバーのいずれかに設定することができます、 [ `Aspect` ](xref:Xamarin.Forms.Aspect)列挙体。

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -完全かつ正確に入力するイメージを拡大、`ImageButton`します。 これにより、イメージ遠近法されている可能性があります。
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -全体に表示されるように、イメージをクリップ、`ImageButton`縦横比を維持しながらします。
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -レター ボックス (必要に応じて) のイメージにイメージ全体が収まるように、`ImageButton`空白かどうかは、イメージ、幅または高さに応じて境界線の上/下に追加でします。 これは、既定値の[ `Aspect` ](xref:Xamarin.Forms.Aspect)列挙体。

> [!NOTE]
> `ImageButton`クラスもあります[ `Margin` ](xref:Xamarin.Forms.View.Margin)と`Padding`のレイアウト動作を制御するプロパティ、`ImageButton`します。 詳細については、次を参照してください。[余白やパディング](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)します。

## <a name="imagebutton-visual-states"></a>ImageButton 表示状態

`ImageButton` `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState)を視覚的な変更の開始に使用できる、`ImageButton`が有効になっている、ユーザーによって押されたときにします。

次の XAML の例のビジュアル状態を定義する方法を示しています、`Pressed`状態。

```xaml
<ImageButton Source="XamarinLogo.png"
             ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ImageButton>
```

`Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState)される場合、`ImageButton`を押すと、その[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)プロパティは 1 に 0.8 の既定値から変更されます。 `Normal` `VisualState`される場合、`ImageButton`通常の状態では、その`Scale`プロパティを 1 に設定されます。 そのため、全体の効果では、ときに、`ImageButton`を押すと、これは再スケーリング、若干小さいとタイミングを`ImageButton`がリリースされると、これは再スケーリングの既定のサイズにします。

表示状態の詳細については、次を参照してください。 [、Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

## <a name="related-links"></a>関連リンク

- [FormsGallery サンプル](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
