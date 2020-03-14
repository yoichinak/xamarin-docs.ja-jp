---
title: Xamarin.Forms ImageButton
description: ImageButton がイメージを表示し、タップまたは特定のタスクを実行するためにアプリケーションに指示するクリックに応答します。
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 7c6647a0299b5ece3caaaa1d322ec1a0efac3557
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305715"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_ImageButton はイメージを表示し、タップまたはクリックすると、特定のタスクを実行するようにアプリケーションに指示します。_

`ImageButton` ビューは[`Button`](xref:Xamarin.Forms.Button)ビューと[`Image`](xref:Xamarin.Forms.Image)ビューを組み合わせて、コンテンツが画像であるボタンを作成します。 ユーザーは、`ImageButton` を指で押すか、マウスでクリックして、特定のタスクを実行するようにアプリケーションに指示します。 ただし、`Button` ビューとは異なり、`ImageButton` ビューにはテキストとテキストの外観の概念がありません。

> [!NOTE]
> [`Button`](xref:Xamarin.Forms.Button)ビューでは、 [`Image`](xref:Xamarin.Forms.Button.Image)プロパティが定義されています。これにより `Button`に画像を表示できますが、このプロパティは `Button` テキストの横に小さいアイコンを表示するときに使用することを目的としています。

このガイドのコード例は、[フォームギャラリーサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)から抜粋したものです。

## <a name="setting-the-image-source"></a>イメージ ソースを設定します。

`ImageButton` は、イメージソースがファイル、URI、リソース、またはストリームのいずれかである、ボタンに表示するイメージに設定する必要がある `Source` プロパティを定義します。 さまざまなソースからのイメージの読み込みの詳細については、「 [Xamarin. Forms のイメージ](images.md)」を参照してください。

次の例は、XAML で `ImageButton` をインスタンス化する方法を示しています。

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

`Source` プロパティは、`ImageButton`に表示されるイメージを指定します。 この例では、次のスクリーン ショットは、その結果、各プラットフォーム プロジェクトから読み込まれるローカル ファイルに設定には。

[![基本 ImageButton](imagebutton-images/BasicImageButton.png "基本 ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "基本 ImageButton")

既定では、`ImageButton` は四角形ですが、`CornerRadius` プロパティを使用して角を丸くすることができます。 `ImageButton` の外観の詳細については、「 [ImageButton の外観](#imagebutton-appearance)」を参照してください。

> [!NOTE]
> `ImageButton` は、アニメーション GIF を読み込むことができますが、GIF の最初のフレームのみが表示されます。

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

`ImageButton` は、ユーザーが指またはマウスポインターを使用して `ImageButton` をタップしたときに発生する `Clicked` イベントを定義します。 このイベントは、`ImageButton`の表面から指またはマウスボタンが離されたときに発生します。 タップに応答するには、`ImageButton` の `IsEnabled` プロパティが `true` に設定されている必要があります。

次の例は、XAML で `ImageButton` をインスタンス化し、その `Clicked` イベントを処理する方法を示しています。

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

`Clicked` イベントは、分離コードファイルにある `OnImageButtonClicked` という名前のイベントハンドラーに設定されます。

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

`ImageButton` がタップされると、`OnImageButtonClicked` メソッドが実行されます。 `sender` 引数は、このイベントを担当する `ImageButton` です。 これを使用すると、`ImageButton` オブジェクトにアクセスしたり、同じ `Clicked` イベントを共有する複数の `ImageButton` オブジェクトを区別したりできます。

この特定の `Clicked` ハンドラーは、カウンターをインクリメントし、 [`Label`](xref:Xamarin.Forms.Label)にカウンター値を表示します。

[![基本 ImageButton クリック](imagebutton-images/ImageButton.png "基本 ImageButton クリック")](imagebutton-images/ImageButton-Large.png#lightbox "基本 ImageButton クリック")

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

アプリケーションが特定の状態にある場合、特定の `ImageButton` クリックが有効な操作ではないことがあります。 そのような場合は、`IsEnabled` プロパティを `false`に設定して、`ImageButton` を無効にする必要があります。

## <a name="using-the-command-interface"></a>コマンド インターフェイスを使用します。

アプリケーションが `Clicked` イベントを処理せずに `ImageButton` タップに応答する可能性があります。 `ImageButton`_には、コマンドまたは_ _コマンド_実行インターフェイスと呼ばれる別の通知メカニズムが実装されています。 これは、2 つのプロパティで構成されます。

- [`System.Windows.Input`](xref:System.Windows.Input)名前空間で定義されているインターフェイス[`ICommand`](xref:System.Windows.Input.ICommand)型の `Command`。
- [`Object`](xref:System.Object)型の `CommandParameter` プロパティです。

この方法は、モデル-ビュー-ビューモデル (MVVM) アーキテクチャを実装する場合に特に関連データ バインディング、およびに適しています。

コマンドインターフェイスの使用方法の詳細については、「[ボタン](button.md)ガイド」の「[コマンドインターフェイスの使用](button.md#using-the-command-interface)」を参照してください。

## <a name="pressing-and-releasing-the-imagebutton"></a>キーを押すと、ImageButton を解放します。

`ImageButton` `Clicked` イベントに加えて、`Pressed` および `Released` イベントも定義します。 `Pressed` イベントは、指が `ImageButton`を押したとき、または `ImageButton`上にポインターを置いた状態でマウスボタンが押されたときに発生します。 `Released` イベントは、指またはマウスボタンが離されたときに発生します。 一般に、`Clicked` イベントも `Released` イベントと同時に発生しますが、指またはマウスポインターが `ImageButton` の画面から離れると、`Clicked` イベントが発生しない可能性があります。

これらのイベントの詳細については、[ボタン](button.md)ガイドの[ボタンの押下と解放](button.md#pressing-and-releasing-the-button)に関する説明を参照してください。

## <a name="imagebutton-appearance"></a>ImageButton 外観

[`View`](xref:Xamarin.Forms.View)クラスから継承 `ImageButton` プロパティに加えて、`ImageButton` の外観に影響を与えるいくつかのプロパティも定義します。

- `Aspect` は、画像が表示領域に合わせて拡大縮小される方法です。
- `BorderColor` は、`ImageButton`を囲む領域の色です。
- `BorderWidth` は、境界線の幅です。
- `CornerRadius` は、`ImageButton`の角の半径です。

`Aspect` プロパティは、 [`Aspect`](xref:Xamarin.Forms.Aspect)列挙体のいずれかのメンバーに設定できます。

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -イメージを完全に拡大し、`ImageButton`を正確に入力します。 これにより、イメージ遠近法されている可能性があります。
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -縦横比を維持したまま、`ImageButton` がいっぱいになるようにイメージをクリップします。
- イメージ全体が `ImageButton`に収まるように、必要に応じてイメージを[`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) letterboxes します。イメージが横または横にあるかどうかによって、上下または左右に空白が追加されます。 これは、 [`Aspect`](xref:Xamarin.Forms.Aspect)列挙型の既定値です。

> [!NOTE]
> `ImageButton` クラスには、`ImageButton`のレイアウト動作を制御するプロパティ[`Margin`](xref:Xamarin.Forms.View.Margin)と `Padding` もあります。 詳細については「[Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」 (余白とスペース) を参照してください。

## <a name="imagebutton-visual-states"></a>ImageButton 表示状態

`ImageButton` には、有効になっている場合にユーザーが押したときに `ImageButton` に対する視覚的な変更を開始するために使用できる `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState)があります。

次の XAML の例は、`Pressed` 状態の表示状態を定義する方法を示しています。

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

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState)は、`ImageButton` が押されたときに[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)プロパティが既定値の1から0.8 に変更されることを指定します。 `Normal` `VisualState` は、`ImageButton` が通常の状態であるときに、その `Scale` プロパティが1に設定されることを指定します。 したがって、全体的な影響として、`ImageButton` が押されているときには少し小さくスケール、`ImageButton` が解放されると、既定のサイズにスケールます。

表示状態の詳細については、「 [Xamarin Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
