---
title: Xamarin.Forms の既存のアプリの更新
description: このドキュメントでは、Unified API にクラシック API から Xamarin.Forms アプリを更新する従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d5c16b034b07d3e9875412f041c16b293557438a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781840"
---
# <a name="updating-existing-xamarinforms-apps"></a>Xamarin.Forms の既存のアプリの更新

_アプリを更新する既存 Xamarin.Forms Unified API を使用して、バージョン 1.3.1 を更新したりする次の手順します。_

> [!IMPORTANT]
> Xamarin.Forms 1.3.1 は、Unified API をサポートしている最初のリリースであるために、統合に iOS アプリを移行すると同時に、最新バージョンを使用するソリューション全体を更新する必要があります。 つまり、ことに加えて、iOS サポートをプロジェクトに統合を更新するには、必要がありますもコードを編集_すべて_ソリューション内のプロジェクトです。

更新プログラムは、2 つの手順で実行されます。

1. IOS アプリを Visual Studio を使用して、移行ツールでの Mac のビルドの Unified API に移行します。

    - 移行ツールを使用すると、自動的にプロジェクトを更新します。

    - 更新プログラムの iOS のネイティブ Api への指示に従って[iOS アプリを更新する](~/cross-platform/macios/unified/updating-ios-apps.md)(具体的にはカスタム レンダラーや依存関係サービス コードに)。

2. Xamarin.Forms バージョン 1.3 をソリューション全体を更新します。

    1. 1.3.1 の Xamarin.Forms NuGet パッケージをインストールします。

    2. 更新プログラム、`App`共有コード内のクラスです。

    3. 更新プログラム、 `AppDelegate` iOS プロジェクトでします。

    4. 更新プログラム、 `MainActivity` Android プロジェクトでします。

    5. 更新プログラム、 `MainPage` Windows Phone プロジェクトでします。

## <a name="1-ios-app-unified-migration"></a>1. iOS アプリ (ユニファイド移行)

移行の一部では、Unified API をサポートするバージョン 1.3、Xamarin.Forms をアップグレードする必要があります。 適切なアセンブリ参照を作成するためには、まず Unified API を使用して iOS プロジェクトを更新する必要があります。

### <a name="migration-tool"></a>移行ツール

IOS プロジェクトをクリックしてが選択されているようにクリックして**プロジェクト > Xamarin.iOS Unified API への移行しています.** 警告メッセージが表示される同意します。

![](updating-xamarin-forms-apps-images/beta-tool1.png "プロジェクトを選択 >. Xamarin.iOS Unified API に移行し、表示される警告メッセージに同意します。")

これは自動的に。

 - 一元化された 64 ビット API をサポートするために、プロジェクトの種類を変更します。
 - フレームワーク参照に変更**Xamarin.iOS** (古いを置き換える**monotouch**参照)。
 - 名前空間の参照を削除するコードでの変更、`MonoTouch`プレフィックス。
 - 更新プログラム、 **csproj** Unified API の適切なビルド ターゲットを使用するファイル。

**クリーン**と**ビルド**プロジェクトを他のエラーの修正がないことを確認します。 これ以上の操作は必要ありません。 詳細については、これらの手順の説明、 [Unified API ドキュメント](~/cross-platform/macios/unified/updating-ios-apps.md)です。

### <a name="update-native-ios-apis-if-required"></a>(必要な場合) は、ネイティブの iOS Api を更新します。

(カスタム レンダラーや依存関係サービス) などその他の iOS のネイティブ コードを追加している場合は、追加の手動によるコードの修正プログラムを実行する必要があります。 アプリを再コンパイルしを参照してください、[既存の更新の iOS アプリの指示](~/cross-platform/macios/unified/updating-ios-apps.md)で必要な変更の詳細についてはします。 [これらのヒント](~/cross-platform/macios/unified/updating-tips.md)必要とされる変化を特定することができます。

## <a name="2-xamarinforms-131-update"></a>2.Xamarin.Forms 1.3.1 更新

Unified API に iOS アプリを更新すると、ソリューションの残りの部分は、Xamarin.Forms バージョン 1.3.1 に更新する必要があります。 バインディングには、以下の項目が含まれます。

 - 各プロジェクト内の Xamarin.Forms NuGet パッケージを更新しています。
 - 新しい Xamarin.Forms を使用するコードを変更する`Application`、 `FormsApplicationDelegate` (iOS)、 `FormsApplicationActivity` (Android)、および`FormsApplicationPage`(Windows Phone) クラス。

これらの手順を説明します。

### <a name="21-update-nuget-in-all-projects"></a>2.1 は、すべてのプロジェクトで NuGet を更新します。

Xamarin.Forms を 1.3.1 に更新リリース前のソリューション内のすべてのプロジェクトの NuGet Package Manager を使用して: PCL (存在する場合)、iOS、Android、および Windows Phone です。 お勧めした**を削除してから再度追加**Xamarin.Forms NuGet パッケージをバージョン 1.3 に更新します。

**注:** Xamarin.Forms バージョン 1.3.1 は現在*プレリリース*です。 つまり、選択する必要があります、**プレリリース**オプション NuGet (ティックのボックスで Visual Studio for Mac) またはの一覧で Visual Studio を使用して最新のリリース前のバージョンを表示します。

> [!IMPORTANT]
> Visual Studio を使用している場合は、最新バージョンの NuGet パッケージ マネージャーがインストールされていることを確認します。 以前のバージョンの Visual Studio での NuGet は Xamarin.Forms 1.3.1 の統一されたバージョンを正しくインストールされません。 移動して**ツール > 拡張機能と更新しています.**  をクリックし、**インストール済み**ことを確認する ボックスの一覧、 **for Visual Studio の NuGet Package Manager**少なくともバージョン 2.8.5 します。 古い場合は、をクリックして、**更新**一覧を最新バージョンをダウンロードします。

Xamarin.Forms 1.3.1 の NuGet パッケージを更新すると新しいにアップグレードするには、各プロジェクトで、次の変更を行う`Xamarin.Forms.Application`クラスです。

### <a name="22-portable-class-library-or-shared-project"></a>2.2 ポータブル クラス ライブラリ (または共有プロジェクト)

変更、 **App.cs**ファイルできるようにします。

 - `App`からクラスを今すぐ継承`Application`です。
 - `MainPage`プロパティを表示する最初のコンテンツ ページに設定されています。

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

完全に削除しました、`GetMainPage`メソッド、代わりに設定し、 `MainPage` *プロパティ*で、`Application`サブクラスです。

この新しい`Application`基本クラスもサポート、 `OnStart`、 `OnSleep`、および`OnResume`アプリケーションのライフ サイクルを管理するためにオーバーライドします。

`App`クラスは、新しいに渡され`LoadApplication`メソッド各アプリ プロジェクトは、次のようにします。

### <a name="23-ios-app"></a>2.3 iOS アプリ

変更、 **<code>appdelegate.cs</code>** ファイルできるようにします。

 - クラスを継承`FormsApplicationDelegate`(の代わりに`UIApplicationDelegate`以前)。
 - `LoadApplication` 新しいインスタンスで呼び出された`App`です。

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="23-android-app"></a>2.3 android アプリ

変更、 **MainActivity.cs**ファイルできるようにします。

 - クラスを継承`FormsApplicationActivity`(の代わりに`FormsActivity`以前)。
 - `LoadApplication` 新しいインスタンスで呼び出されます `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="24-windows-phone-app"></a>2.4 Windows Phone アプリ

更新する必要があります、 **MainPage** -XAML と分離コードの両方です。

変更、 **MainPage.xaml**ファイルできるようにします。

 - XAML ルート要素にする必要があります`winPhone:FormsApplicationPage`です。
 - `xmlns:phone`属性は*変更*に `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

更新された例を次に、- のみが必要 (残りの属性は同じまま) は次の作業を編集します。

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

変更、 **MainPage.xaml.cs**ファイルできるようにします。

 - クラスを継承`FormsApplicationPage`(の代わりに`PhoneApplicationPage`以前)。
 - `LoadApplication` Xamarin.Forms の新しいインスタンスで呼び出されます`App`クラスです。 Windows Phone があるために、この参照を完全に修飾する必要があります、独自の`App`クラスが既に定義されています。

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### <a name="troubleshooting"></a>トラブルシューティング

場合によっては、Xamarin.Forms NuGet パッケージを更新した後は、次のようなエラーが表示されます。 NuGet updater がから以前のバージョンへの参照を完全に削除していないときに発生、 **csproj**ファイル。

>\_PROJECT.csproj: エラー: このプロジェクトは、このコンピューターに不足している NuGet パッケージを参照します。 ダウンロードする NuGet パッケージの復元を有効にします。  詳細については、「http://go.microsoft.com/fwlink/?LinkID=322105」を参照してください。 見つからないファイルは、./../packages/Xamarin.Forms.1.2.3.6257/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets です。 (、\_プロジェクト)

これらのエラーを修正するには、開きます、 **csproj**をテキスト エディターでファイルを探します.`<Target`要素を次に示す要素など、Xamarin.Forms の古いバージョンを参照してください。 この全体の要素から手動で削除する必要があります、 **csproj**ファイルし、変更を保存します。

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

これらの古い参照が削除されると、プロジェクトが正常にビルドする必要があります。

## <a name="considerations"></a>注意事項

次の考慮事項は、コンポーネントまたは NuGet パッケージの 1 つまたは複数のアプリケーションが依存している場合は、新しい Unified API を Classic API から Xamarin.Forms の既存のプロジェクトを変換時に考慮する必要があります。

### <a name="components"></a>コンポーネント

アプリケーションに含まれている任意のコンポーネントも Unified API に更新する必要があります。 またはコンパイルしようとすると、競合が発生します。 含まれるコンポーネントの Unified API をサポートする Xamarin コンポーネント ストアから新しいバージョンで現在のバージョンを置き換えるし、クリーン ビルドを実行します。 32 ビットの唯一のコンポーネント ストアで警告の作成者によって変換されていない任意のコンポーネントが表示されます。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API のサポートを使用する NuGet への変更の原因は、中にされていない、NuGet の新しいリリースを新しい Api を認識する NuGet を取得する方法を検討しているためです。

コンポーネントと同じように、その時点までには、Unified Api をサポートするバージョンに、プロジェクトに含めるすべての NuGet パッケージを切り替えるし、その後、クリーン ビルドを実行する必要があります。

> [!IMPORTANT]
> フォームでエラーがあれば _"エラー 3 は、同じ Xamarin.iOS プロジェクトに 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません - 'Xamarin.iOS.dll' は 'monotouch.dll' によって参照されますが、明示的に参照される ' xxx、バージョン = 0.0.000、Culture = neutral, PublicKeyToken = null'"_ Unified Api にアプリを変換した後は、通常、統合 API が更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージを持つためです。 削除する既存のコンポーネント/NuGet Unified Api をサポートするバージョンに更新して、クリーン ビルドを行う必要があります。

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS アプリのビルド時に 64 ビットの有効化

Unified API に変換された、Xamarin.iOS モバイル アプリケーションを開発者もする必要がありますを使用するアプリのオプションから 64 ビット コンピューターでアプリケーションの構築します。 参照してください、**を有効にする 64 ビット ビルドの Xamarin.iOS アプリ**の[32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64/index.md#enable-64)64 ビットを有効にする方法の詳細な手順についてはドキュメントを作成します。

## <a name="summary"></a>まとめ

Xamarin.Forms アプリケーションがバージョン 1.3.1 に更新され、iOS アプリは、Unified API (iOS プラットフォームでは、64 ビット アーキテクチャをサポートしています) に移行します。

Xamarin.Forms アプリには、カスタム レンダラーなどのネイティブ コードが含まれています。 または、これらも更新が必要な新しい種類を使用し、サービスの依存関係がある場合、上に示したように[Unified API で導入された](~/cross-platform/macios/index.md)です。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](~/cross-platform/macios/unified/updating-apps.md)
- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [ヒントの更新](~/cross-platform/macios/unified/updating-tips.md)
- [従来の vs Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
