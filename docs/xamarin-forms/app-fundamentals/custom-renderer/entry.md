---
title: Entry のカスタマイズ
description: Xamarin.Forms の Entry コントロールによって、1 行のテキストを編集対象にできます。 この記事では、Entry コントロール用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c04626a3139a7fa44b2b13b2c1dd343188322f3c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368506"
---
# <a name="customizing-an-entry"></a>Entry のカスタマイズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/customrenderers-entry)

_Xamarin.Forms の Entry コントロールによって、1 行のテキストを編集対象にできます。この記事では、Entry コントロール用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。_

すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 [`Entry`](xref:Xamarin.Forms.Entry) コントロールが Xamarin.Forms アプリケーションによってレンダリングされると、iOS で `EntryRenderer` クラスがインスタンス化され、それによってネイティブの `UITextField` コントロールもインスタンス化されます。 Android プラットフォーム上では、`EntryRenderer` クラスによって `EditText` コントロールがインスタンス化されます。 ユニバーサル Windows プラットフォーム (UWP) 上では、`EntryRenderer` クラスによって `TextBox` コントロールがインスタンス化されます。 Xamarin.Forms コントロールによってマップされるレンダラーとネイティブ コントロール クラスの詳細については、「[Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)」(レンダラーの基底クラスおよびネイティブ コントロール) を参照してください。

次の図は、[`Entry`](xref:Xamarin.Forms.Entry) コントロールと、それを実装する、対応するネイティブ コントロールの関係を示しています。

![Entry コントロールと実装するネイティブ コントロールの関係](entry-images/entry-classes.png)

レンダリング プロセスを活用して各プラットフォーム上の [`Entry`](xref:Xamarin.Forms.Entry) コントロールにカスタム レンダラーを作成することで、プラットフォーム固有のカスタマイズを実装することができます。 その実行プロセスは次のとおりです。

1. Xamarin.Forms カスタム コントロールを[作成](#creating-the-custom-entry-control)します。
1. Xamarin.Forms からカスタム コントロールを[使用](#consuming-the-custom-control)します。
1. 各プラットフォーム上でコントロールのカスタム レンダラーを[作成](#creating-the-custom-renderer-on-each-platform)します。

プラットフォームごとに背景色が異なる [`Entry`](xref:Xamarin.Forms.Entry) コントロールを実装するため、項目ごとに順番に説明します。

> [!IMPORTANT]
> この記事では、単純なカスタム レンダラーを作成する方法について説明します。 ただし、プラットフォームごとに背景色が異なる `Entry` を実装するためにカスタム レンダラーを作成する必要はありません。 これは、[`Device`](xref:Xamarin.Forms.Device) クラス、またはプラットフォーム固有の値を提供する `OnPlatform` マークアップ拡張を使用すると、より簡単に実行できます。 詳細については、「[Providing Platform-Specific Values](~/xamarin-forms/platform/device.md#provide-platform-specific-values)」(プラットフォーム固有の値の提供) と「[OnPlatform マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」を参照してください。

## <a name="creating-the-custom-entry-control"></a>カスタムの Enrty コントロールの作成

カスタムの [`Entry`](xref:Xamarin.Forms.Entry) コントロールは、次のコード例のように、`Entry` コントロールをサブクラス化することで作成できます。

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` コントロールは .NET Standard ライブラリ プロジェクトに作成されます。これは、単純に [`Entry`](xref:Xamarin.Forms.Entry) コントロールです。 コントロールのカスタマイズはカスタム レンダラーで実行されるため、`MyEntry` コントロールに追加の実装は必要ありません。

## <a name="consuming-the-custom-control"></a>カスタム コントロールの使用

`MyEntry` コントロールは、その場所の名前空間を宣言し、コントロール要素上で名前空間プレフィックスを使用することで .NET Standard ライブラリ プロジェクトの XAML で参照することができます。 次のコード例は、XAML ページでどのように `MyEntry` コントロールを使用できるかを示しています。

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` 名前空間プレフィックスには任意の名前を付けることができます。 ただし、`clr-namespace` と `assembly` の値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタム コントロールが参照されます。

次のコード例は、C# ページがどのように `MyEntry` コントロールを使用できるかを示しています。

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

このコードは、ページ上で垂直および水平の両方向に対して中央に配置される [`Label`](xref:Xamarin.Forms.Label) と `MyEntry` コントロールを表示する新しい [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトをインスタンス化します。

これで、カスタム レンダラーを各アプリケーション プロジェクトに追加して、各プラットフォーム上でコントロールの外観をカスタマイズできるようになります。

## <a name="creating-the-custom-renderer-on-each-platform"></a>各プラットフォーム上でのカスタム レンダラーの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. ネイティブ コントロールをレンダリングする `EntryRenderer` クラスのサブクラスを作成します。
1. ネイティブ コントロールをレンダリングする `OnElementChanged` メソッドをオーバーライドして、ロジックを書き込み、コントロールをカスタマイズします。 対応する Xamarin.Forms コントロールが作成されると、このメソッドが呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加して、Xamarin.Forms コントロールのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラス用の既定のレンダラーが使用されます。

次の図に、サンプル アプリケーション内の各プロジェクトの役割と、それらの関係を示します。

![MyEntry カスタム レンダラーのプロジェクトの役割](entry-images/solution-structure.png)

`MyEntry` コントロールはプラットフォーム固有の `MyEntryRenderer` クラスによってレンダリングされます。このクラスはすべて各プラットフォームの `EntryRenderer` クラスから派生しています。 この結果、次のスクリーンショットに示すように、プラットフォーム固有の背景色を使用してそれぞれの `MyEntry` コントロールがレンダリングされます。

![プラットフォームごとの MyEntry コントロール](entry-images/screenshots.png)

`EntryRenderer` クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ コントロールをレンダリングするために、Xamarin.Forms コントロールの作成時に呼び出されます。 このメソッドでは、`OldElement` および `NewElement` プロパティを含む `ElementChangedEventArgs` パラメーターを受け取ります。 これらのプロパティは、レンダラーがアタッチされて *いた* Xamarin.Forms 要素と、レンダラーが現在アタッチされて *いる* Xamarin.Forms 要素をそれぞれ表しています。 サンプル アプリケーションでは、`OldElement` プロパティが `null` になり、`NewElement` プロパティに `MyEntry` コントロールへの参照が含まれます。

`MyEntryRenderer` クラスの `OnElementChanged` メソッドのオーバーライドされたバージョンで、ネイティブ コントロールのカスタマイズが実行されます。 プラットフォーム上で使用されているネイティブ コントロールへの型指定された参照には、`Control` プロパティを使用してアクセスすることができます。 さらに、サンプル アプリケーションでは使用されていませんが、レンダリングされている Xamarin.Forms コントロールへの参照は、`Element` プロパティを使用して取得することができます。

各カスタム レンダラー クラスは、レンダラーを Xamarin.Forms に登録する `ExportRenderer` 属性で修飾されます。 この属性は、レンダリングされている Xamarin.Forms コントロールの型名と、カスタム レンダラーの型名という 2 つのパラメーターを受け取ります。 属性の `assembly` プレフィックスでは、属性がアセンブリ全体に適用されることを指定します。

次のセクションで、各プラットフォーム固有の `MyEntryRenderer` カスタム レンダラー クラスの実装について説明します。

### <a name="creating-the-custom-renderer-on-ios"></a>iOS 上でのカスタム レンダラーの作成

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

基底クラスの `OnElementChanged` メソッドの呼び出しにより、レンダラーの `Control` プロパティに割り当てられているコントロールを参照して、iOS の `UITextField` コントロールがインスタンス化されます。 これで、`UIColor.FromRGB` メソッドを使用して背景色が薄い紫に設定されます。

### <a name="creating-the-custom-renderer-on-android"></a>Android 上でのカスタム レンダラーの作成

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

基底クラスの `OnElementChanged` メソッドの呼び出しにより、レンダラーの `Control` プロパティに割り当てられているコントロールを参照して、Android の `EditText` コントロールがインスタンス化されます。 これで、`Control.SetBackgroundColor` メソッドを使用して背景色が薄い緑に設定されます。

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP 上でのカスタム レンダラーの作成

次のコード例で、UWP 用のカスタム レンダラーを示します。

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

基底クラスの `OnElementChanged` メソッドの呼び出しにより、レンダラーの `Control` プロパティに割り当てられているコントロールを参照して、`TextBox` コントロールがインスタンス化されます。 これで、`SolidColorBrush` インスタンスが作成され、背景色がシアンに設定されます。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms の [`Entry`](xref:Xamarin.Forms.Entry) コントロール用のカスタム コントロール レンダラーを作成する方法を示しました。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォーム固有のレンダリングでオーバーライドできるようになります。 カスタム レンダラーにより、Xamarin.Forms コントロールの外観をカスタマイズするための強力な方法が提供されます。 それらは、スタイルに関する小さな変更や、洗練されたプラットフォーム固有のレイアウトおよびビヘイビアーのカスタマイズのために使用できます。

## <a name="related-links"></a>関連リンク

- [CustomRendererEntry (サンプル)](/samples/xamarin/xamarin-forms-samples/customrenderers-entry)