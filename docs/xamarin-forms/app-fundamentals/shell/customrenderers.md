---
title: "Xamarin.Forms シェルのカスタム レンダラー" description: "Xamarin.Formsシェル アプリケーションでは、さまざまなシェル クラスが公開しているプロパティとメソッドを利用して、高度なカスタマイズが可能です。 ただし、プラットフォーム固有のより詳細なカスタマイズが必要な場合は、シェルのカスタム レンダラーを作成することも可能です。"
ms.prod: xamarin ms.assetid:3B1A6AE8-1D1E-4C34-B9AB-48F4444FEF32 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:05/06/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-shell-custom-renderers"></a>Xamarin.Forms シェルのカスタム レンダラー

Xamarin.Forms シェル アプリケーションの 1 つのメリットは、さまざまなシェル クラスが公開しているプロパティとメソッドを利用して、外観と動作を高度にカスタマイズできることです。 ただし、プラットフォーム固有のより詳細なカスタマイズが必要な場合は、シェルのカスタム レンダラーを作成することも可能です。 他のカスタム レンダラーと同様に、シェルのカスタム レンダラーは、他のプラットフォーム上での既定の動作を可能にしたまま、1 つのプラットフォーム プロジェクトのみに追加して外観と動作をカスタマイズできます。また、iOS および Android 上の両方で外観と動作をカスタマイズするために、別のシェル カスタム レンダラーを各プラットフォーム プロジェクトに追加することもできます。

シェル アプリケーションは、iOS および Android 上の `ShellRenderer` クラスを使用してレンダリングされます。 iOS 上では、`ShellRenderer` クラスは `Xamarin.Forms.Platform.iOS` 名前空間内で見つかります。 Android 上では、`ShellRenderer` クラスは `Xamarin.Forms.Platform.Android` 名前空間内で見つかります。

シェルのカスタム レンダラーを作成するプロセスは次のとおりです。

1. `Shell` クラスをサブクラス化します。 これは、シェル アプリケーション内で既に行われているでしょう。
1. サブクラス化された `Shell` クラスを利用します。 これは、シェル アプリケーション内で既に行われているでしょう。
1. 必要なプラットフォーム上で、`ShellRenderer` クラスから派生したカスタム レンダラー クラスを作成します。

## <a name="create-a-custom-renderer-class"></a>カスタム レンダラー クラスを作成する

シェルのカスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. `ShellRenderer` クラスのサブクラスを作成します。
1. 必要なメソッドをオーバーライドして、必要なカスタマイズを実行します。
1. `ExportRendererAttribute` を `ShellRenderer` サブクラスに追加して、シェル アプリケーションのレンダリングに使用することを指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用されます。

> [!NOTE]
> プラットフォーム プロジェクトごとにシェルのカスタム レンダラーを指定することは、任意です。 カスタム レンダラーが登録されていない場合は、既定の `ShellRenderer` クラスが使用されます。

`ShellRenderer` クラスでは、オーバーライドできる次のようなメソッドを公開しています。

| iOS | Android |
| --- | --- |
| `SetElementSize`<br />`CreateFlyoutRenderer`<br />`CreateNavBarAppearanceTracker`<br />`CreatePageRendererTracker`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellItemTransition`<br />`CreateShellSearchResultsRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTabBarAppearanceTracker`<br />`Dispose`<br />`OnCurrentItemChanged`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`UpdateBackgroundColor` | `CreateFragmentForPage`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellFlyoutRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTrackerForToolbar`<br />`CreateToolbarAppearanceTracker`<br />`CreateTabLayoutAppearanceTracker`<br />`CreateBottomNavViewAppearanceTracker`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`SwitchFragment`<br />`Dispose` |

`FlyoutItem` および `TabBar` クラスは `ShellItem` クラスの別名であり、`Tab` クラスは `ShellSection` クラスの別名です。 そのため、`FlyoutItem` オブジェクトのカスタム レンダラーを作成するときは、`CreateShellItemRenderer` メソッドをオーバーライドする必要があり、`Tab` オブジェクトのカスタム レンダラーを作成するときは、`CreateShellSectionRenderer` メソッドをオーバーライドする必要があります。

> [!IMPORTANT]
> iOS および Android 上の両方に、`ShellSectionRenderer` および `ShellItemRenderer` など、追加のシェル レンダラー クラスがあります。 ただし、これらの追加のレンダラー クラスは、`ShellRenderer` クラス内でのオーバーライドによって作成されます。 そのため、これらの追加のレンダラー クラスは、サブクラス化したうえで、サブクラス化された `ShellRenderer` クラスでの適切なオーバーライドの中で、そのサブクラスのインスタンスを作成することで、動作のカスタマイズを実現できます。

### <a name="ios-example"></a>iOS の例

次のコード例は、iOS の場合に、シェル アプリケーションのナビゲーション バーに背景イメージを設定するサブクラス化された `ShellRenderer` を示しています。

```csharp
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.iOS.MyShellRenderer))]
namespace Xaminals.iOS
{
    public class MyShellRenderer : ShellRenderer
    {
        protected override IShellSectionRenderer CreateShellSectionRenderer(ShellSection shellSection)
        {
            var renderer = base.CreateShellSectionRenderer(shellSection);
            if (renderer != null)
            {
                (renderer as ShellSectionRenderer).NavigationBar.SetBackgroundImage(UIImage.FromFile("monkey.png"), UIBarMetrics.Default);
            }
            return renderer;
        }
    }
}
```

`MyShellRenderer` クラスは `CreateShellSectionRenderer` メソッドをオーバーライドし、基本クラスによって作成されたレンダラーを取得します。 その後、レンダラーを返す前に、ナビゲーション バーに背景イメージを設定して、レンダラーを変更します。

### <a name="android-example"></a>Android の例

次のコード例は、Android の場合に、シェル アプリケーションのナビゲーション バーに背景イメージを設定するサブクラス化された `ShellRenderer` を示しています。

```csharp
using Android.Content;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.Droid.MyShellRenderer))]
namespace Xaminals.Droid
{
    public class MyShellRenderer : ShellRenderer
    {
        public MyShellRenderer(Context context) : base(context)
        {
        }

        protected override IShellToolbarAppearanceTracker CreateToolbarAppearanceTracker()
        {
            return new MyShellToolbarAppearanceTracker(this);
        }
    }
}
```

`MyShellRenderer` クラスは `CreateToolbarAppearanceTracker` メソッドをオーバーライドして、`MyShellToolbarAppearanceTracker` クラスのインスタンスを返します。 次の例では、`ShellToolbarAppearanceTracker` クラスから派生した `MyShellToolbarAppearanceTracker` クラスを示しています。

```csharp
using Android.Support.V7.Widget;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

namespace Xaminals.Droid
{
    public class MyShellToolbarAppearanceTracker : ShellToolbarAppearanceTracker
    {
        public MyShellToolbarAppearanceTracker(IShellContext context) : base(context)
        {
        }

        public override void SetAppearance(Toolbar toolbar, IShellToolbarTracker toolbarTracker, ShellAppearance appearance)
        {
            base.SetAppearance(toolbar, toolbarTracker, appearance);
            toolbar.SetBackgroundResource(Resource.Drawable.monkey);
        }
    }
}
```

`MyShellToolbarAppearanceTracker` クラスは `SetAppearance` メソッドをオーバーライドし、背景イメージを設定することでツールバーを変更します。

> [!IMPORTANT]
> 必要なのは、`ExportRendererAttribute` を `ShellRenderer` クラスから派生したカスタム レンダラーに追加することだけです。 サブクラス化された追加のシェル レンダラー クラスが、サブクラス化された `ShellRenderer` クラスによって作成されます。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms のカスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
