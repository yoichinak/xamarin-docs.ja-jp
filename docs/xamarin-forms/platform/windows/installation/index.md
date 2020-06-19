---
title: Windows プロジェクトのセットアップ
description: 以前 Xamarin.Forms のソリューション (または macOS で作成されたソリューション) にはユニバーサル Windows プラットフォームプロジェクトはありません。この記事では、既存のソリューションに新しい UWP プロジェクトを追加する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 13b46fd06b0116332241b0d523aea707d56b39ec
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573366"
---
# <a name="setup-windows-projects"></a>Windows プロジェクトのセットアップ

_既存のソリューションへの新しい Windows プロジェクトの追加 Xamarin.Forms_

以前 Xamarin.Forms のソリューション (または macOS で作成されたソリューション) には、ユニバーサル Windows プラットフォーム (UWP) アプリプロジェクトはありません。 そのため、Windows 10 (UWP) アプリをビルドするには、UWP プロジェクトを手動で追加する必要があります。

## <a name="add-a-universal-windows-platform-app"></a>ユニバーサル Windows プラットフォームアプリを追加する

UWP アプリをビルドするには、 **Windows 10**上の**Visual Studio 2019**をお勧めします。 ユニバーサル Windows プラットフォームの詳細については、「[ユニバーサル Windows プラットフォームの概要](/windows/uwp/get-started/universal-application-platform-guide/)」を参照してください。

UWP は Xamarin.Forms 、2.1 以降、およびで使用でき Xamarin.Forms ます。マップは2.2 以降でサポートされてい Xamarin.Forms ます。

<a href="#troubleshooting">トラブルシューティング</a>のセクションで役立つヒントを確認してください。

Windows 10 の電話、タブレット、デスクトップで実行する UWP アプリを追加するには、次の手順に従います。

 1. ソリューションを右クリックし、[**新しいプロジェクトの追加 >** ] を選択して、**空のアプリ (ユニバーサル Windows)** プロジェクトを追加します。

  ![](universal-images/add-wu.png "Add New Project Dialog")

 3. [**新しいユニバーサル Windows プラットフォームプロジェクト**] ダイアログで、アプリが実行される Windows 10 の最小バージョンとターゲットバージョンを選択します。

  ![](universal-images/target-version.png "New Universal Windows Platform Project Dialog")

 番. UWP プロジェクトを右クリックし、[ **NuGet パッケージの管理...** ] を選択して、パッケージを追加します。 **Xamarin.Forms** ソリューション内の他のプロジェクトが同じバージョンのパッケージにも更新されていることを確認し Xamarin.Forms ます。

 4/4. 新しい UWP プロジェクトが**ビルド > Configuration Manager**ウィンドウにビルドされることを確認します (これは既定では発生しない可能性があります)。 ユニバーサルプロジェクトの [**ビルド**] ボックスと [**配置**] ボックスを目盛りします。

  [![](universal-images/configuration-sml.png "Configuration Manager Window")](universal-images/configuration.png#lightbox "Configuration Manager Window")

 5/5. プロジェクトを右クリックし、[ **> の参照の追加**] を選択して、 Xamarin.Forms アプリケーションプロジェクト (.NET Standard または共有プロジェクト) への参照を作成します。

  ![](universal-images/addref-sml.png "Reference Manager Dialog")

 4/6. UWP プロジェクトで、 **App.xaml.cs**を編集して、 `Init` 52 行目のメソッド内にメソッド呼び出しを含め `OnLaunched` ます。

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7. UWP プロジェクトで、要素内に含まれるを削除して**mainpage.xaml**を編集します `Grid` `Page` 。

 8. **Mainpage.xaml**で、次の新しいエントリを追加します `xmlns` `Xamarin.Forms.Platform.UWP` 。

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 ませ. **Mainpage.xaml**で、ルート `<Page` 要素を次のように変更します。 `<forms:WindowsPage`

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 種類. UWP プロジェクトで、 **MainPage.xaml.cs**を編集して、 `: Page` クラス名の継承指定子を削除します (これは、 `WindowsPage` 前の手順で行われた変更によって継承されるようになったためです)。

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 個. **MainPage.xaml.cs**で、アプリを `LoadApplication` 起動するための呼び出しをコンストラクターに追加し `MainPage` Xamarin.Forms ます。

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

> [!NOTE]
> メソッドの引数 `LoadApplication` は、 `Xamarin.Forms.Application` .net standard プロジェクトで定義されているインスタンスです。

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

vdc. ローカルリソースを追加します (例 必要な既存のプラットフォームプロジェクトからのイメージファイル)。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>".NET ネイティブツールチェーンを使用したコンパイル" を使用する場合の "ターゲット呼び出しの例外"

UWP アプリが複数のアセンブリを参照している場合 (たとえば、サードパーティ製のコントロールライブラリの場合、またはアプリ自体が複数のライブラリに分割されている場合)、は Xamarin.Forms それらのアセンブリからオブジェクトを読み込むことができない可能性があります (カスタムレンダラーなど)。

このような状況は、プロジェクトの **[プロパティ > ビルド > 全般**] ウィンドウで UWP アプリのオプションとして [ **.NET ネイティブのコンパイル] ツールチェーン**を使用した場合に発生する可能性があります。

この問題を解決するには、次のコードに示すように、App.xaml.cs で呼び出しの UWP 固有のオーバーロードを使用し `Forms.Init` ます (コードが**App.xaml.cs**参照する `ClassInOtherAssembly` 実際のクラスに置き換える必要があります)。

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

直接参照または NuGet を使用して、ソリューションエクスプローラーに参照として追加した各アセンブリのエントリを追加します。

#### <a name="dependency-services-and-net-native-compilation"></a>依存関係サービスと .NET ネイティブのコンパイル

.NET ネイティブのコンパイルを使用したリリースビルドでは、メインアプリの実行可能ファイル (別のプロジェクトやライブラリなど) の外部で定義されている依存関係サービスの解決に失敗する場合があります。

`DependencyService.Register<T>()`依存関係サービスクラスを手動で登録するには、メソッドを使用します。 上記の例に基づいて、次のような register メソッドを追加します。

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
