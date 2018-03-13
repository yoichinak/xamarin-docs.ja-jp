---
title: "セットアップとインストール"
description: "WatchOS を開発するためのセットアップ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: c423c9bf49c735673793f8e61134f7e705816d54
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="installation"></a>インストール

watchOS 4 には、macOS Xcode 9 Sierra (10.12) が必要です。

当初、watchOS 1 には、Xcode 7 OS X Yosemite (10.10) が必要です。

> [!WARNING]
> [watchOS 1 更新プログラムは、2018 年 4 月 1 日後に受け入れられません](https://developer.apple.com/news/?id=11162017a)です。 今後の更新が watchOS 2 を使用する必要があります SDK またはそれ以降。watchOS でビルドすると 4 SDK お勧めします。

## <a name="project-structure"></a>プロジェクトの構造

Watch アプリは、3 つのプロジェクトで構成されます。

- **Xamarin.iOS iPhone アプリのプロジェクト**-これは、通常の iPhone プロジェクト、Xamarin.iOS テンプレートのいずれかであることができます。 Watch アプリとその拡張は、このメイン プロジェクト内部まとめられます。

- **ウォッチ拡張機能プロジェクト**-Watch アプリの (コント ローラー クラス) などのコードが含まれます。

- **Watch アプリ プロジェクト**-Watch アプリのすべての UI リソースとユーザー インターフェイスのストーリー ボード ファイルを含みます。

[ウォッチ キット Catalog サンプル](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)ソリューションが Xamarin.Studio で次のようになります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/catalog-solution.png "Visual Studio でソリューション")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "Visual Studio でソリューション")

-----

ダウンロードし、実行、 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)作業を開始するサンプルです。
サンプルの画面が見つかりません、[コントロール](~/ios/watchos/user-interface/index.md)ページ。


## <a name="creating-a-new-project"></a>新規プロジェクトの作成

新しい「ウォッチ…」を作成することはできませんではなく、既存の iOS アプリケーションに Watch アプリを追加できます。 Watch アプリを作成する手順に従います。

1. 既存のプロジェクトをお持ちでない場合、最初を選択**ファイル > 新しいソリューション**iOS アプリを作成して (たとえば、 **1 つのアプリの表示**)。

    [![](installation-images/cycle8-2-sml.png "ファイルを選択 > 新しいソリューションと iOS アプリの作成")](installation-images/cycle8-2.png#lightbox)

2. IOS アプリの作成 (または、既存の iOS アプリを使用する場合)、ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.**.**新しいプロジェクト**ウィンドウ選択**watchOS > アプリ > WatchKit アプリ**:

    [![](installation-images/cycle8-6-sml.png "WatchOS を選択 > アプリ > WatchKit アプリ")](installation-images/cycle8-6.png#lightbox)

3. 次の画面を使用して、iOS アプリ プロジェクトは、watch アプリを含める必要がありますを選択できます。

    [![](installation-images/cycle8-7-sml.png "選択する iOS アプリ プロジェクトは、watch アプリを含める必要があります。")](installation-images/cycle8-7.png#lightbox)

4. 最後に、プロジェクトを保存する場所を選択して (およびオプションでソース管理を有効にする)。

    [![](installation-images/cycle8-8-sml.png "プロジェクトを保存する場所を選択します。")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio for Mac を自動的に構成[プロジェクト参照と**Info.plist**設定](~/ios/watchos/get-started/project-references.md)します。

## <a name="creating-the-watch-user-interface"></a>ウォッチ ユーザー インターフェイスの作成

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS デザイナーを使用してください。

Watch アプリのダブルクリック**Interface.storyboard** iOS デザイナーを使用して編集します。 ストーリー ボードにインターフェイスのコント ローラーと UI コントロールをドラッグすることができます、**ツールボックス**し、その構成を使用して、**プロパティ**パッド。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "デザイナーでストーリー ボード")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "デザイナーでストーリー ボード")](installation-images/iosdesigner-vs.png#lightbox)

-----

新しいインターフェイスの各コント ローラーを提供する必要があります、**クラス**を選択し、内の名前を入力して、**プロパティ**パッド (必要な c# 分離コード ファイルを自動的に作成これされます)。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "クラスの新しいインターフェイスの各コント ローラーを与える")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "クラスの新しいインターフェイスの各コント ローラーを与える")

-----

作成して segues **ctrl キーを押しドラッグ +**ボタン、テーブル、またはインターフェイス コント ローラーが別のインターフェイス コント ローラーにします。


### <a name="using-xcode-on-the-mac"></a>Mac で Xcode を使用します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xcode を使って Interface.storyboard ファイルを右クリックしを選択すると、ユーザー インターフェイスを構築する行う**ファイルを開く > Xcode インターフェイス ビルダー**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio ユーザーでは、直接 Mac ビルド ホストを切り替えることによって、ユーザー インターフェイスを構築するのに Xcode も使用できます。
Mac の Visual Studio でソリューションを開くと Interface.storyboard ファイルを右クリックし、選択**ファイルを開く > Xcode インターフェイス ビルダー**:

-----

![](installation-images/openwith-xcode.png "Xcode インターフェイス ビルダーで開くこと、Interface.storyboard")

Xcode を使用する場合に従ってくださいウォッチ アプリの通常の場合と同じ手順[iOS アプリのストーリー ボード](~/ios/user-interface/storyboards/index.md)(コンセントとによってアクションの作成など**ctrl キーを押しながらドラッグ**に、 **.h**ヘッダー ファイル)。

保存すると、ストーリー ボードが自動的に追加されている Xcode インターフェイス ビルダーで、コンセントとアクションを作成する c# **. designer.cs**ウォッチ拡張機能プロジェクト内のファイルです。


### <a name="adding-additional-screens-in-xcode"></a>Xcode での他の画面を追加します。

Xcode インターフェイスのビルダーを使用して、ストーリー ボードに (既定では、テンプレートの内容は) を超える追加の画面を追加すると**の c# コード ファイルを手動で追加する必要があります**インターフェイスの新しい各コント ローラーにします。

参照してください、[ストーリー ボードに新しいインターフェイス コント ローラーを追加する方法の手順を高度な](~/ios/watchos/troubleshooting.md#add)します。

*Xamarin iOS デザイナーは、これを自動的には、手動の手順は必要ありません。*


## <a name="building"></a>ビルド

Watch アプリを含むプロジェクトは、その他の iOS プロジェクトと同様にビルドされます。 ビルド プロセスは、ウォッチ式のコードのないアプリケーション (.app) が含まれ、ウォッチ拡張機能 (.appex) を格納する iPhone アプリケーション (.app) になります。


## <a name="launching"></a>起動します。

(Mac ビルド ホスト上に開始) Studio for Mac またはビジュアルに、Visual Studio を使用してシミュレーターでアプリのウォッチを起動できます。

これには、WatchKit アプリを起動するための 2 つのモードがあります。

 - 通常のアプリのモード (既定)、および
- [通知](~/ios/watchos/platform/notifications.md)(を JSON 形式でテスト通知のペイロードが必要)。

### <a name="xcode-8-support"></a>Xcode 8 のサポート

Xcode 8 (またはそれ以降) がインストールされると、Apple Watch シミュレーターとは別 iOS シミュレーター (とは異なり[Xcode 6](#xcode6)として表示されていたという、*外付けディスプレイ*)。
シミュレーターの一覧が表示されます Watch アプリ プロジェクトを選択して、スタートアップ プロジェクトを作成すると、 *iOS シミュレーター* (下図) から選択できます。

[![](installation-images/xs-xcode8-watchos3-sml.png "シミュレーターの種類の選択")](installation-images/xs-xcode8-watchos3.png#lightbox)

デバッグを開始するときに*2*シミュレーターは、まず - iOS シミュレーター*と*Apple Watch シミュレーター。 使用して**コマンド + Shift + H**ウォッチ メニューと時計の文字盤; に移動しを使用して、**ハードウェア**を設定するメニュー、 **Force タッチ圧力**です。 トラック パッドやマウスのスクロールには、デジタル クラウンの使用をシミュレートします。

#### <a name="troubleshooting"></a>トラブルシューティング

次のエラーが表示されます、**アプリケーション出力**ウォッチ式のペアがないシミュレーターを起動しようとする場合。

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

参照してください[Apple のフォーラム](https://forums.developer.apple.com/thread/7783)手順については、既定の設定が機能しない場合、シミュレーターの構成にします。


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 および watchOS 1

行う必要があります、*ウォッチ拡張機能プロジェクト*、**スタートアップ プロジェクト**の実行や、アプリをデバッグする前にします。 ことはできません"start"watch アプリ自体、および iOS アプリを選択した場合、開始されます iOS シミュレーターでは通常どおりです。

既定では、watch アプリが通常に起動**アプリ**から Visual Studio for Mac のモード (概要ではない、または通知モード)**実行**または**デバッグ**コマンド。

Xcode 6 を使用する場合、iPhone 5、iPhone 5 s、iPhone 6 および iPhone だけ 6 プラスは、いずれかの外付けディスプレイをアクティブ化できます**Apple Watch - 38 mm**または**Apple Watch - 42 mm** watch アプリケーションが表示されます表示されます。

**注:**ウォッチ画面が表示されないことに自動的に、iOS シミュレーターで Xcode 6 を使用する場合に注意してください。
使用して、**ハードウェア > 外部ディスプレイ**ウォッチ画面に表示されるメニュー。

<a name="custommodes" />

## <a name="launching-notification-mode"></a>通知モードを起動します。

参照してください、[通知ページ](~/ios/watchos/platform/notifications.md)についてコードでの通知を処理する方法です。


Visual Studio for Mac は、通知で watch アプリを起動する_スタートアップ モード_通知。



Watch アプリ プロジェクトを右クリックし、選択**実行 > カスタム構成しています.**:


[![](installation-images/runwith-customparams-sml.png "カスタム構成を実行します。")](installation-images/runwith-customparams.png#lightbox)


開き、**カスタム パラメーター**ウィンドウを選択できます**通知**(および JSON ペイロードを提供)、キーを押します**実行**watch アプリをシミュレーターで起動します。


[![](installation-images/runwith-execargs-sml.png "通知およびペイロードの設定")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>デバッグ

デバッグは、Visual Studio for Mac と Visual Studio の両方でサポートされます。
通知モードでデバッグするときに通知 JSON ファイルを指定してください。 このスクリーン ショットは、デバッグ ブレークポイント watch アプリで検出されるを示しています。

![](installation-images/debug-sml.png "このスクリーン ショットは、デバッグ ブレークポイント watch アプリで検出されるを示しています。")

起動手順を実行した後は最終的に、ウォッチ アプリで実行されている、 **iOS シミュレーター (ウォッチ)**です。
通知モードを選択できます**デバッグ > システム ログを開く**(**CMD +/**) を使用して`Console.WriteLine`コードにします。

### <a name="debugging-lifecycle-event-handlers"></a>ライフ サイクル イベント ハンドラーのデバッグ

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

WatchOS テンプレート ファイル (など`InterfaceController`、 `ExtensionDelegate`、 `NotificationController`、および`ComplicationController`) 既に実装されている、必要なライフ サイクル メソッドが付属しています。 追加`Console.WriteLine`呼び出しと読み、**アプリケーション出力**イベント ライフ サイクルを理解するのには。



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [最初の Watch アプリ ビデオ](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple の WatchKit ヒント](https://developer.apple.com/watchkit/tips/)
