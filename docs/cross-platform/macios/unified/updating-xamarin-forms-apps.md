---
title: 既存の Xamarin.Forms アプリを更新します。
description: このドキュメントでは、Unified API にクラシック API から Xamarin.Forms アプリの更新に従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d5c16b034b07d3e9875412f041c16b293557438a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61211869"
---
# <a name="updating-existing-xamarinforms-apps"></a>既存の Xamarin.Forms アプリを更新します。

_Unified API を使用して、バージョン 1.3.1 に更新する既存の Xamarin.Forms アプリを更新する次の手順します。_

> [!IMPORTANT]
> Xamarin.Forms 1.3.1 は、Unified API をサポートしている最初のリリースであるために、iOS アプリ統合への移行と同時に最新のバージョンを使用するソリューション全体を更新する必要があります。 つまり、統合のサポート用の iOS プロジェクトを更新するには、に加えても必要があるコードを編集_すべて_ソリューション内のプロジェクト。

更新プログラムは、2 つの手順で実行されます。

1. IOS アプリを Visual Studio を使用して、移行ツールでの Mac のビルドの Unified API に移行します。

    - 移行ツールを使用すると、自動的にプロジェクトを更新します。

    - 更新プログラムの iOS のネイティブ Api のための指示に従って[iOS アプリを更新する](~/cross-platform/macios/unified/updating-ios-apps.md)(具体的にはカスタム レンダラーまたは依存関係サービス コードで)。

2. Xamarin.Forms バージョン 1.3 のソリューション全体を更新します。

    1. 1.3.1 Xamarin.Forms NuGet パッケージをインストールします。

    2. 更新プログラム、`App`共有コード内のクラス。

    3. 更新プログラム、 `AppDelegate` iOS プロジェクトにします。

    4. 更新プログラム、 `MainActivity` Android プロジェクトでします。

    5. 更新プログラム、 `MainPage` Windows Phone プロジェクト。

## <a name="1-ios-app-unified-migration"></a>1. iOS アプリ (Unified 移行)

移行の一部では、Unified API をサポートするバージョン 1.3 に Xamarin.Forms をアップグレードする必要があります。 適切なアセンブリ参照を作成するためには、まず、Unified API を使用する iOS プロジェクトを更新する必要があります。

### <a name="migration-tool"></a>移行ツール

IOS プロジェクトが選択されているようにし、選択**プロジェクト > Xamarin.iOS Unified API に移行しています.** 同意する警告メッセージが表示されます。

![](updating-xamarin-forms-apps-images/beta-tool1.png "プロジェクトを選択 > Xamarin.iOS Unified API に移行しています.警告メッセージが表示される同意")

これが自動的には。

 - 統合された 64 ビット API をサポートするためにプロジェクトの種類を変更します。
 - フレームワーク参照を変更**Xamarin.iOS** (古いを置き換える**monotouch**参照)。
 - 名前空間の参照を削除するコードでの変更、`MonoTouch`プレフィックス。
 - 更新プログラム、 **csproj** Unified API に適切なビルド ターゲットを使用するファイル。

**クリーン**と**ビルド**プロジェクトを修正するには、他のエラーがないことを確認します。 これ以上の操作は必要ありません。 次の手順で詳しく説明、 [Unified API docs](~/cross-platform/macios/unified/updating-ios-apps.md)します。

### <a name="update-native-ios-apis-if-required"></a>(必要な) 場合は、ネイティブ iOS Api を更新します。

追加の iOS のネイティブ コード (カスタム レンダラーや依存関係サービス) を追加した場合は、追加の手動によるコードの修正プログラムを実行する必要があります。 アプリを再コンパイルしを参照してください、[既存の更新の iOS アプリの指示](~/cross-platform/macios/unified/updating-ios-apps.md)のために必要な変更の詳細についてはします。 [これらのヒント](~/cross-platform/macios/unified/updating-tips.md)に必要な変更を識別することができます。

## <a name="2-xamarinforms-131-update"></a>2.Xamarin.Forms 1.3.1 Update

Unified API を iOS アプリを更新すると、ソリューションの残りの部分は、Xamarin.Forms バージョン 1.3.1 に更新する必要があります。 バインディングには、以下の項目が含まれます。

 - 各プロジェクト内の Xamarin.Forms NuGet パッケージを更新しています。
 - 新しい Xamarin.Forms を使用するコードを変更する`Application`、 `FormsApplicationDelegate` (iOS) `FormsApplicationActivity` (Android) および`FormsApplicationPage`(Windows Phone) クラス。

次の手順は以下について説明します。

### <a name="21-update-nuget-in-all-projects"></a>2.1 のすべてのプロジェクトで NuGet を更新します。

1.3.1 する Xamarin.Forms の更新リリース前のソリューション内のすべてのプロジェクトの NuGet パッケージ マネージャーを使用します。PCL (ある場合)、iOS、Android、および Windows Phone です。 お勧めした**を削除して再度追加**Xamarin.Forms NuGet パッケージ バージョン 1.3 を更新します。

**注:** Xamarin.Forms バージョン 1.3.1 は現在*プレリリース*します。 つまり、選択する必要があります、**プレリリース**オプション NuGet (ティック ボックスを Visual Studio for Mac で) またはドロップダウン リストのリストを Visual Studio で使用して最新のプレリリース バージョンを確認します。

> [!IMPORTANT]
> Visual Studio を使用している場合は、最新バージョンの NuGet パッケージ マネージャーがインストールされていることを確認します。 以前のバージョンの Visual Studio の NuGet は Xamarin.Forms 1.3.1 の統一されたバージョンを正しくインストールされません。 移動して**ツール > 拡張機能と更新しています.**  をクリックし、**インストール済み**ことを確認する ボックスの一覧、 **for Visual Studio の NuGet パッケージ マネージャー**が少なくともバージョン 2.8.5 します。 古い場合は、をクリックして、**更新**一覧を最新バージョンをダウンロードします。

1.3.1 Xamarin.Forms の NuGet パッケージを更新すると、新しいにアップグレードするには、各プロジェクトで、次の変更を行う`Xamarin.Forms.Application`クラス。

### <a name="22-portable-class-library-or-shared-project"></a>2.2 ポータブル クラス ライブラリ (または共有プロジェクト)

変更、 **App.cs**ファイルように。

 - `App`クラスを今すぐ継承`Application`します。
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

完全に削除しました、`GetMainPage`メソッドを代わりに設定し、 `MainPage` *プロパティ*上、`Application`サブクラスです。

この新しい`Application`基本クラスもサポートしています、 `OnStart`、 `OnSleep`、および`OnResume`アプリケーションのライフ サイクルを管理するためにオーバーライドします。

`App`クラスが新しいに渡される、`LoadApplication`以下に示すよう、各アプリ プロジェクトのメソッド。

### <a name="23-ios-app"></a>2.3 iOS アプリ

変更、 **AppDelegate.cs**ファイルように。

 - クラスから継承`FormsApplicationDelegate`(の代わりに`UIApplicationDelegate`以前)。
 - `LoadApplication` 新しいインスタンスを使用して呼び出した`App`します。

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

### <a name="23-android-app"></a>2.3 の android アプリ

変更、 **MainActivity.cs**ファイルように。

 - クラスから継承`FormsApplicationActivity`(の代わりに`FormsActivity`以前)。
 - `LoadApplication` 新しいインスタンスを使用して呼び出した `App`

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

更新する必要があります、 **MainPage** -、XAML と分離コードの両方。

変更、 **MainPage.xaml**ファイルように。

 - ルートの XAML 要素である必要があります`winPhone:FormsApplicationPage`します。
 - `xmlns:phone`属性にする必要があります*変更*に `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

更新された例を次に示します。 以下にのみにこれらの操作 (属性の残りの部分は同じまま) を編集するには

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

変更、 **MainPage.xaml.cs**ファイルように。

 - クラスから継承`FormsApplicationPage`(の代わりに`PhoneApplicationPage`以前)。
 - `LoadApplication` Xamarin.Forms の新しいインスタンスを使用して呼び出した`App`クラス。 Windows Phone があるために、この参照を完全に修飾する必要があります、独自の`App`クラスが既に定義されています。

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

場合によっては、Xamarin.Forms NuGet パッケージを更新した後は、次のようなエラーが表示されます。 NuGet の更新プログラムがから以前のバージョンへの参照を完全に削除しない場合、 **csproj**ファイル。

>YOUR\_PROJECT.csproj:エラー :このプロジェクトは、このコンピューターに不足している NuGet パッケージを参照します。 それらのダウンロードは、NuGet パッケージの復元を有効にします。  詳細については、「 http://go.microsoft.com/fwlink/?LinkID=322105 」を参照してください。 不足しているファイルは./../packages/Xamarin.Forms.1.2.3.6257/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets します。 (、\_プロジェクト)

これらのエラーを修正するのには、開く、 **csproj**テキスト エディターでファイルを探して`<Target`要素を次に示す要素など、Xamarin.Forms の以前のバージョンを参照してください。 この全体の要素から手動で削除する必要があります、 **csproj**ファイルを開き、変更を保存します。

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

これらの以前の参照を削除した後、プロジェクトが正常にビルドする必要があります。

## <a name="considerations"></a>注意事項

次の考慮事項は、そのアプリが 1 つ以上のコンポーネントまたは NuGet パッケージに依存する場合は、新しい Unified API にクラシック API から既存の Xamarin.Forms プロジェクトを変換するときに考慮する必要があります。

### <a name="components"></a>コンポーネント

アプリケーションに含める任意のコンポーネントは、Unified API に更新する必要があります。 またはコンパイルしようとすると、競合が表示されます。 含まれるコンポーネントの現在のバージョン、Unified API をサポートする Xamarin コンポーネント ストアから新しいバージョンに置き換えるし、クリーン ビルドを実行します。 作成者によって変換されていない任意のコンポーネントでは、32 ビットのコンポーネント ストアにのみ警告を表示します。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API サポートを使用する NuGet への変更を投稿しました中がありますが、NuGet の新しいリリースのため、新しい Api を認識する NuGet を取得する方法を評価します。

コンポーネントと同様、その時点までには、Unified Api をサポートするバージョンをプロジェクトに含めるすべての NuGet パッケージを切り替えて、その後、クリーン ビルドを実行する必要があります。

> [!IMPORTANT]
> フォームでエラーがあれば _"エラー 3 は、同じ Xamarin.iOS プロジェクトで 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません - 'Xamarin.iOS.dll' は 'monotouch.dll' によって参照されますが、明示的に参照される ' xxx、バージョン = 0.0.000、Culture = neutral, PublicKeyToken = null'"_ Unified Api にアプリケーションを変換した後は通常、Unified API に更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージすることが原因です。 既存のコンポーネント/NuGet の削除を許可し、Unified Api をサポートするバージョンに更新し、クリーン ビルドを実行する必要があります。

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS アプリのビルドを 64 ビットの有効化

Unified API に変換されている Xamarin.iOS モバイル アプリケーション、開発者もに、アプリのオプションから 64 ビット コンピューターでアプリケーションのビルドを有効にする必要あります。 参照してください、**を有効にする 64 ビット ビルドの Xamarin.iOS アプリ**の[32/64 ビットのプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md#enable-64)64 ビットの有効化の詳細な手順についてはドキュメントを作成します。

## <a name="summary"></a>まとめ

Xamarin.Forms アプリケーション バージョン 1.3.1 に更新する必要があり、iOS アプリが (これは、iOS プラットフォームでは、64 ビット アーキテクチャをサポートしています)、Unified API に移行します。

Xamarin.Forms アプリには、カスタム レンダラーなどのネイティブ コードが含まれています。 または、更新、新しい型を使用するこれら必要がありますし、サービスの依存関係がある場合に、既に説明したように[Unified API で導入された](~/cross-platform/macios/index.md)します。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](~/cross-platform/macios/unified/updating-apps.md)
- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [ヒントを更新しています](~/cross-platform/macios/unified/updating-tips.md)
- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
