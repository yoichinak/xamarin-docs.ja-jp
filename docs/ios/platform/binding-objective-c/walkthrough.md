---
title: 'チュートリアル: iOS の目的 C ライブラリのバインド'
description: この記事では、既存の目的 C ライブラリである InfColorPicker の作成に関する実践的なチュートリアルを提供します。 ここでは、静的な目的 C ライブラリのコンパイル、バインド、および Xamarin. iOS アプリケーションでのバインディングの使用などのトピックについて説明します。
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 342558908c5f42941c9e6e7ef5c7f75d8e0fa9d4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937983"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>チュートリアル: iOS の目的 C ライブラリのバインド

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタム バインディングの使用を調査しています。 今後の開発作業の発展のために、この[**アンケート**](https://www.surveymonkey.com/r/KKBHNLT)にご回答ください。

_この記事では、既存の目的 C ライブラリである InfColorPicker の作成に関する実践的なチュートリアルを提供します。ここでは、静的な目的 C ライブラリのコンパイル、バインド、および Xamarin. iOS アプリケーションでのバインディングの使用などのトピックについて説明します。_

IOS で作業している場合、サードパーティの目標 C ライブラリを使用することが必要になる場合があります。 このような状況では、Xamarin の iOS_バインドプロジェクト_を使用して、Xamarin. ios アプリケーションでライブラリを使用できるようにする[C# バインド](~/cross-platform/macios/binding/overview.md)を作成できます。

一般に、iOS エコシステムでは、次の3つのフレーバーでライブラリを見つけることができます。

- 拡張子をヘッダー (.h ファイル) と共にプリコンパイル済みスタティックライブラリファイルとして使用し `.a` ます。 たとえば、 [Google の分析ライブラリ](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
- プリコンパイル済みフレームワークとして。 これは、スタティックライブラリ、ヘッダー、および場合によっては追加のリソースを含むフォルダーです `.framework` 。 たとえば、 [Google の Admob by ライブラリ](https://developers.google.com/admob/ios/download)です。
- ソースコードファイルとしてのみ。 たとえば、 `.m` と目標 C ファイルのみを含むライブラリ `.h` です。

1番目と2番目のシナリオでは、プリコンパイル済み CocoaTouch スタティックライブラリが既に存在するため、この記事では3番目のシナリオに焦点を当てます。 バインドの作成を開始する前に、ライブラリで提供されているライセンスを必ず確認して、それを確実にバインドできることを確認してください。

この記事では、オープンソースの[Infcolorpicker](https://github.com/InfinitApps/InfColorPicker)目標 c プロジェクトを使用してバインドプロジェクトを作成する手順について説明します。ただし、このガイドに記載されているすべての情報は、サードパーティの目的 c ライブラリで使用するために適合させることができます。 InfColorPicker ライブラリには、ユーザーが HSB 表現に基づいて色を選択できるようにする再利用可能なビューコントローラーが用意されており、色の選択がよりわかりやすくなっています。

[![IOS で実行されている InfColorPicker ライブラリの例](walkthrough-images/run01.png)](walkthrough-images/run01.png#lightbox)

Xamarin でこの特定の目標 C API を使用するために必要なすべての手順について説明します。 iOS:

- まず、Xcode を使用して、C 用のスタティックライブラリを作成します。
- 次に、このスタティックライブラリを Xamarin. iOS にバインドします。
- 次に、マジックペンで必要な API 定義の一部 (すべてではありません) を自動的に生成することにより、目標がワークロードを削減する方法を示します。
- 最後に、バインドを使用する Xamarin iOS アプリケーションを作成します。

このサンプルアプリケーションでは、InfColorPicker API と C# コードの間の通信に強力なデリゲートを使用する方法を示します。 厳密なデリゲートの使用方法について学習した後は、弱いデリゲートを使用して同じタスクを実行する方法について説明します。

## <a name="requirements"></a>必要条件

この記事では、Xcode と C 言語[に関する知識](~/cross-platform/macios/binding/index.md)があることを前提としています。 また、表示される手順を完了するには、次のものが必要です。

- **Xcode と IOS SDK** -Apple の Xcode と最新の ios API は、開発者のコンピューターにインストールして構成する必要があります。
- **[Xcode コマンドラインツール](#Installing_the_Xcode_Command_Line_Tools)**-現在インストールされているバージョンの Xcode には、Xcode コマンドラインツールがインストールされている必要があります (インストールの詳細については、以下を参照してください)。
- **Visual Studio for Mac または Visual studio** -Visual Studio for Mac または visual studio の最新バージョンが開発用コンピューターにインストールされ、構成されている必要があります。 Xamarin iOS アプリケーションを開発するには Apple Mac が必要です。また、Visual Studio を使用する場合は、 [xamarin. ios ビルドホスト](~/ios/get-started/installation/windows/connecting-to-mac/index.md)に接続する必要があります。
- **目標マジックペンの最新バージョン**-[ここ](~/cross-platform/macios/binding/objective-sharpie/get-started.md)からダウンロードした目標マジックペンツールの現在のコピー。 目標マジックペンが既にインストールされている場合は、を使用して最新バージョンに更新できます。`sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"></a>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode コマンドラインツールのインストール

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

前述のように、このチュートリアルでは、Xcode コマンドラインツール (具体的にはと) を使用し `make` `lipo` ます。 `make`コマンドは、プログラムの構築方法を指定する_メイクファイル_を使用して、実行可能プログラムとライブラリのコンパイルを自動化する、非常に一般的な Unix ユーティリティです。 `lipo`コマンドは、マルチアーキテクチャファイルを作成するための OS X コマンドラインユーティリティです。これにより、複数のファイルが、 `.a` すべてのハードウェアアーキテクチャで使用できる1つのファイルに結合されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

前述のように、このチュートリアルでは、 **Mac ビルドホスト**(具体的にはと) で Xcode コマンドラインツールを使用し `make` `lipo` ます。 `make`コマンドは、_メイク_ファイルを使用してプログラムのビルド方法を指定することにより、実行可能プログラムおよびライブラリのコンパイルを自動化する、非常に一般的な Unix ユーティリティです。 `lipo`コマンドは、マルチアーキテクチャファイルを作成するための OS X コマンドラインユーティリティです。これにより、複数のファイルが、 `.a` すべてのハードウェアアーキテクチャで使用できる1つのファイルに結合されます。

-----

Xcode の FAQ ドキュメントを使用した、[コマンドラインから](https://developer.apple.com/library/ios/technotes/tn2339/_index.html)の Apple の構築によれば、OS X 10.9 以降では、Xcode の [**基本設定**] ダイアログボックスの [**ダウンロード**] ウィンドウで、コマンドラインツールのダウンロードがサポートされなくなりました。

ツールをインストールするには、次のいずれかの方法を使用する必要があります。

- **Install Xcode** -Xcode をインストールすると、すべてのコマンドラインツールにバンドルされます。 OS X 10.9 shim (にインストールされている `/usr/bin` ) では、に含まれる任意のツール `/usr/bin` を Xcode 内の対応するツールにマップできます。 たとえば、コマンドを使用する `xcrun` と、コマンドラインから Xcode 内の任意のツールを検索または実行できます。
- **ターミナルアプリケーション**-ターミナルアプリケーションから、コマンドを実行してコマンドラインツールをインストールでき `xcode-select --install` ます。
  - ターミナルアプリケーションを起動します。
  - 「 `xcode-select --install` **」** と入力し、enter キーを押します。次に例を示します。

  ```bash
  Europa:~ kmullins$ xcode-select --install
  ```

  - コマンドラインツールをインストールするように求められます。 [**インストール**] ボタンをクリックします。 [ ![ コマンドラインツールのインストール](walkthrough-images/xcode01.png)](walkthrough-images/xcode01.png#lightbox)

  - ツールは、Apple のサーバーからダウンロードしてインストールされます。 [ ![ ツールのダウンロード](walkthrough-images/xcode02.png)](walkthrough-images/xcode02.png#lightbox)

- **Apple 開発者向けダウンロード**-コマンドラインツールパッケージは、 [apple 開発者向けのダウンロード](https://developer.apple.com/downloads/index.action)web ページで入手できます。 Apple ID でログインし、コマンドラインツールを検索してダウンロードします。 [ ![ コマンドラインツールの](walkthrough-images/xcode03.png)](walkthrough-images/xcode03.png#lightbox)検索

コマンドラインツールがインストールされているので、チュートリアルを続行する準備ができました。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルでは、次の手順について説明します。

- **[スタティックライブラリを作成する](#Creating_A_Static_Library)**-この手順では、 **infcolorpicker**目標 C コードのスタティックライブラリを作成します。 スタティックライブラリはファイル拡張子を持ち、 `.a` ライブラリプロジェクトの .net アセンブリに埋め込まれます。
- **[Xamarin. Ios バインドプロジェクトを作成](#Create_a_Xamarin.iOS_Binding_Project)** する-スタティックライブラリがある場合は、それを使用して Xamarin. ios バインドプロジェクトを作成します。 バインドプロジェクトは、作成したばかりのスタティックライブラリと、目的の C API を使用する方法を説明する C# コード形式のメタデータで構成されています。 このメタデータは、一般に API 定義と呼ばれます。 ここでは、**[目標マジックペン](#Using_Objective_Sharpie)** を使用して、API 定義の作成について説明します。
- **[API の定義を正規化](#Normalize_the_API_Definitions)** する-目標マジックペンは、お客様に役立つ優れたジョブを実行しますが、すべてを行うことはできません。 ここでは、API 定義を使用する前に行う必要がある変更について説明します。
- **[バインドライブラリを使用](#Using_the_Binding)** します。最後に、新しく作成されたバインドプロジェクトの使用方法を示す Xamarin. iOS アプリケーションを作成します。

ここでは、どのような手順が関係しているかを理解したところで、このチュートリアルの残りの部分に移りましょう。

<a name="Creating_A_Static_Library"></a>

## <a name="creating-a-static-library"></a>スタティックライブラリの作成

Github で InfColorPicker のコードを調べる場合は、次のようにします。

[![Github での InfColorPicker のコードの検査](walkthrough-images/image02.png)](walkthrough-images/image02.png#lightbox)

プロジェクトには、次の3つのディレクトリが表示されます。

- **Infcolorpicker** -このディレクトリには、プロジェクトの目的の C コードが格納されています。
- **PickerSamplePad** -このディレクトリには、サンプルの iPad プロジェクトが含まれています。
- **PickerSamplePhone** -このディレクトリには、サンプルの iPhone プロジェクトが含まれています。

[GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip)から InfColorPicker プロジェクトをダウンロードし、選択したディレクトリに解凍してみましょう。 プロジェクトの Xcode ターゲットを開く `PickerSamplePhone` と、Xcode Navigator に次のプロジェクト構造が表示されます。

[![Xcode ナビゲーターのプロジェクト構造](walkthrough-images/image03.png)](walkthrough-images/image03.png#lightbox)

このプロジェクトは、各サンプルプロジェクトに InfColorPicker ソースコード (赤いボックス) を直接追加することによって、コードの再利用を実現します。 サンプルプロジェクトのコードは、青いボックス内にあります。 このプロジェクトは、スタティックライブラリを提供しないため、Xcode プロジェクトを作成してスタティックライブラリをコンパイルする必要があります。

最初の手順として、ヒントのソースコードをスタティックライブラリに追加します。 これを実現するには、次の手順を実行します。

1. Xcode を起動します。
2. [**ファイル**] メニューの [**新しい**  >  **プロジェクト**] をクリックします。

    [![新しいプロジェクトの開始](walkthrough-images/image04.png)](walkthrough-images/image04.png#lightbox)
3. [ **Framework & Library**]、[ **Cocoa タッチスタティックライブラリ**] テンプレートの順に選択し、[**次へ**] ボタンをクリックします。

    [![Cocoa タッチスタティックライブラリテンプレートを選択します](walkthrough-images/image05.png)](walkthrough-images/image05.png#lightbox)

4. `InfColorPicker`**プロジェクト名**として「」と入力し、[**次へ**] ボタンをクリックします。

    [![プロジェクト名として「InfColorPicker」と入力します。](walkthrough-images/image06.png)](walkthrough-images/image06.png#lightbox)
5. プロジェクトを保存する場所を選択し、[ **OK** ] ボタンをクリックします。
6. 次に、ソースを InfColorPicker プロジェクトからスタティックライブラリプロジェクトに追加する必要があります。 (既定**では) Xcode ファイルは**、スタティックライブラリに既に存在するため、上書きすることはできません。 **Finder**から、GitHub から解凍した元のプロジェクトの infcolorpicker ソースコードに移動し、すべての infcolorpicker ファイルをコピーして、新しいスタティックライブラリプロジェクトに貼り付けます。

    [![すべての InfColorPicker ファイルをコピーする](walkthrough-images/image12.png)](walkthrough-images/image12.png#lightbox)

7. Xcode に戻り、[ **Infcolorpicker** ] フォルダーを右クリックして、[ **Add files To The "infcolorpicker..."**] を選択します。

    [![ファイルの追加](walkthrough-images/image08.png)](walkthrough-images/image08.png#lightbox)

8. [ファイルの追加] ダイアログボックスで、先ほどコピーした InfColorPicker ソースコードファイルに移動し、すべてを選択して [**追加**] ボタンをクリックします。

    [![[すべて] を選択し、[追加] ボタンをクリックします。](walkthrough-images/image09.png)](walkthrough-images/image09.png#lightbox)

9. ソースコードがプロジェクトにコピーされます。

    [![ソースコードがプロジェクトにコピーされます](walkthrough-images/image10.png)](walkthrough-images/image10.png#lightbox)

10. Xcode プロジェクトナビゲーターから、 **Infcolorpicker. m**ファイルを選択し、最後の2行をコメントアウトします (このライブラリの作成方法により、このファイルは使用されません)。

    [![InfColorPicker. m ファイルを編集しています](walkthrough-images/image14.png)](walkthrough-images/image14.png#lightbox)

11. 次に、ライブラリに必要なフレームワークがあるかどうかを確認する必要があります。 この情報は、README に記載されているか、提供されているサンプルプロジェクトの1つを開いて確認できます。 この例では、、、を使用し `Foundation.framework` `UIKit.framework` て、それらを `CoreGraphics.framework` 追加してみましょう。

12. [ **Infcolorpicker] ターゲット > ビルドフェーズ**を選択し、[**ライブラリを使用したバイナリのリンク**] セクションを展開します。

    [![[ライブラリとバイナリのリンク] セクションを展開します。](walkthrough-images/image16b.png)](walkthrough-images/image16b.png#lightbox)

13. ボタンを使用して **+** ダイアログを開き、上記の必要なフレームフレームワークを追加できます。

    [![上に一覧表示されている必要なフレームフレームワークを追加します。](walkthrough-images/image16c.png)](walkthrough-images/image16c.png#lightbox)

14. [**ライブラリを含むバイナリをリンク**する] セクションは、次の図のようになります。

    [![リンクバイナリとライブラリセクション](walkthrough-images/image16d.png)](walkthrough-images/image16d.png#lightbox)

この時点で終了しますが、まだ完了していません。 スタティックライブラリが作成されましたが、iOS デバイスと iOS シミュレーターの両方に必要なすべてのアーキテクチャを含む Fat バイナリを作成するには、このライブラリを構築する必要があります。

### <a name="creating-a-fat-binary"></a>Fat バイナリの作成

すべての iOS デバイスには、時間の経過と共に開発された ARM アーキテクチャを搭載したプロセッサが搭載されています。 新しいアーキテクチャでは、旧バージョンとの互換性を維持しながら、新しい命令やその他の機能強化が追加されました。 iOS デバイスには armv6、armv7、armv7s、arm64 命令セットがありますが、 [armv6 は使用されません](~/ios/deploy-test/compiling-for-different-devices.md)。 IOS シミュレーターは ARM を搭載していないので、代わりに x86 および x86_64 の電源シミュレーターです。 これは、各命令セットに対してライブラリを提供する必要があることを意味します。

Fat ライブラリは `.a` 、サポートされているすべてのアーキテクチャを含むファイルです。

Fat バイナリの作成は、次の3つの手順で行います。

- ARM 7 & ARM64 バージョンのスタティックライブラリをコンパイルします。
- X86 および x84_64 バージョンのスタティックライブラリをコンパイルします。
- コマンドラインツールを使用して、 `lipo` 2 つのスタティックライブラリを1つに結合します。

これら3つの手順は比較的簡単ですが、目的の C ライブラリが更新プログラムを受け取ったとき、またはバグ修正が必要な場合は、今後も繰り返すことが必要になる場合があります。 これらの手順を自動化することにした場合は、iOS バインドプロジェクトの将来のメンテナンスとサポートが簡略化されます。

このようなタスクを自動化するために使用できるツールは多数あります。シェルスクリプト、 [rake](https://rake.rubyforge.org/)、 [xbuild](https://www.mono-project.com/docs/tools+libraries/tools/xbuild/)、make などです。 Xcode コマンドラインツールがインストールされている場合 `make` は、もインストールされるため、このチュートリアルで使用されるビルドシステムです。 次に示すのは、iOS デバイスで動作するマルチアーキテクチャ共有ライブラリと、任意のライブラリのシミュレーターを作成するために使用できる**メイクファイル**です。

<!--markdownlint-disable MD010 -->
```makefile
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
<!--markdownlint-enable MD010 -->

任意のプレーンテキストエディターで**Makefile**コマンドを入力し、プロジェクト**名**を使用してセクションをプロジェクトの名前に更新します。 指示内のタブを保持したまま、上記の手順を正確に貼り付けることも重要です。

と**いう名前のファイルを、** 上記で作成した InfColorPicker Xcode スタティックライブラリと同じ場所に保存します。

[![メイクファイルという名前でファイルを保存します。](walkthrough-images/lib00.png)](walkthrough-images/lib00.png#lightbox)

Mac でターミナルアプリケーションを開き、メイクファイルの場所に移動します。 ターミナルに「」と入力し `make` 、 **enter**キーを押すと、**メイクファイル**が実行されます。

[![メイクファイルの出力の例](walkthrough-images/lib01.png)](walkthrough-images/lib01.png#lightbox)

Make を実行すると、によって多数のテキストスクロールが表示されます。 すべてが正常に動作した場合は、**ビルドが成功**し、という語が表示さ `libInfColorPicker-armv7.a` れ、 `libInfColorPicker-i386.a` `libInfColorPickerSDK.a` ファイルは**メイク**ファイルと同じ場所にコピーされます。

[![LibInfColorPicker-armv7、libInfColorPicker-i386. a と libInfColorPickerSDK (メイクファイルによって生成されたファイル)](walkthrough-images/lib02.png)](walkthrough-images/lib02.png#lightbox)

次のコマンドを使用して、Fat バイナリ内のアーキテクチャを確認できます。

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

次のように表示されます。

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

この時点で、Xcode および Xcode コマンドラインツールとを使用してスタティックライブラリを作成することによって、iOS バインディングの最初の手順を完了しました `make` `lipo` 。 次の手順に進み、**マジックペン**を使用して API バインドの作成を自動化しましょう。

<a name="Create_a_Xamarin.iOS_Binding_Project"></a>

## <a name="create-a-xamarinios-binding-project"></a>Xamarin. iOS バインドプロジェクトを作成する

**マジックペン**を使用してバインドプロセスを自動化するには、まず、API 定義を格納するための Xamarin. IOS バインドプロジェクトを作成する必要があります (ここでは、**マジックペン**を使用してビルドを支援します)。また、C# バインドを作成する必要があります。

次の手順を実行してみましょう。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac を起動します。
1. [**ファイル**] メニューの [**新しい**  >  **ソリューション...**] を選択します。

    ![新しいソリューションの開始](walkthrough-images/bind01.png)

1. [新しいソリューション] ダイアログボックスで、[**ライブラリ**] [  >  **iOS バインドプロジェクト**] を選択します。

    ![IOS バインドプロジェクトの選択](walkthrough-images/bind02.png)

1. **[次へ]** をクリックします。

1. **プロジェクト名**として「InfColorPickerBinding」と入力し、[**作成**] ボタンをクリックしてソリューションを作成します。

    ![プロジェクト名として「InfColorPickerBinding」と入力します。](walkthrough-images/bind02a.png)

ソリューションが作成され、次の2つの既定のファイルが含まれます。

![ソリューションエクスプローラー内のソリューション構造](walkthrough-images/bind03.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio を起動します。

1. [**ファイル**] メニューの [**新しい**  >  **プロジェクト**] をクリックします。

    ![新しいプロジェクトの開始](walkthrough-images/bind01vs.png "新しいプロジェクトの開始")

1. [新しいプロジェクト] ダイアログボックスで、[ **VISUAL C# > iPhone & iPad > IOS バインドライブラリ (Xamarin)**] を選択します。

    [![IOS バインドライブラリを選択します](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. **名前**として「InfColorPickerBinding」と入力し、[ **OK** ] ボタンをクリックしてソリューションを作成します。

ソリューションが作成され、次の2つの既定のファイルが含まれます。

![ソリューションエクスプローラー内のソリューション構造](walkthrough-images/bind03vs.png)

-----

- **ApiDefinition.cs** -このファイルには、C# での目標 C API のラップ方法を定義するコントラクトが含まれます。
- **Structs.cs** -このファイルには、インターフェイスとデリゲートで必要とされる構造体または列挙値がすべて格納されます。

これら2つのファイルについては、このチュートリアルの後半で説明します。 まず、InfColorPicker ライブラリをバインドプロジェクトに追加する必要があります。

### <a name="including-the-static-library-in-the-binding-project"></a>バインドプロジェクトにスタティックライブラリを含める

基本バインディングプロジェクトの準備ができたので、前の手順で作成した、 **Infcolorpicker**ライブラリ用の Fat バイナリライブラリを追加する必要があります。

ライブラリを追加するには、次の手順に従います。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Solution Pad で**ネイティブ参照**フォルダーを右クリックし、[**ネイティブ参照の追加**] を選択します。

    ![ネイティブ参照の追加](walkthrough-images/bind04a.png)

1. 前の手順で作成した Fat バイナリ () に移動し、[ `libInfColorPickerSDK.a` **開く**] ボタンを押します。

    ![LibInfColorPickerSDK ファイルを選択します。](walkthrough-images/bind05.png)
1. このファイルはプロジェクトに含まれます。

    ![ファイルを含める](walkthrough-images/bind04.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. `libInfColorPickerSDK.a` **Mac ビルドホスト**からをコピーし、バインドプロジェクトに貼り付けます。

1. プロジェクトを右クリックし、[**既存の項目の追加 >**] を選択します。

    ![既存のファイルの追加](walkthrough-images/bind04vs.png)

1. に移動 `libInfColorPickerSDK.a` し、[**追加**] ボタンを押します。

    ![LibInfColorPickerSDK の追加](walkthrough-images/bind05vs.png)

1. ファイルはプロジェクトに含まれます。

-----

ファイルがプロジェクトに追加されると、Xamarin によってファイルの**ビルドアクション**が自動的に**Objcbindingてライブラリ**に設定され、という名前の特別なファイルが作成さ**れます。** `libInfColorPickerSDK.linkwith.cs`

このファイルには、 `LinkWith` 追加したスタティックライブラリを処理する方法を示す属性が含まれています。 このファイルの内容を次のコードスニペットに示します。

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

属性は、 `LinkWith` プロジェクトのスタティックライブラリと、いくつかの重要なリンカーフラグを識別します。

次に、InfColorPicker プロジェクトの API 定義を作成する必要があります。 このチュートリアルでは、目的のマジックペンを使用して、 **ApiDefinition.cs**ファイルを生成します。

<a name="Using_Objective_Sharpie"></a>

## <a name="using-objective-sharpie"></a>目標マジックペンの使用

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

目標マジックペンは、サードパーティの目的 C ライブラリを C# にバインドするために必要な定義を作成するのに役立つコマンドラインツール (Xamarin によって提供されます) です。 このセクションでは、目的のマジックペンを使用して、InfColorPicker プロジェクトの初期**ApiDefinition.cs**を作成します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

目標マジックペンは、サードパーティの目的 C ライブラリを C# にバインドするために必要な定義を作成するのに役立つコマンドラインツール (Xamarin によって提供されます) です。 このセクションでは、 **Mac ビルドホスト**で目標マジックペンを使用して、InfColorPicker プロジェクトの初期**ApiDefinition.cs**を作成します。

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
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation
```

このチュートリアルでは、次の目的のマジックペンツールを使用します。

- **xcode** -このツールは、現在の xcode インストール、およびインストールされている IOS および Mac api のバージョンに関する情報を提供します。 この情報は、後でバインドを生成するときに使用します。
- **bind** -このツールを使用して、InfColorPicker プロジェクトの **.h**ファイルを最初の**ApiDefinition.cs**ファイルと**StructsAndEnums.cs**ファイルに解析します。

特定の目的のマジックペンツールに関するヘルプを表示するには、ツールの名前とオプションを入力し `-help` ます。 たとえば、は `sharpie xcode -help` 次の出力を返します。

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, -help           Show detailed help
  -v, -verbose        Be verbose with output

Xcode Options:
  -sdks               List all available Xcode SDKs. Pass -verbose for more
                        details.
  -sdkpath SDK        Output the path of the SDK
  -frameworks SDK     List all available framework directories in a given SDK.
```

バインドプロセスを開始する前に、次のコマンドをターミナルに入力して、現在インストールされている Sdk に関する情報を取得する必要があり `sharpie xcode -sdks` ます。

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

この記事では、お客様の `iphoneos9.3` コンピューターに SDK がインストールされていることを確認できます。 この情報を使用して、InfColorPicker プロジェクト `.h` ファイルを最初の**ApiDefinition.cs**および infcolorpicker プロジェクトに解析する準備ができました `StructsAndEnums.cs` 。

ターミナルアプリで次のコマンドを入力します。

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] -scope [full-path-to-project]/InfColorPicker/InfColorPicker [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

ここで、 `[full-path-to-project]` は、コンピューター上の**Infcolorpicker** Xcode プロジェクトファイルが配置されているディレクトリへの完全パスです。また、[iphone-os] は、コマンドに示されているように、インストールされている iOS SDK です `sharpie xcode -sdks` 。 この例では、このディレクトリ内の*すべて*のヘッダーファイルが含まれているパラメーターとして .h を渡してい** \* ます。** 通常は、これを行わないでください。その代わりに、ヘッダーファイルを慎重に読み取って、他のすべての関連ファイルを参照する最上位の **.h**ファイルを検索し、それを目的のマジックペンに渡します。 

> [!TIP] 
> 引数として `-scope` 、バインドするヘッダーが含まれているフォルダーを渡します。 引数を指定しない場合、 `-scope` 目標マジックペンはインポートされたすべての IOS SDK ヘッダーのバインドを生成しようとします。たとえば `#import <UIKit.h>` 、バインドプロジェクトのコンパイル時にエラーが発生する可能性のある大きな定義ファイルが作成されます。 引数を `-scope` 設定した場合、目標マジックペンは、スコープが指定されたフォルダーの外部にあるヘッダーのバインドを生成しません。 

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

[![InfColorPicker.enums.cs ファイルと InfColorPicker.cs ファイル](walkthrough-images/os06.png)](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

上記で作成したバインドプロジェクトで、これらの両方のファイルを開きます。 **InfColorPicker.cs**ファイルの内容をコピーし、 **ApiDefinition.cs**ファイルに貼り付けて、既存の `namespace ...` コードブロックを**InfColorPicker.cs**ファイルの内容に置き換えます (ステートメントはそのままにしておき `using` ます)。

![InfColorPickerControllerDelegate ファイル](walkthrough-images/os07.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

上記で作成したバインドプロジェクトで、これらの両方のファイルを開きます。 ( **Mac ビルドホスト**から) **InfColorPicker.cs**ファイルの内容をコピーし、 **ApiDefinition.cs**ファイルに貼り付けて、既存の `namespace ...` コードブロックを**InfColorPicker.cs**ファイルの内容に置き換えます (ステートメントはそのままにしておき `using` ます)。

-----

<a name="Normalize_the_API_Definitions"></a>

## <a name="normalize-the-api-definitions"></a>API 定義を正規化する

目標マジックペンでは、問題の変換が発生する場合があり `Delegates` ます。そのため、インターフェイスの定義を変更 `InfColorPickerControllerDelegate` し、行を次のコードに置き換える必要があり `[Protocol, Model]` ます。

```csharp
[BaseType(typeof(NSObject))]
[Model]
```

定義は次のようになります。

[![定義](walkthrough-images/os11.png)](walkthrough-images/os11.png#lightbox)

次に、ファイルの内容と同じ処理 `InfColorPicker.enums.cs` を行い、ステートメントをコピーしてファイルに貼り付け `StructsAndEnums.cs` `using` ます。

[![StructsAndEnums.cs ファイルの内容](walkthrough-images/os09.png)](walkthrough-images/os09.png#lightbox)

また、マジックペンが属性を使用してバインディングに注釈を付けていることがわかり `[Verify]` ます。 これらの属性は、バインディングを元の C/マジックペン宣言と比較することによって、目的が正しいことを確認する必要があることを示しています (これは、バインドされた宣言の上にあるコメントで指定されます)。 バインドを確認したら、verify 属性を削除する必要があります。 詳細については、「[検証](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)ガイド」を参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

この時点で、バインドプロジェクトは完成し、ビルドする準備ができています。 バインドプロジェクトをビルドし、エラーが発生していないことを確認してみましょう。

[バインドプロジェクトをビルドし、エラーがないことを確認します。](walkthrough-images/os12.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

この時点で、バインドプロジェクトは完成し、ビルドする準備ができています。 バインドプロジェクトをビルドし、エラーが発生していないことを確認してみましょう。

-----

<a name="Using_the_Binding"></a>

## <a name="using-the-binding"></a>バインディングの使用

上記で作成した iOS バインドライブラリを使用するサンプル iPhone アプリケーションを作成するには、次の手順に従います。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **Xamarin. Ios プロジェクトの作成**-次のスクリーンショットに示すように、ソリューションに**InfColorPickerSample**という名前の新しい Xamarin iOS プロジェクトを追加します。

    ![単一ビューアプリの追加](walkthrough-images/use01.png)

    ![識別子の設定](walkthrough-images/use01a.png)

1. **バインドプロジェクトへの参照の追加**- **InfColorPickerBinding**プロジェクトへの参照が含まれるように**InfColorPickerSample**プロジェクトを更新します。

    ![バインドプロジェクトへの参照の追加](walkthrough-images/use02.png)

1. **IPhone ユーザーインターフェイスを作成**します。 **InfColorPickerSample**プロジェクトの**mainstoryboard.storyboard ファイル**ファイルをダブルクリックして、iOS Designer で編集します。 次に示すように、ビューに**ボタン**を追加し `ChangeColorButton` 、それを呼び出します。

    ![ビューへのボタンの追加](walkthrough-images/use03.png)

1. **InfColorPickerView を追加**します。 xib ライブラリには、 **xib**ファイルが含まれています。 Xamarin では、この**xib**をバインドプロジェクトに含めることはできません。このため、サンプルアプリケーションでは実行時エラーが発生します。 これを回避するには、 **xib**ファイルを Xamarin. iOS プロジェクトに追加します。 Xib プロジェクトを選択し、右クリックして [ **> 追加**] を選択し、次のスクリーンショットに示すように **.xib**ファイルを追加します。

    ![InfColorPickerView を追加します。 xib](walkthrough-images/use04.png)

1. プロンプトが表示されたら、 **xib**ファイルをプロジェクトにコピーします。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **Xamarin. Ios プロジェクトの作成**-**単一ビューアプリ**テンプレートを使用して、 **InfColorPickerSample**という名前の新しい Xamarin iOS プロジェクトを追加します。

    [![iOS アプリ (Xamarin) プロジェクト](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![テンプレートの選択](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **バインドプロジェクトへの参照の追加**- **InfColorPickerBinding**プロジェクトへの参照が含まれるように**InfColorPickerSample**プロジェクトを更新します。

    ![バインドプロジェクトへの参照の追加](walkthrough-images/use02vs.png)

1. **IPhone ユーザーインターフェイスを作成**します。 **InfColorPickerSample**プロジェクトの**mainstoryboard.storyboard ファイル**ファイルをダブルクリックして、iOS Designer で編集します。 次に示すように、ビューに**ボタン**を追加し `ChangeColorButton` 、それを呼び出します。

    ![IPhone ユーザーインターフェイスを作成する](walkthrough-images/use03vs.png)

1. **InfColorPickerView を追加**します。 xib ライブラリには、 **xib**ファイルが含まれています。 Xamarin では、この**xib**をバインドプロジェクトに含めることはできません。このため、サンプルアプリケーションでは実行時エラーが発生します。 これを回避するには、 **Mac ビルドホスト**から**Xib**ファイルを Xamarin. iOS プロジェクトに追加します。 Xamarin プロジェクトを選択し、右クリックして [既存項目の**追加**  >  **...**] を選択し、 **xib**ファイルを追加します。

-----

次に、目的 C のプロトコルと、それをバインドおよび C# コードでどのように処理するかを簡単に見てみましょう。

### <a name="protocols-and-xamarinios"></a>プロトコルと Xamarin. iOS

目的 C では、プロトコルによって、特定の状況で使用できるメソッド (またはメッセージ) が定義されます。 概念的には、C# のインターフェイスによく似ています。 目的の C プロトコルと C# インターフェイスの大きな違いの1つは、クラスが実装する必要のないオプションのメソッドメソッドをプロトコルが持つことができることです。 目的-C では、 @optional 省略可能なメソッドを示すためにキーワードが使用されます。 プロトコルの詳細については[、「イベント、プロトコル、およびデリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)」を参照してください。

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

バインドライブラリをコンパイルすると、という抽象基本クラスが作成されます `InfColorPickerControllerDelegate` 。このクラスは、仮想メソッドを使用してこのインターフェイスを実装します。

Xamarin. iOS アプリケーションでこのインターフェイスを実装するには、次の2つの方法があります。

- **強いデリゲート**-厳密なデリゲートを使用するには、適切なメソッドをサブクラス化してオーバーライドする C# クラスを作成する必要が `InfColorPickerControllerDelegate` あります。 **InfColorPickerController**は、このクラスのインスタンスを使用して、クライアントと通信します。
- **弱いデリゲート**-弱いデリゲートは、一部のクラス (など) でパブリックメソッドを作成 `InfColorPickerSampleViewController` し、属性を使用してそのメソッドをプロトコルに公開するという、若干異なる手法です `InfColorPickerDelegate` `Export` 。

強いデリゲートは、Intellisense、タイプセーフ、およびより適切なカプセル化を提供します。 これらの理由から、弱いデリゲートではなく、可能な場合は強力なデリゲートを使用する必要があります。

このチュートリアルでは、次の2つの手法について説明します。最初に強力なデリゲートを実装し、次に弱いデリゲートを実装する方法について説明します。

### <a name="implementing-a-strong-delegate"></a>強いデリゲートの実装

強力なデリゲートを使用してメッセージに応答することで、Xamarin iOS アプリケーションを完成させ `colorPickerControllerDidFinish:` ます。

**サブクラス InfColorPickerControllerDelegate** -新しいクラスをという名前のプロジェクトに追加 `ColorSelectedDelegate` します。 クラスを編集して、次のコードを記述します。

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

Xamarin は、という抽象基本クラスを作成することによって、目的の C デリゲートをバインドします `InfColorPickerControllerDelegate` 。 この型をサブクラス化し、メソッドをオーバーライドして、 `ColorPickerControllerDidFinish` のプロパティの値にアクセスし `ResultColor` `InfColorPickerController` ます。

**ColorSelectedDelegate のインスタンスを作成**します。イベントハンドラーには、 `ColorSelectedDelegate` 前の手順で作成した型のインスタンスが必要です。 クラスを編集 `InfColorPickerSampleViewController` し、次のインスタンス変数をクラスに追加します。

```csharp
ColorSelectedDelegate selector;
```

**ColorSelectedDelegate 変数の初期化**-が有効なインスタンスであることを確認するに `selector` は、 `ViewDidLoad` 次のスニペットに一致するようにのメソッドを更新し `ViewController` ます。

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();
  ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
  selector = new ColorSelectedDelegate (this);
}
```

**HandleTouchUpInsideWithStrongDelegate メソッドを実装**します。次に、ユーザーが**colorchangebutton**に触れるときのイベントハンドラーを実装します。 `ViewController`を編集し、次のメソッドを追加します。

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

まず、静的メソッドを使用してのインスタンスを取得 `InfColorPickerController` し、そのインスタンスに対して、プロパティを介して、強力なデリゲートを認識させ `InfColorPickerController.Delegate` ます。 このプロパティは、目標マジックペンによって自動的に生成されました。 最後に、を呼び出して、 `PresentModallyOverViewController` ユーザーが色を選択できるようにビューを表示し `InfColorPickerSampleViewController.xib` ます。

**アプリケーションを実行し**ます。この時点で、すべてのコードが完成しています。 アプリケーションを実行すると、 `InfColorColorPickerSampleView` 次のスクリーンショットに示すように、の背景色を変更できるようになります。

[![アプリケーションの実行](walkthrough-images/run01.png)](walkthrough-images/run01.png#lightbox)

お疲れさまでした。 この時点で、Xamarin iOS アプリケーションで使用するために、目的の C ライブラリを作成してバインドしました。 次に、弱いデリゲートの使用について説明します。

### <a name="implementing-a-weak-delegate"></a>弱いデリゲートの実装

Xamarin では、特定のデリゲートに対して目的の C プロトコルにバインドされたクラスをサブクラス化するのではなく、から派生する任意のクラスのプロトコルメソッドを実装し、を使用して `NSObject` メソッドを装飾し、 `ExportAttribute` 適切なセレクターを指定することもできます。 この方法を使用する場合は、プロパティではなく、クラスのインスタンスを `WeakDelegate` プロパティに割り当て `Delegate` ます。 弱いデリゲートを使用すると、デリゲートクラスを別の継承階層にする柔軟性が得られます。 ここでは、Xamarin. iOS アプリケーションで弱いデリゲートを実装して使用する方法について説明します。

**TouchUpInside のイベントハンドラーを作成**する- `TouchUpInside` [背景色の変更] ボタンのイベントの新しいイベントハンドラーを作成します。 このハンドラーは、前のセクションで作成したハンドラーと同じロールを設定し `HandleTouchUpInsideWithStrongDelegate` ますが、厳密なデリゲートではなく弱いデリゲートを使用します。 クラスを編集 `ViewController` し、次のメソッドを追加します。

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```

**Update ViewDidLoad** -先ほど `ViewDidLoad` 作成したイベントハンドラーを使用するように変更する必要があります。 `ViewController` `ViewDidLoad` 次のコードスニペットのように、を編集して変更します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**ColorPickerControllerDidFinish: message を処理します**。が完了すると、iOS によって `ViewController` にメッセージが送信され `colorPickerControllerDidFinish:` `WeakDelegate` ます。 このメッセージを処理できる C# メソッドを作成する必要があります。 これを行うには、C# メソッドを作成し、を使用して装飾し `ExportAttribute` ます。 を編集 `ViewController` し、次のメソッドをクラスに追加します。

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

- [Objective-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [バインディングの詳細](~/cross-platform/macios/binding/overview.md)
- [バインディングの種類のリファレンスガイド](~/cross-platform/macios/binding/binding-types-reference.md)
- [目標 C 開発者向けの Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [フレームワークデザインのガイドライン](https://msdn.microsoft.com/library/ms229042.aspx)
