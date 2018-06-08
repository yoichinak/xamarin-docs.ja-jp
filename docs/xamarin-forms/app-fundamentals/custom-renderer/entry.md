---
title: エントリをカスタマイズします。
description: Xamarin.Forms エントリ コントロールには、1 行のテキストを編集することが可能です。 この記事では、開発者が独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングのオーバーライドを有効にすると、入力コントロールのカスタム レンダラーを作成する方法を示します。
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 5f23b65fab24b447a9f534ed7403797a60cc284f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847226"
---
# <a name="customizing-an-entry"></a>エントリをカスタマイズします。

_Xamarin.Forms エントリ コントロールには、1 行のテキストを編集することが可能です。この記事では、開発者が独自のプラットフォーム固有のカスタマイズと既定のネイティブ レンダリングのオーバーライドを有効にすると、入力コントロールのカスタム レンダラーを作成する方法を示します。_

各 Xamarin.Forms コントロールには、ネイティブなコントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 ときに、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールが iOS での Xamarin.Forms アプリケーションによって表示される、`EntryRenderer`クラスをインスタンス化、ネイティブ インスタンス化それに続いて`UITextField`コントロール。 Android のプラットフォームでは、`EntryRenderer`クラスをインスタンス化、`EditText`コントロール。 ユニバーサル Windows プラットフォーム (UWP) に、`EntryRenderer`クラスをインスタンス化、`TextBox`コントロール。 レンダラーと Xamarin.Forms のコントロールにマップするネイティブ コントロール クラスの詳細については、次を参照してください。[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

次の図の間のリレーションシップを示しています、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールおよびそれを実装する対応するネイティブ コントロール。

![](entry-images/entry-classes.png "入力コントロールとネイティブの制御を実装する間のリレーションシップ")

表示処理に実行できるの利点のカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズ設定を実装する、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)各プラットフォームで制御します。 これを行うためのプロセスは次のとおりです。

1. [作成](#Creating_the_Custom_Entry_Control)Xamarin.Forms のカスタム コントロールです。
1. [消費](#Consuming_the_Custom_Control)Xamarin.Forms からカスタム コントロールです。
1. [作成](#Creating_the_Custom_Renderer_on_each_Platform)各プラットフォームでコントロールのカスタム レンダラーです。

各項目を実装する順番に説明するようになりましたが、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)を各プラットフォームで別の背景色を持つコントロール。

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>エントリのカスタム コントロールを作成します。

カスタム[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)サブクラス化してコントロールを作成することができます、`Entry`コントロールが次のコード例に示すようにします。

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry`コントロールが、標準の .NET ライブラリ プロジェクトが作成され、単に[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロール。 コントロールのカスタマイズを実行するカスタムのレンダラーでの他の実装は必要ありませんので、`MyEntry`コントロール。

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`MyEntry`コントロールで参照できます XAML で標準的な .NET のライブラリ プロジェクトの場所の名前空間の宣言してコントロール要素で名前空間プレフィックスを使用します。 次のコード例に示す方法、`MyEntry`コントロールを XAML ページで利用できることができます。

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local`何も名前空間プレフィックスを付けることができます。 ただし、`clr-namespace`と`assembly`値がカスタム コントロールの詳細情報と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用して、カスタム コントロールを参照できます。

次のコード例に示す方法、`MyEntry`コントロールは、c# のページで利用できることができます。

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

このコードの新しいインスタンスを作成[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)オブジェクトには、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)と`MyEntry`コントロール両方垂直方向および水平方向にページの中央にします。

カスタム レンダラーは、各プラットフォームでコントロールの外観をカスタマイズするには、各アプリケーション プロジェクトを今すぐ追加できます。

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォームでは、カスタム レンダラーを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. サブクラスを作成、`EntryRenderer`ネイティブ コントロールを描画するクラス。
1. 上書き、`OnElementChanged`コントロールをカスタマイズするネイティブ コントロールと書き込みのロジックを表示するメソッド。 このメソッドは、Xamarin.Forms、対応するコントロールの作成時に呼び出されます。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms コントロールを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> 各プラットフォームのプロジェクトでのカスタム レンダラーを提供する省略可能であります。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。

次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](entry-images/solution-structure.png "MyEntry カスタム レンダラーのプロジェクトの責任")

`MyEntry`コントロールがプラットフォーム固有の仕様を表示する`MyEntryRenderer`から派生するクラス、`EntryRenderer`各プラットフォームのクラスです。 これは、結果、各`MyEntry`次のスクリーン ショットに示すように、プラットフォーム固有の背景色でレンダリングされるを制御します。

![](entry-images/screenshots.png "各プラットフォームで MyEntry コントロール")

`EntryRenderer`クラスが公開、 `OnElementChanged` Xamarin.Forms コントロールが、対応するネイティブ コントロールを表示するために作成されるときに呼び出されるメソッド。 このメソッドは、`ElementChangedEventArgs`パラメーターを含む`OldElement`と`NewElement`プロパティです。 これらのプロパティは、Xamarin.Forms 要素を表すをレンダラーでは、*が*に接続されていると Xamarin.Forms の要素をレンダラーでは、*は*に、それぞれをアタッチします。 サンプル アプリケーションで、`OldElement`プロパティ`null`と`NewElement`プロパティへの参照が格納されます、`MyEntry`コントロール。

オーバーライドのバージョン、`OnElementChanged`メソッドで、`MyEntryRenderer`クラスは、ネイティブ コントロールのカスタマイズを実行する場所です。 型指定されたコントロールへの参照、ネイティブ プラットフォームで使用されている経由でアクセスできる、`Control`プロパティです。 さらに、表示される Xamarin.Forms コントロールへの参照を取得できます、`Element`プロパティが、サンプル アプリケーションでは使用されません。

各カスタム レンダラー クラスがで修飾された、 `ExportRenderer` Xamarin.Forms では、レンダラーを登録する属性。 属性は、–、表示される Xamarin.Forms コントロールの型名とカスタム レンダラーの種類の名前の 2 つのパラメーターを受け取ります。 `assembly`属性にプレフィックスは、属性がアセンブリ全体に適用されることを指定します。

次のセクションでは、各プラットフォームに固有の実装を説明する`MyEntryRenderer`カスタム レンダラー クラスです。

### <a name="creating-the-custom-renderer-on-ios"></a>IOS では、カスタム レンダラーを作成します。

次のコード例は、iOS プラットフォーム用のカスタム レンダラーを示しています。

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

基本クラスの呼び出し`OnElementChanged`メソッドは、iOS をインスタンス化`UITextField`コントロールの場合、レンダラーに割り当てられているコントロールへの参照と`Control`プロパティです。 背景色が薄い紫色でに設定し、`UIColor.FromRGB`メソッドです。

### <a name="creating-the-custom-renderer-on-android"></a>Android では、カスタム レンダラーを作成します。

次のコード例は、Android プラットフォーム用のカスタム レンダラーを示しています。

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

基本クラスの呼び出し`OnElementChanged`メソッドは、Android をインスタンス化`EditText`コントロールの場合、レンダラーに割り当てられているコントロールへの参照と`Control`プロパティです。 背景色がで明るい緑に設定し、`Control.SetBackgroundColor`メソッドです。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP にカスタム レンダラーを作成します。

次のコード例は、UWP のカスタム レンダラーを示しています。

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

基本クラスの呼び出し`OnElementChanged`メソッドがインスタンス化、`TextBox`コントロールの場合、レンダラーに割り当てられているコントロールへの参照と`Control`プロパティです。 作成することで、水色背景色が設定し、`SolidColorBrush`インスタンス。

## <a name="summary"></a>まとめ

この記事は、Xamarin.Forms のカスタム コントロール レンダラーを作成する方法を示しましたが[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロール、開発者は、独自のプラットフォーム固有の表示で、既定のネイティブ レンダリングをオーバーライドします。 カスタムのレンダラーでは、Xamarin.Forms コントロールの外観のカスタマイズに強力なアプローチを提供します。 これらは、小規模のスタイル設定の変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズ使用できます。


## <a name="related-links"></a>関連リンク

- [CustomRendererEntry (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
