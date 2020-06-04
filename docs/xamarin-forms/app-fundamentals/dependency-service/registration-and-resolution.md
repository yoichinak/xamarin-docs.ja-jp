---
title: Xamarin.Forms の DependencyService の登録と解決
description: この記事では、Xamarin.Forms の DependencyService クラスを使用してネイティブ プラットフォームの機能を呼び出す方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 50d77e9ba41767aa1f676bf21994431844fc4530
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138776"
---
# <a name="xamarinforms-dependencyservice-registration-and-resolution"></a>Xamarin.Forms の DependencyService の登録と解決

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

Xamarin.Forms の [`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用してネイティブ プラットフォームの機能を呼び出す場合、プラットフォームの実装を `DependencyService` に登録してから、共有コードから解決してそれらを呼び出す必要があります。

## <a name="register-platform-implementations"></a>プラットフォームの実装の登録

プラットフォームの実装を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録して、実行時に Xamarin.Forms でそれらを検索できるようにする必要があります。

登録は、[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)、または [`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドを使用して実行できます。

> [!IMPORTANT]
> .NET ネイティブ コンパイルを使用する UWP プロジェクトのリリース ビルドでは、[`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドを使用してプラットフォームの実装を登録する必要があります。

### <a name="registration-by-attribute"></a>属性による登録

[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) を使用して、プラットフォームの実装を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録できます。 属性は、指定した型によってインターフェイスの具象実装が提供されることを示します。

次の例は、[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) を使用した `IDeviceOrientationService` インターフェイスの iOS 実装の登録を示しています。

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DeviceOrientationService))]
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ...
        }
    }
}
```

この例では、[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) によって `DeviceOrientationService` を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録しています。 その結果として、実装されるインターフェイスに対して具象型が登録されます。

同様に、[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) を使用して他のプラットフォーム上の `IDeviceOrientationService` インターフェイスの実装を登録する必要があります。

> [!NOTE]
> [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) による登録は、名前空間レベルで実行されます。

### <a name="registration-by-method"></a>メソッドによる登録

[`DependencyService.Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドを使用して、プラットフォームの実装を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録できます。

次の例は、[`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドを使用した `IDeviceOrientationService` インターフェイスの iOS 実装の登録を示しています。

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new App());
        DependencyService.Register<IDeviceOrientationService, DeviceOrientationService>();
        return base.FinishedLaunching(app, options);
    }
}
```

この例では、[`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドにより `IDeviceOrientationService` インターフェイスに対して具象型 `DeviceOrientationService` を登録しています。 または、[`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドのオーバーロードを使用して、プラットフォームの実装を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録できます。

```csharp
DependencyService.Register<DeviceOrientationService>();
```

この例では、[`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドによって `DeviceOrientationService` を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録しています。 その結果として、実装されるインターフェイスに対して具象型が登録されます。

同様に、[`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドを使用して他のプラットフォーム上の `IDeviceOrientationService` インターフェイスの実装を登録できます。

> [!IMPORTANT]
> [`Register`](xref:Xamarin.Forms.DependencyService.Register*) メソッドによる登録は、プラットフォームの実装によって提供される機能が共有コードから呼び出される前に、プラットフォーム プロジェクト内で実行される必要があります。

## <a name="resolve-the-platform-implementations"></a>プラットフォームの実装の解決

プラットフォームの実装は、呼び出される前に解決される必要があります。 これは一般に、[`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドを使用して共有コード内で実行されます。 ただし、これは [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) メソッドを使用して実現することもできます。

既定では、パラメーターなしのコンストラクターがあるプラットフォームの実装だけが、[`DependencyService`](xref:Xamarin.Forms.DependencyService) によって解決されます。 ただし、依存関係挿入コンテナーまたはファクトリの方法を使用してプラットフォームの実装を解決する依存関係解決方法を、Xamarin.Forms に挿入することができます。 この方法を使用して、パラメーターがあるコンストラクターを持つプラットフォームの実装を解決できます。 詳細については、「[Xamarin.Forms での依存関係の解決](~/xamarin-forms/internals/dependency-resolution.md)」を参照してください。

> [!IMPORTANT]
> [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録されていないプラットフォームの実装を呼び出すと、`NullReferenceException` がスローされます。

### <a name="resolve-using-the-getlttgt-method"></a>Get&lt;T&gt; メソッドを使用した解決

[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドでは、実行時にインターフェイス `T` のプラットフォームの実装が取得され、そのインスタンスがシングルトンとして作成されます。 このインスタンスは、アプリケーションの有効期間にわたって存続し、同じプラットフォームの実装を解決するための後続の呼び出しでは同じインスタンスが取得されます。

次のコードは、[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドを呼び出して `IDeviceOrientationService` インターフェイスを解決してから、その `GetOrientation` メソッドを呼び出す例を示しています。

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

または、このコードは、1 行に圧縮できます。

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドでは、既定でインターフェイス `T` のプラットフォームの実装のインスタンスがシングルトンとして作成されます。 ただし、この動作は変更可能です。 詳細については、「[解決済みのオブジェクトの有効期間の管理](#manage-the-lifetime-of-resolved-objects)」をご覧ください。

### <a name="resolve-using-the-resolvelttgt-method"></a>Resolve&lt;T&gt; メソッドを使用した解決

[`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) メソッドでは、[`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver) クラスで Xamarin.Forms に挿入された依存関係の解決メソッドを使用して、実行時にインターフェイス `T` のプラットフォームの実装が取得されます。 依存関係の解決メソッドが Xamarin.Forms に挿入されていない場合、`Resolve<T>` メソッドでは、[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドの呼び出しにフォールバックしてプラットフォームの実装が取得されます。 Xamarin.Forms への依存関係の解決メソッドの挿入の詳細については、「[Xamarin.Forms での依存関係の解決](~/xamarin-forms/internals/dependency-resolution.md)」をご覧ください。

次のコードは、[`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) メソッドを呼び出して `IDeviceOrientationService` インターフェイスを解決してから、その `GetOrientation` メソッドを呼び出す例を示しています。

```csharp
IDeviceOrientationService service = DependencyService.Resolve<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

または、このコードは、1 行に圧縮できます。

```csharp
DeviceOrientation orientation = DependencyService.Resolve<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) メソッドで [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドの呼び出しにフォールバックする場合、既定でインターフェイス `T` のプラットフォームの実装のインスタンスがシングルトンとして作成されます。 ただし、この動作は変更可能です。 詳細については、「[解決済みのオブジェクトの有効期間の管理](#manage-the-lifetime-of-resolved-objects)」をご覧ください。

## <a name="manage-the-lifetime-of-resolved-objects"></a>解決済みのオブジェクトの有効期間の管理

[`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスの既定の動作では、プラットフォームの実装がシングルトンとして解決されます。 そのため、プラットフォームの実装は、アプリケーションの有効期間にわたって存続します。

この動作は、[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドと [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) メソッドの省略可能な [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) 引数によって指定されます。 [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) 列挙体で 2 つのメンバーを定義します。

- プラットフォームの実装をシングルトンとして返す `GlobalInstance`。
- プラットフォームの実装の新しいインスタンスを返す `NewInstance`。 アプリケーションは、プラットフォームの実装インスタンスの有効期間を管理する責任を負います。

[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) メソッドと [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) メソッドの両方で、省略可能な引数が [`DependencyFetchTarget.GlobalInstance`](xref:Xamarin.Forms.DependencyFetchTarget) に設定されるので、プラットフォームの実装は常にシングルトンとして解決されます。 [`DependencyFetchTarget.NewInstance`](xref:Xamarin.Forms.DependencyFetchTarget) を `Get<T>` メソッドと `Resolve<T>` メソッドへの引数として指定して、プラットフォームの実装の新しいインスタンスが作成されるように、この動作を変更できます。

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
```

この例では、[`DependencyService`](xref:Xamarin.Forms.DependencyService) によって `ITextToSpeechService` インターフェイス用にプラットフォームの実装の新しいインスタンスが作成されます。 `ITextToSpeechService` を解決するための後続の呼び出しでも、新しいインスタンスが作成されます。

プラットフォームの実装の新しいインスタンスが常に作成される結果として、アプリケーションがインスタンスの有効期間を管理する責任を負うようになります。 これは、プラットフォームの実装内に定義されているイベントに登録する場合に、プラットフォームの実装が不要になったときにイベントから登録を解除する必要があることを意味します。 さらにこれは、プラットフォームの実装では `IDisposable` を実装し、`Dispose` メソッド内でそれらのリソースをクリーンアップすることが必要な場合があることを意味します。 サンプル アプリケーションでは、`TextToSpeechService` プラットフォームの実装におけるこのシナリオを示しています。

アプリケーションでは、`IDisposable` を実装するプラットフォームの実装を使い終わったら、オブジェクトの `Dispose` 実装を呼び出す必要があります。 これを行うための 1 つの方法は、`using` ステートメントを使用することです。

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
using (service as IDisposable)
{
    await service.SpeakAsync("Hello world");
}
```

この例では、`SpeakAsync` メソッドが呼び出された後に、`using` ステートメントによってプラットフォームの実装オブジェクトが自動的に破棄されます。 その結果として、必要なクリーンアップを実行するオブジェクトの `Dispose` メソッドが呼び出されます。

オブジェクトの `Dispose` メソッドの呼び出しの詳細については、「[IDisposable を実装するオブジェクトの使用](/dotnet/standard/garbage-collection/using-objects)」をご覧ください。

## <a name="related-links"></a>関連リンク

- [DependencyService のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Xamarin.Forms での依存関係の解決](~/xamarin-forms/internals/dependency-resolution.md)
