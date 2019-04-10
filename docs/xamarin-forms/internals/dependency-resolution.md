---
title: Xamarin.Forms での依存関係の解決
description: この記事では、アプリケーションの依存関係注入コンテナーがある作成およびカスタム レンダラー、エフェクト、および DependencyService 実装の有効期間を制御できるように、Xamarin.Forms に依存関係の解決方法を挿入する方法について説明します。
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
ms.openlocfilehash: 56e50f0c3dffd54fe3d95f4cd140883613c9206f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53052716"
---
# <a name="dependency-resolution-in-xamarinforms"></a>Xamarin.Forms での依存関係の解決

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/DIContainerDemo/)

_この記事では、アプリケーションの依存関係注入コンテナーがある作成およびカスタム レンダラー、エフェクト、および DependencyService 実装の有効期間を制御できるように、Xamarin.Forms に依存関係の解決方法を挿入する方法について説明します。この記事のコード例がから取得した、[コンテナーを使用して依存関係の解決](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/DIContainerDemo/)サンプル。_

モデル-ビュー-ビューモデル (MVVM) パターンを使用して、Xamarin.Forms アプリケーションのコンテキストでは、依存関係の注入コンテナーを登録およびモデルの表示を解決するため、サービスを登録すると、ビュー モデルに挿入することに使用できます。 ビュー モデルの作成時に、コンテナーは、必要なすべての依存関係を挿入します。 これらの依存関係が作成されていない場合、コンテナーが作成し、最初の依存関係を解決します。 ビューのモデルへの依存関係の挿入の例など、依存関係の挿入の詳細については、[依存関係の注入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)を参照してください。

作成の制御のプラットフォーム プロジェクトの種類の有効期間は従来、Xamarin.Forms を使用して実行し、`Activator.CreateInstance`カスタム レンダラーでは、特殊効果のインスタンスを作成するメソッドをおよび[ `DependencyService` ](xref:Xamarin.Forms.DependencyService)実装。 残念ながら、これは開発者の作成と、これらの型とそれらに依存関係を挿入する機能の有効期間の制御点を制限します。 この動作は、Xamarin.Forms にアプリケーションの依存関係注入コンテナー、または Xamarin.Forms 制御の種類を作成する方法: 依存関係の解決方法を挿入することで変更できます。 ただし、依存関係の解決方法を Xamarin.Forms に挿入するための要件がないことに注意してください。 Xamarin.Forms は引き続き作成し、依存関係の解決方法が挿入されていない場合は、プラットフォームのプロジェクト内の型の有効期間を管理します。

> [!NOTE]
> 解決するのにはファクトリ メソッドを使用する依存関係解決メソッドを挿入できるもこの記事は、依存関係の注入コンテナーを使用して登録済みの型を解決する Xamarin.Forms に依存関係の解決方法を挿入するのでは、登録済みの型。 詳細については、次を参照してください。、[ファクトリ メソッドを使用して依存関係の解決](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/FactoriesDemo/)サンプル。

## <a name="injecting-a-dependency-resolution-method"></a>依存関係の解決方法を挿入します。

[ `DependencyResolver` ](xref:Xamarin.Forms.Internals.DependencyResolver)を Xamarin.Forms では、依存関係の解決方法を挿入する機能を提供するクラスを使用して、 [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*)メソッド。 次に、Xamarin.Forms に特定の型のインスタンスが必要がある場合、依存関係の解決方法にインスタンスを提供する機会が与えられます。 依存関係の解決方法を返す場合`null`の種類を作成しようとしてにフォールバックを Xamarin.Forms では、要求された型のインスタンスを使用して自体、`Activator.CreateInstance`メソッド。

次の例と依存関係の解決方法を設定する方法を示しています、 [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*)メソッド。

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

この例では、依存関係の解決方法は、コンテナーに登録されている任意の型を解決するのには、Autofac 依存関係注入コンテナーを使用するラムダ式に設定されます。 それ以外の場合、`null`返される、その結果、Xamarin.Forms の型を解決しようとしています。

> [!NOTE]
> 依存関係の注入コンテナーで使用される API は、コンテナーに固有です。 この記事のコード例を提供する依存関係挿入のコンテナーとして Autofac を使用して、`IContainer`と`ContainerBuilder`型。 別の依存関係注入コンテナーは均等に使用できるはここではよりさまざまな Api を使用します。

アプリケーションの起動時に依存関係の解決方法を設定する必要がないことに注意してください。 これは、いつでも設定できます。 唯一の制約は、Xamarin.Forms は、アプリケーションが依存関係の注入コンテナーに格納されている型を使用しようとする時間によって、依存関係の解決方法について知っておく必要があります。 そのため、アプリケーションが起動中に必要な依存関係挿入コンテナーにサービスがある場合は、依存関係の解決方法がアプリケーションのライフ サイクルの早い段階で設定する必要があります。 同様に、依存関係の注入コンテナーを作成し、特定の有効期間管理かどうか[ `Effect` ](xref:Xamarin.Forms.Effect)Xamarin.Forms は、ビューを作成しようとする前に、依存関係の解決方法について知っておく必要がありますが使用して`Effect`します。

> [!WARNING]
> 登録して、依存関係の注入コンテナーを持つ型を解決する依存関係は、アプリケーションでは、各ページ ナビゲーションの再構築中の場合に特にのリフレクションの種類ごとに、作成するためのコンテナーの使用のためのコスト パフォーマンスが。 多くまたは詳細な依存関係がある場合は、作成のコストを大幅に向上できます。

## <a name="registering-types"></a>型の登録

型は、依存関係の解決方法を使用してそれらを解決できる前に、依存関係の注入コンテナーを登録する必要があります。 次のコード例を示しています、登録メソッドでサンプル アプリケーションを公開する、 `App` Autofac コンテナー クラス。

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void RegisterTypeWithParameters<TInterface, T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        }).As<TInterface>();
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

アプリケーションは、コンテナーからの型を解決するのには、依存関係の解決方法を使用するときに、型の登録は通常プラットフォーム プロジェクトから実行します。 これにより、カスタム レンダラーでは、効果の種類を登録するプラットフォームのプロジェクトと[ `DependencyService` ](xref:Xamarin.Forms.DependencyService)実装します。

プラットフォームのプロジェクトから次の種類の登録、`IContainer`呼び出すことによって実現されるオブジェクトを構築する必要があります、`BuildContainer`メソッド。 このメソッドは Autofac の`Build`メソッドを`ContainerBuilder`インスタンスで、行われた登録を含む新しい依存関係注入コンテナーをビルドします。

次のセクションで、`Logger`を実装するクラス、`ILogger`インターフェイスはクラスのコンス トラクターに挿入されます。 `Logger`クラス実装の簡単なログ記録機能を使用して、`Debug.WriteLine`メソッド、カスタム レンダラーでは、特殊効果にサービスを挿入する方法をデモンストレーションするために使用して、 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService)実装します。

### <a name="registering-custom-renderers"></a>カスタム レンダラーを登録します。

サンプル アプリケーションには、XAML ソースが次の例で示すように、web のビデオを再生するページが含まれます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

`VideoPlayer`ビューが、各プラットフォームで実装されている、`VideoPlayerRenderer`ビデオを再生するための機能を提供するクラス。 これらのカスタム レンダラー クラスの詳細については、[ビデオ プレーヤーを実装する](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)を参照してください。

IOS、ユニバーサル Windows プラットフォーム (UWP) で、`VideoPlayerRenderer`クラスが必要と次のコンス トラクターがある、`ILogger`引数。

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

により、すべてのプラットフォームで依存関係の注入コンテナーの種類の登録が実行される、`RegisterTypes`メソッドを使用してアプリケーションを読み込み、プラットフォームの前に呼び出される、`LoadApplication(new App())`メソッド。 次の例は、 `RegisterTypes` iOS プラットフォームでのメソッド。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

この例で、`Logger`具象型がそのインターフェイスの型に対してマッピングを使用して登録されていると、`VideoPlayerRenderer`型がインターフェイスの割り当てをせずに直接登録します。 ユーザーが含まれるページに移動するときに、`VideoPlayer`ビュー、解決するのには依存関係の解決メソッドが呼び出される、`VideoPlayerRenderer`また解決し、挿入される依存関係挿入コンテナーからの型、`Logger`に入力`VideoPlayerRenderer`コンス トラクター。

`VideoPlayerRenderer` Android プラットフォームでのコンス トラクターには少し複雑になりますが、`Context`引数に加え、`ILogger`引数。

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

次の例は、`RegisterTypes`メソッドは、Android プラットフォーム。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

この例で、`App.RegisterTypeWithParameters`メソッド レジスタ、`VideoPlayerRenderer`依存関係の注入コンテナーでします。 登録方法により、`MainActivity`として挿入されるインスタンス、`Context`引数とする、`Logger`型として挿入される、`ILogger`引数。

### <a name="registering-effects"></a>効果を登録します。

サンプル アプリケーションには、ドラッグ効果を追跡するタッチを使用するページが含まれています。 [ `BoxView` ](xref:Xamarin.Forms.BoxView)ページの周囲のインスタンス。 [ `Effect` ](xref:Xamarin.Forms.Effect)に追加されます、`BoxView`次のコードを使用します。

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

`TouchEffect`クラスは、 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect)によって各プラットフォームで実装される、`TouchEffect`クラスですが、 `PlatformEffect`。 プラットフォーム`TouchEffect`クラスをドラッグする機能を提供、`BoxView`ページの周りにします。 これらのエフェクト クラスの詳細については、[効果からイベントを呼び出す](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)を参照してください。

すべてのプラットフォームで、`TouchEffect`クラスに必要と次のコンス トラクター、`ILogger`引数。

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

により、すべてのプラットフォームで依存関係の注入コンテナーの種類の登録が実行される、`RegisterTypes`メソッドを使用してアプリケーションを読み込み、プラットフォームの前に呼び出される、`LoadApplication(new App())`メソッド。 次の例は、`RegisterTypes`メソッドは、Android プラットフォーム。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

この例で、`Logger`具象型がそのインターフェイスの型に対してマッピングを使用して登録されていると、`TouchEffect`型がインターフェイスの割り当てをせずに直接登録します。 ユーザーが含まれるページに移動するときに、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)インスタンスを持つ、`TouchEffect`接続して、依存関係の解決メソッドが、プラットフォームを解決するのには呼び出されます`TouchEffect`依存関係からの種類注入コンテナーも解決するには、挿入、`Logger`に入力、`TouchEffect`コンス トラクター。

### <a name="registering-dependencyservice-implementations"></a>DependencyService 実装を登録します。

サンプル アプリケーションには、使用するページが含まれています。 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService)ユーザーがデバイスの画像ライブラリから写真を選択できるようにするには、各プラットフォームで実装します。 `IPhotoPicker`インターフェイスによって実装されている機能を定義する、`DependencyService`実装では、次の例に示したと。

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

各プラットフォーム プロジェクトで、`PhotoPicker`クラスが実装する、`IPhotoPicker`プラットフォーム Api を使用してインターフェイス。 これらの依存関係サービスの詳細については、[画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)を参照してください。

IOS と UWP での`PhotoPicker`クラスが必要と次のコンス トラクターがある、`ILogger`引数。

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

により、すべてのプラットフォームで依存関係の注入コンテナーの種類の登録が実行される、`RegisterTypes`メソッドを使用してアプリケーションを読み込み、プラットフォームの前に呼び出される、`LoadApplication(new App())`メソッド。 次の例は、 `RegisterTypes` UWP のメソッド。

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

この例で、`Logger`具象型がそのインターフェイスの型に対してマッピングを使用して登録されていると、`PhotoPicker`型はインターフェイス マップを使用しても登録されます。

`PhotoPicker` Android プラットフォームでのコンス トラクターには少し複雑になりますが、`Context`引数に加え、`ILogger`引数。

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

次の例は、`RegisterTypes`メソッドは、Android プラットフォーム。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

この例で、`App.RegisterTypeWithParameters`メソッド レジスタ、`PhotoPicker`依存関係の注入コンテナーでします。 登録方法により、`MainActivity`として挿入されるインスタンス、`Context`引数とする、`Logger`型として挿入される、`ILogger`引数。

ユーザーが写真の選択 ページに移動すると、写真を選択することを選択、`OnSelectPhotoButtonClicked`ハンドラーが実行されます。

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

ときに、 [ `DependencyService.Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*)メソッドが呼び出される、解決するのには依存関係の解決メソッドが呼び出される、`PhotoPicker`また解決し、挿入される依存関係挿入コンテナーからの型、`Logger`型`PhotoPicker`コンス トラクター。

> [!NOTE]
> [ `Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*)を使用して、アプリケーションの依存関係挿入コンテナーからの型を解決するときに、メソッドを使用する必要があります、 [ `DependencyService`](xref:Xamarin.Forms.DependencyService)します。

## <a name="related-links"></a>関連リンク

- [コンテナー (サンプル) を使用して依存関係の解決](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/DIContainerDemo/)
- [依存関係の挿入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [ビデオ プレーヤーの実装](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [効果からのイベントの呼び出し](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [画像ライブラリから写真を選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
