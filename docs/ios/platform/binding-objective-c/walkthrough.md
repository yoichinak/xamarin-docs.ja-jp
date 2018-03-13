---
title: "チュートリアル: バインド iOS Objective C ライブラリ"
description: "この記事では、既存の Objective C ライブラリ、InfColorPicker 用 Xamarin.iOS バインディングの作成の実践的なチュートリアルを提供します。 Objective C のスタティック ライブラリをコンパイルし、バインド、Xamarin.iOS アプリケーションでバインディングを使用してなどのトピックについて説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: e4619f5b1d3f888b2557cf894aaa83106504766f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>チュートリアル: バインド iOS Objective C ライブラリ

_この記事では、既存の Objective C ライブラリ、InfColorPicker 用 Xamarin.iOS バインディングの作成の実践的なチュートリアルを提供します。Objective C のスタティック ライブラリをコンパイルし、バインド、Xamarin.iOS アプリケーションでバインディングを使用してなどのトピックについて説明します。_

Ios を使用する場合は、サード パーティ製 Objective C のライブラリを使用する場合が発生する可能性があります。 ような状況では、Xamarin.iOS を使用することができます_バインド プロジェクト_を作成する、 [c# バインディング](~/cross-platform/macios/binding/overview.md)ことになります、Xamarin.iOS アプリケーションでライブラリを使用します。

一般に iOS のエコシステムには、3 種類がありますライブラリを見つけることができます。

* プリコンパイル済みのスタティック ライブラリのファイルとして`.a`と共にそのヘッダー (.h ファイル) の拡張機能です。 たとえば、 [Google の分析ライブラリ](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* プリコンパイル済みのフレームワークです。 これは、スタティック ライブラリ、ヘッダーを追加する場合がリソースを含むフォルダーで`.framework`拡張機能です。 たとえば、 [Google の AdMob ライブラリ](https://developers.google.com/admob/ios/download)です。
* 単ソース コード ファイル。 だけを含むライブラリなど、`.m`と`.h`Objective C ファイル。

最初と 2 番目のシナリオでが既にありますプリコンパイル CocoaTouch スタティック ライブラリのため、この記事で 3 番目のシナリオに焦点を当てます。 ただし、バインドを作成、自由にもバインドしていることを確認するライブラリに用意されている現在のライセンスを常に確認を開始する前にします。

この記事は、オープン ソースを使用してバインド プロジェクトの作成の詳細なチュートリアル[InfColorPicker](https://github.com/InfinitApps/InfColorPicker) OBJECTIVE-C 例として、プロジェクトのいずれかとのこのガイドのすべての情報が使用するために合わせて変更できますが、サード パーティ製 Objective C のライブラリです。 InfColorPicker ライブラリでは、ユーザーが、HSB 表現をよりわかりやすい色の選択を行ったに基づいて色を選択できる再利用可能なビュー コント ローラーを提供します。

[![](walkthrough-images/run01.png "IOS で実行されている InfColorPicker ライブラリの例")](walkthrough-images/run01.png#lightbox)

Xamarin.iOS でこの特定の Objective C API を使用するために必要なすべての手順を説明します。

- 最初に、Xcode を使って OBJECTIVE-C スタティック ライブラリを作成します。
- 次に、このスタティック ライブラリ Xamarin.iOS をバインドします。
- 目標ペンを使わずが一部 (すべてではない) を自動的に生成することによって、ワークロードを軽減する方法を次に、表示する Xamarin.iOS バインドによって要求されるために必要な API 定義します。
- 最後に、バインディングを使用する Xamarin.iOS アプリケーションが作成されます。

サンプル アプリケーションは、InfColorPicker API と c# コード間の通信が強力なデリゲートを使用する方法をデモンストレーションします。 強力なデリゲートを使用する方法を説明し、脆弱なデリゲートを使用して同じタスクを実行する方法がここします。

## <a name="requirements"></a>必要条件

この記事では、Xcode と Objective C 言語の一部に関する知識があり、その読み取りが前提としています、[バインド OBJECTIVE-C](~/cross-platform/macios/binding/index.md)ドキュメント。 さらに、次は、表示する手順を完了するために必要。

-  **Xcode および iOS SDK** -Apple の Xcode と最新の iOS API をインストールして、開発者のコンピューター上に構成する必要があります。
-  **[Xcode コマンド ライン ツール](#Installing_the_Xcode_Command_Line_Tools)** -Xcode (インストールの詳細については以下を参照してください) の現在インストールされているバージョンについては、Xcode コマンド ライン ツールをインストールする必要があります。
-  **Mac または Visual Studio 用の visual Studio** -Mac または Visual Studio 用の Visual Studio の最新バージョンをインストールし、開発用コンピューター上で構成します。 Apple Mac、Xamarin.iOS アプリケーションの開発に必要なは、Visual Studio を使用する場合に接続している[Xamarin.iOS ホストを構築します。](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **最新バージョンの目標ペンを使わず**-目標ペンを使わずツールの現在のコピーからダウンロード[ここ](~/cross-platform/macios/binding/objective-sharpie/get-started.md)です。 あれば既にインストールされている目標ペンを使わず、ことができますに更新する最新バージョンを使用して、 `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode コマンド ライン ツールをインストールします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


前述のように、使用する Xcode コマンド ライン ツール (特に`make`と`lipo`) このチュートリアルでします。 `make`コマンドは、非常に一般的な Unix のユーティリティを使用して実行可能プログラムとライブラリのコンパイルを自動化する、_メイクファイル_プログラムの構築方法を指定します。 `lipo`コマンドは、複数のアーキテクチャのファイルを作成するため、OS X のコマンド ライン ユーティリティです。 複数を組み合わせることは`.a`ファイルをすべてのハードウェア アーキテクチャで使用できる 1 つのファイルにします。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


前述のように、使用する Xcode コマンド ライン ツールで、 **Mac ビルド ホスト**(具体的には`make`と`lipo`) このチュートリアルでします。 `make`コマンドは、非常に一般的な Unix のユーティリティを使用して実行可能プログラムとライブラリのコンパイルを自動化する、_メイクファイル_にプログラムをビルドする方法を指定します。 `lipo`コマンドは、複数のアーキテクチャのファイルを作成するため、OS X のコマンド ライン ユーティリティです。 複数を組み合わせることは`.a`ファイルをすべてのハードウェア アーキテクチャで使用できる 1 つのファイルにします。


-----

Apple のに従って[Xcode よく寄せられる質問をコマンドラインからのビルド](https://developer.apple.com/library/ios/technotes/tn2339/_index.html)ドキュメント、OS X 10.9 以降では、**ダウンロード**Xcode のウィンドウ**設定**ダイアログされませんダウンロードのコマンド ライン ツールをサポートしなくなりました。

使用して次のいずれかのツールをインストールする必要があります。

- **Xcode をインストール**- Xcode をインストールする場合、すべてのコマンド ライン ツールが付属しており、します。 OS X 10.9 で shim (にインストールされている`/usr/bin`) に含まれる任意のツールをマップできます`/usr/bin`Xcode の内部対応するツールです。 たとえば、`xcrun`コマンドでは、見つからないか、コマンドラインから Xcode の内部の任意のツールを実行することができます。
- **ターミナル アプリケーション**-ターミナルのアプリケーションから実行して、コマンド ライン ツールをインストールすることができます、`xcode-select --install`コマンド。
    - ターミナル アプリケーションを起動します。
    - 型`xcode-select --install`とキーを押します**Enter**、例を示します。

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - コマンド ライン ツールをインストールする たずね、**インストール**ボタン: [ ![ ](walkthrough-images/xcode01.png "コマンド ライン ツールをインストールします。")](walkthrough-images/xcode01.png#lightbox)

    - ツールがダウンロードされ、Apple のサーバーからインストール: [ ![ ](walkthrough-images/xcode02.png "ツールをダウンロードします。")](walkthrough-images/xcode02.png#lightbox)

- **Apple 開発者向けダウンロード**-可能なパッケージは、コマンド ライン ツール、 [Apple 開発者向けダウンロード]()web ページ。 Apple ID を使用してログインし、検索、およびコマンド ライン ツールをダウンロード: [ ![ ](walkthrough-images/xcode03.png "コマンド ライン ツールを検索します。")](walkthrough-images/xcode03.png#lightbox)

インストールされているコマンド ライン ツールで、チュートリアルを続行する準備ができました。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルでここには、次の手順について説明します。

- **[スタティック ライブラリの作成](#Creating_A_Static_Library)** -のスタティック ライブラリを作成するこの手順では、 **InfColorPicker** Objective C コード。 スタティック ライブラリには、`.a`ファイル拡張子、およびライブラリ プロジェクトの .NET アセンブリに埋め込まれます。
- **[Xamarin.iOS バインド プロジェクトを作成する](#Create_a_Xamarin.iOS_Binding_Project)**の使用を Xamarin.iOS バインド プロジェクトを作成するスタティック ライブラリを作成したら、します。 バインディングのプロジェクトは、作成したばかりのスタティック ライブラリと OBJECTIVE-C API を使用する方法を説明する c# コードの形式でメタデータで構成されます。 このメタデータは、API の定義とも呼ばれます。 使用して**[目標ペンを使わず](#Using_Objective_Sharpie)**ご協力に API の定義を作成します。
- **[API 定義を正規化](#Normalize_the_API_Definitions)** - 目標ペンを使わずに大変のうえが、そのすべてを行うことはできません。 API 定義に使用する前に加える必要があるいくつかの変更について説明します。
- **[バインドのライブラリを使用して](#Using_the_Binding)** -最後に、新しく作成されたバインディングのプロジェクトを使用する方法を示す Xamarin.iOS アプリケーションを作成します。

どのような手順が関係している理解できたためは、チュートリアルの残りの部分に移ります。

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>スタティック ライブラリを作成します。

Github で InfColorPicker 用のコードを調べてお: 場合

[![](walkthrough-images/image02.png "InfColorPicker Github でコードを検査します。")](walkthrough-images/image02.png#lightbox)

プロジェクトに次の 3 つのディレクトリが分かります。

- **InfColorPicker** -このディレクトリには、プロジェクトの Objective C コードが含まれています。
- **PickerSamplePad** -このディレクトリには、iPad、サンプルのプロジェクトが含まれています。
- **PickerSamplePhone** -このディレクトリには、iPhone、サンプルのプロジェクトが含まれています。

それでは、InfColorPicker からプロジェクトをダウンロード[GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip)し、選択したディレクトリに解凍します。 対象を Xcode を開いて`PickerSamplePhone`プロジェクトは Xcode ナビゲーターで次のプロジェクト構造を参照してください。

[![](walkthrough-images/image03.png "ナビゲーターの Xcode プロジェクト構造")](walkthrough-images/image03.png#lightbox)

このプロジェクトは、サンプルの各プロジェクトに InfColorPicker のソース コード (赤いボックス) を直接追加することによって、コードの再利用を実現します。 サンプル プロジェクトのコードは、青いボックス内です。 この特定のプロジェクトが提供されない us スタティック ライブラリとのための必要は us スタティック ライブラリをコンパイルする Xcode プロジェクトを作成します。

最初の手順では、スタティック ライブラリに InfoColorPicker ソース コードを追加します。 みましょうこれを実現するために、次の操作します。

1. Xcode を起動します。
2. **ファイル**メニュー選択**新規** > **プロジェクト.**:

    [![](walkthrough-images/image04.png "新しいプロジェクトを開始")](walkthrough-images/image04.png#lightbox)
3. 選択**フレームワークとライブラリ**、 **Cocoa タッチのスタティック ライブラリ**テンプレートをクリック、**次**ボタン。

    [![](walkthrough-images/image05.png "Cocoa タッチのスタティック ライブラリ テンプレートを選択します。")](walkthrough-images/image05.png#lightbox)
4. 入力`InfColorPicker`の**プロジェクト名** をクリックし、 **次へ**ボタン。

    [![](walkthrough-images/image06.png "プロジェクト名の InfColorPicker を入力してください。")](walkthrough-images/image06.png#lightbox)
5. プロジェクトを保存し、をクリックする場所を選択、 **OK**ボタンをクリックします。
6. スタティック ライブラリ プロジェクトに InfColorPicker プロジェクトからソースを追加する必要があります。 **InfColorPicker.h**ファイルは既に存在、スタティック ライブラリで (既定) で Xcode いないことが上書きされます。 **Finder**は、GitHub からお解凍した元のプロジェクトに InfColorPicker ソース コードに移動し、すべて InfColorPicker ファイルのコピー、新規のスタティック ライブラリ プロジェクトに貼り付けます。

    [![](walkthrough-images/image12.png "すべての InfColorPicker ファイル コピーします。")](walkthrough-images/image12.png#lightbox)

7. Xcode に戻り、右クリックして、 **InfColorPicker**フォルダーと選択**「InfColorPicker…」にファイルを追加**:

    [![](walkthrough-images/image08.png "ファイルを追加します。")](walkthrough-images/image08.png#lightbox)

8. ファイルの追加 ダイアログ ボックスからコピーした InfColorPicker ソース コード ファイルに移動し、それらすべてを選択してをクリックして、**追加**ボタンをクリックします。

    [![](walkthrough-images/image09.png "すべてを選択し、[追加] をクリックします。")](walkthrough-images/image09.png#lightbox)

9. ソース コードは、プロジェクトにコピーされます。

    [![](walkthrough-images/image10.png "ソース コードをプロジェクトにコピーされます。")](walkthrough-images/image10.png#lightbox)

10. Xcode プロジェクト ナビゲーターから選択、 **InfColorPicker.m**ファイルし、行をコメント アウト、最後の 2 つ (方法により、このライブラリが書き込まれたこのファイルは使用されません)。

    [![](walkthrough-images/image14.png "InfColorPicker.m ファイルの編集")](walkthrough-images/image14.png#lightbox)

11. ここで、ライブラリで必要な任意のフレームワークがあるかを確認する必要があります。 Readme ファイル、または提供されたサンプル プロジェクトの 1 つ開くことによって、この情報を参照することができます。 この例では`Foundation.framework`、 `UIKit.framework`、および`CoreGraphics.framework`ではに追加します。

12. 選択、 **InfColorPicker ターゲット > ビルド フェーズ**を展開し、**リンク バイナリとライブラリ**セクション。

    [![](walkthrough-images/image16b.png "リンク バイナリとライブラリのセクションを展開します。")](walkthrough-images/image16b.png#lightbox)

13. 使用して、  **+** 上必要なフレーム フレームワークの一覧に追加することができます ダイアログを開くボタンをクリックします。

    [![](walkthrough-images/image16c.png "上記の必要なフレーム フレームワークを追加します。")](walkthrough-images/image16c.png#lightbox)

14. **リンク バイナリとライブラリ**セクションは次の図のようになります。

    [![](walkthrough-images/image16d.png "リンク バイナリとライブラリ セクション")](walkthrough-images/image16d.png#lightbox)

この時点で私たちは、閉じるが終わってそうではありません。 スタティック ライブラリが作成されましたが、Fat のバイナリを含むすべての iOS デバイスと iOS シミュレーターの両方に必要なアーキテクチャを作成するためにビルドする必要があります。

### <a name="creating-a-fat-binary"></a>Fat バイナリを作成します。

すべての iOS デバイスでは、時間の経過と共に開発電源 ARM アーキテクチャを使用してプロセッサを搭載します。 各新しいアーキテクチャは、後方互換性を維持しながら新しい手順とその他の改善を追加します。 IOS デバイスである、armv6、常に armv7、armv7s arm64 命令セットが[は今後使用しない armv6](~/ios/deploy-test/compiling-for-different-devices.md)です。 IOS シミュレーター ARM での電源が入らない、あの古くて、x86、x86_64 電源シミュレーター。 各命令でライブラリを提供する必要がありますがつまり、ご利用の米国を設定します。

Fat ライブラリは`.a`サポートされているすべてのアーキテクチャを含むファイルです。

作成、fat をバイナリは、次の 3 ステップ プロセスです。

- スタティック ライブラリの ARM 7 & ARM64 バージョンをコンパイルします。
- スタティック ライブラリの x86 と x84_64 バージョンをコンパイルします。
- 使用して、 `lipo` 2 つの静的ライブラリを 1 つに結合するコマンド ライン ツールです。

これら 3 つの手順は簡単ではなく、あり、もう一度実行して、今後 Objective C ライブラリが更新プログラムを受信すると、またはバグの修正が必要な場合に必要な場合があります。 次の手順を自動化する場合は、その簡単になります、今後のメンテナンスとサポート iOS バインド プロジェクト。

シェル スクリプトでは、- などのタスクを自動化に使用できる多くのツールがある[rake](http://rake.rubyforge.org/)、 [xbuild](http://www.mono-project.com/Microsoft.Build)、および[ように](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html)です。 Xcode コマンド ライン ツールをインストールするとおも make、このチュートリアルで使用されるビルド システムはこれをインストールします。 ここでは、**メイクファイル**iOS デバイスおよびシミュレーターでの任意のライブラリで動作する複数のアーキテクチャの共有ライブラリを作成に使用できます。

```bash
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=./YOUR-PROJECT-NAME
PROJECT=$(PROJECT_ROOT)/YOUR-PROJECT-NAME.xcodeproj
TARGET=YOUR-PROJECT-NAME

all: lib$(TARGET).a

lib$(TARGET)-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

lib$(TARGET)-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET)-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET).a: lib$(TARGET)-i386.a lib$(TARGET)-armv7.a lib$(TARGET)-arm64.a
    xcrun -sdk iphoneos lipo -create -output $@ $^

clean:
    -rm -f *.a *.dll
```

入力、**メイクファイル**、任意のプレーン テキスト エディターでのコマンドでセクションを更新し、 **、プロジェクト名**プロジェクトの名前に置き換えます。 いることを確認する必要も、上記の説明、手順内のタブが維持されていることを貼り付けること。

名前のファイルを保存**メイクファイル**上で作成した InfColorPicker Xcode のスタティック ライブラリと同じ場所に。

[![](walkthrough-images/lib00.png "メイクファイルの名前を持つファイルを保存します。")](walkthrough-images/lib00.png#lightbox)

Mac でターミナル アプリケーションを開き、Makefile の場所に移動します。 型`make`端末にキーを押します。 **Enter**と**メイクファイル**実行されます。

[![](walkthrough-images/lib01.png "メイクファイルのサンプル出力")](walkthrough-images/lib01.png#lightbox)

Make を実行すると、多数のスクロール テキストが表示されます。 正しく動作していたすべての場合は、単語が表示されます**ビルドに成功した**と`libInfColorPicker-armv7.a`、`libInfColorPicker-i386.a`と`libInfColorPickerSDK.a`ファイルのコピー先と同じ場所、**メイクファイル**:

[![](walkthrough-images/lib02.png "メイクファイルで生成された libInfColorPicker armv7.a、libInfColorPicker i386.a および libInfColorPickerSDK.a ファイル")](walkthrough-images/lib02.png#lightbox)

次のコマンドを使用して、Fat バイナリ内で、アーキテクチャを確認できます。

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

これは、次が表示されます。

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

Xcode および Xcode コマンド ライン ツールを使用してスタティック ライブラリを作成することで、iOS バインディングの最初のステップが終了しましたこの時点では、`make`と`lipo`です。 次の手順に移動しを使用してみましょう**目標ペンを使わず**us の API バインドの作成を自動化します。

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>プロジェクトのバインド Xamarin.iOS を作成します。

使用する前に**目標ペンを使わず**をバインディング プロセスを自動化する必要があります、API の定義を格納する Xamarin.iOS バインド プロジェクトを作成する (を使用する**目標ペンを使わず**にご協力ください構築) し、ご利用の米国の c# バインディングを作成します。

それでは、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for mac を起動します。
1. **ファイル**メニューの **新規** > **ソリューションしています.**:

    ![](walkthrough-images/bind01.png "新しいソリューションの開始")

1. 新しいソリューション ダイアログ ボックスで、次のように選択します**ライブラリ** > **iOS プロジェクトのバインド**:。

    ![](walkthrough-images/bind02.png "IOS プロジェクトのバインドの選択します。")

1. クリックして、**次**ボタンをクリックします。

1. "InfColorPickerBinding"を入力として、**プロジェクト名** をクリックし、**作成**ソリューションを作成するにはボタン。

    ![](walkthrough-images/bind02a.png "プロジェクト名として InfColorPickerBinding を入力してください。")

ソリューションが作成され、2 つの既定のファイルが含まれます。

![](walkthrough-images/bind03.png "ソリューション エクスプ ローラーでソリューションの構造")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Visual Studio を起動します。

1. **ファイル**メニューの **新規** > **プロジェクト.**:

    ![](walkthrough-images/bind01vs.png "新しいプロジェクトを開始")

1. 新しいプロジェクト ダイアログ ボックスから選択**iOS** > **バインド ライブラリ**:

    ![](walkthrough-images/bind02vs.png "IOS のバインドのライブラリを選択します。")

1. "InfColorPickerBinding"を入力、**名前** をクリックし、 **ok**クリックすると、ソリューションを作成します。

ソリューションが作成され、2 つの既定のファイルが含まれます。

![](walkthrough-images/bind03vs.png "ソリューション エクスプ ローラーでソリューションの構造")

-----



- **ApiDefinition.cs** -このファイルは c# で Objective C の API をラップする方法を定義するコントラクトを含まれます。
- **Structs.cs** - このファイルは、構造を保持するか、列挙値をインターフェイスとデリゲートが必要です。

チュートリアルの後半でこれら 2 つのファイルを操作します。 最初に、バインド プロジェクトに InfColorPicker ライブラリを追加する必要があります。

### <a name="including-the-static-library-in-the-binding-project"></a>バインディングのプロジェクトでのスタティック ライブラリを含む

ようになりました、基本バインド プロジェクト準備ができての上で作成したバイナリ Fat ライブラリを追加する必要があります、 **InfColorPicker**ライブラリです。

ライブラリを追加する手順に従います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 右クリックし、**ネイティブ参照**ソリューション パッドし、フォルダー**ネイティブ参照の追加**:

    ![](walkthrough-images/bind04a.png "ネイティブ参照を追加します。")

1. Fat バイナリ行いました前に移動 (`libInfColorPickerSDK.a`) キーを押すと、**開く**ボタン。

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK.a ファイルを選択します。")
1. ファイルは、プロジェクトに含めるは。

    ![](walkthrough-images/bind04.png "ファイルを含む")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. コピー、`libInfColorPickerSDK.a`から、 **Mac ビルド ホスト**し、バインディングのプロジェクトに貼り付けます。

1. プロジェクトを右クリックし、選択**追加 > 既存アイテム.**:

    ![](walkthrough-images/bind04vs.png "既存のファイルを追加します。")

1. 移動、`libInfColorPickerSDK.a`キーを押すと、**追加**ボタンをクリックします。

    ![](walkthrough-images/bind05vs.png "LibInfColorPickerSDK.a を追加します。")

1. ファイルがプロジェクトに含まれます。

-----


Xamarin.iOS は自動的に設定されている .a ファイルがプロジェクトに追加されると、**ビルド アクション**するファイルの**ObjcBindingNativeLibrary**と呼ばれる特別なファイルを作成および`libInfColorPickerSDK.linkwith.cs`です。


このファイルが含まれています、 `LinkWith` Xamarin.iOS ハンドル直前おスタティック ライブラリを追加する方法を通知する属性。 このファイルの内容は次のコード スニペットに示します。

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith`属性は、プロジェクトといくつかの重要なリンカー フラグ用のスタティック ライブラリを識別します。


次のことを行う必要があります InfColorPicker プロジェクトの API 定義の作成を開始します。 このチュートリアルの目的で、使用目的ペンを使わずファイルを生成する**ApiDefinition.cs**です。

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>目標ペンを使わずに使用します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


目標ペンを使わずには、コマンド ライン ツール (Xamarin により提供) サード パーティ製 Objective C のライブラリを c# にバインドするために必要な定義を作成する場合に利用できます。 このセクションでを使用して目標ペンを使わず作成初期**ApiDefinition.cs** InfColorPicker プロジェクト。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


目標ペンを使わずには、コマンド ライン ツール (Xamarin により提供) サード パーティ製 Objective C のライブラリを c# にバインドするために必要な定義を作成する場合に利用できます。 このセクションで使わ目標ペンを使わずに、 **Mac ビルド ホスト**初期を作成する**ApiDefinition.cs** InfColorPicker プロジェクト用。


-----

最初に、この詳細として目標ペンを使わずインストーラー ファイルをダウンロードしましょう[ガイド](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing)です。 インストーラーを実行し、目標ペンを使わずを開発用コンピューターにインストールするインストール ウィザードからのすべての画面の指示に従います。

目標ペンを使わずを正常に取得したら、インストールされていてみましょう、ターミナル アプリを起動およびすべてのバインドを支援するために提供するツールのヘルプを表示するには、次のコマンドを入力してください。

```bash
sharpie -help
```

上記のコマンドを実行して、次の出力が生成されます。

```bash
Europa:Resources kmullins$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, --helpShow detailed help
  -v, --versionShow version information

Available Tools:

  xcode    Get information about Xcode installations and available SDKs.

  bind     Create a Xamarin C# binding to Objective-C APIs
Europa:Resources kmullins$
```

このチュートリアルの目的で使用する、次の目標ペンを使わずツール。

- **xcode** - このツールにより、現在の Xcode インストールと iOS および Mac の Api にインストールされているのバージョンに関する情報。 使用するこの情報は後で、バインディングが生成されたとき。
- **バインド**-このツールを使用して、解析、 **.h**初期に InfColorPicker プロジェクト内のファイル**ApiDefinition.cs**と**StructsAndEnums.cs**ファイル。

特定の目標ペンを使わずツールのヘルプを表示するには、ツールの名前を入力し、`-help`オプション。 たとえば、`sharpie xcode -help`次の出力を返します。

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, --help                 Show detailed help
  -v, --verbose              Be verbose with output
      --sdks                 List all available Xcode SDKs. Pass -verbose for
                               more details.
Europa:Resources kmullins$
```

バインディング プロセスを始めることができます、前に、ターミナルに次のコマンドを入力して、現在インストールされている Sdk に関する情報を取得する必要があります`sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

上記から、表示があること、 `iphoneos8.1` SDK がコンピューターにインストールされています。 配置でこの情報を使用する準備が整いました InfColorPicker プロジェクトを解析`.h`最初にファイルを**ApiDefinition.cs**と`StructsAndEnums.cs`InfColorPicker プロジェクト。

次のコマンドを入力して、ターミナル アプリ。

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

ここで`[full-path-to-project]`ディレクトリへの完全パスは、ここで、 **InfColorPicker** Xcode プロジェクト ファイルが、コンピューター上にあり、によって示された [iphone os] は、iOS SDK にインストールされている、`sharpie xcode -sdks`コマンド。 この例ではした渡されることに注意してください **\*.h**をパラメーターとして含む*すべて*- このディレクトリ内のヘッダー ファイルを通常必要があります、これが実行されませんが、代わりに慎重に目を通して、最上位レベルを検索するヘッダー ファイル**.h**他の関連ファイルがすべてを参照してだけを目的のペンを使わずに渡すファイル。

次[出力](walkthrough-images/os05.png)端末に生成されます。

```bash
Europa:Resources kmullins$ sharpie bind -output InfColorPicker -namespace InfColorPicker -sdk iphoneos8.1 /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h -unified
Compiler configuration:
    -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk -miphoneos-version-min=8.1 -resource-dir /Library/Frameworks/ObjectiveSharpie.framework/Versions/1.1.1/clang-resources -arch armv7 -ObjC

[  0%] parsing /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h
In file included from /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h:60:
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* sourceColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* resultColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
4 warnings generated.
[100%] parsing complete
[bind] InfColorPicker.cs
Europa:Resources kmullins$
```

および**InfColorPicker.enums.cs**と**InfColorPicker.cs**ファイルは、ディレクトリに作成されます。

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs と InfColorPicker.cs ファイル")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


上で作成したバインド プロジェクトでは、これらのファイルを開きます。 内容をコピー、 **InfColorPicker.cs**ファイルし、貼り付けます、 **ApiDefinition.cs**ファイルを置き換えて、既存`namespace ...`コード ブロックの内容を**InfColorPicker.cs**ファイル (まま、`using`ステートメントをそのまま)。

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate ファイル")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


上で作成したバインド プロジェクトでは、これらのファイルを開きます。 内容をコピー、 **InfColorPicker.cs**ファイル (から、 **Mac ビルド ホスト**) に貼り付けると、 **ApiDefinition.cs**ファイルを置き換えて、既存`namespace ...`コード ブロックの内容を**InfColorPicker.cs**ファイル (まま、`using`ステートメントをそのまま)。


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API 定義を正規化します。

変換する、問題がある場合が目標ペンを使わず`Delegates`おの定義を変更する必要がありますので、`InfColorPickerControllerDelegate`インターフェイスし、置換、`[Protocol, Model]`を次の行。

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
次のように定義します。

[![](walkthrough-images/os11.png "定義")](walkthrough-images/os11.png#lightbox)

内容と同じ作業を行う次に、`InfColorPicker.enums.cs`ファイルをコピーして貼り付けることで、`StructsAndEnums.cs`ファイルを残して、`using`ステートメントをそのまま。

[![](walkthrough-images/os09.png "内容、StructsAndEnums.cs ファイル ")](walkthrough-images/os09.png#lightbox)

目標ペンを使わずにバインディングが、注釈付けられている場合もあります`[Verify]`属性。 これらの属性を示す目標ペンを使わずが元の C/Objective C 宣言 (にバインドされている宣言の上にコメントが表示されます) を使用してバインディングを比較することによって、正しいことをだったことを確認する必要があります。 バインドを確認した後は、検証属性を削除してください。 詳細についてを参照してください、[を確認してください](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)ガイドです。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


この時点では、バインディングのプロジェクトでは、完全でビルドする準備をする必要があります。 みましょうバインド プロジェクトをビルドししてエラーなしで終了するかどうかを確認します。

[バインド プロジェクトをビルドし、エラーがないことを確認](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


この時点では、バインディングのプロジェクトでは、完全でビルドする準備をする必要があります。 みましょうバインド プロジェクトをビルドししてエラーなしで終了するかどうかを確認します。


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>バインディングの使用

上記で作成されたバインドのライブラリ、iOS を使用するサンプル iPhone アプリケーションを作成する手順に従います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Xamarin.iOS プロジェクトを作成**-と呼ばれる新しい Xamarin.iOS プロジェクトに追加**InfColorPickerSample**をソリューションに次のスクリーン ショットに示すようにします。

    ![](walkthrough-images/use01.png "1 つのビュー アプリの追加")

    ![](walkthrough-images/use01a.png "識別子の設定")

1. **バインド プロジェクト参照を追加**-更新プログラム、 **InfColorPickerSample**プロジェクトへの参照があるように、 **InfColorPickerBinding**プロジェクト。

    ![](walkthrough-images/use02.png "バインド プロジェクトへの参照を追加します。")

1. **IPhone のユーザー インターフェイスを作成する**-をダブルクリックして、 **MainStoryboard.storyboard**ファイルを**InfColorPickerSample** iOS デザイナーで編集するプロジェクトです。 追加、**ボタン**ビューにそれを呼び出すと`ChangeColorButton`次に示すように。

    ![](walkthrough-images/use03.png "ビューにボタンの追加")
1. **追加、InfColorPickerView.xib** -InfColorPicker OBJECTIVE-C ライブラリが含まれています、 **.xib**ファイル。 Xamarin.iOS に含めないこの**.xib**バインド プロジェクトでこのサンプル アプリケーションでの実行時エラーが発生します。 この回避策は追加する、 **.xib** Xamarin.iOS プロジェクトへのファイルです。 Xamarin.iOS プロジェクトを右クリックして選択をオン**追加 > ファイルの追加**、追加し、 **.xib**次のスクリーン ショットに示すように、ファイルします。

    ![](walkthrough-images/use04.png "InfColorPickerView.xib を追加します。")

1. メッセージが表示されたら、コピー、 **.xib**プロジェクト ファイルです。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. **Xamarin.iOS プロジェクトを作成**-と呼ばれる新しい Xamarin.iOS プロジェクトに追加**InfColorPickerSample**をソリューションに次のスクリーン ショットに示すようにします。

    ![](walkthrough-images/use01vs.png "Create Xamarin.iOS Project")

1. **バインド プロジェクト参照を追加**-更新プログラム、 **InfColorPickerSample**プロジェクトへの参照があるように、 **InfColorPickerBinding**プロジェクト。

    ![](walkthrough-images/use02vs.png "バインド プロジェクト参照を追加します。")

1. **IPhone のユーザー インターフェイスを作成する**-をダブルクリックして、 **MainStoryboard.storyboard**ファイルを**InfColorPickerSample** iOS デザイナーで編集するプロジェクトです。 追加、**ボタン**ビューにそれを呼び出すと`ChangeColorButton`次に示すように。

    ![](walkthrough-images/use03vs.png "IPhone のユーザー インターフェイスを作成します。")

1. **追加、InfColorPickerView.xib** -InfColorPicker OBJECTIVE-C ライブラリが含まれています、 **.xib**ファイル。 Xamarin.iOS に含めないこの**.xib**バインド プロジェクトでこのサンプル アプリケーションでの実行時エラーが発生します。 この回避策は追加する、 **.xib**から、Xamarin.iOS プロジェクトへのファイル、 **Mac ビルド ホスト**です。 Xamarin.iOS プロジェクトを右クリックして選択を選択して**追加** > **既存アイテム.**、追加し、 **.xib**ファイル。


-----



次に、クイック プロトコル Objective C とどのようにここでそれらを処理バインドと c# コードを見てみましょう。

### <a name="protocols-and-xamarinios"></a>プロトコルおよび Xamarin.iOS

メソッド (またはメッセージ)、OBJECTIVE-C でプロトコルを定義する特定の状況で使用できます。 概念的には、c# でのインターフェイスに非常に似ています。 Objective C プロトコルと c# インターフェイスの 1 つの大きな違いは、プロトコルは省略可能なメソッドのクラスが実装する必要はありませんメソッドであることができます。 Objective C を使用して、@optionalキーワードが省略可能などの方法を示すために使用します。 プロトコルの詳細については、次を参照してください。[イベント、プロトコル、およびデリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)です。

**InfColorPickerController**が次のコード スニペットに示すように、このような 1 つのプロトコル。

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

このプロトコルを使用して**InfColorPickerController**新しい色と、ユーザーが選択したクライアントに通知する、 **InfColorPickerController**が終了します。 目標ペンを使わずでは、次のコード スニペットで示すように、このプロトコルがマップされます。

```csharp
[BaseType(typeof(NSObject))]
[Model]
public partial interface InfColorPickerControllerDelegate {

    [Export ("colorPickerControllerDidFinish:")]
    void ColorPickerControllerDidFinish (InfColorPickerController controller);

    [Export ("colorPickerControllerDidChangeColor:")]
    void ColorPickerControllerDidChangeColor (InfColorPickerController controller);
}

```

Xamarin.iOS でと呼ばれる抽象基本クラスを作成するバインド ライブラリがコンパイルされると、 `InfColorPickerControllerDelegate`、仮想メソッドをこのインターフェイスを実装します。

ストアには、Xamarin.iOS アプリケーションでこのインターフェイスを実装して 2 つの方法があります。

- **厳密なデリゲート**-強力なデリゲートを使用する c# クラスをサブクラス化を作成するには`InfColorPickerControllerDelegate`と適切なメソッドをオーバーライドします。 **InfColorPickerController**クライアントと通信するためにこのクラスのインスタンスを使用します。
- **脆弱なデリゲート**-脆弱なデリゲートは少し異なる手法では、いずれかのクラスのパブリック メソッドを作成する (など`InfColorPickerSampleViewController`) し、そのメソッドを公開して、`InfColorPickerDelegate`プロトコルを使用して、`Export`属性。

強力なデリゲートは、Intellisense、タイプ セーフ、および効率的にカプセル化を提供します。 これらの理由を使用する強力なデリゲートを脆弱なデリゲートではなく。

このチュートリアルでは両方の手法について説明します。 最初に、強力なデリゲートを実装すると、脆弱なデリゲートを実装する方法を説明します。

### <a name="implementing-a-strong-delegate"></a>強力なデリゲートを実装します。

Xamarin.iOS アプリケーションを終了に応答する強力なデリゲートを使用して、`colorPickerControllerDidFinish:`メッセージ。

**サブクラス InfColorPickerControllerDelegate** -というプロジェクトに新しいクラスを追加`ColorSelectedDelegate`です。 次のコードがあるクラスを編集します。

```csharp
using InfColorPickerBinding;
using UIKit;

namespace InfColorPickerSample
{
    public class ColorSelectedDelegate:InfColorPickerControllerDelegate
    {
        readonly UIViewController parent;

        public ColorSelectedDelegate (UIViewController parent)
        {
            this.parent = parent;
        }

        public override void ColorPickerControllerDidFinish (InfColorPickerController controller)
        {
            parent.View.BackgroundColor = controller.ResultColor;
            parent.DismissViewController (false, null);
        }
    }
}
```

Xamarin.iOS と呼ばれる抽象基本クラスを作成することで、OBJECTIVE-C デリゲートをバインド`InfColorPickerControllerDelegate`です。 サブクラスこの型と上書き、`ColorPickerControllerDidFinish`の値にアクセスするメソッド、`ResultColor`プロパティ`InfColorPickerController`です。

**ColorSelectedDelegate のインスタンスを作成**のイベント ハンドラーには、インスタンスは必要があります、`ColorSelectedDelegate`前の手順で作成した型。 クラスの編集`InfColorPickerSampleViewController`クラスに次のインスタンス変数を追加します。

```csharp
ColorSelectedDelegate selector;
```

**ColorSelectedDelegate 変数を初期化**: いることを確認する`selector`は有効なインスタンスのメソッドを更新して`ViewDidLoad`で`ViewController`に次のスニペットを一致します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**HandleTouchUpInsideWithStrongDelegate メソッドを実装して**-次のユーザーが触れたときにイベント ハンドラーを実装**ColorChangeButton**です。 編集`ViewController`、し、次のメソッドを追加します。

```csharp
using InfColorPicker;
...

private void HandleTouchUpInsideWithStrongDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.Delegate = selector;
    picker.PresentModallyOverViewController (this);
}

```

最初のインスタンスを取得して`InfColorPickerController`静的メソッド、およびインスタンス プロパティを使用して、強力なデリゲートの対応することを確認を介して`InfColorPickerController.Delegate`です。 このプロパティは、目標ペンを使わず、ご利用の米国自動的に生成されました。 最後に呼び出して`PresentModallyOverViewController`ビューを表示する`InfColorPickerSampleViewController.xib`されるため、ユーザーは、色を選択ことがあります。

**アプリケーションを実行**- この時点ですべてのコードが終了しました。 アプリケーションを実行する場合の背景色を変更できる必要があります、`InfColorColorPickerSampleView`次のスクリーン ショットに示すようにします。

[![](walkthrough-images/run01.png "アプリケーションの実行")](walkthrough-images/run01.png#lightbox)

おめでとうございます!  この時点で正常に作成が完了し、Xamarin.iOS アプリケーションで使用するため、OBJECTIVE-C ライブラリをバインドします。 次に、弱いデリゲートの使用について説明します。

### <a name="implementing-a-weak-delegate"></a>脆弱なデリゲートを実装します。

特定の委任について、OBJECTIVE-C プロトコルにバインドされているクラスをサブクラス化ではなく Xamarin.iOS することもできますから派生した任意のクラス、プロトコル メソッドを実装する`NSObject`を使用してメソッドを修飾すること、 `ExportAttribute`、し、適切なセレクターを指定します。 この方法を採用する場合に、クラスのインスタンスを割り当てる、`WeakDelegate`プロパティではなく、`Delegate`プロパティです。 脆弱なデリゲートでは、別の継承階層の下位デリゲート クラスを実行する柔軟性を提供します。 実装して、Xamarin.iOS アプリケーションでは弱いデリゲートを使用する方法を確認してみましょう。

**TouchUpInside のイベント ハンドラーを作成する**-に対して新しいイベント ハンドラーを作成しましょう、`TouchUpInside`背景色の変更 ボタンのイベントです。 このハンドラーと同じ役割を入力します。、`HandleTouchUpInsideWithStrongDelegate`ハンドラーは、前のセクションで作成が強力なデリゲートではなく、脆弱なデリゲートを使用します。 クラスの編集`ViewController`、し、次のメソッドを追加します。

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**更新 ViewDidLoad** -を変更する必要があります`ViewDidLoad`先ほど作成したイベント ハンドラーを使用するようにします。 編集`ViewController`変更と`ViewDidLoad`を次のコード スニペットのようにします。


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**処理、colorPickerControllerDidFinish: メッセージ**:、`ViewController`は終了し、iOS がメッセージを送信`colorPickerControllerDidFinish:`を`WeakDelegate`です。 このメッセージを処理できる c# メソッドを作成する必要があります。 これを行うには、c# のメソッドを作成しを装飾、`ExportAttribute`です。 編集`ViewController`クラスに次のメソッドを追加します。

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

アプリケーションを実行します。 、以前と同じですが、強力なデリゲートではなく、脆弱なデリゲートを使用しているとおりに動作する必要があります。 この時点でこのチュートリアルを正常に完了しました。 これで、作成および Xamarin.iOS バインド プロジェクトを使用する方法の理解が必要です。

## <a name="summary"></a>まとめ

この記事の内容をウォーク Xamarin.iOS バインド プロジェクトの作成と処理を実行します。 まずをスタティック ライブラリに既存の Objective C ライブラリをコンパイルする方法を説明します。 Xamarin.iOS バインド プロジェクトを作成する方法と目標ペンを使わずを使用して、Objective C ライブラリの API 定義を生成する方法を説明します。 更新し、パブリックでの使用に適したように生成される API 定義を調整する方法を説明したとします。 Xamarin.iOS バインド プロジェクトが完了した後はに移動強力なデリゲートと脆弱なデリゲートを使用してに重点を置いて、Xamarin.iOS アプリケーションでそのバインドを使用します。

## <a name="related-links"></a>関連リンク

- [バインディングの例 (サンプル)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインディングの種類のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [フレームワーク デザインのガイドライン](http://msdn.microsoft.com/en-us/library/ms229042.aspx)
