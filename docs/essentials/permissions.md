---
title: Xamarin.Essentials:アクセス許可
description: このドキュメントでは、Xamarin.Essentials の Permissions クラスについて説明します。これを使用すると、実行時のアクセス許可を確認および要求することができます。
ms.assetid: 34062D84-3E55-4AF7-A688-8551068B1E57
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3d0ec65b363f727834b12e6a12e832fbcf446ea9
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972358"
---
# <a name="no-locxamarinessentials-permissions"></a>Xamarin.Essentials:アクセス許可

**Permissions** クラスでは、実行時のアクセス許可を確認および要求する機能が提供されます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

[!include[](~/essentials/includes/android-permissions.md)]

## <a name="using-permissions"></a>Permissions の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="checking-permissions"></a>アクセス許可の確認

アクセス許可の現在の状態を確認するには、状態を取得する特定のアクセス許可を指定して `CheckStatusAsync` メソッドを使用します。

```csharp
var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
```

必要なアクセス許可が宣言されていない場合は、`PermissionException` がスローされます。

アクセス許可を要求する前に、状態を確認することをお勧めします。 ユーザーにメッセージが表示されない場合は、オペレーティング システムごとに異なる既定の状態を返します。 iOS は `Unknown` を返し、その他は `Denied` を返します。 状態が `Granted` 場合は、他の呼び出しを行う必要はありません。 iOS では、状態が `Denied` の場合は、設定でアクセス許可を変更するようユーザーに求める必要があります。Android では、`ShouldShowRationale` を呼び出して、ユーザーが過去に既にアクセス許可を拒否しているかどうかを検出することができます。

## <a name="requesting-permissions"></a>権限の要求

ユーザーからのアクセス許可を要求するには、要求する特定のアクセス許可を指定して `RequestAsync` メソッドを使用します。 ユーザーが以前にアクセス許可を付与していて、まだ取り消していない場合は、このメソッドによって直ちに `Granted` が返され、ダイアログは表示されません。

```csharp
var status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
```

必要なアクセス許可が宣言されていない場合は、`PermissionException` がスローされます。

一部のプラットフォームでは、アクセス許可要求をアクティブにできるのは 1 回だけであることに注意してください。 アクセス許可が `Denied` 状態であるかどうかを確認し、手動で有効にするようにユーザーに依頼するには、開発者がさらにプロンプトを処理する必要があります。 

## <a name="permission-status"></a>アクセス許可の状態

`CheckStatusAsync` または `RequestAsync` を使用すると、`PermissionStatus` が返されます。これを使って次の手順を決定できます。

* Unknown - アクセス許可は不明な状態です
* Denied - ユーザーがアクセス許可要求を拒否しました
* Disabled - この機能はデバイス上で無効になっています
* Granted - ユーザーがアクセス許可を付与したか、自動的に付与されます
* Restricted - 制限された状態です


## <a name="explain-why-permission-is-needed"></a>アクセス許可が必要な理由を説明する

アプリケーションに特定のアクセス許可が必要な理由を説明することをお勧めします。 iOS では、ユーザーに表示される文字列を指定する必要があります。 Android にはこの機能がなく、アクセス許可の状態の既定値は "Disabled" です。 そのため、ユーザーがアクセス許可を拒否したかどうか、またはユーザーにプロンプトを表示するのが初めてかどうかはわかりません。 `ShouldShowRationale` メソッドを使用すると、教育用 UI を表示する必要があるかどうかを判断できます。 このメソッドが `true` を返す場合、ユーザーは、過去にアクセス許可を拒否または無効にしています。 このメソッドを呼び出すと、他のプラットフォームは常に `false` を返します。

## <a name="available-permissions"></a>利用可能なアクセス許可

Xamarin.Essentials では、可能な限り多くのアクセス許可を抽象化しようとします。 しかし、各オペレーティング システムには、それぞれ異なる実行時のアクセス許可セットが備わっています。 また、一部のアクセス許可に対して 1 つの API を提供する場合には、違いが生じます。 現在利用可能なアクセス許可についてのガイドを次に示します。

アイコンによるガイド:

* ![完全サポート](~/media/shared/yes.png "完全サポート") - サポートされています
* ![サポートされていません](~/media/shared/no.png "サポートされていないか、不要です") - サポートされていないか、不要です

| アクセス許可 | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---:
| CalendarRead   | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされています](~/media/shared/yes.png "watchOS はサポートされています") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| CalendarWrite | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされています](~/media/shared/yes.png "watchOS はサポートされています") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Camera | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされています](~/media/shared/yes.png "Tizen はサポートされています") |
| ContactsRead | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされています](~/media/shared/yes.png "UWP はサポートされています") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| ContactsWrite | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされています](~/media/shared/yes.png "UWP はサポートされています") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Flashlight | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされていません](~/media/shared/no.png "iOS はサポートされていません") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされています](~/media/shared/yes.png "Tizen はサポートされています") |
| LocationWhenInUse | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされています](~/media/shared/yes.png "UWP はサポートされています") | ![watchOS はサポートされています](~/media/shared/yes.png "watchOS はサポートされています") | ![tvOS はサポートされています](~/media/shared/yes.png "tvOS はサポートされています")  | ![Tizen はサポートされています](~/media/shared/yes.png "Tizen はサポートされています") |
| LocationAlways | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされています](~/media/shared/yes.png "UWP はサポートされています") | ![watchOS はサポートされています](~/media/shared/yes.png "watchOS はサポートされています") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされています](~/media/shared/yes.png "Tizen はサポートされています") |
| Media | ![Android はサポートされていません](~/media/shared/no.png "Android はサポートされていません") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Microphone | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされています](~/media/shared/yes.png "UWP はサポートされています") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされています](~/media/shared/yes.png "Tizen はサポートされています") |
| Phone | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Photos | ![Android はサポートされていません](~/media/shared/no.png "Android はサポートされていません") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされています](~/media/shared/yes.png "tvOS はサポートされています") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Reminders | ![Android はサポートされていません](~/media/shared/no.png "Android はサポートされていません") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされています](~/media/shared/yes.png "watchOS はサポートされています") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Sensors | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされています](~/media/shared/yes.png "UWP はサポートされています") | ![watchOS はサポートされています](~/media/shared/yes.png "watchOS はサポートされています") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Sms | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| Speech | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされています](~/media/shared/yes.png "iOS はサポートされています") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| StorageRead | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされていません](~/media/shared/no.png "iOS はサポートされていません") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |
| StorageWrite | ![Android はサポートされています](~/media/shared/yes.png "Android はサポートされています") | ![iOS はサポートされていません](~/media/shared/no.png "iOS はサポートされていません") | ![UWP はサポートされていません](~/media/shared/no.png "UWP はサポートされていません") | ![watchOS はサポートされていません](~/media/shared/no.png "watchOS はサポートされていません") | ![tvOS はサポートされていません](~/media/shared/no.png "tvOS はサポートされていません") | ![Tizen はサポートされていません](~/media/shared/no.png "Tizen はサポートされていません") |

アクセス許可に ![サポートされていません](~/media/shared/no.png "サポート外")マークが付いている場合は、確認または要求されたときに常に `Granted` が返されます。

## <a name="general-usage"></a>一般的な使用法

次のコードは、アクセス許可が付与されているかどうかを判断し、付与されていない場合は要求するための一般的な使用パターンを示しています。 このコードは、Xamarin.Essentials バージョン 1.6.0 以降で使用できる機能を使用しています。

```csharp
public async Task<PermissionStatus> CheckAndRequestLocationPermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
    
    if (status == PermissionStatus.Granted)
        return status;        
    
    if (status == PermissionStatus.Denied && DeviceInfo.Platform == DevicePlatform.iOS)
    {
        // Prompt the user to turn on in settings
        // On iOS once a permission has been denied it may not be requested again from the application
        return status;
    }
    
    if (Permissions.ShouldShowRationale<Permissions.LocationWhenInUse>())
    {
        // Prompt the user with additional information as to why the permission is needed
    }   

    status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();

    return status;
}
```

各種類のアクセス許可では、そのインスタンスを作成して、メソッドを直接呼び出すことができます。

```csharp
public async Task GetLocationAsync()
{
    var status = await CheckAndRequestPermissionAsync(new Permissions.LocationWhenInUse());
    if (status != PermissionStatus.Granted)
    {
        // Notify user permission was denied
        return;
    }

    var location = await Geolocation.GetLocationAsync();
}

public async Task<PermissionStatus> CheckAndRequestPermissionAsync<T>(T permission)
            where T : BasePermission
{
    var status = await permission.CheckStatusAsync();
    if (status != PermissionStatus.Granted)
    {
        status = await permission.RequestAsync();
    }

    return status;
}
```

## <a name="extending-permissions"></a>アクセス許可の拡張

Permissions API は、Xamarin.Essentials に含まれていない追加の検証やアクセス許可が必要なアプリケーションに対して、柔軟かつ拡張可能であるように作成されています。 `BasePermission` から継承した新しいクラスを作成し、必要な抽象メソッドを実装します。

```csharp
public class MyPermission : BasePermission
{
    // This method checks if current status of the permission
    public override Task<PermissionStatus> CheckStatusAsync()
    {
        throw new System.NotImplementedException();
    }

    // This method is optional and a PermissionException is often thrown if a permission is not declared
    public override void EnsureDeclared()
    {
        throw new System.NotImplementedException();
    }

    // Requests the user to accept or deny a permission
    public override Task<PermissionStatus> RequestAsync()
    {
        throw new System.NotImplementedException();
    }
}
```

特定のプラットフォームでアクセス許可を実装する場合、`BasePlatformPermission` クラスから継承することができます。 これにより、宣言を自動的に確認するための、追加のプラットフォーム ヘルパー メソッドが提供されます。 これは、グループ化を行うカスタム アクセス許可を作成する場合に役立ちます。 たとえば、次のカスタム アクセス許可を使用して、Android 上のストレージへの読み取りアクセスと書き込みアクセスの両方を要求できます。

```csharp
public class ReadWriteStoragePermission : Xamarin.Essentials.Permissions.BasePlatformPermission
{
    public override (string androidPermission, bool isRuntime)[] RequiredPermissions => new List<(string androidPermission, bool isRuntime)>
    {
        (Android.Manifest.Permission.ReadExternalStorage, true),
        (Android.Manifest.Permission.WriteExternalStorage, true)
    }.ToArray();
}
```

その後、Android プロジェクトから新しいアクセス許可を呼び出すことができます。

```csharp
await Permissions.RequestAsync<ReadWriteStoragePermission>();
```

共有コードからこの API を呼び出す場合、インターフェイスを作成し、[依存関係サービス](../xamarin-forms/app-fundamentals/dependency-service/index.md)を使用して実装を登録したり、取得したりできます。

```csharp
public interface IReadWritePermission
{        
    Task<PermissionStatus> CheckStatusAsync();
    Task<PermissionStatus> RequestAsync();
}
```

その後、プラットフォーム プロジェクト内にインターフェイスを実装します。

```csharp
public class ReadWriteStoragePermission : Xamarin.Essentials.Permissions.BasePlatformPermission, IReadWritePermission
{
    public override (string androidPermission, bool isRuntime)[] RequiredPermissions => new List<(string androidPermission, bool isRuntime)>
    {
        (Android.Manifest.Permission.ReadExternalStorage, true),
        (Android.Manifest.Permission.WriteExternalStorage, true)
    }.ToArray();
}
```

次に、特定の実装を登録できます。

```csharp
DependencyService.Register<IReadWritePermission, ReadWriteStoragePermission>();
```
その後、共有プロジェクトからそれを解決したり、使用したりできます。

```csharp
var readWritePermission = DependencyService.Get<IReadWritePermission>();
var status = await readWritePermission.CheckStatusAsync();
if (status != PermissionStatus.Granted)
{
    status = await readWritePermission.RequestAsync();
}
```

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="android"></a>[Android](#tab/android)

アクセス許可には、Android マニフェスト ファイルに一致する属性が設定されている必要があります。 アクセス許可の状態の既定値は "Denied" です。

詳細については、「[Xamarin.Android のアクセス許可](../android/app-fundamentals/permissions.md)」のドキュメントをご覧ください。

# <a name="ios"></a>[iOS](#tab/ios)

アクセス許可は、`Info.plist` ファイルの文字列に一致する必要があります。 一度アクセス許可を要求して拒否されると、もう一度アクセス許可を要求してもポップアップが表示されなくなります。 iOS のアプリケーション設定画面で設定を手動で調整するようにユーザーに要求する必要があります。 アクセス許可の状態の既定値は "Unknown" です。

詳細については、「[iOS のセキュリティとプライバシーの機能](../ios/app-fundamentals/security-privacy.md)」のドキュメントをご覧ください。

# <a name="uwp"></a>[UWP](#tab/uwp)

アクセス許可には、パッケージ マニフェストで宣言された機能と一致する機能が備わっている必要があります。 ほとんどのインスタンスで、アクセス許可の状態の既定値は "Unknown" です。

詳細については、[アプリ機能の宣言](/windows/uwp/packaging/app-capability-declarations)に関するドキュメントをご覧ください。

--------------

## <a name="api"></a>API

- [アクセス許可のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Permissions)
- [Permissions API のドキュメント](xref:Xamarin.Essentials.Permissions)


## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Permissions-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
