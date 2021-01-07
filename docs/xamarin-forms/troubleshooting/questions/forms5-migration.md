---
title: アプリを5.0 に移行操作方法 Xamarin.Forms ますか?
description: UWP で Android に重点を置いて、アプリを5.0 に移行する方法につい Xamarin.Forms て説明します。
ms.assetid: AD04FEE9-B8F5-4CA5-AB31-EF1225867E4B
ms.prod: xamarin
ms.technology: xamarin-forms
ms.topic: troubleshooting
author: davidbritch
ms.author: dabritch
ms.date: 10/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8f93d20dac789abed57f8f41bf41778ad50a5fb5
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972319"
---
# <a name="how-do-i-migrate-my-app-to-no-locxamarinforms-50"></a>アプリを5.0 に移行操作方法 Xamarin.Forms ますか?

Xamarin.Forms 5.0 には、次のような重大な変更が含まれています。

- `Expander` が Xamarin Community Toolkit に移動しました。 詳細については[、「」 Xamarin.Forms を参照](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms)してください。
- `MediaElement` が Xamarin Community Toolkit に移動しました。 詳細については[、「」 Xamarin.Forms を参照](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms)してください。
- DataPages および関連するプロジェクトは、から削除されました Xamarin.Forms 。
- `MasterDetailPage` の名前が `FlyoutPage` に変更されました。 同様に、 `MasterBehavior` 列挙体の名前がに変更されました `FlyoutLayoutBehavior` 。
- への参照 `UIWebView` は、iOS のから削除されました Xamarin.Forms 。
- Visual Studio 2017 のサポートは削除されました。
- XFCorePostProcessor が削除されました。 このプロジェクトは、2.5 互換性を維持するために IL を挿入 Xamarin.Forms しています。

また、5.0 でビルドされた Android および UWP プロジェクトで Xamarin.Forms は、更新が必要になります。

## <a name="android"></a>Android

5.0 でビルドされた android プロジェクトを使用する Xamarin.Forms には、開発環境に AndroidX (Android 10.0) プラットフォームをインストールしておく必要があります。 これは、Android SDK マネージャーを使用して実現できます。 AndroidX の詳細については、「 [」の Xamarin.Forms 「androidx の移行](~/xamarin-forms/platform/android/androidx-migration.md)」を参照してください。

Android プロジェクトでは、複数の更新プログラムが正しくビルドされる必要があります。

### <a name="minimum-targetframeworkversion"></a>最小 TargetFrameworkVersion

Xamarin.Forms 5.0 では、Android プロジェクト用に最小のターゲットフレームワークバージョン 10.0 (AndroidX) が必要です。 ターゲットフレームワークのバージョンは、Visual Studio または Android の .csproj ファイルで設定できます。

```xml
<TargetFrameworkVersion>v10.0</TargetFrameworkVersion>
```

この最小要件を満たしていない場合、ビルドエラーが生成されます。

```
error XF005: The $(TargetFrameworkVersion) for MyProject.Android (v9.0) is less than the minimum required $(TargetFrameworkVersion) for Xamarin.Forms (10.0). You need to increase the $(TargetFrameworkVersion) for MyProject.Android.
```

### <a name="minimum-targetsdkversion"></a>最小 TargetSDKVersion

AndroidX を使用するには、Android マニフェストでを 29 + に設定する必要があり `targetSdkVersion` ます。

```xml
<uses-sdk android:minSdkVersion="21" android:targetSdkVersion="29" />
```

これを行わないと、がに `targetSdkVersion` 設定され `minSdkVersion` ます。 また、 `Android.Views.InflateException` が適切に設定されていない場合は、次のような状況でが生成され `targetSdkVersion` ます。

```
Android.View.InflateException has been thrown.

Binary XML file line #1 in com.companyname.myproject:layout/toolbar: Binary XML file line #1 in com.companyname.myproject:/layout/toolbar: Error inflating class android.support.v7.widget.Toolbar
```

> [!NOTE]
> Visual Studio 2019 では、ターゲットフレームワークのバージョンが v2.0 に設定されている場合、Android マニフェストは自動的に更新され、API 29 のが指定され `targetSdkVersion` ます。

### <a name="automatic-migration-to-androidx"></a>AndroidX への自動移行

Android プロジェクトが直接の依存関係または推移的な依存関係として Android サポートライブラリを参照している場合、これらのサポートライブラリの依存関係とバインドは、ビルド処理中に AndroidX 依存関係に自動的にスワップされます。 自動 AndroidX 移行の詳細については、「 [」 Xamarin.Forms の「自動移行](~/xamarin-forms/platform/android/androidx-migration.md#automatic-migration-in-xamarinforms)」を参照してください。

### <a name="manual-migration-to-androidx"></a>AndroidX への手動移行

Android プロジェクトに Android サポートライブラリに対する直接または推移性の依存関係がなくても、コードを使用してサポートライブラリの種類を使用しようとする場合は、アプリを AndroidX に手動で移行する必要があります。 これは、AndroidX 型を使用し、カスタマイズされていないすべての AXML ファイルを削除することによって実現できます。

> [!TIP]
> AndroidX に手動で移行すると、アプリのビルドプロセスが最速になります。

#### <a name="use-androidx-types"></a>AndroidX 型を使用する

AndroidX は Android サポートライブラリを置き換えます。そのため、Android サポートライブラリの種類への参照は、AndroidX 型への参照に置き換える必要があります。

これは `using` `AndroidX` 、名前空間ではなく名前空間を使用するようにステートメントを更新することで実現でき `Android.Support` ます。 次の表は、Android サポートライブラリから AndroidX に移行するときの一般的な名前空間の変更の一部を示しています。

| Android サポートライブラリの名前空間 | AndroidX 名前空間 |
| --- | --- |
| `Android.Support.V4.App` | `AndroidX.Core.App` |
| `Android.Support.V4.Content` | `AndroidX.Core.Content` |
| `Android.Support.V4.App` | `AndroidX.Fragment.App` |
| `Android.Support.V7.App` | `AndroidX.AppCompat.App` |
| `Android.Support.V7.Widget` | `AndroidX.AppCompat.Widget` |

サポートライブラリから AndroidX へのクラスマッピングの完全な一覧については、「 [support library class mappings](https://developer.android.com/jetpack/androidx/migrate/class-mappings) on developer.android.com」を参照してください。

#### <a name="remove-axml-files"></a>AXML ファイルの削除

カスタマイズした AXML ファイルが使用されていない場合は、Android プロジェクトからすべての AXML ファイルを削除する必要があります。 削除した後、次の行をクラスから削除する必要があり `MainActivity` ます。

```csharp
TabLayoutResource = Resource.Layout.Tabbar;
ToolbarResource = Resource.Layout.Toolbar;
```

> [!IMPORTANT]
> カスタマイズした AXML ファイルを含む Android プロジェクトを更新して、これらのファイルが AndroidX 型を使用するようにする必要があります。

## <a name="uwp"></a>UWP

Xamarin.Forms 5.0 は、UWP プロジェクト用の >= 10.0.18362.0 のターゲットプラットフォームバージョンを推奨します。 ターゲットプラットフォームのバージョンは、Visual Studio または UWP ファイルで設定できます。

```xml
<TargetPlatformVersion Condition=" '$(TargetPlatformVersion)' == '' ">10.0.18362.0</TargetPlatformVersion>
```

UWP プロジェクトが下位のターゲットプラットフォームバージョンを使用している場合は、ビルド警告が生成されます。

## <a name="related-links"></a>関連リンク

- [移動した機能 Xamarin.Forms](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms)
- [AndroidX の移行 Xamarin.Forms](~/xamarin-forms/platform/android/androidx-migration.md)
