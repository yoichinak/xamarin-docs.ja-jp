---
title: セットアップの Windows プロジェクト
description: 既存の Xamarin.Forms ソリューションに新しい Windows プロジェクトを追加します。
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: aed240dd403957e5935666d4179a6d642c411b86
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="setup-windows-projects"></a>セットアップの Windows プロジェクト

_既存の Xamarin.Forms ソリューションに新しい Windows プロジェクトを追加します。_

Xamarin.Forms ソリューションの古い (または macOS で作成したもの) には、ユニバーサル Windows プラットフォーム (UWP) アプリ プロジェクトはありません。 そのため、Windows 10 (UWP) アプリをビルドするには、ある UWP プロジェクトを手動で追加する必要があります。

## <a name="add-a-universal-windows-platform-app"></a>追加のユニバーサル Windows プラットフォーム アプリ

実行する必要があります**Visual Studio 2017**で**Windows 10** UWP アプリをビルドします。 ユニバーサル Windows プラットフォームの詳細については、次を参照してください。[にユニバーサル Windows プラットフォーム (入門)](/windows/uwp/get-started/universal-application-platform-guide/)です。

UWP 利用可能で、Xamarin.Forms 2.1 では、後であり、Xamarin.Forms.Maps が Xamarin.Forms 2.2 以降にサポートされます。

チェック、<a href="#troubleshooting">トラブルシューティング</a>のに役立つヒントについてのセクションです。

Windows 10 の携帯電話、タブレット、およびデスクトップで実行される UWP アプリを追加する手順に従います。

 1。 クリックし、ソリューションを右クリックして**追加 > 新しいプロジェクト.** を追加し、**空のアプリケーション (ユニバーサル Windows)** プロジェクト。

  ![](universal-images/add-wu.png "新しいプロジェクト ダイアログ ボックスを追加します。")

 2 になります。 **新しいユニバーサル Windows プラットフォーム プロジェクト**ダイアログ ボックスで、アプリが実行される Windows 10 の最小値とターゲットのバージョンを選択します。

  ![](universal-images/target-version.png "新しいユニバーサル Windows プラットフォーム プロジェクト ダイアログ ボックス")

 3 です。 UWP プロジェクトを右クリックし、選択**NuGet パッケージを管理しています.** を追加し、 **Xamarin.Forms**パッケージです。 ソリューション内の他のプロジェクトは、Xamarin.Forms パッケージの同じバージョンにも更新を確認します。

 4 です。 新しい UWP プロジェクトがビルドされることを確認してください、**ビルド > Configuration Manager**ウィンドウ (この可能性がありますしない起きた既定)。 ティック、**ビルド**と**展開**ユニバーサル プロジェクトのボックス。

  [![](universal-images/configuration-sml.png "構成マネージャー ウィンドウ")](universal-images/configuration.png#lightbox "構成マネージャー ウィンドウ")

 5 です。 クリックし、プロジェクトを右クリックして**追加 > 参照**Xamarin.Forms アプリケーション プロジェクト (.NET 標準またはプロジェクトの共有) への参照を作成します。

  ![](universal-images/addref-sml.png "参照マネージャー ダイアログ ボックス")

 6。 UWP プロジェクトで、編集**App.xaml.cs**に含める、`Init`内のメソッドの呼び出し、 `OnLaunched` 52 'system.ftpserver メソッド。

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 です。 UWP プロジェクトで、編集**MainPage.xaml**削除することによって、`Grid`内に含まれる、`Page`要素。

 8 です。 **MainPage.xaml**、追加、新しい`xmlns`エントリ`Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 です。 **MainPage.xaml**、ルートを変更`<Page`要素`<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 です。 UWP プロジェクトで、編集**MainPage.xaml.cs**を削除する、`: Page`クラス名の指定子を継承 (から継承されるようになりましたので`WindowsPage`前の手順で行われた変更により)。

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11。 **MainPage.xaml.cs**、追加、`LoadApplication`で呼び出し、 `MainPage` Xamarin.Forms アプリを起動するコンス トラクター。

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

12 です。 (ローカル リソースを追加します。 イメージ ファイル) に必要な既存のプラットフォーム プロジェクトからです。

## <a name="troubleshooting"></a>トラブルシューティング

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>「ターゲットの呼び出しの例外」「.NET ネイティブ ツール チェーンでコンパイル」を使用する場合

UWP アプリで複数のアセンブリ (たとえば、3 番目のパーティ制御ライブラリ、または、アプリ自体に複数のライブラリは、分割) を参照している場合は、Xamarin.Forms がそれらのアセンブリ (カスタム レンダラー) などからオブジェクトを読み込むことができません。

これを使用する場合に発生する可能性があります、 **.NET ネイティブ ツール チェーンでコンパイル**UWP アプリのためのオプションは、**プロパティ > ビルド > 全般**プロジェクトのウィンドウ。

UWP に固有のオーバー ロードを使用してこの問題を解決できる、`Forms.Init`で呼び出す**App.xaml.cs**次のコードに示すように (置換する必要があります`ClassInOtherAssembly`実際のクラスにコードを参照)。

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

アプリによって参照される各アセンブリへの参照を追加します。

#### <a name="dependency-services-and-net-native-compilation"></a>依存サービスおよび .NET ネイティブ コンパイル

メイン アプリケーション実行可能ファイル (このような個別のプロジェクトまたはライブラリと同様に) 外部で定義される依存サービスを解決するのには .NET ネイティブ コンパイルを使用してリリース ビルドは失敗します。

使用して、`DependencyService.Register<T>()`サービス クラスの依存関係を手動で登録するメソッド。 上記の例に基づいて、次のようにして register メソッドを追加します。

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
