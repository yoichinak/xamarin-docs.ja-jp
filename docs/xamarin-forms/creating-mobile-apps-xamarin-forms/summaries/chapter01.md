---
title: 第 1 章の概要です。 Xamarin.Forms はどのように適合するでしょうか。
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 1 章の概要。 Xamarin.Forms はどのように適合するでしょうか。'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: dfe2d81673804e2f77694a794ef82931eb88357d
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563915"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>第 1 章の概要です。 Xamarin.Forms はどのように適合するでしょうか。

> [!NOTE] 
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

そのプラットフォームが異なるプログラミング言語に関係する場合に特に、1 つのプラットフォームから基本コードの移植プログラミングで最も不快なジョブの 1 つです。 同様に、リファクタリング、コードを移植するときは衝動が場合は並列で両方のプラットフォームを維持する必要がありますし、2 つのコード ベースの違い、将来のメンテナンスより難しくなります。

## <a name="cross-platform-mobile-development"></a>クロス プラットフォームのモバイル開発

モバイル プラットフォームを対象とする場合、この問題が一般的です。 現時点では、存在する 2 つの主要なモバイル プラットフォーム、Apple Iphone と Ipad の iOS オペレーティング システムでは、およびさまざまな携帯電話とタブレットで実行されている Android オペレーティング システムを実行しているファミリ。 もう 1 つの重要なプラットフォームは、マイクロソフトのユニバーサル Windows プラットフォーム (UWP)、1 つのプログラムを Windows 10 および Windows 10 Mobile の両方を対象にできます。

別のユーザー インターフェイスのパラダイム、3 つのさまざまな開発環境、3 つの異なるプログラミング インターフェイスを処理する必要がこれら 3 つのプラットフォームを対象にしようとしているソフトウェア ベンダーと&mdash;おそらく最も転び&mdash;次の 3 つの異なるプログラミング言語: OBJECTIVE-C で iPhone と iPad、android、Java と c# の Windows 用。

## <a name="the-c-and-net-solution"></a>C# と .NET ソリューション

OBJECTIVE-C、Java、および C# の場合はすべて、C プログラミング言語から派生、ですが、非常にさまざまなパスで、進化してきました。 C# はこれらの言語の最新と非常に便利な方法で成熟されています。 さらに、c# は密接に関連付けられた .NET では、数学、デバッグ、リフレクション、コレクション、グローバリゼーション、ファイル I/O、ネットワーク、セキュリティ、スレッド処理、web サービス、データ処理、および XML のサポートを提供するというプログラミング インフラストラクチャ全体で、読み取りと書き込みの JSON です。

現在、Xamarin には、ネイティブの Mac、iOS、および c# と .NET を使用して Android の Api を対象とするツールが用意されています。 これらのツールは、Xamarin.Mac、Xamarin.iOS、Xamarin.Android、総称して、Xamarin プラットフォームと呼びます。 これらは、ライブラリと .NET の表現方法でこれらのプラットフォームのネイティブ Api を表すバインディング。

開発者は、Xamarin プラットフォームを使用して、そのターゲット Mac、iOS、または Android で c# アプリケーションを記述することができます。 非常に役立ちますが、ターゲット プラットフォーム間でコードの一部を共有する複数のプラットフォームを対象とすると、作成します。 これは、プラットフォームに依存しないコードは、一般に、基本の .NET framework のみが必要です (通常はユーザー インターフェイスを含む)、プラットフォームに依存するコードに、プログラムを分ける必要があります。 このプラットフォームに依存しないコードは、ポータブル クラス ライブラリ (PCL)、または多くの場合、共有アセット プロジェクトや SAP と呼ばれる共有プロジェクト内に配置できますか。

> [!NOTE] 
> ポータブル クラス ライブラリが .NET Standard ライブラリに置き換えられました。 .NET standard ライブラリを使用するブックからのすべてのサンプル コードが変換されました。

## <a name="introducing-xamarinforms"></a>Xamarin.Forms の概要

複数のモバイル プラットフォームを対象とする、Xamarin.Forms はさらに多くのコード共有が許可されます。 Xamarin.Forms 用に記述された 1 つのプログラムは、5 つの異なるプラットフォームを対象します。

- iOS の iPhone、iPad、iPod touch 上で実行されるプログラム
- Android 端末およびタブレットで実行されるプログラムの android
- 対象の Windows 10 および Windows 10 Mobile にユニバーサル Windows プラットフォーム
- Windows 8.1 の Windows ランタイム API
- Windows Phone 8.1 の Windows ランタイム API

> [!NOTE] 
> Xamarin.Forms Windows 8.1、Windows Phone 8.1、または Windows 10 Mobile のをサポートしていませんが、Xamarin.Forms アプリケーションは、Windows 10 デスクトップで実行しないでください。 プレビュー サポートされても、 [Mac](~/xamarin-forms/platform/mac.md)、 [WPF](~/xamarin-forms/platform/wpf.md)、 [GTK #](~/xamarin-forms/platform/gtk.md)、および[Tizen](/xamarin-forms/platform/tizen.md)プラットフォーム。

Xamarin.Forms のプログラムの大部分は、ライブラリや、SAP に存在します。 この共有コードを呼び出す小さなアプリケーション スタブの各プラットフォームで構成されます。 

Xamarin.Forms Api は、各プラットフォームは、その特性のルック アンド フィールを維持するために、各プラットフォームのネイティブ コントロールにマップされます。

[![プラットフォームのビジュアルの共有の 3 倍になるスクリーン ショット](images/ch01fg03-small.png "Xamarin.Forms Controls on Each Platform")](images/ch01fg03-large.png#lightbox "Xamarin.Forms Controls on Each Platform")

左から右にスクリーン ショットでは、iPhone、Android フォン、および Windows 10 Mobile のスマート フォンを表示します。 

> [!NOTE] 
> Xamarin.Forms は、Windows 10 Mobile をサポートしていません。

各画面で、このページには、Xamarin.Forms [ `Label` ](xref:Xamarin.Forms.Label)テキストを表示するため、 [ `Button` ](xref:Xamarin.Forms.Button) 、操作を開始するため、 [ `Switch` ](xref:Xamarin.Forms.Switch)のオン/オフ値を選択して、 [ `Slider` ](xref:Xamarin.Forms.Slider)継続的な範囲内の値を指定するためです。 これらのビューの 4 つすべての子である、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)上、 [ `ContentPage`](xref:Xamarin.Forms.ContentPage)します。

いくつかから成る Xamarin.Forms ツールバーには、ページにも接続されている[ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)オブジェクト。 これらは、iOS と Android の画面の上部にあると、Windows 10 Mobile の画面の下部にあるアイコンとして表示されます。

Xamarin.Forms は XAML もサポートしています、いくつかのアプリケーション プラットフォームの Microsoft で開発された Extensible Application Markup Language。 説明するように上記のプログラムのすべてのビジュアルが XAML で定義されて、 [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals)サンプル。

Xamarin.Forms のプログラムで実行されているプラットフォームを判断し、それに応じてさまざまなコードを実行できます。 非常に優れており、開発者は、さまざまなプラットフォーム用のカスタム コードを記述し、プラットフォームに依存しない方法で Xamarin.Forms プログラムからそのコードを実行できます。 開発者は、各プラットフォームのレンダラーを記述することでもその他のコントロールを作成できます。

Xamarin.Forms は、基幹業務アプリケーションに最適なソリューションやプロトタイプ作成、または概念実証の簡単なデモンストレーションを行う、小さいベクター グラフィックスや複雑なタッチ操作を必要とするアプリケーションに最適です。

## <a name="your-development-environment"></a>開発環境

開発環境は、どのようなプラットフォームを対象としてどのようなマシンが使用する場合に依存します。

IOS をターゲットにする場合は、Xcode と Xamarin プラットフォームがインストールされている Mac 必要があります。 Android をサポートするには、Java と必要な Sdk をインストールする必要があります。 Visual Studio for mac を使用する Android と iOS の両方を対象にできますし、

Visual Studio をインストールすることができます、PC で iOS、Android、およびすべての Windows プラットフォームを対象します。 ただし、Visual Studio から iOS を対象とする必要があります Mac で Xcode と Xamarin プラットフォームをインストールします。

いずれかを実際のデバイスで、コンピューターに USB で接続された、または、シミュレーターでのプログラムをテストできます。

## <a name="installation"></a>インストール

作成と、Xamarin.Forms アプリケーションをビルドする前に、iOS アプリケーション、Android アプリケーション、および UWP アプリケーションをターゲットと、開発環境とプラットフォームに応じて個別にビルドを作成してください。

Xamarin と Microsoft の web サイトには、これを行う方法に関する情報が含まれます。

- [IOS の概要](~/ios/get-started/index.md)
- [Android の概要](~/android/get-started/index.md)
- [Windows デベロッパー センター](http://dev.windows.com)

1 回作成することができ、これらの各プラットフォーム用のプロジェクトを実行する必要がありますありません問題を作成して、Xamarin.Forms アプリケーションを実行しています。

## <a name="related-links"></a>関連リンク

- [第 1 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [第 1 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
