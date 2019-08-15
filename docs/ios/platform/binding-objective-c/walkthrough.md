---
title: 'チュートリアル: iOS Objective-C ライブラリのバインド'
description: この記事では、既存の目的 C ライブラリである InfColorPicker の作成に関する実践的なチュートリアルを提供します。 ここでは、静的な目的 C ライブラリのコンパイル、バインド、および Xamarin. iOS アプリケーションでのバインディングの使用などのトピックについて説明します。
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: b73f00eb704d80da6b0bab3a34f08f2d1cb70a16
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646176"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>チュートリアル: iOS Objective-C ライブラリのバインド

_この記事では、既存の目的 C ライブラリである InfColorPicker の作成に関する実践的なチュートリアルを提供します。ここでは、静的な目的 C ライブラリのコンパイル、バインド、および Xamarin. iOS アプリケーションでのバインディングの使用などのトピックについて説明します。_

IOS で作業している場合、サードパーティの目標 C ライブラリを使用することが必要になる場合があります。 このような状況では、xamarin. ios_バインドプロジェクト_を使用して、xamarin. ios アプリケーションでライブラリを使用するための[ C#バインド](~/cross-platform/macios/binding/overview.md)を作成できます。

一般に、iOS エコシステムでは、次の3つのフレーバーでライブラリを見つけることができます。

* 拡張子をヘッダー (.h ファイル) `.a`と共にプリコンパイル済みスタティックライブラリファイルとして使用します。 たとえば、 [Google の分析ライブラリ](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* プリコンパイル済みフレームワークとして。 これは、スタティックライブラリ、ヘッダー、および場合によって`.framework`は追加のリソースを含むフォルダーです。 たとえば、 [Google の Admob by ライブラリ](https://developers.google.com/admob/ios/download)です。
* ソースコードファイルとしてのみ。 たとえば、と`.h`目標 C ファイルのみ`.m`を含むライブラリです。

1番目と2番目のシナリオでは、プリコンパイル済み CocoaTouch スタティックライブラリが既に存在するため、この記事では3番目のシナリオに焦点を当てます。 バインドの作成を開始する前に、ライブラリで提供されているライセンスを必ず確認して、それを確実にバインドできることを確認してください。

この記事では、オープンソースの[Infcolorpicker](https://github.com/InfinitApps/InfColorPicker)目標 c プロジェクトを使用してバインドプロジェクトを作成する手順について説明します。ただし、このガイドに記載されているすべての情報は、サードパーティの目的 c ライブラリで使用するために適合させることができます. InfColorPicker ライブラリには、ユーザーが HSB 表現に基づいて色を選択できるようにする再利用可能なビューコントローラーが用意されており、色の選択がよりわかりやすくなっています。

[![](walkthrough-images/run01.png "IOS で実行されている InfColorPicker ライブラリの例")](walkthrough-images/run01.png#lightbox)

Xamarin でこの特定の目標 C API を使用するために必要なすべての手順について説明します。 iOS:

- まず、Xcode を使用して、C 用のスタティックライブラリを作成します。
- 次に、このスタティックライブラリを Xamarin. iOS にバインドします。
- 次に、マジックペンで必要な API 定義の一部 (すべてではありません) を自動的に生成することにより、目標がワークロードを削減する方法を示します。
- 最後に、バインドを使用する Xamarin iOS アプリケーションを作成します。

このサンプルアプリケーションでは、InfColorPicker API とC#コード間の通信に強力なデリゲートを使用する方法を示します。 厳密なデリゲートの使用方法について学習した後は、弱いデリゲートを使用して同じタスクを実行する方法について説明します。

## <a name="requirements"></a>必要条件

この記事では、Xcode と C 言語に関する知識があり、「[Objective-C のバインド](~/cross-platform/macios/binding/index.md)」を読んだことがあることを前提としています。 また、表示される手順を完了するには、次のものが必要です。

-  **Xcode と IOS SDK** -Apple の Xcode と最新の ios API は、開発者のコンピューターにインストールして構成する必要があります。
-  **[Xcode コマンドラインツール](#Installing_the_Xcode_Command_Line_Tools)** -現在インストールされているバージョンの Xcode には、Xcode コマンドラインツールがインストールされている必要があります (インストールの詳細については、以下を参照してください)。
-  **Visual Studio for Mac または Visual studio** -Visual Studio for Mac または visual studio の最新バージョンが開発用コンピューターにインストールされ、構成されている必要があります。 Xamarin iOS アプリケーションを開発するには Apple Mac が必要です。また、Visual Studio を使用する場合は、 [xamarin. ios ビルドホスト](~/ios/get-started/installation/windows/connecting-to-mac/index.md)に接続する必要があります。
-  **目標マジックペンの最新バージョン**-[ここ](~/cross-platform/macios/binding/objective-sharpie/get-started.md)からダウンロードした目標マジックペンツールの現在のコピー。 目標マジックペンが既にインストールされている場合は、を使用して最新バージョンに更新できます。`sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode コマンドラインツールのインストール

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


前述のように、このチュートリアルでは、Xcode コマンドラインツール`make` ( `lipo`具体的にはと) を使用します。 コマンドは、プログラムの構築方法を指定する_メイクファイル_を使用して、実行可能プログラムとライブラリのコンパイルを自動化する、非常に一般的な Unix ユーティリティです。 `make` コマンド`lipo`は、マルチアーキテクチャファイルを作成するための OS X コマンドラインユーティリティです。これに`.a`より、複数のファイルが、すべてのハードウェアアーキテクチャで使用できる1つのファイルに結合されます。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


前述のように、このチュートリアルでは、 **Mac ビルドホスト**(具体的`make`にはと`lipo`) で Xcode コマンドラインツールを使用します。 コマンドは、メイクファイルを使用してプログラムのビルド方法を指定することにより、実行可能プログラムおよびライブラリのコンパイルを自動化する、非常に一般的な Unix ユーティリティです。 `make` コマンド`lipo`は、マルチアーキテクチャファイルを作成するための OS X コマンドラインユーティリティです。これに`.a`より、複数のファイルが、すべてのハードウェアアーキテクチャで使用できる1つのファイルに結合されます。


-----

Xcode の FAQ ドキュメントを使用した、[コマンドラインから](https://developer.apple.com/library/ios/technotes/tn2339/_index.html)の Apple の構築によれば、OS X 10.9 以降では、Xcode の **[基本設定]** ダイアログボックスの **[ダウンロード]** ウィンドウで、コマンドラインツールのダウンロードがサポートされなくなりました。

ツールをインストールするには、次のいずれかの方法を使用する必要があります。

- **Install Xcode** -Xcode をインストールすると、すべてのコマンドラインツールにバンドルされます。 OS X 10.9 shim (に`/usr/bin`インストールされている) では、に`/usr/bin`含まれる任意のツールを Xcode 内の対応するツールにマップできます。 たとえば`xcrun` 、コマンドを使用すると、コマンドラインから Xcode 内の任意のツールを検索または実行できます。
- **ターミナルアプリケーション**-ターミナルアプリケーションから、 `xcode-select --install`コマンドを実行してコマンドラインツールをインストールできます。
    - ターミナルアプリケーションを起動します。
    - 「 `xcode-select --install` **」** と入力し、enter キーを押します。次に例を示します。

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - コマンドラインツールをインストールするよう求めるメッセージが表示されたら、 **[インストール]** ボタンをクリックします。 [![](walkthrough-images/xcode01.png "コマンドラインツールのインストール")](walkthrough-images/xcode01.png#lightbox)

    - ツールは、Apple のサーバーからダウンロードしてインストールされます。 [![](walkthrough-images/xcode02.png "ツールをダウンロードする")](walkthrough-images/xcode02.png#lightbox)

- **Apple 開発者向けダウンロード**-コマンドラインツールパッケージは、 [apple 開発者向けのダウンロード](https://developer.apple.com/downloads/index.action)web ページで入手できます。 Apple ID でログインし、コマンドラインツールを検索してダウンロードします。[![](walkthrough-images/xcode03.png "コマンドラインツールの検索")](walkthrough-images/xcode03.png#lightbox)

コマンドラインツールがインストールされているので、チュートリアルを続行する準備ができました。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルでは、次の手順について説明します。

- **[スタティックライブラリを作成する](#Creating_A_Static_Library)** -この手順では、 **infcolorpicker**目標 C コードのスタティックライブラリを作成します。 スタティックライブラリは`.a`ファイル拡張子を持ち、ライブラリプロジェクトの .net アセンブリに埋め込まれます。
- **[Xamarin. Ios バインドプロジェクトを作成](#Create_a_Xamarin.iOS_Binding_Project)** する-スタティックライブラリがある場合は、それを使用して Xamarin. ios バインドプロジェクトを作成します。 バインドプロジェクトは、作成したばかりのスタティックライブラリと、目的の C API C#を使用する方法を説明するコード形式のメタデータで構成されます。 このメタデータは、一般に API 定義と呼ばれます。 使用して **[目標油性](#Using_Objective_Sharpie)** にご協力に API 定義を作成します。
- **[API の定義を正規化](#Normalize_the_API_Definitions)** する-目標マジックペンは、お客様に役立つ優れたジョブを実行しますが、すべてを行うことはできません。 ここでは、API 定義を使用する前に行う必要がある変更について説明します。
- **[バインドライブラリを使用](#Using_the_Binding)** します。最後に、新しく作成されたバインドプロジェクトの使用方法を示す Xamarin. iOS アプリケーションを作成します。

ここでは、どのような手順が関係しているかを理解したところで、このチュートリアルの残りの部分に移りましょう。

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>スタティックライブラリの作成

Github で InfColorPicker のコードを調べる場合は、次のようにします。

[![](walkthrough-images/image02.png "Github での InfColorPicker のコードの検査")](walkthrough-images/image02.png#lightbox)

プロジェクトには、次の3つのディレクトリが表示されます。

- **Infcolorpicker** -このディレクトリには、プロジェクトの目的の C コードが格納されています。
- **PickerSamplePad** -このディレクトリには、サンプルの iPad プロジェクトが含まれています。
- **PickerSamplePhone** -このディレクトリには、サンプルの iPhone プロジェクトが含まれています。

[GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip)から InfColorPicker プロジェクトをダウンロードし、選択したディレクトリに解凍してみましょう。 プロジェクトの`PickerSamplePhone` Xcode ターゲットを開くと、Xcode Navigator に次のプロジェクト構造が表示されます。

[![](walkthrough-images/image03.png "Xcode ナビゲーターのプロジェクト構造")](walkthrough-images/image03.png#lightbox)

このプロジェクトは、各サンプルプロジェクトに InfColorPicker ソースコード (赤いボックス) を直接追加することによって、コードの再利用を実現します。 サンプルプロジェクトのコードは、青いボックス内にあります。 このプロジェクトは、スタティックライブラリを提供しないため、Xcode プロジェクトを作成してスタティックライブラリをコンパイルする必要があります。

最初の手順として、ヒントのソースコードをスタティックライブラリに追加します。 これを実現するには、次の手順を実行します。

1. Xcode を起動します。
2. **[ファイル]** メニューの [**新しい** > **プロジェクト**] をクリックします。

    [![](walkthrough-images/image04.png "新しいプロジェクトの開始")](walkthrough-images/image04.png#lightbox)
3. **[Framework & Library]** 、 **[Cocoa タッチスタティックライブラリ]** テンプレートの順に選択し、 **[次へ]** ボタンをクリックします。

    [![](walkthrough-images/image05.png "Cocoa タッチスタティックライブラリテンプレートを選択します")](walkthrough-images/image05.png#lightbox)

4. `InfColorPicker` **プロジェクト名**として「」と入力し、 **[次へ]** ボタンをクリックします。

    [![](walkthrough-images/image06.png "プロジェクト名として「InfColorPicker」と入力します。")](walkthrough-images/image06.png#lightbox)
5. プロジェクトを保存する場所を選択し、 **[OK]** ボタンをクリックします。
6. 次に、ソースを InfColorPicker プロジェクトからスタティックライブラリプロジェクトに追加する必要があります。 (既定では) **Xcode ファイルは**、スタティックライブラリに既に存在するため、上書きすることはできません。 **Finder**から、GitHub から解凍した元のプロジェクトの infcolorpicker ソースコードに移動し、すべての infcolorpicker ファイルをコピーして、新しいスタティックライブラリプロジェクトに貼り付けます。

    [![](walkthrough-images/image12.png "すべての InfColorPicker ファイルをコピーする")](walkthrough-images/image12.png#lightbox)

7. Xcode に戻り、 **[Infcolorpicker]** フォルダーを右クリックして、 **[Add files To The "infcolorpicker..."]** を選択します。

    [![](walkthrough-images/image08.png "ファイルの追加")](walkthrough-images/image08.png#lightbox)

8. ファイルの追加 ダイアログボックスで、先ほどコピーした InfColorPicker ソースコードファイルに移動し、すべてを選択して **追加** ボタンをクリックします。

    [![](walkthrough-images/image09.png "[すべて] を選択し、[追加] ボタンをクリックします。")](walkthrough-images/image09.png#lightbox)

9. ソースコードがプロジェクトにコピーされます。

    [![](walkthrough-images/image10.png "ソースコードがプロジェクトにコピーされます")](walkthrough-images/image10.png#lightbox)

10. Xcode プロジェクトナビゲーターから、 **Infcolorpicker. m**ファイルを選択し、最後の2行をコメントアウトします (このライブラリの作成方法により、このファイルは使用されません)。

    [![](walkthrough-images/image14.png "InfColorPicker. m ファイルを編集しています")](walkthrough-images/image14.png#lightbox)

11. 次に、ライブラリに必要なフレームワークがあるかどうかを確認する必要があります。 この情報は、README に記載されているか、提供されているサンプルプロジェクトの1つを開いて確認できます。 この例で`Foundation.framework`は`UIKit.framework`、、 `CoreGraphics.framework` 、を使用して、それらを追加してみましょう。

12. [ **Infcolorpicker] ターゲット > ビルドフェーズ**を選択し、 **[ライブラリを使用したバイナリのリンク]** セクションを展開します。

    [![](walkthrough-images/image16b.png "[ライブラリとバイナリのリンク] セクションを展開します。")](walkthrough-images/image16b.png#lightbox)

13. **+** ボタンを使用してダイアログを開き、上記の必要なフレームフレームワークを追加できます。

    [![](walkthrough-images/image16c.png "上に一覧表示されている必要なフレームフレームワークを追加します。")](walkthrough-images/image16c.png#lightbox)

14. [**ライブラリを含むバイナリをリンク**する] セクションは、次の図のようになります。

    [![](walkthrough-images/image16d.png "リンクバイナリとライブラリセクション")](walkthrough-images/image16d.png#lightbox)

この時点で終了しますが、まだ完了していません。 スタティックライブラリが作成されましたが、iOS デバイスと iOS シミュレーターの両方に必要なすべてのアーキテクチャを含む Fat バイナリを作成するには、このライブラリを構築する必要があります。

### <a name="creating-a-fat-binary"></a>Fat バイナリの作成

すべての iOS デバイスには、時間の経過と共に開発された ARM アーキテクチャを搭載したプロセッサが搭載されています。 新しいアーキテクチャでは、旧バージョンとの互換性を維持しながら、新しい命令やその他の機能強化が追加されました。 iOS デバイスには armv6、armv7、armv7s、arm64 命令セットがありますが、 [armv6 は使用されません](~/ios/deploy-test/compiling-for-different-devices.md)。 IOS シミュレーターは ARM を搭載していないので、代わりに x86 と x86_64 の電源が供給されます。 これは、各命令セットに対してライブラリを提供する必要があることを意味します。

Fat ライブラリは、 `.a`サポートされているすべてのアーキテクチャを含むファイルです。

Fat バイナリの作成は、次の3つの手順で行います。

- ARM 7 & ARM64 バージョンのスタティックライブラリをコンパイルします。
- X86 および x84_64 バージョンのスタティックライブラリをコンパイルします。
- `lipo`コマンドラインツールを使用して、2つのスタティックライブラリを1つに結合します。

これら3つの手順は比較的簡単ですが、目的の C ライブラリが更新プログラムを受け取ったとき、またはバグ修正が必要な場合は、今後も繰り返すことが必要になる場合があります。 これらの手順を自動化することにした場合は、iOS バインドプロジェクトの将来のメンテナンスとサポートが簡略化されます。

このようなタスクを自動化するために使用できるツールは多数あります。シェルスクリプト、 [rake](http://rake.rubyforge.org/)、 [xbuild](https://www.mono-project.com/docs/tools+libraries/tools/xbuild/)、 [make](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html)などです。 Xcode コマンドラインツールがインストール`make`されている場合は、もインストールされるため、このチュートリアルで使用されるビルドシステムです。 次に示すのは、iOS デバイスで動作するマルチアーキテクチャ共有ライブラリと、任意のライブラリのシミュレーターを作成するために使用できる**メイクファイル**です。

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

任意のプレーンテキストエディターで**Makefile**コマンドを入力し、プロジェクト**名**を使用してセクションをプロジェクトの名前に更新します。 指示内のタブを保持したまま、上記の手順を正確に貼り付けることも重要です。

と**いう名前の**ファイルを、上記で作成した InfColorPicker Xcode スタティックライブラリと同じ場所に保存します。

[![](walkthrough-images/lib00.png "メイクファイルという名前でファイルを保存します。")](walkthrough-images/lib00.png#lightbox)

Mac でターミナルアプリケーションを開き、メイクファイルの場所に移動します。 ターミナル`make`に「」と入力し、 **enter キーを**押すと、**メイクファイル**が実行されます。

[![](walkthrough-images/lib01.png "メイクファイルの出力の例")](walkthrough-images/lib01.png#lightbox)

Make を実行すると、によって多数のテキストスクロールが表示されます。 すべてが正常に動作した場合は、**ビルドが成功**し`libInfColorPicker-armv7.a` `libInfColorPicker-i386.a` 、と`libInfColorPickerSDK.a`いう語が表示され、ファイルは**メイク**ファイルと同じ場所にコピーされます。

[![](walkthrough-images/lib02.png "LibInfColorPicker-armv7、libInfColorPicker-i386. a と libInfColorPickerSDK (メイクファイルによって生成されたファイル)")](walkthrough-images/lib02.png#lightbox)

次のコマンドを使用して、Fat バイナリ内のアーキテクチャを確認できます。

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

次のように表示されます。

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

この時点で、Xcode および Xcode コマンドラインツール`make`と`lipo`を使用してスタティックライブラリを作成することによって、iOS バインディングの最初の手順を完了しました。 次の手順に進み、**マジックペン**を使用して API バインドの作成を自動化しましょう。

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Xamarin. iOS バインドプロジェクトを作成する

**マジックペン**を使用してバインドプロセスを自動化するには、まず、API 定義を格納するための Xamarin. IOS バインドプロジェクトを作成する必要があります (ここでは、ビルドに役立つ**マジックペン**を使用C#します)。また、バインドを作成する必要があります。

次の手順を実行してみましょう。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac を開始します。
1. **[ファイル]** メニューの [**新しい** > **ソリューション...** ] を選択します。

    ![](walkthrough-images/bind01.png "新しいソリューションの開始")

1. [新しいソリューション] ダイアログボックスで、[**ライブラリ** > ] **[iOS バインドプロジェクト]** を選択します。

    ![](walkthrough-images/bind02.png "IOS バインドプロジェクトの選択")

1. **[次へ]** ボタンをクリックします。

1. **プロジェクト名**として「InfColorPickerBinding」と入力し、 **[作成]** ボタンをクリックしてソリューションを作成します。

    ![](walkthrough-images/bind02a.png "プロジェクト名として「InfColorPickerBinding」と入力します。")

ソリューションが作成され、次の2つの既定のファイルが含まれます。

![](walkthrough-images/bind03.png "ソリューションエクスプローラー内のソリューション構造")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. Visual Studio を起動します。

1. **[ファイル]** メニューの [**新しい** > **プロジェクト**] をクリックします。

    ![新しいプロジェクトの開始](walkthrough-images/bind01vs.png "新しいプロジェクトの開始")

1. [新しいプロジェクト] ダイアログボックスで、 **[ C# Visual > iPhone & IPad > iOS バインドライブラリ (Xamarin)** ] を選択します。

    [![IOS バインドライブラリを選択します](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. **名前**として「InfColorPickerBinding」と入力し、 **[OK]** ボタンをクリックしてソリューションを作成します。

ソリューションが作成され、次の2つの既定のファイルが含まれます。

![](walkthrough-images/bind03vs.png "ソリューションエクスプローラー内のソリューション構造")

-----

- **ApiDefinition.cs** -このファイルには、目標 C API がどのようにC#ラップされるかを定義するコントラクトが含まれます。
- **Structs.cs** -このファイルには、インターフェイスとデリゲートで必要とされる構造体または列挙値がすべて格納されます。

これら2つのファイルについては、このチュートリアルの後半で説明します。 まず、InfColorPicker ライブラリをバインドプロジェクトに追加する必要があります。

### <a name="including-the-static-library-in-the-binding-project"></a>バインドプロジェクトにスタティックライブラリを含める

基本バインディングプロジェクトの準備ができたので、前の手順で作成した、 **Infcolorpicker**ライブラリ用の Fat バイナリライブラリを追加する必要があります。

ライブラリを追加するには、次の手順に従います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Solution Pad で**ネイティブ参照**フォルダーを右クリックし、 **[ネイティブ参照の追加]** を選択します。

    ![](walkthrough-images/bind04a.png "ネイティブ参照の追加")

1. 前の手順で作成した Fat バイナリ`libInfColorPickerSDK.a`() に移動し、 **[開く]** ボタンを押します。

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK ファイルを選択します。")
1. このファイルはプロジェクトに含まれます。

    ![](walkthrough-images/bind04.png "ファイルを含める")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Mac ビルドホスト**からをコピーし、バインドプロジェクトに貼り付けます。`libInfColorPickerSDK.a`

1. プロジェクトを右クリックし、 **[既存の項目の追加 >]** を選択します。

    ![](walkthrough-images/bind04vs.png "既存のファイルの追加")

1. に`libInfColorPickerSDK.a`移動し、 **[追加]** ボタンを押します。

    ![](walkthrough-images/bind05vs.png "LibInfColorPickerSDK の追加")

1. ファイルはプロジェクトに含まれます。

-----

**ファイルが**プロジェクトに追加されると、Xamarin によってファイルの**ビルドアクション**が自動的に**objcbindingてライブラリ**に設定され、という名前`libInfColorPickerSDK.linkwith.cs`の特別なファイルが作成されます。


このファイルには`LinkWith` 、追加したスタティックライブラリを処理する方法を示す属性が含まれています。 このファイルの内容を次のコードスニペットに示します。

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

属性`LinkWith`は、プロジェクトのスタティックライブラリと、いくつかの重要なリンカーフラグを識別します。


次に、InfColorPicker プロジェクトの API 定義を作成する必要があります。 このチュートリアルでは、目的のマジックペンを使用して、 **ApiDefinition.cs**ファイルを生成します。

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>目標マジックペンの使用

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


目標マジックペンは、(Xamarin によって提供される) コマンドラインツールです。このツールを使用すると、サードパーティの目的 C C#ライブラリをにバインドするために必要な定義を作成できます。 このセクションでは、目的のマジックペンを使用して、InfColorPicker プロジェクトの初期**ApiDefinition.cs**を作成します。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


目標マジックペンは、(Xamarin によって提供される) コマンドラインツールです。このツールを使用すると、サードパーティの目的 C C#ライブラリをにバインドするために必要な定義を作成できます。 このセクションでは、 **Mac ビルドホスト**で目標マジックペンを使用して、InfColorPicker プロジェクトの初期**ApiDefinition.cs**を作成します。


-----

まず、この[ガイド](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing)で説明されているように、目的のマジックペンインストーラファイルをダウンロードしてみましょう。 インストーラーを実行し、インストールウィザードの画面に表示されるすべてのプロンプトに従って、開発用コンピューターに目的のマジックペンをインストールします。

目標マジックペンが正常にインストールされたら、ターミナルアプリを起動し、次のコマンドを入力して、バインドに役立つすべてのツールに関するヘルプを表示します。

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

このチュートリアルでは、次の目的のマジックペンツールを使用します。

- **xcode** -このツールは、現在の xcode インストール、およびインストールされている IOS および Mac api のバージョンに関する情報を提供します。 この情報は、後でバインドを生成するときに使用します。
- **bind** -このツールを使用して、InfColorPicker プロジェクトの **.h**ファイルを最初の**ApiDefinition.cs**ファイルと**StructsAndEnums.cs**ファイルに解析します。

特定の目的のマジックペンツールに関するヘルプを表示するには、ツールの名前`-help`とオプションを入力します。 たとえば、は`sharpie xcode -help`次の出力を返します。

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

バインドプロセスを開始する前に、次のコマンドをターミナル`sharpie xcode -sdks`に入力して、現在インストールされている sdk に関する情報を取得する必要があります。

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

この記事では、お客様のコンピューターに`iphoneos9.3` SDK がインストールされていることを確認できます。 この情報を使用して、infcolorpicker プロジェクト`.h`ファイルを最初の**ApiDefinition.cs**および`StructsAndEnums.cs` infcolorpicker プロジェクトに解析する準備ができました。

ターミナルアプリで次のコマンドを入力します。

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

ここ`[full-path-to-project]`で、は、コンピューター上の**infcolorpicker** Xcode プロジェクトファイルが配置されているディレクトリへの完全パスです。また、[iphone-os] は、 `sharpie xcode -sdks`コマンドに示されているように、インストールされている iOS SDK です。 この例では、パラメーターとして **\*.h**を渡しています。これには、このディレクトリ内の*すべて*のヘッダーファイルが含まれます。通常、この操作は行わないでください。代わりに、ヘッダーファイルを注意深く読み取って、最上位のを検索し**ます。** 関連する他のすべてのファイルを参照し、そのファイルを目的のマジックペンに渡すだけです。

ターミナルで次の[出力](walkthrough-images/os05.png)が生成されます。

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

**InfColorPicker.enums.cs**ファイルと**InfColorPicker.cs**ファイルがディレクトリに作成されます。

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs ファイルと InfColorPicker.cs ファイル")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


上記で作成したバインドプロジェクトで、これらの両方のファイルを開きます。 **InfColorPicker.cs**ファイルの内容をコピーし、 **ApiDefinition.cs**ファイルに貼り付けて、 `namespace ...`既存のコードブロックを**InfColorPicker.cs**ファイルの内容に置き換えます (ステートメントを`using`そのまま使用します)。そのままです):

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate ファイル")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


上記で作成したバインドプロジェクトで、これらの両方のファイルを開きます。 ( **Mac ビルドホスト**から) **InfColorPicker.cs**ファイルの内容をコピーし、 **ApiDefinition.cs**ファイルに貼り付けて、既存`namespace ...`のコードブロックを**InfColorPicker.cs**ファイルの内容に置き換えます (ステートメントを`using`そのまま残します)。


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API 定義を正規化する

目標マジックペンでは、問題の`Delegates`変換が発生する場合があります。そのため`InfColorPickerControllerDelegate` 、インターフェイスの定義`[Protocol, Model]`を変更し、行を次のコードに置き換える必要があります。

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
定義は次のようになります。

[![](walkthrough-images/os11.png "定義")](walkthrough-images/os11.png#lightbox)

次に、 `InfColorPicker.enums.cs`ファイルの内容と同じ処理を行い、ステートメントを`using`コピーして`StructsAndEnums.cs`ファイルに貼り付けます。

[![](walkthrough-images/os09.png "StructsAndEnums.cs ファイルの内容")](walkthrough-images/os09.png#lightbox)

また、マジックペンが属性を使用してバインディングに注釈`[Verify]`を付けていることがわかります。 これらの属性は、バインディングを元の C/マジックペン宣言と比較することによって、目的が正しいことを確認する必要があることを示しています (これは、バインドされた宣言の上にあるコメントで指定されます)。 バインドを確認したら、verify 属性を削除する必要があります。 詳細については、「[検証](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)ガイド」を参照してください。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


この時点で、バインドプロジェクトは完成し、ビルドする準備ができています。 バインドプロジェクトをビルドし、エラーが発生していないことを確認してみましょう。

[バインドプロジェクトをビルドし、エラーがないことを確認します。](walkthrough-images/os12.png)


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


この時点で、バインドプロジェクトは完成し、ビルドする準備ができています。 バインドプロジェクトをビルドし、エラーが発生していないことを確認してみましょう。


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>バインディングの使用

上記で作成した iOS バインドライブラリを使用するサンプル iPhone アプリケーションを作成するには、次の手順に従います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Xamarin. Ios プロジェクトの作成**-次のスクリーンショットに示すように、ソリューションに**InfColorPickerSample**という名前の新しい Xamarin iOS プロジェクトを追加します。

    ![](walkthrough-images/use01.png "単一ビューアプリの追加")

    ![](walkthrough-images/use01a.png "識別子の設定")

1. **バインドプロジェクトへの参照の追加**- **InfColorPickerBinding**プロジェクトへの参照が含まれるように**InfColorPickerSample**プロジェクトを更新します。

    ![](walkthrough-images/use02.png "バインドプロジェクトへの参照の追加")

1. **IPhone ユーザーインターフェイスを作成**します。 **InfColorPickerSample**プロジェクトの**mainstoryboard.storyboard ファイル**ファイルをダブルクリックして、iOS Designer で編集します。 次に示すように、ビューにボタン`ChangeColorButton`を追加し、それを呼び出します。

    ![](walkthrough-images/use03.png "ビューへのボタンの追加")

1. **InfColorPickerView を追加**します。 xib ライブラリには、 **xib**ファイルが含まれています。 Xamarin では、この**xib**をバインドプロジェクトに含めることはできません。このため、サンプルアプリケーションでは実行時エラーが発生します。 これを回避するには、 **xib**ファイルを Xamarin. iOS プロジェクトに追加します。 Xib プロジェクトを選択し、右クリックして **[> 追加]** を選択し、次のスクリーンショットに示すようにファイルを追加します。

    ![](walkthrough-images/use04.png "InfColorPickerView を追加します。 xib")

1. プロンプトが表示されたら、 **xib**ファイルをプロジェクトにコピーします。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Xamarin. Ios プロジェクトの作成**-**単一ビューアプリ**テンプレートを使用して、 **InfColorPickerSample**という名前の新しい Xamarin iOS プロジェクトを追加します。

    [![iOS アプリ (Xamarin) プロジェクト](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![テンプレートの選択](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **バインドプロジェクトへの参照の追加**- **InfColorPickerBinding**プロジェクトへの参照が含まれるように**InfColorPickerSample**プロジェクトを更新します。

    ![](walkthrough-images/use02vs.png "バインドプロジェクトへの参照の追加")

1. **IPhone ユーザーインターフェイスを作成**します。 **InfColorPickerSample**プロジェクトの**mainstoryboard.storyboard ファイル**ファイルをダブルクリックして、iOS Designer で編集します。 次に示すように、ビューにボタン`ChangeColorButton`を追加し、それを呼び出します。

    ![](walkthrough-images/use03vs.png "IPhone ユーザーインターフェイスを作成する")

1. **InfColorPickerView を追加**します。 xib ライブラリには、 **xib**ファイルが含まれています。 Xamarin では、この**xib**をバインドプロジェクトに含めることはできません。このため、サンプルアプリケーションでは実行時エラーが発生します。 これを回避するには、 **Mac ビルドホスト**から**Xib**ファイルを Xamarin. iOS プロジェクトに追加します。 Xamarin プロジェクトを選択し、右クリックして [既存項目の**追加** >  **...** ] を選択し、 **xib**ファイルを追加します。

-----

次に、目的 C のプロトコルと、それをバインドおよびC#コードで処理する方法を簡単に見ていきましょう。

### <a name="protocols-and-xamarinios"></a>プロトコルと Xamarin. iOS

目的 C では、プロトコルによって、特定の状況で使用できるメソッド (またはメッセージ) が定義されます。 概念的には、のC#インターフェイスによく似ています。 目的の C プロトコルとC#インターフェイスの大きな違いの1つは、クラスが実装する必要のないオプションのメソッドメソッドをプロトコルが持つことができることです。 目的-C では@optional 、省略可能なメソッドを示すためにキーワードが使用されます。 プロトコルの詳細については[、「イベント、プロトコル、およびデリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)」を参照してください。

**InfColorPickerController**には、次のコードスニペットに示すようなプロトコルが1つ含まれています。

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

このプロトコルは、ユーザーが新しい色を選択したこと、および**InfColorPickerController**が終了したことをクライアントに通知するために、 **InfColorPickerController**によって使用されます。 目標マジックペンは、次のコードスニペットに示すように、このプロトコルをマップしています。

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

バインドライブラリをコンパイルすると、という`InfColorPickerControllerDelegate`抽象基本クラスが作成されます。このクラスは、仮想メソッドを使用してこのインターフェイスを実装します。

Xamarin. iOS アプリケーションでこのインターフェイスを実装するには、次の2つの方法があります。

- **強いデリゲート**-強力なデリゲートを使用するにC#は、適切`InfColorPickerControllerDelegate`なメソッドをサブクラス化してオーバーライドするクラスを作成する必要があります。 **InfColorPickerController**は、このクラスのインスタンスを使用して、クライアントと通信します。
- **弱いデリゲート**-弱いデリゲートは、一部のクラス ( `InfColorPickerSampleViewController`など) でパブリックメソッドを作成し、 `Export`属性を使用して`InfColorPickerDelegate`そのメソッドをプロトコルに公開するという、若干異なる手法です。

強いデリゲートは、Intellisense、タイプセーフ、およびより適切なカプセル化を提供します。 これらの理由から、弱いデリゲートではなく、可能な場合は強力なデリゲートを使用する必要があります。

このチュートリアルでは、次の2つの手法について説明します。最初に強力なデリゲートを実装し、次に弱いデリゲートを実装する方法について説明します。

### <a name="implementing-a-strong-delegate"></a>強いデリゲートの実装

強力なデリゲートを使用して`colorPickerControllerDidFinish:`メッセージに応答することで、Xamarin iOS アプリケーションを完成させます。

**サブクラス InfColorPickerControllerDelegate** -新しいクラスをという名前`ColorSelectedDelegate`のプロジェクトに追加します。 クラスを編集して、次のコードを記述します。

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

Xamarin は、という`InfColorPickerControllerDelegate`抽象基本クラスを作成することによって、目的の C デリゲートをバインドします。 この型をサブクラス化`ColorPickerControllerDidFinish`し、メソッドをオーバーライドして`ResultColor` 、の`InfColorPickerController`プロパティの値にアクセスします。

**Colorselecteddelegate のインスタンスを作成**します。イベントハンドラーには、前の`ColorSelectedDelegate`手順で作成した型のインスタンスが必要です。 クラス`InfColorPickerSampleViewController`を編集し、次のインスタンス変数をクラスに追加します。

```csharp
ColorSelectedDelegate selector;
```

**Colorselecteddelegate 変数の初期化**-が有効な`selector`インスタンスであることを確認するに`ViewDidLoad`は`ViewController` 、次のスニペットに一致するようにのメソッドを更新します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**HandleTouchUpInsideWithStrongDelegate メソッドを実装**します。次に、ユーザーが**colorchangebutton**に触れるときのイベントハンドラーを実装します。 を`ViewController`編集し、次のメソッドを追加します。

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

まず、静的メソッドを使用`InfColorPickerController`してのインスタンスを取得し、そのインスタンスに対して、プロパティ`InfColorPickerController.Delegate`を介して、強力なデリゲートを認識させます。 このプロパティは、目標マジックペンによって自動的に生成されました。 最後に、 `PresentModallyOverViewController`を呼び出して、 `InfColorPickerSampleViewController.xib`ユーザーが色を選択できるようにビューを表示します。

**アプリケーションを実行し**ます。この時点で、すべてのコードが完成しています。 アプリケーションを実行すると、次のスクリーンショットに示すように、 `InfColorColorPickerSampleView`の背景色を変更できるようになります。

[![](walkthrough-images/run01.png "アプリケーションの実行")](walkthrough-images/run01.png#lightbox)

おめでとうございます! この時点で、Xamarin iOS アプリケーションで使用するために、目的の C ライブラリを作成してバインドしました。 次に、弱いデリゲートの使用について説明します。

### <a name="implementing-a-weak-delegate"></a>弱いデリゲートの実装

Xamarin は、特定のデリゲートに対して目的の C プロトコルにバインドされたクラスをサブクラス化するのではなく、から`NSObject`派生する任意のクラスのプロトコルメソッドを実装し、メソッド`ExportAttribute`をで装飾し、適切なセレクターを指定します。 この方法を使用する場合は、 `WeakDelegate` `Delegate`プロパティではなく、クラスのインスタンスをプロパティに割り当てます。 弱いデリゲートを使用すると、デリゲートクラスを別の継承階層にする柔軟性が得られます。 ここでは、Xamarin. iOS アプリケーションで弱いデリゲートを実装して使用する方法について説明します。

**TouchUpInside のイベントハンドラーを作成**する-[背景色の変更] ボタン`TouchUpInside`のイベントの新しいイベントハンドラーを作成します。 このハンドラーは、前のセクションで作成`HandleTouchUpInsideWithStrongDelegate`したハンドラーと同じロールを設定しますが、厳密なデリゲートではなく弱いデリゲートを使用します。 クラス`ViewController`を編集し、次のメソッドを追加します。

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**Update ViewDidLoad** -先ほど作成し`ViewDidLoad`たイベントハンドラーを使用するように変更する必要があります。 次`ViewController`のコード`ViewDidLoad`スニペットのように、を編集して変更します。


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**ColorPickerControllerDidFinish を処理します。メッセージ** - `ViewController`が終了すると、iOS からにメッセージ`colorPickerControllerDidFinish:`が`WeakDelegate`送信されます。 このメッセージを処理できるC#メソッドを作成する必要があります。 これを行うには、メソッドC#を作成し、を使用`ExportAttribute`して装飾します。 を`ViewController`編集し、次のメソッドをクラスに追加します。

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

アプリケーションを実行します。 これは、以前とまったく同じように動作しますが、厳密なデリゲートではなく弱いデリゲートを使用しています。 この時点で、このチュートリアルは正常に完了しています。 これで、Xamarin の iOS バインドプロジェクトを作成および使用する方法を理解できるようになりました。

## <a name="summary"></a>まとめ

この記事では、Xamarin の iOS バインドプロジェクトを作成して使用するプロセスについて説明します。 まず、既存の目的 C ライブラリをスタティックライブラリにコンパイルする方法について説明しました。 次に、Xamarin の iOS バインドプロジェクトを作成する方法と、目標マジックペンを使用して目的の C ライブラリの API 定義を生成する方法について説明します。 ここでは、生成された API 定義を更新および調整して、パブリックの使用に適したものにする方法について説明しました。 Xamarin のバインドプロジェクトが終了した後、Xamarin. iOS アプリケーションでそのバインドを使用するようになりました。強力なデリゲートと弱いデリゲートの使用に重点を置いています。

## <a name="related-links"></a>関連リンク

- [Binding の例 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/infcolorpicker)
- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインディングの種類のリファレンスガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [フレームワーク デザインのガイドライン](https://msdn.microsoft.com/library/ms229042.aspx)
