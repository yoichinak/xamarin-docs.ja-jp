---
title: セットアップの Windows プロジェクト
description: 以前の Xamarin.Forms ソリューション (または macOS で作成されたもの) がないユニバーサル Windows プラットフォーム プロジェクトの場合、この記事が新しい UWP プロジェクトを既存の Xamarin.Forms ソリューションに追加する方法を説明しますので。
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: b0f06cf15d3a3ec7eae4742d5d037e233be46d08
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58855186"
---
# <a name="setup-windows-projects"></a>セットアップの Windows プロジェクト

_既存の Xamarin.Forms ソリューションに新しい Windows プロジェクトを追加します。_

以前の Xamarin.Forms ソリューション (または macOS で作成されたもの) には、ユニバーサル Windows プラットフォーム (UWP) アプリ プロジェクトはありません。 そのため、Windows 10 (UWP) アプリを構築する UWP プロジェクトを手動で追加する必要があります。

## <a name="add-a-universal-windows-platform-app"></a>追加するユニバーサル Windows プラットフォーム アプリ

**Visual Studio 2019**で**Windows 10** UWP アプリの構築をお勧めします。 ユニバーサル Windows プラットフォームの詳細については、次を参照してください。[ユニバーサル Windows プラットフォームの紹介](/windows/uwp/get-started/universal-application-platform-guide/)します。

UWP は、Xamarin.Forms 2.1 で使用可能な以降と、Xamarin.Forms 2.2 以降、Xamarin.Forms.Maps はサポートされます。

チェック、<a href="#troubleshooting">トラブルシューティング</a>役に立つヒントのセクション。

Windows 10 スマート フォン、タブレット、およびデスクトップで実行される UWP アプリを追加するこれらの手順に従います。

 1 . クリックし、ソリューションを右クリックして**追加 > 新しいプロジェクト.** を追加し、**空のアプリ (ユニバーサル Windows)** プロジェクト。

  ![](universal-images/add-wu.png "新しいプロジェクト ダイアログ ボックスを追加します。")

 2 . **新しいユニバーサル Windows プラットフォーム プロジェクト**ダイアログ ボックスで、アプリで実行される Windows 10 の最小値とターゲットのバージョンを選択します。

  ![](universal-images/target-version.png "新しいユニバーサル Windows プラットフォーム プロジェクト ダイアログ ボックス")

 3 . UWP プロジェクトを右クリックし、選択**NuGet パッケージの管理.** を追加し、 **Xamarin.Forms**パッケージ。 ソリューション内の他のプロジェクトは、Xamarin.Forms パッケージの同じバージョンにも更新を確認します。

 4 . 新しい UWP プロジェクトはビルド確認、**ビルド > Configuration Manager**ウィンドウ (このおそらくしません起きた既定)。 ティック、**ビルド**と**デプロイ**ユニバーサル プロジェクトのボックス。

  [![](universal-images/configuration-sml.png "構成マネージャー ウィンドウ")](universal-images/configuration.png#lightbox "Configuration Manager ウィンドウ")

 5 . クリックし、プロジェクトを右クリックして**追加 > 参照**し (.NET Standard または共有プロジェクト) は、Xamarin.Forms アプリケーションのプロジェクトへの参照を作成します。

  ![](universal-images/addref-sml.png "参照マネージャー ダイアログ ボックス")

 6 . UWP プロジェクトで編集**App.xaml.cs**に含める、`Init`内でメソッドの呼び出し、 `OnLaunched` 52 行目の周囲のメソッド。

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . UWP プロジェクトで編集**MainPage.xaml**削除することによって、`Grid`内に含まれる、`Page`要素。

 8 . **MainPage.xaml**、新しい追加`xmlns`エントリ`Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . **MainPage.xaml**、ルートを変更`<Page`要素`<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 . UWP プロジェクトで編集**MainPage.xaml.cs**を削除する、`: Page`クラス名の指定子を継承 (から継承するようになりましたので`WindowsPage`前の手順で行われた変更のため)。

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . **MainPage.xaml.cs**、追加、`LoadApplication`呼び出し、 `MainPage` Xamarin.Forms アプリを起動するコンス トラクター。

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12 . (例: ローカル リソースを追加します。 イメージ ファイル) に必要な既存のプラットフォーム プロジェクトから。

## <a name="troubleshooting"></a>トラブルシューティング

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>「ターゲット呼び出し例外」「.NET ネイティブ ツール チェーンでコンパイル」を使用する場合

UWP アプリが複数のアセンブリ (たとえば、3 番目のパーティ制御ライブラリ、または、アプリ自体が複数のライブラリに分割されます) を参照している場合は、Xamarin.Forms がそれらのアセンブリ (カスタム レンダラー) などからオブジェクトを読み込むことができません。

これを使用する場合に発生する可能性があります、 **.NET ネイティブ ツール チェーンでコンパイル**で UWP アプリ用のオプション、**プロパティ > ビルド > 全般**プロジェクトのウィンドウ。

これを修正するにはの UWP 固有のオーバー ロードを使用して、`Forms.Init`呼び出す**App.xaml.cs**以下のコードに示すように (置き換える必要があります`ClassInOtherAssembly`実際のクラスを使用してコードを参照)。

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

ソリューション エクスプ ローラーで直接参照または NuGet 経由での参照として追加した各アセンブリのエントリを追加します。

#### <a name="dependency-services-and-net-native-compilation"></a>依存関係サービスと .NET のネイティブ コンパイル

.NET ネイティブ コンパイルを使用して、リリース ビルドは、メイン アプリケーション実行可能ファイル (別のプロジェクトやライブラリなど) の外部で定義された依存関係サービスの解決に失敗します。

使用して、`DependencyService.Register<T>()`サービス クラスの依存関係を手動で登録するメソッド。 上記の例に基づいて、このような register メソッドを追加します。

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
