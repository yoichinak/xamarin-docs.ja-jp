---
title: 'チュートリアル: iOS Objective-C ライブラリのバインド'
description: この記事では、既存の OBJECTIVE-C ライブラリ、InfColorPicker の Xamarin.iOS バインディングを作成する実践的なチュートリアルを示します。 静的 OBJECTIVE-C ライブラリのコンパイル、バインドすることで、Xamarin.iOS アプリケーションでバインドを使用してなどのトピックについて説明します。
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: c8adc7ec7f717cf0004f79e3b71123d6daeaee86
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978440"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>チュートリアル: iOS Objective-C ライブラリのバインド

_この記事では、既存の OBJECTIVE-C ライブラリ、InfColorPicker の Xamarin.iOS バインディングを作成する実践的なチュートリアルを示します。静的 OBJECTIVE-C ライブラリのコンパイル、バインドすることで、Xamarin.iOS アプリケーションでバインドを使用してなどのトピックについて説明します。_

IOS での作業時に、サード パーティ製の Objective C ライブラリを使用する必要がある場合があります。 これらの状況で、Xamarin.iOS を使用することができます_バインド プロジェクト_を作成する、 [ C#バインド](~/cross-platform/macios/binding/overview.md)ことができます、Xamarin.iOS アプリケーションでライブラリを使用します。

一般に、iOS のエコシステムで、ライブラリを見つけるの 3 種類があります。

* プリコンパイル済みの静的ライブラリのファイルとして`.a`と共にそのヘッダー (.h ファイル) の拡張機能。 たとえば、 [Google の Analytics ライブラリ](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* プリコンパイル済みフレームワーク。 これは、スタティック ライブラリ、ヘッダー、および場合によっては追加のリソースを含むフォルダーで`.framework`拡張機能。 たとえば、 [Google の AdMob ライブラリ](https://developers.google.com/admob/ios/download)します。
* 先ほどのソース コード ファイル。 のみが含まれるライブラリなど`.m`と`.h`Objective C ファイル。

最初と 2 つ目のシナリオが既にありますプリコンパイル済みの CocoaTouch スタティック ライブラリ 3 番目のシナリオでこの記事であるため。 ただし、バインドを作成、自由にバインドしていることを確認するためにライブラリに付属するライセンスを常に確認を開始する前に。

この記事では、オープン ソースを使用してバインド プロジェクトの作成のステップ バイ ステップ チュートリアル[InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective C 例として、プロジェクトのいずれかでこのガイドのすべての情報が使用に合わせて変更することができますが、サード パーティ製の Objective C ライブラリ。 InfColorPicker ライブラリは、ユーザーよりユーザー フレンドリな色の選択を行う、HSB 形式に基づいて色を選択できる再利用可能なビュー コント ローラーを提供します。

[![](walkthrough-images/run01.png "IOS で実行されている InfColorPicker ライブラリの例")](walkthrough-images/run01.png#lightbox)

Xamarin.iOS でこの特定の Objective C API を使用するすべての必要な手順について説明します。

- 最初に、Xcode を使用して、OBJECTIVE-C で静的ライブラリを作成します。
- 次に、この静的ライブラリを Xamarin.iOS をバインドします。
- 目的の油性がいくつか (ただしすべてでは、) を自動的に生成することによって、ワークロードを軽減する方法を次に、表示の Xamarin.iOS バインディングに必要なために必要な API 定義します。
- 最後に、そのバインドが使用する Xamarin.iOS アプリケーションを作成します。

サンプル アプリケーションは InfColorPicker API 間の通信用の強力なデリゲートを使用する方法を紹介し、C#コード。 強力なデリゲートを使用する方法を説明し、脆弱なデリゲートを使用して同じタスクを実行する方法について説明します。

## <a name="requirements"></a>必要条件

この記事では、Xcode と Objective C 言語のいくつかの知識があり、その読み取りが前提としています、 [OBJECTIVE-C のバインド](~/cross-platform/macios/binding/index.md)ドキュメント。 さらに、次に表示される手順を完了するために必要。

-  **Xcode と iOS SDK** -Apple の Xcode と最新の iOS API がインストールされ、開発者のコンピューターに構成する必要があります。
-  **[Xcode コマンド ライン ツール](#Installing_the_Xcode_Command_Line_Tools)** -現在インストールされているバージョンの Xcode (インストールの詳細については以下を参照してください)、Xcode コマンド ライン ツールをインストールする必要があります。
-  **Visual Studio for Mac または Visual Studio** -Visual Studio for Mac または Visual Studio の最新バージョンをインストールして、開発用コンピューターで構成されている必要があります。 Apple Mac は、Xamarin.iOS アプリケーションの開発に必要なされ、Visual Studio を使用する場合に接続する必要があります[Xamarin.iOS ビルド ホスト](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **目標油性の最新バージョン**-目標油性ツールの現在のコピーからダウンロード[ここ](~/cross-platform/macios/binding/objective-sharpie/get-started.md)します。 インストールされている目的油性既にある場合に更新できますが、最新バージョンを使用して、 `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode コマンド ライン ツールをインストールします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


前述のように、使用する Xcode コマンド ライン ツール (具体的には`make`と`lipo`) このチュートリアルでします。 `make`コマンドは、非常に一般的な Unix ユーティリティを使用して実行可能プログラムとライブラリのコンパイルを自動化する、_メイクファイル_プログラムを構築する方法を指定します。 `lipo`コマンドは、マルチ アーキテクチャのファイルを作成するため、OS X コマンド ライン ユーティリティは、複数を組み合わせることは`.a`ファイルをすべてのハードウェア アーキテクチャで使用できる 1 つのファイルにします。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


前述のように、使用する Xcode コマンド ライン ツールで、 **Mac Build Host** (具体的には`make`と`lipo`) このチュートリアルでします。 `make`コマンドは、非常に一般的な Unix ユーティリティを使用して実行可能プログラムとライブラリのコンパイルを自動化する、_メイクファイル_にプログラムをビルドする方法を指定します。 `lipo`コマンドは、マルチ アーキテクチャのファイルを作成するため、OS X コマンド ライン ユーティリティは、複数を組み合わせることは`.a`ファイルをすべてのハードウェア アーキテクチャで使用できる 1 つのファイルにします。


-----

に従って、Apple の[Xcode FAQ では、コマンドラインからビルドする](https://developer.apple.com/library/ios/technotes/tn2339/_index.html)ドキュメントについては、OS X 10.9 以降、**ダウンロード**Xcode のウィンドウ**設定**ダイアログされませんダウンロードのコマンド ライン ツールをサポートしなくなりました。

次のメソッドのいずれかを使用して、ツールをインストールする必要があります。

- **Xcode のインストール**- Xcode をインストールするときに、すべてのコマンド ライン ツールを使用してバンドルします。 OS X 10.9 で shim (にインストールされている`/usr/bin`) に含まれる任意のツールをマップできます`/usr/bin`を Xcode 内で対応するツール。 たとえば、`xcrun`コマンドで、検索またはコマンドラインから Xcode 内の任意のツールを実行することができます。
- **ターミナル アプリケーション**-ターミナルのアプリケーションから実行して、コマンド ライン ツールをインストールすることができます、`xcode-select --install`コマンド。
    - ターミナルを起動します。
    - 型`xcode-select --install`キーを押します**Enter**など。

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - コマンド ライン ツールをインストールする 求め、**インストール**ボタンをクリックします。 [![](walkthrough-images/xcode01.png "コマンド ライン ツールをインストールします。")](walkthrough-images/xcode01.png#lightbox)

    - ツールがダウンロードされ、Apple のサーバーからインストールします。 [![](walkthrough-images/xcode02.png "ツールのダウンロード")](walkthrough-images/xcode02.png#lightbox)

- **Apple の開発者向けダウンロード**-コマンド ライン ツールのパッケージが使用可能な[Apple の開発者向けダウンロード](https://developer.apple.com/downloads/index.action)web ページ。 Apple ID を使用してログインを検索し、コマンド ライン ツールをダウンロードします。[![](walkthrough-images/xcode03.png "コマンド ライン ツールを見つける")](walkthrough-images/xcode03.png#lightbox)

インストールされているコマンド ライン ツール、チュートリアルを続行する準備ができました。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルで、次の手順について説明します。

- **[スタティック ライブラリを作成](#Creating_A_Static_Library)** -この手順では、スタティック ライブラリを作成、 **InfColorPicker** Objective C コード。 スタティック ライブラリが必要があります、`.a`ファイル拡張子、およびライブラリ プロジェクトの .NET アセンブリに埋め込まれます。
- **[Xamarin.iOS のバインド プロジェクトを作成する](#Create_a_Xamarin.iOS_Binding_Project)** -スタティック ライブラリを取得したらを Xamarin.iOS バインド プロジェクトの作成に使用します。 バインド プロジェクトは、先ほど作成した、スタティック ライブラリおよびの形式でメタデータC#Objective C API の使用方法について説明するコードです。 このメタデータは、API 定義とも呼ばれます。 使用して **[目標油性](#Using_Objective_Sharpie)** にご協力に API 定義を作成します。
- **[API 定義を正規化](#Normalize_the_API_Definitions)** - 目標油性のうえ、良い仕事をするは、すべてを行うことはできません。 使用する API 定義をする必要があるいくつかの変更について説明します。
- **[バインド ライブラリを使用して](#Using_the_Binding)** -最後に、新しく作成したバインド プロジェクトを使用する方法について説明 Xamarin.iOS アプリケーションを作成します。

どのような手順が必要に理解できたためは、このチュートリアルの残りの部分に移りましょう。

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>スタティック ライブラリを作成します。

コードを Github で InfColorPicker の検査: 場合

[![](walkthrough-images/image02.png "Github で InfColorPicker のコードを調べる")](walkthrough-images/image02.png#lightbox)

プロジェクトの次の 3 つのディレクトリことがわかります。

- **InfColorPicker** -このディレクトリには、プロジェクトの OBJECTIVE-C コードが含まれています。
- **PickerSamplePad** -このディレクトリには、iPad、サンプルのプロジェクトが含まれています。
- **PickerSamplePhone** -このディレクトリにサンプルの iPhone プロジェクトが含まれています。

InfColorPicker プロジェクトをダウンロードしましょう[GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip)し、選択したディレクトリに解凍します。 Xcode のターゲットを開く`PickerSamplePhone`プロジェクト、Xcode のナビゲーターで、次のプロジェクト構造がわかります。

[![](walkthrough-images/image03.png "ナビゲーターの Xcode プロジェクトの構造")](walkthrough-images/image03.png#lightbox)

このプロジェクトは、各サンプル プロジェクトに直接 (赤いボックス) で InfColorPicker ソース コードを追加することで、コードの再利用を実現します。 サンプル プロジェクトのコードは、青のボックス内でです。 この特定のプロジェクトをスタティック ライブラリとの提供はありません、ため、必要がスタティック ライブラリをコンパイルする Xcode プロジェクトを作成します。

最初の手順は、スタティック ライブラリに InfoColorPicker のソース コードを追加するためです。 それではこれを実現するために、次の操作を行います。

1. Xcode を起動します。
2. **ファイル**メニューの **新規** > **プロジェクト.**:

    [![](walkthrough-images/image04.png "新しいプロジェクトを開始")](walkthrough-images/image04.png#lightbox)
3. 選択**フレームワークとライブラリ**、 **Cocoa Touch のスタティック ライブラリ**テンプレートをクリックして、**次**ボタン。

    [![](walkthrough-images/image05.png "Cocoa Touch のスタティック ライブラリ テンプレートを選択します。")](walkthrough-images/image05.png#lightbox)

4. 入力`InfColorPicker`の**プロジェクト名** をクリックし、**次**ボタン。

    [![](walkthrough-images/image06.png "プロジェクト名の InfColorPicker を入力します。")](walkthrough-images/image06.png#lightbox)
5. プロジェクトを保存し、をクリックする場所を選択、 **OK**ボタンをクリックします。
6. InfColorPicker プロジェクトからソースをスタティック ライブラリ プロジェクトに追加する必要があります。 **InfColorPicker.h**ファイルが既に (既定)、スタティック ライブラリに存在、Xcode は上書きすることを許可しません。 **Finder**、GitHub から解凍した元のプロジェクトで InfColorPicker ソース コードに移動し、すべて InfColorPicker ファイルのコピーおよび新しいスタティック ライブラリ プロジェクトに貼り付けること。

    [![](walkthrough-images/image12.png "すべての InfColorPicker ファイル コピーします。")](walkthrough-images/image12.png#lightbox)

7. Xcode に戻り、右クリックし、 **InfColorPicker**フォルダーと選択 **「InfColorPicker…」にファイルを追加**:

    [![](walkthrough-images/image08.png "ファイルを追加します。")](walkthrough-images/image08.png#lightbox)

8. ファイルの追加 ダイアログ ボックスからコピーした InfColorPicker のソース コード ファイルに移動します。 をすべて選択 をクリック、**追加**ボタンをクリックします。

    [![](walkthrough-images/image09.png "すべてを選択し、[追加] ボタンをクリックします。")](walkthrough-images/image09.png#lightbox)

9. ソース コードは、このプロジェクトにコピーされます。

    [![](walkthrough-images/image10.png "ソース コードは、プロジェクトにコピーします。")](walkthrough-images/image10.png#lightbox)

10. Xcode プロジェクト ナビゲーターに、選択、 **InfColorPicker.m**ファイルを開き (このファイルは使用されませんが、このライブラリの書き込み方法) のための最後の 2 つの行をコメントします。

    [![](walkthrough-images/image14.png "InfColorPicker.m ファイルの編集")](walkthrough-images/image14.png#lightbox)

11. これで、ライブラリで必要な任意のフレームワークがないか確認する必要があります。 Readme ファイル、または提供するサンプル プロジェクトのいずれかを開くことでは、この情報を確認できます。 この例では`Foundation.framework`、 `UIKit.framework`、および`CoreGraphics.framework`に追加してみましょう。

12. 選択、 **InfColorPicker ターゲット > ビルド フェーズ**を展開し、 **Link Binary With Libraries**セクション。

    [![](walkthrough-images/image16b.png "Link Binary With Libraries セクションを展開します。")](walkthrough-images/image16b.png#lightbox)

13. 使用して、 **+** ボタン上に示した必要なフレームのフレームワークを追加することができます ダイアログを開きます。

    [![](walkthrough-images/image16c.png "必要なフレーム フレームワークが上記を追加します。")](walkthrough-images/image16c.png#lightbox)

14. **Link Binary With Libraries**セクションで次の図のようになります。

    [![](walkthrough-images/image16d.png "Link Binary With Libraries セクション")](walkthrough-images/image16d.png#lightbox)

この時点で、閉じるが終わってそうではありません。 スタティック ライブラリが作成されたらが構築するため、Fat バイナリを含むすべての iOS シミュレーターと iOS デバイスの両方の必要なアーキテクチャを作成する必要があります。

### <a name="creating-a-fat-binary"></a>Fat バイナリを作成します。

すべての iOS デバイスでは、ARM アーキテクチャを搭載した時間の経過と共に開発したプロセッサを搭載します。 各新しいアーキテクチャは、下位互換性を保ちながら新しい手順とその他の改善を追加します。 IOS デバイスである、armv6、armv7、armv7s arm64 命令セットが[armv6 にアンドリュー](~/ios/deploy-test/compiling-for-different-devices.md)します。 IOS シミュレーターでは、ARM によってが入っていないと、istead は、x86、x86_64 電源シミュレーター。 各命令でライブラリを提供する必要がありますが私たちにとっては意味を設定します。

Fat ライブラリは`.a`サポートされているすべてのアーキテクチャを含むファイル。

Fat バイナリの作成は、次の 3 ステップ プロセスです。

- スタティック ライブラリの ARM 7 & ARM64 バージョンをコンパイルします。
- スタティック ライブラリの x86 および x84_64 バージョンをコンパイルします。
- 使用して、`lipo`コマンド ライン ツールを 2 つの静的ライブラリを 1 つに結合します。

これら 3 つの手順は、比較的簡単な OBJECTIVE-C ライブラリの更新プログラムを受信するとき、またはバグの修正が必要な場合、後で繰り返す必要があります。 次の手順を自動化する場合は、将来のメンテナンスとバインドの iOS プロジェクトのサポートによって簡略化されます。

シェル スクリプトでは、このようなタスクの自動化に使用できる多くのツールがある[rake](http://rake.rubyforge.org/)、 [xbuild](https://www.mono-project.com/docs/tools+libraries/tools/xbuild/)、および[ように](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html)します。 ここには、Xcode コマンド ライン ツールがインストールされている、ときにして、このチュートリアルで使用されるビルド システムができるのでもインストールします。 ここでは、**メイクファイル**iOS デバイスと、任意のライブラリ用のシミュレーターで動作するマルチ アーキテクチャの共有ライブラリの作成に使用できます。

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

入力、**メイクファイル**任意のプレーン テキスト エディターでのコマンドのセクションを更新し、 **YOUR、プロジェクトの名前**プロジェクトの名前に置き換えます。 いることを確認する重要な手順については、内のタブが維持されていること、上記の手順を貼り付けています。

名前のファイルを保存**メイクファイル**上で作成した InfColorPicker Xcode スタティック ライブラリと同じ場所に。

[![](walkthrough-images/lib00.png "メイクファイルの名前を持つファイルを保存します。")](walkthrough-images/lib00.png#lightbox)

Mac でターミナル アプリケーションを開き、Makefile の場所に移動します。 型`make`、ターミナルにキーを押します。 **」と入力**と**メイクファイル**が実行されます。

[![](walkthrough-images/lib01.png "メイクファイルのサンプル出力")](walkthrough-images/lib01.png#lightbox)

作成を実行すると、大量のによってスクロール テキストが表示されます。 すべて正常である場合、単語が表示されます**ビルドが成功しました**と`libInfColorPicker-armv7.a`、`libInfColorPicker-i386.a`と`libInfColorPickerSDK.a`と同じ場所にファイルをコピーするは、**メイクファイル**:

[![](walkthrough-images/lib02.png "メイクファイルで生成された libInfColorPicker armv7.a、libInfColorPicker i386.a および libInfColorPickerSDK.a ファイル")](walkthrough-images/lib02.png#lightbox)

Fat バイナリ内で、次のコマンドを使用して、アーキテクチャを確認できます。

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

これには、次が表示されます。

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

Xcode と、Xcode コマンド ライン ツールを使用してスタティック ライブラリを作成して、iOS バインドの最初の手順が終了しましたこの時点では、`make`と`lipo`します。 次の手順に移動して使用して、、**目標油性**私たちの API バインドの作成を自動化します。

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>プロジェクトをバインドする Xamarin.iOS を作成します。

使用する前に**目標油性**API 定義を格納するための Xamarin.iOS バインディング プロジェクトの作成に必要なバインド プロセスを自動化する (を使用する**目標油性**にご協力くださいビルド) を作成し、C#をバインドします。

それでは、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for mac の起動
1. **ファイル**メニューの **新規** > **ソリューション.**:

    ![](walkthrough-images/bind01.png "新しいソリューションの開始")

1. 新しいソリューション ダイアログ ボックスで次のように選択します**ライブラリ** > **iOS プロジェクトのバインド**:。

    ![](walkthrough-images/bind02.png "IOS プロジェクトのバインドを選択します。")

1. をクリックして、**次**ボタンをクリックします。

1. "InfColorPickerBinding"を入力として、**プロジェクト名** をクリックし、**作成**ソリューションを作成するボタン。

    ![](walkthrough-images/bind02a.png "プロジェクト名として InfColorPickerBinding を入力します。")

ソリューションが作成され、2 つの既定のファイルが含まれます。

![](walkthrough-images/bind03.png "ソリューション エクスプ ローラーでソリューションの構造")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. Visual Studio を起動します。

1. **ファイル**メニューの **新規** > **プロジェクト.**:

    ![新しいプロジェクトを開始](walkthrough-images/bind01vs.png "新しいプロジェクトを開始")

1. 新しいプロジェクト ダイアログ ボックスで次のように選択します**Visual C# > iPhone と iPad > iOS バインド ライブラリ (Xamarin)**:。

    [![IOS バインド ライブラリを選択します。](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. "InfColorPickerBinding"を入力、**名前** をクリックし、 **ok**ソリューションを作成するボタン。

ソリューションが作成され、2 つの既定のファイルが含まれます。

![](walkthrough-images/bind03vs.png "ソリューション エクスプ ローラーでソリューションの構造")

-----

- **ApiDefinition.cs** -このファイルの Objective C API をラップする方法を定義するコントラクトを含むC#します。
- **Structs.cs** - このファイルは、構造を保持する、または列挙値によってインターフェイスおよびデリゲートが必要です。

チュートリアルの後半でこれら 2 つのファイルを操作します。 まず、InfColorPicker ライブラリ バインド プロジェクトを追加する必要があります。

### <a name="including-the-static-library-in-the-binding-project"></a>バインド プロジェクトでスタティック ライブラリを含む

Fat バイナリを上記で作成したライブラリを追加する必要がありますが、基本バインド プロジェクトが検出された、 **InfColorPicker**ライブラリ。

ライブラリを追加するこれらの手順に従います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 右クリックし、**ネイティブ参照**フォルダーをクリックし、Solution Pad で**ネイティブ参照の追加**:

    ![](walkthrough-images/bind04a.png "ネイティブ参照を追加します。")

1. Fat バイナリ以前に行ったに移動します (`libInfColorPickerSDK.a`) キーを押すと、**オープン**ボタン。

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK.a ファイルを選択します。")
1. ファイルは、プロジェクトに含めるは。

    ![](walkthrough-images/bind04.png "ファイルを含む")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. コピー、`libInfColorPickerSDK.a`から、 **Mac Build Host**バインド プロジェクトに貼り付けます。

1. プロジェクトを右クリックし、選択**追加 > 既存の項目.**:

    ![](walkthrough-images/bind04vs.png "既存のファイルを追加します。")

1. 移動し、`libInfColorPickerSDK.a`キーを押すと、**追加**ボタン。

    ![](walkthrough-images/bind05vs.png "Adding libInfColorPickerSDK.a")

1. ファイルは、プロジェクトに含めるは。

-----

ときに、 **.a** Xamarin.iOS は自動的に設定をプロジェクトにファイルが追加された、**ビルド アクション**するファイルの**ObjcBindingNativeLibrary**、特殊なファイルを作成呼ばれる`libInfColorPickerSDK.linkwith.cs`します。


このファイルが含まれています、`LinkWith`ハンドルばかりのスタティック ライブラリを追加する方法を Xamarin.iOS に通知する属性です。 このファイルの内容は次のコード スニペットに示します。

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith`属性は、プロジェクトといくつかの重要なリンカー フラグ用のスタティック ライブラリを識別します。


InfColorPicker プロジェクトの API 定義を作成するを行う必要があります。 次のことです。 このチュートリアルの目的で、使用目的油性ファイルを生成する**ApiDefinition.cs**します。

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>目標油性を使用します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


目標油性はコマンド ライン ツール (Xamarin によって提供される) にサード パーティ製の Objective C ライブラリにバインドするために必要な定義を作成する際のC#します。 このセクションで目的の油性の作成に使用初期**ApiDefinition.cs** InfColorPicker プロジェクト。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


目標油性はコマンド ライン ツール (Xamarin によって提供される) にサード パーティ製の Objective C ライブラリにバインドするために必要な定義を作成する際のC#します。 このセクションで使用します油性の目的上、 **Mac Build Host**初期を作成する**ApiDefinition.cs** InfColorPicker プロジェクト。


-----

最初に、この詳細として目的油性インストーラー ファイルをダウンロードしましょう[ガイド](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing)します。 インストーラーを実行し、目標油性を開発用コンピューターにインストールするインストール ウィザードからすべての画面の指示に従います。

目標油性を正常に取得したら、インストールされているターミナル アプリを起動してすべてのバインドを支援するために提供するツールのヘルプを表示するには、次のコマンドを入力します。

```bash
sharpie -help
```

上記のコマンドを実行すると、次の出力が生成されます。

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

このチュートリアルの目的で使用する、次の目的油性ツール。

- **xcode** - このツールは私たちに、現在の Xcode インストールと iOS デバイスと Mac Api にインストールされているのバージョンに関する情報を提供します。 使用するこの情報は後で、バインドが生成されたとき。
- **バインド**-このツールを使用して、解析、 **.h** 、最初に InfColorPicker プロジェクト内のファイル**ApiDefinition.cs**と**StructsAndEnums.cs**ファイル。

特定の目標油性ツールのヘルプを表示するには、ツールの名前を入力し、`-help`オプション。 たとえば、`sharpie xcode -help`次の出力を返します。

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

バインディング プロセスを始めることができます、前に、ターミナルに次のコマンドを入力して、現在インストールされている各種 Sdk についての情報を取得する必要があります`sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

上記からわかりますがあること、 `iphoneos9.3` SDK は、コンピューターにインストールされています。 この記事を使用する準備が整いました InfColorPicker プロジェクトの解析`.h`、最初にファイルを**ApiDefinition.cs**と`StructsAndEnums.cs`InfColorPicker プロジェクト。

ターミナル アプリでは、次のコマンドを入力します。

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

場所`[full-path-to-project]`ディレクトリへの完全パスは、場所、 **InfColorPicker** Xcode プロジェクト ファイルが、コンピューター上にありで説明したように、[iphone os] は、iOS SDK がインストールされている`sharpie xcode -sdks`コマンド。 この例ではした渡されることに注意してください **\*.h**をパラメーターとして含む*すべて*- このディレクトリ内のヘッダー ファイルを通常必要がありますいないしませんが、代わりに注意深くお読み、。最上位を検索するヘッダー ファイル **.h**ファイルをすべて、その他の関連するファイルを参照し、目標油性に渡すだけです。

次[出力](walkthrough-images/os05.png)ターミナルが生成されます。

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

および**InfColorPicker.enums.cs**と**InfColorPicker.cs**ディレクトリにファイルが作成されます。

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs と InfColorPicker.cs ファイル")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


前に作成したバインド プロジェクトでは、これらのファイルを開きます。 内容をコピー、 **InfColorPicker.cs**貼り付けますファイルを開き、 **ApiDefinition.cs**ファイルを置き換えて、既存`namespace ...`コード ブロックの内容を**InfColorPicker.cs**ファイル (まま、`using`ステートメントをそのまま)。

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate ファイル")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


前に作成したバインド プロジェクトでは、これらのファイルを開きます。 内容をコピー、 **InfColorPicker.cs**ファイル (から、 **Mac Build Host**) に貼り付けます、 **ApiDefinition.cs**ファイルを置き換えて、既存`namespace ...`コード ブロックの内容を**InfColorPicker.cs**ファイル (まま、`using`ステートメントをそのまま)。


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API 定義を正規化します。

目標油性がある場合、問題の変換が`Delegates`での定義を変更する必要があるため、`InfColorPickerControllerDelegate`インターフェイスし、置換、`[Protocol, Model]`を次の行。

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
次のように定義します。

[![](walkthrough-images/os11.png "定義")](walkthrough-images/os11.png#lightbox)

内容で同じ処理を行う次に、`InfColorPicker.enums.cs`ファイルをコピーして貼り付けることで、`StructsAndEnums.cs`ままファイル、`using`ステートメントをそのまま残ります。

[![](walkthrough-images/os09.png "内容、StructsAndEnums.cs ファイル ")](walkthrough-images/os09.png#lightbox)

目標油性がによるバインディング注釈を付けることもあります`[Verify]`属性。 これらの属性を示す目的油性が (これが提供する、バインド宣言の上にコメントで) 元の C/Objective C 宣言とのバインドを比較することによって、正しいことをでしたを確認する必要があります。 バインドを確認した後は、検証属性を削除する必要があります。 詳細についてを参照してください、[確認](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)ガイド。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


この時点では、完全でビルドできるように、バインド プロジェクトがあります。 みましょうバインド プロジェクトをビルドし、エラーなしでして終了するかどうかを確認します。

[バインド プロジェクトをビルドし、エラーがないかどうかを確認](walkthrough-images/os12.png)


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


この時点では、完全でビルドできるように、バインド プロジェクトがあります。 みましょうバインド プロジェクトをビルドし、エラーなしでして終了するかどうかを確認します。


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>バインディングの使用

上記のバインドのライブラリで作成した iOS の使用を iPhone サンプル アプリケーションを作成する次の手順に従います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Xamarin.iOS プロジェクトを作成する**-と呼ばれる新しい Xamarin.iOS プロジェクトに追加**InfColorPickerSample**を次のスクリーン ショットに示すように、ソリューションに。

    ![](walkthrough-images/use01.png "単一ビュー アプリを追加します。")

    ![](walkthrough-images/use01a.png "識別子の設定")

1. **バインド プロジェクトへの参照を追加**-更新プログラム、 **InfColorPickerSample**プロジェクトへの参照があるように、 **InfColorPickerBinding**プロジェクト。

    ![](walkthrough-images/use02.png "バインド プロジェクトへの参照を追加します。")

1. **IPhone のユーザー インターフェイスを作成**-をダブルクリック、 **MainStoryboard.storyboard**ファイル、 **InfColorPickerSample** iOS Designer で編集するプロジェクト。 追加、**ボタン**ビューに、呼び出す`ChangeColorButton`次のように。

    ![](walkthrough-images/use03.png "ビューにボタンの追加")

1. **追加、InfColorPickerView.xib** -InfColorPicker OBJECTIVE-C ライブラリが含まれています、 **.xib**ファイル。 Xamarin.iOS はこれは含まれません **.xib**バインド プロジェクトでサンプル アプリケーションでは実行時エラーが発生します。 この回避を追加するには、 **.xib** Xamarin.iOS プロジェクトへのファイル。 Xamarin.iOS プロジェクトを右クリックして選びますを選択します。**追加 > ファイルの追加**、追加、 **.xib**ファイルの次のスクリーン ショットに示すように。

    ![](walkthrough-images/use04.png "追加、InfColorPickerView.xib")

1. メッセージが表示されたら、コピー、 **.xib**ファイルをプロジェクトにします。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Xamarin.iOS プロジェクトを作成する**-という名前の新しい Xamarin.iOS プロジェクトを追加**InfColorPickerSample**を使用して、**単一ビュー アプリ**テンプレート。

    [![iOS アプリ (Xamarin) プロジェクト](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![テンプレートを選択します。](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **バインド プロジェクトへの参照を追加**-更新プログラム、 **InfColorPickerSample**プロジェクトへの参照があるように、 **InfColorPickerBinding**プロジェクト。

    ![](walkthrough-images/use02vs.png "バインド プロジェクトへの参照を追加します。")

1. **IPhone のユーザー インターフェイスを作成**-をダブルクリック、 **MainStoryboard.storyboard**ファイル、 **InfColorPickerSample** iOS Designer で編集するプロジェクト。 追加、**ボタン**ビューに、呼び出す`ChangeColorButton`次のように。

    ![](walkthrough-images/use03vs.png "IPhone のユーザー インターフェイスを作成します。")

1. **追加、InfColorPickerView.xib** -InfColorPicker OBJECTIVE-C ライブラリが含まれています、 **.xib**ファイル。 Xamarin.iOS はこれは含まれません **.xib**バインド プロジェクトでサンプル アプリケーションでは実行時エラーが発生します。 この回避を追加するには、 **.xib**から Xamarin.iOS プロジェクトにファイル、 **Mac Build Host**します。 Xamarin.iOS プロジェクトを右クリックして選びます選択**追加** > **既存の項目.** を追加し、 **.xib**ファイル。

-----

次に、OBJECTIVE-C およびバインドの処理にどのプロトコルを簡単に見てをみましょうとC#コード。

### <a name="protocols-and-xamarinios"></a>プロトコルと Xamarin.iOS

メソッド (またはメッセージ)、OBJECTIVE-C でプロトコルを定義する特定の状況で使用できます。 内のインターフェイスとよく似ている概念的には、C#します。 1 つの主な違い、OBJECTIVE-C プロトコルとC#インターフェイスは、プロトコルは省略可能なメソッドのクラスが実装する必要はありませんメソッドであることができます。 Objective C を使用して、@optionalが省略可能な方法を示すキーワードを使用します。 プロトコルの詳細については、次を参照してください。[イベント、プロトコル、デリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)します。

**InfColorPickerController**が次のコード スニペットに示すように、このような 1 つのプロトコル。

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

このプロトコルを使用して**InfColorPickerController**をクライアントに通知されますが、新しい色と、ユーザーの取得、 **InfColorPickerController**が終了します。 目標油性では、次のコード スニペットに示すように、このプロトコルが割り当てられます。

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

Xamarin.iOS がという抽象基本クラスを作成するバインド ライブラリがコンパイルされると、 `InfColorPickerControllerDelegate`、仮想メソッドでこのインターフェイスを実装します。

Xamarin.iOS アプリケーションでこのインターフェイスを実装できる 2 つの方法はあります。

- **強力な委任**-作成するには強力なデリゲートを使用して、C#クラスをサブクラスとして持つ`InfColorPickerControllerDelegate`し、適切なメソッドをオーバーライドします。 **InfColorPickerController**そのクライアントとの通信にこのクラスのインスタンスを使用します。
- **弱い委任**-弱い委任は少し異なる手法をいくつかのクラスのパブリック メソッドを作成する必要があります (など`InfColorPickerSampleViewController`) し、そのメソッドを公開して、`InfColorPickerDelegate`プロトコルを使用して、`Export`属性。

強力なデリゲートでは、Intellisense、タイプ セーフ、および優れたカプセル化を提供します。 これらの理由には行うことができます、脆弱なデリゲートではなく強力なデリゲート使用する必要があります。

このチュートリアルでは両方の手法について説明します。 最初に、強力なデリゲートを実装すると、弱い委任を実装する方法を説明しします。

### <a name="implementing-a-strong-delegate"></a>強力なデリゲートを実装します。

対応する強力なデリゲートを使用して Xamarin.iOS アプリケーションを終了、`colorPickerControllerDidFinish:`メッセージ。

**サブクラス InfColorPickerControllerDelegate** -という名前のプロジェクトに新しいクラスを追加`ColorSelectedDelegate`します。 次のコードがあるクラスを編集します。

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

Xamarin.iOS では OBJECTIVE-C でデリゲートをバインドと呼ばれる抽象基本クラスを作成して`InfColorPickerControllerDelegate`します。 サブクラスこの型とオーバーライド、`ColorPickerControllerDidFinish`の値にアクセスするメソッド、`ResultColor`プロパティの`InfColorPickerController`します。

**ColorSelectedDelegate のインスタンスを作成**-イベント ハンドラーには、インスタンスは必要があります、`ColorSelectedDelegate`前の手順で作成した型。 クラスの編集`InfColorPickerSampleViewController`クラスに次のインスタンス変数を追加します。

```csharp
ColorSelectedDelegate selector;
```

**ColorSelectedDelegate 変数を初期化**ことを確認する -`selector`は有効なインスタンスでメソッドを更新`ViewDidLoad`で`ViewController`次のスニペットを一致するように。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**HandleTouchUpInsideWithStrongDelegate メソッドを実装する**-次のユーザーが触れたときにイベント ハンドラーを実装**ColorChangeButton**します。 編集`ViewController`、次のメソッドを追加します。

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

最初のインスタンスを取得した`InfColorPickerController`静的メソッドとインスタンスのプロパティを使用して、強力なデリゲートの対応するを通じて`InfColorPickerController.Delegate`します。 このプロパティは、目的の油性によってを自動的に生成されました。 最後に呼び出して`PresentModallyOverViewController`ビューを表示する`InfColorPickerSampleViewController.xib`ユーザーが色を選択できるようにします。

**アプリケーションを実行**- この時点で、コードをすべて終了しました。 アプリケーションを実行する場合の背景色を変更できる必要があります、`InfColorColorPickerSampleView`次のスクリーン ショットに示すようにします。

[![](walkthrough-images/run01.png "アプリケーションの実行")](walkthrough-images/run01.png#lightbox)

おめでとうございます! この時点で正常に作成し、Xamarin.iOS アプリケーションで使用するため、OBJECTIVE-C ライブラリのバインドします。 次に、脆弱なデリゲートの使用について説明します。

### <a name="implementing-a-weak-delegate"></a>脆弱なデリゲートを実装します。

OBJECTIVE-C プロトコルにバインドして、特定のデリゲート クラスをサブクラス化ではなく Xamarin.iOS こともできますから派生する任意のクラスにプロトコル メソッドを実装する`NSObject`、修飾することで、メソッド、 `ExportAttribute`、し、適切なセレクターを指定します。 この方法で実行するときに、クラスのインスタンスを割り当てる、`WeakDelegate`プロパティではなく、`Delegate`プロパティ。 弱い委任では、別の継承階層の下位デリゲート クラスを取得する柔軟性を提供します。 実装し、Xamarin.iOS アプリケーションでは、弱い委任を使用する方法を見てみましょう。

**TouchUpInside のイベント ハンドラー作成**-新しいイベント ハンドラーを作成しましょう、`TouchUpInside`背景色の変更ボタンのイベント。 このハンドラーと同じロールを入力、`HandleTouchUpInsideWithStrongDelegate`ハンドラー、前のセクションで作成したが、強力なデリゲートではなく、弱い委任を使用します。 クラスの編集`ViewController`、次のメソッドを追加します。

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**ViewDidLoad を更新**-変更する必要があります`ViewDidLoad`先ほど作成したイベント ハンドラーを使用するようにします。 編集`ViewController`変更と`ViewDidLoad`に次のコード スニペットのようになります。


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**ハンドル、colorPickerControllerDidFinish:メッセージ**:、`ViewController`が完了したら、iOS は、メッセージを送信は`colorPickerControllerDidFinish:`を`WeakDelegate`します。 作成する必要があります、C#このメッセージを処理できるメソッド。 これを行うには、作成、C#メソッドでそれを装飾して、`ExportAttribute`します。 編集`ViewController`クラスに次のメソッドを追加します。

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

アプリケーションを実行します。 前が強力なデリゲートではなく、弱い委任を使用しているとおりに動作する必要があります。 この時点で、このチュートリアルを正常に完了しました。 作成し、プロジェクトを Xamarin.iOS バインディングを使用する方法を理解できました。

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS のバインド プロジェクトの作成とプロセスを説明しました。 まず、スタティック ライブラリに既存の OBJECTIVE-C ライブラリをコンパイルする方法を説明します。 バインドの Xamarin.iOS プロジェクトを作成する方法および目的油性を使用して OBJECTIVE-C ライブラリの API 定義を生成する方法を説明します。 更新およびパブリックでの使用に適したように生成された API 定義を調整する方法を説明しました。 Xamarin.iOS のバインド プロジェクトが完了した後に移行した強力なデリゲートと弱い委任の使用に重点を置いて、Xamarin.iOS アプリケーションでは、そのバインドを使用します。

## <a name="related-links"></a>関連リンク

- [バインディングの例 (サンプル)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインドの種類のリファレンス ガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [フレームワーク デザインのガイドライン](https://msdn.microsoft.com/library/ms229042.aspx)
