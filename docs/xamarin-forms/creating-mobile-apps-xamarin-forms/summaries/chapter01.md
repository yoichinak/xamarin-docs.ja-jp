---
title: "第 1 章の概要です。 Xamarin.Forms どのように適合しますか。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c0f3313fa3c4d1075be7deeb871e303006c533e8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>第 1 章の概要です。 Xamarin.Forms どのように適合しますか。

そのプラットフォームにはさまざまなプログラミング言語が含まれる場合に特に、1 つのプラットフォームから基本コードを移植プログラミングで最も不快なジョブの 1 つです。 同様に、それをリファクターするコードを移植するときに、という誘惑が並列では、両方のプラットフォームを保持する必要がある場合の 2 つのコード ベースの相違点は将来保守が困難します。

## <a name="cross-platform-mobile-development"></a>クロス プラットフォーム モバイル開発

この問題は一般的なモバイル プラットフォームを対象とする場合です。 現時点では、存在 2 つの主要なモバイル プラットフォームで、Apple のファミリ Iphone と Ipad の iOS オペレーティング システムおよびさまざまな携帯電話とタブレットで実行されている Android オペレーティング システムを実行します。 別の大幅なプラットフォームは、Microsoft のユニバーサル Windows プラットフォーム (UWP)、これにより Windows 10 および Windows 10 Mobile の両方を対象とする 1 つのプログラムです。

これら 3 つのプラットフォームを対象とすることを希望するソフトウェア ベンダーは、別のユーザー インターフェイスのパラダイム、3 つの別の開発環境、3 つの異なるプログラミング インターフェイスを処理する必要がありますと & #x 2014、おそらくほとんどながら & #x 2014; 次の 3 つプログラミング言語によって異なります。 iPhone と iPad、android では、Java や c# の Windows の Objective-c です。

## <a name="the-c-and-net-solution"></a>C# と .NET ソリューション

OBJECTIVE-C、Java、および C# の場合はすべて、C プログラミング言語から派生が、非常に異なるパスを進化してきました。 C# の場合は、これらの言語の最新と非常に便利な方法で成熟されましたです。 さらに、c# は密接に関連付けられている数値演算、デバッグ、リフレクション、コレクション、グローバリゼーション、ファイル I/O、ネットワーク、セキュリティ、スレッド、web サービス、データ処理、および XML のサポートを提供する .NET と呼ばれるプログラミング インフラストラクチャ全体で、読み取りと書き込みの JSON。

現在、Xamarin はネイティブの Mac、iOS、および c# と .NET を使用して Android の Api を対象とするツールを提供します。 これらのツールは、Xamarin.Mac、Xamarin.iOS、Xamarin.Android、Xamarin プラットフォームと総称と呼ばれます。 これらは、ライブラリおよび .NET の表現方法でこれらのプラットフォームのネイティブ Api の express バインディング。

開発者は、Xamarin プラットフォームを使用して、そのターゲット Mac、iOS、または Android を c# でアプリケーションを作成することができます。 複数のプラットフォームを対象にすると、多数のターゲット プラットフォーム間でコードの一部を共有する意味を作成します。 これは、プラットフォームに依存するコードが (一般に、ユーザー インターフェイスを含む)、およびプラットフォームに依存しないコードは、一般にのみ、基本 .NET framework が必要にプログラムを分ける必要があります。 このプラットフォームに依存しないコードは、ポータブル クラス ライブラリ (PCL)、または多くの場合、共有アセット プロジェクトや SAP と呼ばれる、共有プロジェクトに配置できますか。

## <a name="introducing-xamarinforms"></a>Xamarin.Forms の概要

複数のモバイル プラットフォームを対象とする場合 Xamarin.Forms によりさらに多くのコードを共有できます。 Xamarin.Forms 用に記述された 1 つのプログラムは、5 つの異なるプラットフォームを対象できます。

- iOS の iPhone、iPad、iPod touch 上で実行されるプログラム
- Android 端末およびタブレット上で実行されるプログラム用の android
- 対象 Windows 10 および Windows 10 Mobile にユニバーサル Windows プラットフォーム
- Windows 8.1 の Windows ランタイム API
- Windows Phone 8.1 の Windows ランタイム API

現在の Xamarin.Forms ソリューション テンプレートは、Windows 8.1 および Windows Phone 8.1 プラットフォーム用のプロジェクト テンプレートを含めないでください。

Xamarin.Forms プログラムの大半は、PCL または SAP に存在します。 サイズの小さいアプリケーション スタブ PCL を呼び出すの各プラットフォームで構成されます。 Xamarin.Forms Api は、各プラットフォームは、その特性のルック アンド フィールを維持できるように、各プラットフォームでネイティブ コントロールにマップします。

[![共有プラットフォーム ビジュアルのトリプル スクリーン ショット](images/ch01fg03-small.png "Xamarin.Forms Controls on Each Platform")](images/ch01fg03-large.png "Xamarin.Forms Controls on Each Platform")

左から右にスクリーン ショットは、iPhone、Android フォンの場合、および Windows 10 Mobile の電話を表示します。 各画面で、ページには、Xamarin.Forms [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 、テキストを表示するため、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 、アクションを開始するため、 [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)のオン/オフ値を選択して、 [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)を継続的な範囲内の値を指定します。 これらのビューの 4 つのすべての子である、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)上、 [ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)です。

いくつかから成る Xamarin.Forms ツールバーには、ページにも添付[ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)オブジェクト。 これらは、iOS および Android の画面の上部にあると、Windows 10 Mobile の画面の下部にアイコンとして表示します。

Xamarin.Forms は XAML もサポートしている、いくつかのアプリケーション プラットフォーム用に Microsoft に開発された拡張アプリケーション マークアップ言語。 示したように、前に示したプログラムのすべてのビジュアルは XAML で定義された、 [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals)サンプルです。

Xamarin.Forms プログラムでは、どのようなプラットフォームで実行されているを確認でき、それに応じて別のコードを実行することができます。 非常に優れており、開発者は、さまざまなプラットフォーム用のカスタム コードを記述し、プラットフォームに依存しない方法で Xamarin.Forms プログラムからそのコードを実行できます。 開発者は、各プラットフォームのレンダラーを記述してもその他のコントロールを作成できます。

Xamarin.Forms は、基幹業務アプリケーション、またはプロトタイプを作成、または簡単な概念実証のデモンストレーションを行うのための良いソリューションは、小さいベクター グラフィックスまたは複雑なタッチとの対話を必要とするアプリケーションに最適です。

## <a name="your-development-environment"></a>開発環境

開発環境は、どのようなプラットフォームを対象としてどのようなマシンを使用するのに依存します。

IOS をターゲットにする場合は、Xcode と Xamarin プラットフォームがインストールされている Mac 必要があります。 同様に、Android をサポートするには、Java および必要な Sdk をインストールする必要があります。 IOS と Android for mac Visual Studio の使用の両方を対象にできますし、

Visual Studio をインストールすることができます、PC の iOS、Android、およびすべての Windows プラットフォームを対象します。 ただし、Visual Studio から iOS をターゲットとすると、Xcode と Xamarin プラットフォームがインストールされている Mac も必要です。

またはシミュレーターで、実際のデバイスで、コンピューターに USB で接続されているプログラムをテストすることができます。

## <a name="installation"></a>インストール

を作成し、Xamarin.Forms アプリケーションを構築する前に、iOS アプリケーション、Android アプリケーション、およびターゲットと、開発環境を対象のプラットフォームに応じての UWP アプリケーションを個別にビルドを作成してください。

Xamarin と Microsoft の web サイトには、これを行う方法が記載されています。

- [IOS の概要](~/ios/get-started/index.md)
- [Android の概要](~/android/get-started/index.md)
- [Windows デベロッパー センター](http://dev.windows.com)

1 回作成することができ、これらの各プラットフォーム用のプロジェクトを実行する問題は生じませんの作成と Xamarin.Forms のアプリケーションを実行します。



## <a name="related-links"></a>関連リンク

- [第 1 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [第 1 章サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
