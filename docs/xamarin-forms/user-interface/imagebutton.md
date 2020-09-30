---
title: Xamarin.Forms ImageButton
description: ImageButton はイメージを表示し、タップまたはクリックすると、特定のタスクを実行するようにアプリケーションに指示します。
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3b27ef8ecbd5f357eabd728423b5787ea222c593
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562302"
---
# <a name="no-locxamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_ImageButton はイメージを表示し、タップまたはクリックすると、特定のタスクを実行するようにアプリケーションに指示します。_

ビューは、ビューとビューを組み合わせて、 `ImageButton` [`Button`](xref:Xamarin.Forms.Button) [`Image`](xref:Xamarin.Forms.Image) コンテンツが画像であるボタンを作成します。 ユーザーは、を `ImageButton` 指で押すか、マウスでクリックして、特定のタスクを実行するようにアプリケーションに指示します。 ただし、ビューとは異なり、 `Button` `ImageButton` ビューにはテキストとテキストの外観の概念がありません。

> [!NOTE]
> ビューで [`Button`](xref:Xamarin.Forms.Button) [`Image`](xref:Xamarin.Forms.Button.Image) は、にイメージを表示できるプロパティが定義されてい `Button` ますが、このプロパティは、テキストの横に小さいアイコンを表示するときに使用することを目的としてい `Button` ます。

このガイドのコード例は、 [フォームギャラリーサンプル](/samples/xamarin/xamarin-forms-samples/formsgallery)から抜粋したものです。

## <a name="setting-the-image-source"></a>画像ソースの設定

`ImageButton` イメージ `Source` ソースがファイル、URI、リソース、またはストリームのいずれかである、ボタンに表示するイメージに設定する必要があるプロパティを定義します。 さまざまなソースからのイメージの読み込みの詳細については、「 [」の Xamarin.Forms 「イメージ](images.md)」を参照してください。

次の例は、XAML でをインスタンス化する方法を示してい `ImageButton` ます。

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

`Source`プロパティは、に表示されるイメージを指定し `ImageButton` ます。 この例では、各プラットフォームプロジェクトから読み込まれるローカルファイルに設定されます。その結果、次のスクリーンショットが表示されます。

[![基本 ImageButton](imagebutton-images/BasicImageButton.png "基本 ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "基本 ImageButton")

既定では、は `ImageButton` 四角形ですが、プロパティを使用して角を丸くすることができ `CornerRadius` ます。 外観の詳細について `ImageButton` は、「 [ImageButton 外観](#imagebutton-appearance)」を参照してください。

> [!NOTE]
> は、 `ImageButton` アニメーション gif を読み込むことができますが、gif の最初のフレームのみを表示します。

次の例は、前の XAML の例と機能的に等価なページを作成する方法を示していますが、完全に C# では次のようになります。

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

## <a name="handling-imagebutton-clicks"></a>ImageButton クリックの処理

`ImageButton``Clicked`ユーザーが `ImageButton` 指またはマウスポインターを使用してをタップしたときに発生するイベントを定義します。 このイベントは、の表面から指またはマウスボタンが離されたときに発生し `ImageButton` ます。 `ImageButton` `IsEnabled` タップに応答するには、プロパティがに設定されている必要があり `true` ます。

次の例は、XAML でをインスタンス化し、そのイベントを処理する方法を示してい `ImageButton` `Clicked` ます。

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

イベントは、 `Clicked` `OnImageButtonClicked` 分離コードファイルにあるという名前のイベントハンドラーに設定されます。

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

`ImageButton` をタップすると、`OnImageButtonClicked` メソッドが実行されます。 `sender`引数は、 `ImageButton` このイベントを担当するです。 これを使用すると、オブジェクトにアクセスし `ImageButton` たり、同じイベントを共有する複数のオブジェクトを区別したりでき `ImageButton` `Clicked` ます。

この `Clicked` ハンドラーは、カウンターをインクリメントし、にカウンター値を表示し [`Label`](xref:Xamarin.Forms.Label) ます。

[![基本 ImageButton クリック](imagebutton-images/ImageButton.png "基本 ImageButton クリック")](imagebutton-images/ImageButton-Large.png#lightbox "基本 ImageButton クリック")

次の例は、前の XAML の例と機能的に等価なページを作成する方法を示していますが、完全に C# では次のようになります。

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

## <a name="disabling-the-imagebutton"></a>ImageButton を無効にする

アプリケーションが特定の状態にある場合、特定の `ImageButton` クリックが有効な操作ではないことがあります。 そのような場合は、 `ImageButton` プロパティをに設定することで、を無効にする必要があり `IsEnabled` `false` ます。

## <a name="using-the-command-interface"></a>コマンドインターフェイスの使用

アプリケーションは、イベントを処理することなく、タップに応答することができ `ImageButton` `Clicked` ます。 は、 `ImageButton` コマンドまたは_コマンド_実行インターフェイスと呼ば_commanding_れる別の通知機構を実装します。 これは、次の2つのプロパティで構成されます。

- `Command` 型の [`ICommand`](xref:System.Windows.Input.ICommand) 。名前空間で定義されているインターフェイス [`System.Windows.Input`](xref:System.Windows.Input) 。
- `CommandParameter` 型のプロパティ [`Object`](xref:System.Object) 。

この方法は、データバインディングとの接続に適しています。特に、モデルビュービューモデル (MVVM) アーキテクチャを実装する場合に適しています。

コマンドインターフェイスの使用方法の詳細については、「[ボタン](button.md)ガイド」の「[コマンドインターフェイスの使用](button.md#using-the-command-interface)」を参照してください。

## <a name="pressing-and-releasing-the-imagebutton"></a>ImageButton の押下と解放

`Clicked` イベントのほかに、`ImageButton` は `Pressed` イベントと `Released` イベントも定義します。 イベントは、指がで押され `Pressed` たとき、 `ImageButton` またはマウスボタンが上にポインターを置いた状態で押されると発生し `ImageButton` ます。 `Released`イベントは、指またはマウスボタンが離されたときに発生します。 一般に、イベントは `Clicked` イベントと同時に発生し `Released` ますが、指またはマウスポインターがの表面から離れ `ImageButton` てから離されると、イベントは発生し `Clicked` ない可能性があります。

これらのイベントの詳細については、[ボタン](button.md)ガイドの[ボタンの押下と解放](button.md#pressing-and-releasing-the-button)に関する説明を参照してください。

## <a name="imagebutton-appearance"></a>ImageButton の外観

では、クラスから継承されるプロパティに加えて `ImageButton` [`View`](xref:Xamarin.Forms.View) 、 `ImageButton` 外観に影響を与えるいくつかのプロパティも定義されています。

- `Aspect` 画像を表示領域に合わせて拡大縮小する方法を指定します。
- `BorderColor` を囲む領域の色です `ImageButton` 。
- `BorderWidth` 境界線の幅を示します。
- `CornerRadius` は、の角の半径です `ImageButton` 。

プロパティは、 `Aspect` 列挙体のいずれかのメンバーに設定でき [`Aspect`](xref:Xamarin.Forms.Aspect) ます。

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -イメージを完全に拡大し、を正確に塗りつぶし `ImageButton` ます。 これにより、イメージがゆがんでしまう可能性があります。
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -画像をクリップして、 `ImageButton` 縦横比を維持したままにします。
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -イメージ全体がに収まるように (必要な場合)、イメージ全体がに収まるように (必要に応じて)、イメージの `ImageButton` 幅が広いか高さに応じて、上下または左右に空白を追加して letterboxes ます。 これは、列挙体の既定値です [`Aspect`](xref:Xamarin.Forms.Aspect) 。

> [!NOTE]
> クラスには `ImageButton` [`Margin`](xref:Xamarin.Forms.View.Margin) `Padding` 、のレイアウト動作を制御するプロパティとプロパティもあり `ImageButton` ます。 詳細については「[Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」 (余白とスペース) を参照してください。

## <a name="imagebutton-visual-states"></a>ImageButton ビジュアルの状態

`ImageButton` が有効になっている場合、ユーザーが押された `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) ときにに対するビジュアルの変更を開始するために使用できる `ImageButton` 。

次の XAML の例は、状態の表示状態を定義する方法を示してい `Pressed` ます。

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

が押されたときに、その `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) `ImageButton` [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) プロパティが既定値の1から0.8 に変更されることを指定します。 `Normal` `VisualState` が通常の状態の場合、 `ImageButton` `Scale` プロパティは1に設定されることを指定します。 したがって、全体の効果として、 `ImageButton` が押されている場合は、スケールが少し小さくなり、が解放されると、 `ImageButton` 既定のサイズにスケールされます。

表示状態の詳細については、「 [ Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [フォームギャラリーのサンプル](/samples/xamarin/xamarin-forms-samples/formsgallery)