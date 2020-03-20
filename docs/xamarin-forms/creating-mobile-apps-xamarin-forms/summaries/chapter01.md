---
title: 第 1 章の概要。 Xamarin.Forms はどのように適合しますか?
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 第 1 章の概要。 Xamarin.Forms はどのように適合しますか?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 6dfa473bdfb4c1dd88ca833dbf5011a0bbdec42a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73032881"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>第 1 章の概要。 Xamarin.Forms はどのように適合しますか?

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

> [!NOTE]
> このページの注記では、Xamarin.Forms が本に記載されている資料と異なる部分が示されています。

プログラミングにおいて最も好ましくないジョブの 1 つは、特にプラットフォームで異なるプログラミング言語を使用する場合に、プラットフォーム間でコード ベースを移植することです。 また、コードを移植する際にリファクタリングしがちですが、両方のプラットフォームを並列して維持する必要がある場合、2 つのコード ベースに違いがあると、将来のメンテナンスがより困難になります。

## <a name="cross-platform-mobile-development"></a>クロス プラットフォームのモバイル開発

この問題は、モバイル プラットフォームをターゲットとする場合によく見られます。 現在、2 つの主要なモバイル プラットフォームである、iOS オペレーティング システムを実行する iPhone と iPad の Apple ファミリ、およびさまざまな携帯電話やタブレットで実行される Android オペレーティング システムが存在します。 もう 1 つの重要なプラットフォームは、Microsoft のユニバーサル Windows プラットフォーム (UWP) です。これにより、単一プログラムで両方の Windows 10 をターゲットとすることができます。

これらのプラットフォームをターゲットとするソフトウェア ベンダーでは、さまざまなユーザーインターフェイス パラダイム、3 つの異なる開発環境、3 つの異なるプログラミング インターフェイス、および &mdash; おそらく最も面倒な &mdash; 3 つの異なるプログラミング言語の Objective-C (iPhone と iPad の場合)、Java (Android の場合)、および C# (Windows の場合) を扱う必要があります。

## <a name="the-c-and-net-solution"></a>C# および .NET ソリューション

Objective-C、Java、および C# はすべて C プログラミング言語から派生したものですが、これらはまったく異なる経路で進化しています。 C# はこれらの言語の最新のものであり、非常に有効な方法で成熟しています。 さらに、C# は、.NET と呼ばれるプログラミング インフラストラクチャ全体と密接に関連しています。これにより、数値演算、デバッグ、リフレクション、コレクション、グローバリゼーション、ファイル I/O、ネットワーク、セキュリティ、スレッド処理、Web サービス、データ処理、XML および JSON の読み取りと書き込みがサポートされます。

Xamarin では、現在、C# と .NET を使用するネイティブの Mac、iOS、および Android API をターゲットとするツールが提供されています。 これらのツールは、Xamarin.Mac、Xamarin.iOS、および Xamarin.Android と呼ばれ、Xamarin プラットフォームと総称されます。 これらは、こうしたプラットフォームのネイティブ API を .NET イディオムで表現するライブラリおよびバインディングです。

開発者は、Xamarin プラットフォームを使用して、Mac、iOS、または Android をターゲットとするアプリケーションを C# で作成できます。 しかし、複数のプラットフォームをターゲットとする場合は、コードの一部をターゲット プラットフォーム間で共有するのが非常に賢明です。 これには、プラットフォームに依存するコード (通常はユーザー インターフェイスを含む) と、プラットフォームに依存しないコード (通常は基本 .NET Framework のみが必要) へのプログラムの分割が含まれます。 このプラットフォームに依存しないコードは、ポータブル クラス ライブラリ (PCL)、または共有プロジェクト (多くの場合、共有アセット プロジェクト、つまり、SAP と呼ばれる) に配置することができます。

> [!NOTE]
> ポータブル クラス ライブラリは、.NET Standard ライブラリに置き換えられています。 本のすべてのサンプル コードは、.NET Standard ライブラリを使用するように変換されています。

## <a name="introducing-xamarinforms"></a>Xamarin. Forms の概要

複数のモバイル プラットフォームをターゲットとする場合、Xamarin.Forms ではさらに多くのコード共有を行うことができます。 Xamarin.Forms 用に作成された単一プログラムでは、これらのプラットフォームをターゲットにすることができます。

- iPhone、iPad、iPod touch で実行されるプログラム用の iOS
- Android フォンとタブレットで実行されるプログラム用の Android
- Windows 10 をターゲットとするユニバーサル Windows プラットフォーム

> [!NOTE]
> Xamarin.Forms では Windows 8.1、Windows Phone 8.1、Windows 10 Mobile がサポートされなくなりましたが、Xamarin.Forms アプリケーションは Windows 10 デスクトップで実行されます。 [Mac](~/xamarin-forms/platform/other/mac.md)、[WPF](~/xamarin-forms/platform/other/wpf.md)、[GTK #](~/xamarin-forms/platform/other/gtk.md)、および [Tizen](~/xamarin-forms/platform/other/tizen.md) プラットフォーム用のプレビュー サポートもあります。

Xamarin. Forms プログラムの大部分は、ライブラリまたは SAP に存在します。 各プラットフォームは、この共有コードを呼び出す小さなアプリケーション スタブで構成されています。

Xamarin. Forms API は各プラットフォーム上のネイティブ コントロールにマップされるため、各プラットフォームでその特性のルック アンド フィールが維持されます。

[![プラットフォームの視覚エフェクト共有のトリプル スクリーンショット](images/ch01fg03-small.png "各プラットフォームの Xamarin.Forms コントロール")](images/ch01fg03-large.png#lightbox "各プラットフォームの Xamarin.Forms コントロール")

スクリーンショットの左から右に、iPhone と Android フォンが示されています。

各画面のページには、テキストを表示するための Xamarin.Forms の [`Label`](xref:Xamarin.Forms.Label)、アクションを開始するための [`Button`](xref:Xamarin.Forms.Button)、オン/オフの値を選択するための [`Switch`](xref:Xamarin.Forms.Switch)、および連続する範囲内の値を指定するための [`Slider`](xref:Xamarin.Forms.Slider) が含まれています。 これら 4 つのビューはすべて、[`ContentPage`](xref:Xamarin.Forms.ContentPage)上の [`StackLayout`](xref:Xamarin.Forms.StackLayout) の子です。

また、このページには、複数の [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) オブジェクトで構成される Xamarin.Forms ツールバーがアタッチされています。 これらは、iOS および Android 画面の上部と、Windows 10 Mobile 画面の下部にアイコンとして表示されます。

また、Xamarin.Forms では XAML がサポートされています。これは、複数のアプリケーション プラットフォーム用に Microsoft で開発された Extensible Application Markup Language です。 上に示されたプログラムのすべての視覚エフェクトは、[**PlatformVisuals**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) サンプルに示されているように、XAML で定義されています。

Xamarin.Forms プログラムでは、実行されているプラットフォームを特定し、それに応じて別のコードを実行することができます。 開発者は、より効果的に、さまざまなプラットフォーム用のカスタム コードを作成し、そのコードをプラットフォームに依存しない方法で Xamarin.Forms プログラムから実行することができます。 また、開発者は、プラットフォームごとにレンダラーを記述して、追加のコントロールを作成することができます。

Xamarin.Forms は、基幹業務アプリケーション、プロトタイプ作成、または簡単な概念実証デモを行うための優れたソリューションですが、ベクター グラフィックスや複雑なタッチ操作を必要とするアプリケーションには最適ではありません。

## <a name="your-development-environment"></a>開発環境

開発環境は、ターゲットとするプラットフォームと使用するマシンによって異なります。

iOS をターゲットとする場合は、Xcode と Xamarin プラットフォームがインストールされた Mac が必要になります。 また、Android をサポートするには、Java および必要な SDK のインストールが必要です。 その後、Visual Studio for Mac を使用して、iOS と Android の両方をターゲットにすることができます。

Visual Studio をインストールすると、PC で iOS、Android、およびすべての Windows プラットフォームをターゲットにすることができます。 しかし、Visual Studio から iOS をターゲットとする場合は、引き続き、Xcode と Xamarin プラットフォームがインストールされた Mac が必要です。

USB でコンピューターに接続されている実際のデバイス、またはシミュレーターでプログラムをテストすることができます。

## <a name="installation"></a>インストール

Xamarin.Forms アプリケーションを作成してビルドする前に、ターゲットとするプラットフォームと開発環境に応じて、iOS アプリケーション、Android アプリケーション、および UWP アプリケーションを個別に作成してビルドしてみる必要があります。

Xamarin および Microsoft Web サイトには、この方法についての情報が含まれています。

- [iOS の概要](~/ios/get-started/index.md)
- [Android の概要](~/android/get-started/index.md)
- [Windows デベロッパー センター](https://dev.windows.com)

これらの個々のプラットフォーム用にプロジェクトを作成して実行できたら、Xamarin.Forms アプリケーションの作成と実行に問題はないはずです。

## <a name="related-links"></a>関連リンク

- [第 1 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [第 1 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
