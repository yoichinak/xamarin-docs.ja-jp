---
title: Xamarin.iOS for Visual Studio の概要
description: このドキュメントでは、Visual Studio を使用して Xamarin iOS アプリケーションをビルドし、テストする方法を示します。 プロジェクトを作成する方法、アプリを実行し、デバッグする方法、Windows から Mac ビルド ホストに接続する方法について説明しています。
ms.prod: xamarin
ms.assetid: bf3c779f-959f-428d-babb-428f363f7e4e
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2018
ms.openlocfilehash: e6f95713fdf3dbe8983c9f51554df7165637fc9a
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855134"
---
# <a name="introduction-to-xamarinios-for-visual-studio"></a>Xamarin.iOS for Visual Studio の概要

Xamarin for Windows では、iOS アプリケーションを Visual Studio 内で作成してテストすることができます。その際に、ビルドと配置サービスを提供するネットワークに接続された Mac を使用します。

この記事では、Visual Studio を使用して iOS アプリケーションをビルドするために、各コンピューターで Xamarin.iOS ツールをインストールして構成する手順について説明します。

Visual Studio 内での iOS 向けの開発には、次のような多くの利点があります。

- iOS、Android および Windows アプリケーション用のクロスプラットフォーム ソリューションの作成。
- iOS ソース コードを含む、すべてのクロスプラットフォーム プロジェクトでのお気に入りの Visual Studio ツール (**Resharper** や **Team Foundation Server** など) の使用。
- Apple のすべての API の Xamarin.iOS バインディングを活用しながら、使い慣れた IDE を使用。

## <a name="requirements-and-installation"></a>要件とインストール

Visual Studio での iOS 向けの開発時に従う必要があるいくつかの要件があります。 概要で簡単に説明したとおり、IPA ファイルをコンパイルするには Mac が必要であり、Apple の証明書とコード署名ツールがないと、デバイスにアプリケーションを展開することはできません。

いくつかの構成オプションを使用できるため、開発のニーズに最適なものを判断できます。 それらを以下に示します。

- メインの開発用コンピューターとして Mac を使用して、Visual Studio がインストールされている Windows 仮想マシンを実行します。 [Parallels](http://www.parallels.com/products/desktop/) や [VMWare](http://www.vmware.com/products/fusion/) などの VM ソフトウェアを使用することをお勧めします。
- ビルド ホストと同じように Mac を使用します。 このシナリオでは、[必要な](~/get-started/installation/windows.md#installation)ツールがインストールされている Windows コンピューターと同じネットワークに接続されます。

いずれの場合も、次の手順に従う必要があります。

- [Visual Studio for Mac をインストールする](https://docs.microsoft.com/visualstudio/mac/installation)
- [Windows に Xamarin ツールをインストールする](~/get-started/installation/windows.md)

## <a name="connecting-to-the-mac"></a>Mac への接続

Visual Studio を Mac ビルド ホストに接続するには、「[Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」ガイドの説明に従ってください。

## <a name="visual-studio-toolbar-overview"></a>Visual Studio ツール バーの概要

Xamarin iOS for Visual Studio は、アイテムを標準のツール バーと新しい iOS ツール バーに追加します。
これらのツール バーの機能については以下に説明します。

### <a name="standard-toolbar"></a>標準のツール バー

Xamarin iOS 開発に関連するコントロールは赤い円で囲まれています。

[![](introduction-to-xamarin-ios-for-visual-studio-images/03.png "Xamarin iOS 開発に関連するコントロールは赤い円で囲まれています")](introduction-to-xamarin-ios-for-visual-studio-images/03.png#lightbox "Xamarin iOS 開発に関連するコントロールは赤い円で囲まれています")

- **開始** - 選択されたプラットフォームでアプリケーションのデバッグまたは実行を開始します。 接続された Mac が必要です (iOS ツール バーのステータス インジケーターを確認してください)。
- **ソリューション構成** – 使用する構成 (デバッグ、リリースなど) を選択できます。
- **ソリューション プラットフォーム** - 展開する iPhone または iPhoneSimulator を選択できます。

### <a name="ios-toolbar"></a>iOS ツール バー

Visual Studio の各バージョンでの、Visual Studio の iOS ツール バーは似ています。 それらすべてを以下に示します。

[![](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png "iOS ツール バー")](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png#lightbox)

各アイテムについては以下に説明します。

- **Mac Agent/接続マネージャー** – Xamarin Mac Agent のダイアログ ボックスを表示します。 このアイコンは、接続中の場合は*オレンジ*になり、接続済みの場合は*緑*になります。
- **iOS シミュレーターの表示** – Mac の前面に iOS シミュレーター ウィンドウを移動します。
- **ビルド サーバーに IPA ファイルを表示** - アプリケーションの IPA 出力ファイルの場所に Mac のファインダーを開きます。

## <a name="ios-output-options"></a>iOS 出力オプション

### <a name="output-window"></a>[出力] ウィンドウ

ビルド、展開、および接続メッセージとエラーを検出するために表示できる *[出力]* ウィンドウにはオプションがいくつかあります。

次のスクリーンショットは、プロジェクトの種類に応じて異なる場合がある、使用可能な出力ウィンドウを示しています。

[![](introduction-to-xamarin-ios-for-visual-studio-images/output-sml.png "使用可能な出力ウィンドウ")](introduction-to-xamarin-ios-for-visual-studio-images/output-large.png#lightbox)

- **Xamarin** – これには、Mac への接続やアクティブ化の状態など、Xamarin のみに関連する情報が含まれます。

  [![](introduction-to-xamarin-ios-for-visual-studio-images/output3-sml.png "Mac への接続やアクティブ化の状態など、Xamarin のみに関連する情報")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

- **Xamarin 診断** – Android との対話など、Xamarin プロジェクトに関する詳細情報が示されます。

  [![](introduction-to-xamarin-ios-for-visual-studio-images/output4-sml.png "Xamarin プロジェクトに関する詳細情報")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

デバッグやビルドなどの Visual Studio の他の既定の出力ウィンドウは出力ビュー内でも使用でき、次のようにデバッグ出力や MSBuild 出力で使用されます。

- **デバッグ**

  [![](introduction-to-xamarin-ios-for-visual-studio-images/output2-sml.png "デバッグの出力")](introduction-to-xamarin-ios-for-visual-studio-images/output2-large.png#lightbox)

- **ビルド** & **ビルドの順序**

  [![](introduction-to-xamarin-ios-for-visual-studio-images/output1-sml.png "MSBuild 出力")](introduction-to-xamarin-ios-for-visual-studio-images/output1-large.png#lightbox)

## <a name="ios-project-properties"></a>iOS プロジェクト プロパティ

Visual Studio のプロジェクト プロパティには、プロジェクト名を右クリックし、コンテキスト メニューの *[プロパティ]* を選択することでアクセス可能です。 これにより、次のスクリーンショットのように、iOS アプリケーションを構成できるようになります。

![](introduction-to-xamarin-ios-for-visual-studio-images/iosproperties.png "iOS アプリケーションの構成")

- *iOS バンドル署名* – Mac に接続して、コード署名 ID とプロビジョニング プロファイルを設定します。

  ![コード署名 ID とプロビジョニング プロファイルを設定する](introduction-to-xamarin-ios-for-visual-studio-images/bundlesigning.png)

- *iOS IPA オプション* – IPA ファイルが Mac のファイル システムに保存されます。

  ![iOS IPA オプション](introduction-to-xamarin-ios-for-visual-studio-images/ipaoptions.png)

- *iOS 実行オプション* – その他のパラメーターを構成します。

  ![iOS 実行オプション](introduction-to-xamarin-ios-for-visual-studio-images/iosrunoptions.png)

## <a name="creating-a-new-project-for-ios-applications"></a>iOS アプリケーション用の新しいプロジェクトの作成

Visual Studio 内からの新しい iOS プロジェクトの作成は、他のプロジェクトの種類と同じように行われます。 **[ファイル]、[新しいプロジェクト]** の順に選択すると、以下のようなダイアログが開きます。ここには、新しい iOS プロジェクトを作成するために使用可能なプロジェクト タイプがいくつか示されています。

![新規プロジェクトの作成](introduction-to-xamarin-ios-for-visual-studio-images/newproject.w157.png)

**[iOS アプリ (Xamarin)]** を選択すると、新しい Xamarin.iOS アプリケーションを作成するための次のテンプレートが表示されます。

![iOS アプリのテンプレートの選択](introduction-to-xamarin-ios-for-visual-studio-images/newproject-2.w157.png)

ストーリーボードと .xib ファイルは、Visual Studio で iOS Designer を使用して編集できます。 ストーリーボードを作成するには、ストーリーボード テンプレートのいずれかを選択します。 これにより、以下のスクリーンショットのように、**ソリューション エクスプローラー**で **Main.storyboard** ファイルが生成されます。

![ソリューション エクスプローラーの Main.storyboard ファイル](introduction-to-xamarin-ios-for-visual-studio-images/solution-explorer-new.w157.png)

ストーリーボードの作成または編集を開始するには、`Main.storyboard` をダブルクリックして iOS Designer で開きます。

![](introduction-to-xamarin-ios-for-visual-studio-images/iosdesigner.png "iOS Designer の Main.storyboard")

オブジェクトをビューに追加するには、**[ツールボックス]** ウィンドウを使用して、デザイン サーフェイスにアイテムをドラッグ アンド ドロップします。 ツールボックスがまだ追加されていない場合は、**[ビュー]、[ツールボックス]** の順に選択して追加できます。 以下のように **[プロパティ]** ウィンドウを使用して、オブジェクト プロパティを変更し、そのレイアウトを調整し、イベントを作成することができます。

![](introduction-to-xamarin-ios-for-visual-studio-images/properties.png "プロパティ ウィンドウ")

 iOS Designer の使用の詳細については、[Designer](~/ios/user-interface/designer/index.md) のガイドを参照してください。

## <a name="running--debugging-ios-applications"></a>iOS アプリケーションの実行とデバッグ

### <a name="device-logging"></a>デバイスのログ

Visual Studio 2017 では、Android および iOS のログ パッドが統合されています。

Visual Studio 用の新しい [デバイス ログ] ツール ウィンドウでは、Android および iOS デバイスのログを表示できます。 次のコマンドのいずれかを実行して表示できます。

- **[ビュー]、[その他のウィンドウ]、[デバイス ログ]**
- **[ツール]、[iOS]、[デバイス ログ]**
- **[iOS ツール バー]、[デバイス ログ]**

ツール ウィンドウが表示されたら、ユーザーはデバイスのドロップダウン リストから物理デバイスを選択できます。 デバイスが選択されると、ログがテーブルに自動的に追加されます。 デバイスを切り替えると、デバイスのログが停止し、開始されます。

コンボ ボックスにデバイスを表示するには、iOS プロジェクトを読み込む必要があります。 さらに、iOS の場合、Mac に接続されている iOS デバイスを検出するには、Visual Studio を [Mac サーバーに接続する](~/ios/get-started/installation/windows/connecting-to-mac/index.md)必要があります。

このツール ウィンドウでは、ログ エントリのテーブル、デバイス選択用のドロップダウン リスト、ログ エントリをクリアする方法、検索ボックス、および再生/停止/一時停止ボタンが提供されます。

### <a name="set-debugging-stops"></a>デバッグの停止を設定する

ブレークポイントは、プログラムの実行を一時的に停止するように、アプリケーションがデバッガーに通知する任意の時点に設定できます。 Visual Studio でブレークポイントを設定するには、エディターで、中断するコードの行番号の横にある余白領域をクリックします。

![](introduction-to-xamarin-ios-for-visual-studio-images/image18.png "デバッグ ポイントの設定")

デバッグを開始し、シミュレーターまたはデバイスを使用して、ブレークポイントにアプリケーションを移動します。 ブレークポイントにヒットすると、行が強調表示され、Visual Studio の通常のデバッグ動作が有効になります。コードのステップ イン、ステップ オーバー、またはステップ アウトを行ったり、ローカル変数を確認したり、あるいはイミディエイト ウィンドウを使用することができます。

このスクリーンショットでは、macOS の Parallels を使用する Visual Studio の横に実行されている iOS シミュレーターが示されています。

![このスクリーンショットでは、macOS の Parallels を使用する Visual Studio の横に実行されている iOS シミュレーターが示されている](introduction-to-xamarin-ios-for-visual-studio-images/image19.png)

### <a name="examine-local-variables"></a>ローカル変数を確認する

![デバッグでのローカル変数の確認](introduction-to-xamarin-ios-for-visual-studio-images/image20.png)

## <a name="summary"></a>まとめ

この記事では、Xamarin iOS for Visual Studio を使用する方法について説明しました。 Visual Studio 内から iOS アプリを作成、ビルド、およびテストするために使用可能なさまざまな機能を示しました。また、簡単な iOS アプリケーションのビルドとデバッグについても説明しました。

## <a name="related-links"></a>関連リンク

- [Xamarin.iOS のインストール](~/ios/get-started/installation/windows/index.md)
- [デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)
- [コードでの iOS UI の作成](~/ios/app-fundamentals/ios-code-only.md)
- [XMA を使用する Visual Studio 環境への Mac の接続 (ビデオ)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
