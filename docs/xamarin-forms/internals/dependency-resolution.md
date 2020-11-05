---
title: Xamarin.Forms での依存関係の解決
description: この記事では、依存関係の解決方法をに挿入する方法について説明し Xamarin.Forms ます。これにより、アプリケーションの依存関係の挿入コンテナーが、カスタムレンダラー、効果、および DependencyService 実装の作成と有効期間を制御できるようになります。
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 58210aaaa7c017c50ba3e2aee3e8402c1b76be16
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373927"
---
# <a name="dependency-resolution-in-no-locxamarinforms"></a>Xamarin.Forms での依存関係の解決

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)

_この記事では、依存関係の解決方法をに挿入する方法について説明し Xamarin.Forms ます。これにより、アプリケーションの依存関係の挿入コンテナーが、カスタムレンダラー、効果、および DependencyService 実装の作成と有効期間を制御できるようになります。この記事のコード例は、「 [コンテナーを使用した依存関係の解決](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo) 」のサンプルから抜粋したものです。_

Xamarin.Formsモデルビュービューモデル (MVVM) パターンを使用するアプリケーションのコンテキストでは、依存関係挿入コンテナーを使用して、ビューモデルの登録と解決、およびサービスの登録とビューモデルへの挿入を行うことができます。 ビューモデルの作成中に、コンテナーは必要な依存関係を挿入します。 これらの依存関係が作成されていない場合、コンテナーはまず依存関係を作成して解決します。 ビューモデルへの依存関係の挿入の例を含む、依存関係の挿入の詳細については、「 [依存関係の挿入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)」を参照してください。

プラットフォームプロジェクトでの型の作成と有効期間の制御は、従来 Xamarin.Forms 、メソッドを使用して `Activator.CreateInstance` カスタムレンダラー、効果、および実装のインスタンスを作成するによって実行され [`DependencyService`](xref:Xamarin.Forms.DependencyService) ます。 残念ながら、開発者がこれらの型の作成と有効期間を制御し、依存関係を挿入する機能を制限しています。 この動作は、 Xamarin.Forms アプリケーションの依存関係挿入コンテナーまたはによって、型の作成方法を制御する依存関係の解決メソッドをに挿入することによって変更できます Xamarin.Forms 。 ただし、依存関係の解決方法をに挿入する必要はありません Xamarin.Forms 。 Xamarin.Forms 依存関係の解決方法が挿入されていない場合、はプラットフォームプロジェクトでの型の有効期間の作成と管理を続行します。

> [!NOTE]
> この記事では、依存関係 Xamarin.Forms の挿入コンテナーを使用して登録された型を解決する依存関係の解決方法をに挿入することに焦点を当てていますが、ファクトリメソッドを使用して登録済みの型を解決する依存関係の解決方法を挿入することもできます。 詳細については、「 [ファクトリメソッドを使用した依存関係の解決](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-factoriesdemo) 」サンプルを参照してください。

## <a name="injecting-a-dependency-resolution-method"></a>依存関係の解決方法の挿入

クラスは、 [`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver) Xamarin.Forms メソッドを使用して、依存関係の解決メソッドをに挿入する機能を提供し [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) ます。 次に、 Xamarin.Forms が特定の型のインスタンスを必要とする場合、依存関係の解決方法にインスタンスを提供する機会が与えられます。 要求された型に対して依存関係解決メソッドがを返す場合は `null` 、 Xamarin.Forms メソッドを使用して型インスタンス自体を作成しようとすることにフォールバックし `Activator.CreateInstance` ます。

次の例は、メソッドを使用して依存関係の解決方法を設定する方法を示してい [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) ます。

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

この例では、依存関係の解決方法は、コンテナーに登録されているすべての型を解決するために、Autofac dependency インジェクションコンテナーを使用するラムダ式に設定されています。 それ以外の場合は、が返されます。これにより、 `null` Xamarin.Forms 型の解決が試行されます。

> [!NOTE]
> 依存関係の注入コンテナーによって使用される API は、コンテナーに固有です。 この記事のコード例では、型と型を提供する依存関係挿入コンテナーとして Autofac を使用して `IContainer` `ContainerBuilder` います。 代替の依存関係挿入コンテナーを使用することもできますが、ここに記載されているものとは異なる Api を使用します。

アプリケーションの起動時に依存関係の解決方法を設定する必要はないことに注意してください。 いつでも設定できます。 唯一の制約は、依存関係の Xamarin.Forms 挿入コンテナーに格納されている型をアプリケーションが使用しようとしたときに、依存関係の解決方法について認識する必要があることです。 そのため、アプリケーションが起動時に必要とするサービスが依存関係の挿入コンテナーに存在する場合、依存関係の解決方法はアプリケーションのライフサイクルの早い段階で設定する必要があります。 同様に、依存関係の挿入コンテナーが特定のの作成と有効期間を管理している場合、 [`Effect`](xref:Xamarin.Forms.Effect) Xamarin.Forms は、それを使用するビューを作成する前に、依存関係の解決方法について把握しておく必要があり `Effect` ます。

> [!WARNING]
> 依存関係の挿入コンテナーを使用して型を登録および解決すると、各型を作成するためにコンテナーがリフレクションを使用するため、特にアプリケーション内のページナビゲーションごとに依存関係が再構築される場合に、パフォーマンスコストがかかります。 依存関係が多いか深い場合、作成コストが大幅に増加する可能性があります。

## <a name="registering-types"></a>登録 (型を)

依存関係の解決方法を使用して型を解決するには、型を依存関係挿入コンテナーに登録する必要があります。 次のコード例は、Autofac コンテナーのクラスでサンプルアプリケーションが公開する登録メソッドを示してい `App` ます。

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

アプリケーションで依存関係の解決方法を使用してコンテナーの型を解決する場合、通常、型の登録はプラットフォームプロジェクトから実行されます。 これにより、プラットフォームプロジェクトはカスタムレンダラー、効果、および実装の型を登録でき [`DependencyService`](xref:Xamarin.Forms.DependencyService) ます。

プラットフォームプロジェクトからの型の登録後、 `IContainer` オブジェクトをビルドする必要があります。これは、メソッドを呼び出すことによって実現され `BuildContainer` ます。 このメソッドは、インスタンスで Autofac のメソッドを呼び出します。このメソッドは、作成された `Build` `ContainerBuilder` 登録を含む新しい依存関係挿入コンテナーを構築します。

次のセクションで `Logger` は、インターフェイスを実装するクラスを `ILogger` クラスコンストラクターに挿入します。 クラスは、 `Logger` メソッドを使用して単純なログ記録機能を実装 `Debug.WriteLine` し、カスタムレンダラー、効果、および実装にサービスを挿入する方法を示すために使用され [`DependencyService`](xref:Xamarin.Forms.DependencyService) ます。

### <a name="registering-custom-renderers"></a>カスタムレンダラーの登録

サンプルアプリケーションには、web ビデオを再生するページが含まれています。このページには、次の例のような XAML ソースが示されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

ビューは、 `VideoPlayer` ビデオを再生する機能を提供するクラスによって各プラットフォームに実装され `VideoPlayerRenderer` ます。 これらのカスタムレンダラークラスの詳細については、「 [ビデオプレーヤーの実装](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)」を参照してください。

IOS とユニバーサル Windows プラットフォーム (UWP) では、クラスには、 `VideoPlayerRenderer` 引数を必要とする次のコンストラクターがあり `ILogger` ます。

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

すべてのプラットフォームで、依存関係挿入コンテナーによる型登録がメソッドによって実行されます。このメソッドは、メソッドを使用して `RegisterTypes` アプリケーションを読み込むプラットフォームの前に呼び出され `LoadApplication(new App())` ます。 次の例は、iOS プラットフォームのメソッドを示してい `RegisterTypes` ます。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

この例では、 `Logger` 具象型はそのインターフェイス型に対するマッピングを介して登録され、 `VideoPlayerRenderer` 型はインターフェイスマッピングなしで直接登録されます。 ユーザーがビューを含むページに移動すると、依存関係の `VideoPlayer` 解決メソッドが呼び出され、 `VideoPlayerRenderer` 依存関係の挿入コンテナーから型を解決します。これにより、型も解決され、コンストラクターに挿入され `Logger` `VideoPlayerRenderer` ます。

`VideoPlayerRenderer`Android プラットフォームのコンストラクターは、引数に加えて引数を必要とするため、少し複雑になり `Context` `ILogger` ます。

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

次の例は、Android プラットフォームのメソッドを示してい `RegisterTypes` ます。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

この例では、 `App.RegisterTypeWithParameters` メソッドはを `VideoPlayerRenderer` 依存関係挿入コンテナーに登録します。 登録メソッドは、 `MainActivity` インスタンスが引数として挿入され、 `Context` 型が `Logger` 引数として挿入されることを保証し `ILogger` ます。

### <a name="registering-effects"></a>登録 (効果を)

サンプルアプリケーションには、タッチ追跡効果を使用して、ページの周りにインスタンスをドラッグするページが含まれてい [`BoxView`](xref:Xamarin.Forms.BoxView) ます。 は、 [`Effect`](xref:Xamarin.Forms.Effect) `BoxView` 次のコードを使用してに追加されます。

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

クラスは、である `TouchEffect` [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) クラスによって各プラットフォームに実装されるです `TouchEffect` `PlatformEffect` 。 Platform クラスには `TouchEffect` 、ページの周囲をドラッグする機能が用意されて `BoxView` います。 これらの効果クラスの詳細については、「 [効果からのイベントの呼び出し](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)」を参照してください。

すべてのプラットフォームで、 `TouchEffect` クラスには、引数を必要とする次のコンストラクターがあり `ILogger` ます。

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

すべてのプラットフォームで、依存関係挿入コンテナーによる型登録がメソッドによって実行されます。このメソッドは、メソッドを使用して `RegisterTypes` アプリケーションを読み込むプラットフォームの前に呼び出され `LoadApplication(new App())` ます。 次の例は、Android プラットフォームのメソッドを示してい `RegisterTypes` ます。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

この例では、 `Logger` 具象型はそのインターフェイス型に対するマッピングを介して登録され、 `TouchEffect` 型はインターフェイスマッピングなしで直接登録されます。 がアタッチされているインスタンスを含むページに移動すると、依存関係の [`BoxView`](xref:Xamarin.Forms.BoxView) `TouchEffect` 解決メソッドが呼び出され、 `TouchEffect` 依存関係の挿入コンテナーからプラットフォームの種類を解決します。これにより、型が解決され、コンストラクターにも挿入され `Logger` `TouchEffect` ます。

### <a name="registering-dependencyservice-implementations"></a>DependencyService 実装の登録

サンプルアプリケーションには、各プラットフォームでの実装を使用するページが含まれており、 [`DependencyService`](xref:Xamarin.Forms.DependencyService) ユーザーはデバイスの画像ライブラリから写真を選択できます。 インターフェイスは、 `IPhotoPicker` 実装によって実装される機能を定義 `DependencyService` します。次に例を示します。

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

各プラットフォームプロジェクトでは、 `PhotoPicker` クラスは、 `IPhotoPicker` プラットフォーム api を使用してインターフェイスを実装します。 これらの依存サービスの詳細については、「 [画像ライブラリからの写真の選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)」を参照してください。

IOS と UWP のクラスには、 `PhotoPicker` 引数を必要とする次のコンストラクターがあり `ILogger` ます。

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

すべてのプラットフォームで、依存関係挿入コンテナーによる型登録がメソッドによって実行されます。このメソッドは、メソッドを使用して `RegisterTypes` アプリケーションを読み込むプラットフォームの前に呼び出され `LoadApplication(new App())` ます。 次の例は、 `RegisterTypes` UWP のメソッドを示しています。

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

この例では、 `Logger` 具象型はインターフェイス型に対するマッピングを介して登録され、 `PhotoPicker` 型もインターフェイスマッピングを介して登録されます。

`PhotoPicker`Android プラットフォームのコンストラクターは、引数に加えて引数を必要とするため、少し複雑になり `Context` `ILogger` ます。

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

次の例は、Android プラットフォームのメソッドを示してい `RegisterTypes` ます。

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

この例では、 `App.RegisterTypeWithParameters` メソッドはを `PhotoPicker` 依存関係挿入コンテナーに登録します。 登録メソッドは、 `MainActivity` インスタンスが引数として挿入され、 `Context` 型が `Logger` 引数として挿入されることを保証し `ILogger` ます。

ユーザーがフォトの選択ページに移動し、写真を選択すると、 `OnSelectPhotoButtonClicked` ハンドラーが実行されます。

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

メソッドが呼び出されると、依存関係の [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 解決メソッドが呼び出され、 `PhotoPicker` 依存関係の挿入コンテナーから型を解決します。これにより、型も解決され、コンストラクターに挿入され `Logger` `PhotoPicker` ます。

> [!NOTE]
> メソッドは、を使用して [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 、アプリケーションの依存関係の挿入コンテナーから型を解決する場合に使用する必要があり [`DependencyService`](xref:Xamarin.Forms.DependencyService) ます。

## <a name="related-links"></a>関連リンク

- [コンテナーを使用した依存関係の解決 (サンプル)](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)
- [依存関係の挿入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [ビデオ プレーヤーの実装](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [呼び出し (影響からイベントを)](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [画像ライブラリからの写真の選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)