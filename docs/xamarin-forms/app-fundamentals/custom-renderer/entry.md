---
title: エントリのカスタマイズ
description: Xamarin.Forms のエントリのコントロールは、1 行の編集対象のテキストを使用できます。 この記事では、独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドする開発者を有効にすると、エントリのコントロールのカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/26/2018
ms.openlocfilehash: 7fea736b0a04a69fd64100ae1d6bcd42c244359f
ms.sourcegitcommit: 2f6a5c1abf90fbdb0475fd8a3ce6de3cd7c7d575
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52459851"
---
# <a name="customizing-an-entry"></a>エントリのカスタマイズ

_Xamarin.Forms のエントリのコントロールは、1 行の編集対象のテキストを使用できます。この記事では、独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドする開発者を有効にすると、エントリのコントロールのカスタム レンダラーを作成する方法を示します。_

すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `Entry` ](xref:Xamarin.Forms.Entry) iOS での Xamarin.Forms アプリケーションでコントロールが表示される、`EntryRenderer`クラスがインスタンス化、これは、ネイティブをインスタンス化`UITextField`コントロール。 Android のプラットフォームで、`EntryRenderer`クラスをインスタンス化、`EditText`コントロール。 ユニバーサル Windows プラットフォーム (UWP) で、`EntryRenderer`クラスをインスタンス化、`TextBox`コントロール。 レンダラーと Xamarin.Forms コントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラーの基本クラスおよびネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。

次の図の間のリレーションシップを示しています、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールとそれを実装するネイティブ コントロールの対応します。

![](entry-images/entry-classes.png "入力コントロールとのネイティブ コントロールを実装する間のリレーションシップ")

レンダリング プロセスに実行できる活用用のカスタム レンダラーを作成してプラットフォーム固有のカスタマイズを実装、 [ `Entry` ](xref:Xamarin.Forms.Entry)各プラットフォーム上のコントロール。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Entry_Control)Xamarin.Forms カスタム コントロール。
1. [消費](#Consuming_the_Custom_Control)Xamarin.Forms からカスタム コントロール。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームでコントロールのカスタム レンダラーです。

各項目が実装するためにさらに、説明するようになりましたが、 [ `Entry` ](xref:Xamarin.Forms.Entry)を各プラットフォームで別の背景色を持つコントロール。

> [!IMPORTANT]
> この記事では、単純なカスタム レンダラーを作成する方法について説明します。 ただし、実装するためにカスタム レンダラーを作成する必要はありません、`Entry`各プラットフォームに対する異なる背景色を持っています。 これでより簡単に実行することができます、 [ `Device` ](xref:Xamarin.Forms.Device)クラス、または`OnPlatform`プラットフォーム固有の値を提供する、マークアップ拡張機能。 詳細については、次を参照してください。[プラットフォーム固有の値を提供する](~/xamarin-forms/platform/device.md#providing-platform-specific-values)と[OnPlatform マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)します。

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>エントリのカスタム コントロールを作成します。

カスタム[ `Entry` ](xref:Xamarin.Forms.Entry)をサブクラス化してコントロールを作成することができます、`Entry`コントロールが次のコード例に示すようにします。

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry`コントロールが、.NET Standard ライブラリ プロジェクトが作成され、単純に[ `Entry` ](xref:Xamarin.Forms.Entry)コントロール。 コントロールのカスタマイズを実行するカスタムのレンダラーでの他の実装は必要ありませんので、`MyEntry`コントロール。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`MyEntry`コントロールで参照できます XAML で .NET Standard ライブラリ プロジェクトの場所の名前空間の宣言してコントロール要素の名前空間プレフィックスを使用します。 次のコード例に示す方法、 `MyEntry` XAML ページでコントロールを使用できます。

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用して、カスタム コントロールを参照します。

次のコード例に示す方法、`MyEntry`コントロールは、c# のページで使用できます。

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

このコードの新しいインスタンスを作成[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)を表示するオブジェクト、 [ `Label` ](xref:Xamarin.Forms.Label)と`MyEntry`コントロール両方垂直方向および水平方向にページの中央にします。

カスタム レンダラーは、各プラットフォームでコントロールの外観をカスタマイズするには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`EntryRenderer`ネイティブ コントロールをレンダリングするクラス。
1. 上書き、`OnElementChanged`制御をカスタマイズするネイティブ コントロールと書き込みロジックをレンダリングするメソッド。 対応する Xamarin.Forms コントロールの作成時に、このメソッドが呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms コントロールを表示するために使用することを指定します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> 各プラットフォーム プロジェクトにカスタム レンダラーを提供する省略可能になります。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。

次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](entry-images/solution-structure.png "MyEntry カスタム レンダラーのプロジェクトの責任")

`MyEntry`プラットフォームに固有でコントロールが表示される`MyEntryRenderer`から派生するクラス、`EntryRenderer`各プラットフォームのクラス。 これは、結果、各`MyEntry`の次のスクリーン ショットに示すようにプラットフォーム固有の背景の色でレンダリングされるを制御します。

![](entry-images/screenshots.png "各プラットフォームで MyEntry コントロール")

`EntryRenderer`クラスでは、`OnElementChanged`メソッドで、Xamarin.Forms コントロールが、対応するネイティブ コントロールを表示するために作成されたときに呼び出されます。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティ。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラー*が*に接続されていると Xamarin.Forms 要素をレンダラー*は*に、それぞれに接続されています。 サンプル アプリケーションでは、`OldElement`プロパティになります`null`と`NewElement`プロパティへの参照には、`MyEntry`コントロール。

オーバーライドされたバージョン、`OnElementChanged`メソッドで、`MyEntryRenderer`クラスは、ネイティブ コントロールのカスタマイズを実行する場所。 を介してアクセスできる、プラットフォームで使用されているネイティブ コントロールへの参照を型指定された、`Control`プロパティ。 さらに、レンダリングされている Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティが、サンプル アプリケーションでは使用されません。

各カスタム レンダラー クラスで修飾された、`ExportRenderer`レンダラーを Xamarin.Forms で登録される属性。 属性は、– 表示するには、Xamarin.Forms コントロールの型名と、カスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有の実装を説明する`MyEntryRenderer`カスタム レンダラー クラス。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS でのカスタム レンダラーの作成

次のコード例では、iOS プラットフォーム用のカスタム レンダラーを示します。

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドは、iOS をインスタンス化`UITextField`コントロールの場合、レンダラーに割り当てられているコントロールへの参照と`Control`プロパティ。 背景色が明るい紫で設定し、`UIColor.FromRGB`メソッド。

### <a name="creating-the-custom-renderer-on-android"></a>Android でのカスタム レンダラーの作成

次のコード例では、Android プラットフォーム用のカスタム レンダラーを示します。

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドには、Android がインスタンス化`EditText`コントロールの場合、レンダラーに割り当てられているコントロールへの参照と`Control`プロパティ。 背景色が明るい緑でに設定し、`Control.SetBackgroundColor`メソッド。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP のカスタム レンダラーを作成します。

次のコード例では、UWP 用のカスタム レンダラーを示します。

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

基本クラスの呼び出し`OnElementChanged`メソッドをインスタンス化、`TextBox`コントロールの場合、レンダラーに割り当てられているコントロールへの参照と`Control`プロパティ。 背景色が作成では cyan に設定し、`SolidColorBrush`インスタンス。

## <a name="summary"></a>まとめ

この記事では、xamarin.forms カスタム コントロール レンダラーを作成する方法を示しましたが[ `Entry` ](xref:Xamarin.Forms.Entry)コントロール、開発者は、独自のプラットフォーム固有の表示を使用した既定のネイティブ レンダリングをオーバーライドします。 カスタム レンダラーは、Xamarin.Forms コントロールの外観をカスタマイズする強力な手段を提供します。 小規模なスタイルの変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズが使用できます。


## <a name="related-links"></a>関連リンク

- [CustomRendererEntry (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
