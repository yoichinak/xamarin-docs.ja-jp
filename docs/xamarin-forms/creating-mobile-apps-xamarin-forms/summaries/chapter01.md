---
title: 第1章の概要。 Xamarin. フォームはどのようになっていますか。
description: 'Xamarin. Forms での Mobile Apps の作成: 第1章の概要。 Xamarin. フォームはどのようになっていますか。'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 6dfa473bdfb4c1dd88ca833dbf5011a0bbdec42a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032881"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>第1章の概要。 Xamarin. フォームはどのようになっていますか。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

> [!NOTE]
> このページのメモでは、分岐が書籍に記載されている素材から見た領域を示しています。

プログラミングにおける最も望ましくないジョブの1つは、特にプラットフォームが異なるプログラミング言語を必要とする場合に、あるプラットフォームから別のプラットフォームにコードベースを移植することです。 コードを移植してリファクタリングすることもありますが、両方のプラットフォームを並行して維持する必要がある場合は、2つのコードベースの違いによって、将来のメンテナンスがより困難になります。

## <a name="cross-platform-mobile-development"></a>クロス プラットフォームのモバイル開発

この問題は、モバイルプラットフォームを対象とする場合によく見られます。 現在、2つの主要なモバイルプラットフォーム、iOS オペレーティングシステムを実行している iPhones と Ipad の Apple ファミリ、およびさまざまな携帯電話やタブレットで実行される Android オペレーティングシステムが存在します。 もう1つの重要なプラットフォームは、Microsoft のユニバーサル Windows プラットフォーム (UWP) です。これにより、1つのプログラムで両方の Windows 10 を対象とすることができます。

これらのプラットフォームを対象とするソフトウェアベンダーは、さまざまなユーザーインターフェイスのパラダイム、3つの異なる開発環境、3つの異なるプログラミングインターフェイスを処理する必要があります。また&mdash;最も awkwardly&mdash;3 種類あります。プログラミング言語: iPhone と iPad、Java for Android、およびC# Windows 用の目標-C。

## <a name="the-c-and-net-solution"></a>C#および .net ソリューション

目標 C、Java、およびはすべてC# c プログラミング言語から派生していますが、まったく異なるパスによって進化しています。 C#は、これらの言語の最新のもので、非常に便利な方法で成熟しています。 さらにC# 、は .net と呼ばれるプログラミングインフラストラクチャ全体に密接に関連付けられています。これは、数学、デバッグ、リフレクション、コレクション、グローバリゼーション、ファイル i/o、ネットワーク、セキュリティ、スレッド処理、web サービス、データ処理をサポートします。および XML および JSON の読み取りと書き込みを行います。

Xamarin には、現在、および .NET を使用してネイティブの Mac、 C# iOS、および Android api を対象とするツールが用意されています。 これらのツールは、xamarin、xamarin、iOS、および xamarin Android と呼ばれています。これは、Xamarin プラットフォームと総称されます。 これらは、これらのプラットフォームのネイティブ Api を .NET 表現で表現するライブラリおよびバインドです。

開発者は、Xamarin プラットフォームを使用しC#て、Mac、iOS、または Android を対象とするアプリケーションを作成できます。 しかし、複数のプラットフォームを対象とする場合は、コードの一部をターゲットプラットフォーム間で共有するのが非常に理にかなっています。 これには、プログラムをプラットフォームに依存するコード (通常はユーザーインターフェイスを含む) とプラットフォームに依存しないコード (一般に基本 .NET framework のみが必要) に分割する必要があります。 このプラットフォームに依存しないコードは、ポータブルクラスライブラリ (PCL) に配置することも、共有プロジェクト (共有アセットプロジェクトまたは SAP と呼ばれることもあります) に置くこともできます。

> [!NOTE]
> ポータブルクラスライブラリは .NET Standard ライブラリに置き換えられました。 本のすべてのサンプルコードは、.NET standard ライブラリを使用するように変換されています。

## <a name="introducing-xamarinforms"></a>Xamarin. Forms の概要

複数のモバイルプラットフォームを対象とする場合、Xamarin. Forms ではさらに多くのコード共有を行うことができます。 Xamarin 用に作成された1つのプログラムは、次のプラットフォームを対象にすることができます。

- iPhone、iPad、iPod touch で実行されるプログラムの iOS
- Android フォンとタブレットで実行されるプログラム用 android
- Windows 10 を対象とするユニバーサル Windows プラットフォーム

> [!NOTE]
> Windows 8.1、Windows Phone 8.1、または Windows 10 Mobile はサポートされなくなりましたが、Xamarin. フォームアプリケーションは Windows 10 デスクトップで実行されます。 [Mac](~/xamarin-forms/platform/other/mac.md)、 [WPF](~/xamarin-forms/platform/other/wpf.md)、 [GTK #](~/xamarin-forms/platform/other/gtk.md)、 [Tizen](~/xamarin-forms/platform/other/tizen.md)の各プラットフォームでもプレビューがサポートされています。

Xamarin. Forms プログラムの大部分は、ライブラリまたは SAP に存在します。 各プラットフォームは、この共有コードを呼び出す小さなアプリケーションスタブで構成されています。

各プラットフォーム上のネイティブコントロールには、各プラットフォームが特性のルックアンドフィールを維持するために、Xamarin. Forms Api が割り当てられます。

[![プラットフォームビジュアルの共有のトリプルスクリーンショット](images/ch01fg03-small.png "各プラットフォームでの Xamarin. フォームコントロール")](images/ch01fg03-large.png#lightbox "各プラットフォームでの Xamarin. フォームコントロール")

左から右のスクリーンショットには、iPhone と Android フォンが示されています。

各画面のページには、テキストを表示するための Xamarin [`Label`](xref:Xamarin.Forms.Label) 、アクションを開始するための[`Button`](xref:Xamarin.Forms.Button) 、オン/オフ値を選択するための[`Switch`](xref:Xamarin.Forms.Switch) 、および連続する範囲内の値を指定するための[`Slider`](xref:Xamarin.Forms.Slider)が含まれています. これらの4つのビューはすべて、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)上の[`StackLayout`](xref:Xamarin.Forms.StackLayout)の子です。

また、このページには、複数の[`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)オブジェクトで構成される Xamarin. フォームツールバーもあります。 これらは、iOS および Android の画面の上部と、Windows 10 Mobile の画面の下部にアイコンとして表示されます。

また、Xamarin は XAML もサポートしており、複数のアプリケーションプラットフォーム用に Microsoft で開発された Extensible Application Markup Language ます。 上に示したプログラムのすべてのビジュアルは、 [**platformvisuals**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals)サンプルで示すように、XAML で定義されています。

Xamarin. Forms プログラムは、実行されているプラットフォームを特定し、それに応じて別のコードを実行できます。 さらに、開発者は、さまざまなプラットフォーム用のカスタムコードを作成し、そのコードをシンプルかつ強力プログラムからプラットフォームに依存しない方法で実行できます。 開発者は、プラットフォームごとにレンダラーを記述して、追加のコントロールを作成することもできます。

Xamarin は、基幹業務アプリケーション、プロトタイプ作成、または概念実証の簡単なデモを行うための優れたソリューションですが、ベクターグラフィックスや複雑なタッチ操作を必要とするアプリケーションには最適ではありません。

## <a name="your-development-environment"></a>開発環境

開発環境は、対象とするプラットフォームと使用するマシンによって異なります。

IOS を対象とする場合は、Xcode と Xamarin プラットフォームがインストールされた Mac が必要です。 Android をサポートするには、Java および必要な Sdk をインストールする必要があります。 その後、Visual Studio for Mac を使用して、iOS と Android の両方を対象にすることができます。

Visual Studio をインストールすると、PC で iOS、Android、およびすべての Windows プラットフォームを対象にすることができます。 ただし、Visual Studio から iOS を対象とする場合は、Xcode と Xamarin プラットフォームがインストールされた Mac が必要です。

USB によってコンピューターに接続されている実際のデバイスまたはシミュレーターで、プログラムをテストすることができます。

## <a name="installation"></a>インストール

Xamarin. フォームアプリケーションを作成してビルドする前に、対象とするプラットフォームと開発環境に応じて、iOS アプリケーション、Android アプリケーション、および UWP アプリケーションを個別に作成してビルドすることをお勧めします。

Xamarin および Microsoft web サイトには、その方法についての情報が含まれています。

- [IOS でのはじめに](~/ios/get-started/index.md)
- [Android でのはじめに](~/android/get-started/index.md)
- [Windows デベロッパー センター](https://dev.windows.com)

これらの個々のプラットフォーム用にプロジェクトを作成して実行すると、Xamarin. Forms アプリケーションの作成と実行に問題はありません。

## <a name="related-links"></a>関連リンク

- [第1章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [第1章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
