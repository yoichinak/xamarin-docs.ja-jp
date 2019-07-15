---
title: インストールして、Xamarin で watchOS の使用
description: このドキュメントでは、インストールして、Xamarin で watchOS を使用する方法について説明します。 インストールでは、watchOS プロジェクトについて説明して構造体、iOS デザイナー、Xcode の統合を使用する方法とトラブルシューティングのヒントを提供します。
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/05/2017
ms.openlocfilehash: cb8aabb3649da3818c1b020508b78a03f513963b
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830679"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>インストールして、Xamarin で watchOS の使用

watchOS 4 では、macOS Sierra (10.12) Xcode 9 で必要です。

当初、watchOS 1 には、Xcode 7 では、OS X Yosemite (10.10) が必要です。

> [!WARNING]
> [watchOS 1 更新プログラムは、2018 年 4 月 1 日後に受け入れられません](https://developer.apple.com/news/?id=11162017a)します。 今後の更新プログラムは、watchOS 2 を使用する必要があります SDK またはそれ以降。watchOS で構築 4 SDK はお勧めします。

## <a name="project-structure"></a>プロジェクトの構造

Watch アプリは、3 つのプロジェクトで構成されます。

- **IPhone アプリの Xamarin.iOS プロジェクト**-これは、通常の iPhone プロジェクト、Xamarin.iOS のテンプレートのいずれかのことができます。 Watch アプリとその拡張機能は、このメイン プロジェクト内でまとめられます。

- **ウォッチ拡張機能プロジェクト**-Watch アプリのコント ローラー クラス) などのコードが含まれます。

- **Watch アプリ プロジェクト**-Watch アプリのすべての UI リソースとユーザー インターフェイスのストーリー ボード ファイルが含まれます。

[ウォッチ キット Catalog サンプル](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)Xamarin.Studio でこのようなソリューションを示します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](installation-images/catalog-solution.png "Visual Studio でソリューション")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/catalog-solution-vs.png "Visual Studio でソリューション")

-----

ダウンロードし、実行、 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)サンプルを開始します。
サンプル画面で確認できます、[コントロール](~/ios/watchos/user-interface/index.md)ページ。


## <a name="creating-a-new-project"></a>新規プロジェクトの作成

新しい「ウォッチ…」を作成できませんではなく、既存の iOS アプリケーションに Watch アプリを追加することができます。 Watch アプリを作成する次の手順に従います。

1. 既存のプロジェクトを持っていない場合がまず選択**ファイル > 新しいソリューション**iOS アプリを作成し、(たとえば、**単一ビュー アプリ**)。

    [![](installation-images/cycle8-2-sml.png "ファイルを選択 > 新しいソリューションと iOS アプリの作成")](installation-images/cycle8-2.png#lightbox)

2. IOS アプリの作成 (または既存の iOS アプリを使用する予定)、ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.** .**新しいプロジェクト**ウィンドウ選択**watchOS > アプリ > WatchKit アプリ**:

    [![](installation-images/cycle8-6-sml.png "WatchOS の選択 > アプリ > WatchKit アプリ")](installation-images/cycle8-6.png#lightbox)

3. 次の画面では、iOS アプリ プロジェクトは、watch アプリを含める必要がありますを選択できます。

    [![](installation-images/cycle8-7-sml.png "選択する iOS アプリ プロジェクトは、watch アプリを含める必要があります。")](installation-images/cycle8-7.png#lightbox)

4. 最後に、プロジェクトを保存する場所を選択 (およびオプションでソース管理を有効にする)。

    [![](installation-images/cycle8-8-sml.png "プロジェクトを保存する場所を選択します。")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio for Mac が自動的に構成されます[プロジェクト参照と**Info.plist**設定](~/ios/watchos/get-started/project-references.md)できます。

## <a name="creating-the-watch-user-interface"></a>ウォッチのユーザー インターフェイスを作成します。

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS デザイナーを使用してください。

Watch アプリのダブルクリック**Interface.storyboard** iOS デザイナーを使用して編集します。 ストーリー ボードに、インターフェイス コント ローラーと UI コントロールをドラッグすることができます、**ツールボックス**し、構成を使用して、**プロパティ**パッド。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](installation-images/iosdesigner-sml.png "デザイナーでストーリー ボード")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](installation-images/iosdesigner-sml-vs.png "デザイナーでストーリー ボード")](installation-images/iosdesigner-vs.png#lightbox)

-----

各新しいインターフェイス コント ローラーを指定する必要があります、**クラス**を選択し、名前を入力して、**プロパティ**パッド (これは、必要な作成C#分離コード ファイルを自動的に)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](installation-images/iosdesigner-classname.png "各新しいインターフェイス コント ローラー クラスを提供します。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/iosdesigner-classname-vs.png "各新しいインターフェイス コント ローラー クラスを提供します。")

-----

作成してセグエ**Ctrl + ドラッグ**別のインターフェイス コント ローラー上にボタン、テーブル、またはインターフェイス コント ローラーから。


### <a name="using-xcode-on-the-mac"></a>Mac で Xcode を使用します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xcode を使用して、Interface.storyboard ファイルを右クリックして、ユーザー インターフェイスを構築する続行することができます**プログラムから開く > Xcode の Interface Builder**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio のユーザーは、Xcode を使用して、Mac Build Host を直接使用に切り替えることで、ユーザー インターフェイスを構築することができますも。
Mac の Visual Studio でソリューションを開く Interface.storyboard ファイルを右クリックし、選択**プログラムから開く > Xcode の Interface Builder**:

-----

![](installation-images/openwith-xcode.png "Xcode インターフェイス ビルダーで開くこと、Interface.storyboard")

Xcode を使用する場合する必要があります、同じ手順に従います標準と watch アプリの[iOS アプリのストーリー ボード](~/ios/user-interface/storyboards/index.md)(outlet と action での作成など**Ctrl + ドラッグ**に、 **.h**ヘッダー ファイル)。

自動的に追加されている Xcode の Interface Builder のストーリー ボードを保存すると、outlet とアクションを作成する、 C# **. designer.cs**ウォッチ拡張機能プロジェクト内のファイル。


### <a name="adding-additional-screens-in-xcode"></a>Xcode で追加の画面を追加します。

Xcode の Interface Builder を使用して、ストーリー ボードに (何がテンプレートに既定で含まれます) を超える追加の画面を追加すると**手動で追加する必要があります、C#コード ファイル**各新しいインターフェイス コント ローラー。

参照してください、[ストーリー ボードに新しいインターフェイス コント ローラーを追加する方法の手順を高度な](~/ios/watchos/troubleshooting.md#add)します。

*Xamarin iOS Designer は、これを自動的には、手動の手順は必要ありません。*


## <a name="building"></a>ビルド

Watch アプリを含むプロジェクトは、その他の iOS プロジェクトと同様にビルドされます。 ビルド プロセスは、コードのないウォッチ アプリケーション (.app) を格納するウォッチ拡張機能 (.appex) を含む iPhone アプリケーション (.app) になります。


## <a name="launching"></a>起動します。

Studio (Mac Build Host で起動)、Visual Studio for Mac または Visual を使用して、シミュレーターで watch アプリを起動できます。

これには、WatchKit アプリを起動するための 2 つのモードがあります。

- 通常のアプリ モード (既定値) と
- [通知](~/ios/watchos/platform/notifications.md)(が JSON 形式でのテスト通知ペイロードが必要)。

### <a name="xcode-8-support"></a>Xcode 8 のサポート

Xcode 8 (またはそれ以降) がインストールされている Apple Watch のシミュレーターが iOS シミュレーターとは別 (とは異なり[Xcode 6](#xcode6)として表示された場合、*外付けディスプレイ*)。
Watch アプリ プロジェクトを選択してスタートアップ プロジェクトとシミュレーターの一覧が表示されます*iOS シミュレーター* (次に示す) から選択します。

[![](installation-images/xs-xcode8-watchos3-sml.png "シミュレーターの種類の選択")](installation-images/xs-xcode8-watchos3.png#lightbox)

デバッグを開始するときに*2*シミュレーターが開始 - iOS シミュレーター*と*Apple Watch のシミュレーター。 使用して、**コマンド + Shift + H**ウォッチのメニューとクロック表面; に移動し、使用、**ハードウェア**を設定する メニュー、 **Force Touch 圧力**します。 トラック パッドやマウスのスクロールには、デジタル クラウンを使用してシミュレートされます。

#### <a name="troubleshooting"></a>トラブルシューティング

次のエラーが表示されます、**アプリケーション出力**いる watch がないシミュレーターを起動しようとする場合。

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

参照してください[Apple のフォーラム](https://forums.developer.apple.com/thread/7783)手順については、既定値が機能しない場合は、シミュレーターの構成。


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 と watchOS 1

行う必要があります、*ウォッチ拡張機能プロジェクト*、**スタートアップ プロジェクト**の実行や、アプリをデバッグする前にします。 自体には、watch アプリを「起動」できないし、iOS アプリを選択する場合が起動します。 iOS シミュレーターでは通常どおりです。

既定では、watch アプリは標準で起動**アプリ**モード (概要ではないまたは通知モード) から Visual Studio for Mac の**実行**または**デバッグ**コマンド。

Xcode 6 を使用する場合、iPhone 5、iPhone 5 s、iPhone 6、および iPhone のみ 6 Plus は、いずれかの外付けディスプレイをアクティブ化できる**Apple Watch - 38 mm**または**Apple Watch - 42 mm** watch アプリケーションが表示されます表示されます。

> [!NOTE]
> ウォッチの画面が表示されないことに自動的に iOS シミュレーターで Xcode 6 を使用する場合に注意してください。
> 使用して、**ハードウェア > 外部ディスプレイ**ウォッチ画面を表示するメニュー。

<a name="custommodes" />

## <a name="launching-notification-mode"></a>通知モードを起動します。

参照してください、[通知ページ](~/ios/watchos/platform/notifications.md)はコードで通知を処理する方法。


Visual Studio for Mac は、通知、watch アプリを開始できます_スタートアップ モード_通知。



Watch アプリのプロジェクトを右クリックし、選択**実行 > カスタム構成しています.** :


[![](installation-images/runwith-customparams-sml.png "カスタム構成を実行しています。")](installation-images/runwith-customparams.png#lightbox)


開き、**カスタム パラメーター**ウィンドウを選択することができます**通知**(および JSON ペイロードを提供)、キーを押します**実行**watch アプリをシミュレーターで起動します。


[![](installation-images/runwith-execargs-sml.png "通知とペイロードの設定")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>デバッグ

デバッグは、Visual Studio for Mac と Visual Studio の両方でサポートされます。
通知モードでデバッグするときに、通知の JSON ファイルを指定してください。 このスクリーン ショットは、watch アプリで検出されるデバッグ ブレークポイントを示しています。

![](installation-images/debug-sml.png "このスクリーン ショットは、watch アプリで検出されるデバッグ ブレークポイントを示しています。")

実行されている、watch アプリの起動手順を実行した後終了は、 **iOS シミュレーター (ウォッチ)** します。
通知モードを選択することができます**デバッグ > システム ログを開く**(**CMD +/** ) を使用して、`Console.WriteLine`コードにします。

### <a name="debugging-lifecycle-event-handlers"></a>ライフ サイクル イベント ハンドラーのデバッグ

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

WatchOS のテンプレート ファイル (など`InterfaceController`、 `ExtensionDelegate`、 `NotificationController`、および`ComplicationController`) 既に実装されている、必要なライフ サイクル メソッドが付属しています。 追加`Console.WriteLine`呼び出しと、読み取り、**アプリケーション出力**イベントのライフ サイクルを理解します。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Watch アプリの最初のビデオ](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple の WatchKit ヒント](https://developer.apple.com/watchkit/tips/)
