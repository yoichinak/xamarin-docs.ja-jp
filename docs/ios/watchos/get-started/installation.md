---
title: インストールして、Xamarin で watchOS の使用
description: このドキュメントでは、インストールして、Xamarin で watchOS を使用する方法について説明します。 インストールでは、watchOS プロジェクトについて説明して構造体、iOS デザイナー、Xcode の統合を使用する方法とトラブルシューティングのヒントを提供します。
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/05/2017
ms.openlocfilehash: f986099011dbccb0eb43c62d253ee497d46ca08e
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915478"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>インストールして、Xamarin で watchOS の使用

watchOS 4 では、macOS Sierra (10.12) Xcode 9 で必要です。

当初、watchOS 1 には、Xcode 7 では、OS X Yosemite (10.10) が必要です。

> [!WARNING]
> [2018 年4月1日以降、watchOS 1 の更新は受け付けられません](https://developer.apple.com/news/?id=11162017a)。 今後の更新プログラムは、watchOS 2 を使用する必要があります SDK またはそれ以降。watchOS で構築 4 SDK はお勧めします。

## <a name="project-structure"></a>プロジェクト構造

Watch アプリは、3 つのプロジェクトで構成されます。

- **Xamarin ios アプリプロジェクト**-これは通常の iphone プロジェクトであり、Xamarin の ios テンプレートのいずれでもかまいません。 Watch アプリとその拡張機能は、このメイン プロジェクト内でまとめられます。

- **Watch Extension プロジェクト**-watch アプリのコード (コントローラークラスなど) が含まれています。

- **アプリプロジェクトを見る**: これには、watch アプリのすべての UI リソースを含むユーザーインターフェイスストーリーボードファイルが含まれています。

[Watch Kit Catalog のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)ソリューションは、Xamarin. Studio で次のようになります。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![](installation-images/catalog-solution.png "The solution in Visual Studio")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![](installation-images/catalog-solution-vs.png "The solution in Visual Studio")

-----

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルをダウンロードして実行し、作業を開始します。
サンプルの画面は、[[コントロール](~/ios/watchos/user-interface/index.md)] ページにあります。

## <a name="creating-a-new-project"></a>新規プロジェクトの作成

新しい「ウォッチ…」を作成できませんではなく、既存の iOS アプリケーションに Watch アプリを追加することができます。 Watch アプリを作成する次の手順に従います。

1. 既存のプロジェクトがない場合は、まず **[ファイル > 新しいソリューション]** を選択し、iOS アプリを作成します ( **1 つのビューアプリ**など)。

    [![](installation-images/cycle8-2-sml.png "Choose File > New Solution and create an iOS app")](installation-images/cycle8-2.png#lightbox)

2. IOS アプリが作成されたら (または、既存の iOS アプリを使用する予定の場合)、ソリューションを右クリックし、追加、**新しいプロジェクトの追加 >** の順に選択します。**新しいプロジェクト** ウィンドウで、**watchOS > App > WatchKit app** を選択します。

    [![](installation-images/cycle8-6-sml.png "Select watchOS > App > WatchKit App")](installation-images/cycle8-6.png#lightbox)

3. 次の画面では、iOS アプリ プロジェクトは、watch アプリを含める必要がありますを選択できます。

    [![](installation-images/cycle8-7-sml.png "Choose which iOS app project should include the watch app")](installation-images/cycle8-7.png#lightbox)

4. 最後に、プロジェクトを保存する場所を選択 (およびオプションでソース管理を有効にする)。

    [![](installation-images/cycle8-8-sml.png "Choose the location to save the project")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio for Mac により、[プロジェクト参照と**情報 plist**設定](~/ios/watchos/get-started/project-references.md)が自動的に構成されます。

## <a name="creating-the-watch-user-interface"></a>ウォッチのユーザー インターフェイスを作成します。

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS デザイナーを使用してください。

ウォッチ アプリの **インターフェイス** をダブルクリックして、iOS Designer を使用して編集します。 **[ツールボックス]** から、インターフェイスコントローラー と UI コントロール をストーリーボードにドラッグして、 **[プロパティ]** パッドを使用して構成することができます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![](installation-images/iosdesigner-sml.png "The storyboard in the Designer")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![](installation-images/iosdesigner-sml-vs.png "The storyboard in the Designer")](installation-images/iosdesigner-vs.png#lightbox)

-----

新しい各インターフェイスコントローラーを選択し、 **[プロパティ]** パッドで名前を入力することによって、**クラス**を指定する必要C#があります (これにより、必要な分離コードファイルが自動的に作成されます)。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![](installation-images/iosdesigner-classname.png "Give each new interface controller a Class")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![](installation-images/iosdesigner-classname-vs.png "Give each new interface controller a Class")

-----

**Ctrl キーを押しながら**、ボタン、テーブル、またはインターフェイスコントローラーから別のインターフェイスコントローラーにドラッグして、セグエを作成します。

### <a name="using-xcode-on-the-mac"></a>Mac で Xcode を使用します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Xcode を使用してユーザーインターフェイスを作成し続けるには、Xcode ファイルを右クリックし、[ **Open With >] \ (Interface Builder を使用**して開く \) を選択します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio のユーザーは、Xcode を使用して、Mac Build Host を直接使用に切り替えることで、ユーザー インターフェイスを構築することができますも。
Visual Studio for Mac でソリューションを開き、Xcode ファイルを右クリックして、 **[Open With > Interface Builder]** を選択します。

-----

![](installation-images/openwith-xcode.png "Open the Interface.storyboard in Xcode Interface Builder")

Xcode を使用する場合は、通常の[iOS アプリのストーリーボード](~/ios/user-interface/storyboards/index.md)の場合と同じ手順に従う必要があります (たとえば、 **Ctrl キーを押しながら** **.h**ヘッダーファイルにドラッグして、コンセントとアクションを作成するなど)。

ストーリーボードを Interface Builder Xcode に保存すると、作成したアウトレットとアクションが watch 拡張プロジェクトC#の**designer.cs**ファイルに自動的に追加されます。

### <a name="adding-additional-screens-in-xcode"></a>Xcode で追加の画面を追加します。

Xcode を使用して (既定では、テンプレートに含まれているもの以外の) 画面をストーリーボードに追加する場合は Interface Builder 新しい各インターフェイスコントローラーの **C#コードファイルを手動で追加する必要があり**ます。

[新しいインターフェイスコントローラーをストーリーボードに追加する方法](~/ios/watchos/troubleshooting.md#add)については、詳細な手順を参照してください。

*Xamarin iOS Designer はこれを自動的に行いますが、手動の操作は必要ありません。*

## <a name="building"></a>ビルド

Watch アプリを含むプロジェクトは、その他の iOS プロジェクトと同様にビルドされます。 ビルド プロセスは、コードのないウォッチ アプリケーション (.app) を格納するウォッチ拡張機能 (.appex) を含む iPhone アプリケーション (.app) になります。

## <a name="launching"></a>起動します。

Studio (Mac Build Host で起動)、Visual Studio for Mac または Visual を使用して、シミュレーターで watch アプリを起動できます。

これには、WatchKit アプリを起動するための 2 つのモードがあります。

- 通常のアプリ モード (既定値) と
- [通知](~/ios/watchos/platform/notifications.md)(JSON 形式のテスト通知ペイロードを必要とします)。

### <a name="xcode-8-support"></a>Xcode 8 のサポート

Xcode 8 (またはそれ以降) がインストールされると Apple Watch シミュレーターは iOS シミュレーターとは別になります (*外部ディスプレイ*として表示される[Xcode 6](#xcode6)とは異なります)。
Watch App プロジェクトを選択してスタートアッププロジェクトにすると、シミュレーターの一覧に*iOS*シミュレーターが表示され、次のように選択できます。

[![](installation-images/xs-xcode8-watchos3-sml.png "Selecting the Simulator type")](installation-images/xs-xcode8-watchos3.png#lightbox)

デバッグを開始すると、iOS シミュレーター*と*Apple Watch シミュレーターの*2 つ*のシミュレーターが起動します。 **コマンド + Shift + H**を使用して、ウォッチ メニューと時計の表面に移動します。また、**ハードウェア** メニューを使用して、 **Force Touch の負荷**を設定します。 トラック パッドやマウスのスクロールには、デジタル クラウンを使用してシミュレートされます。

#### <a name="troubleshooting"></a>トラブルシューティング

対になっているウォッチがないシミュレーターを起動しようとすると、**アプリケーションの出力**に次のエラーが表示されます。

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

既定値が機能しない場合、シミュレーターの構成手順については、 [Apple のフォーラム](https://forums.developer.apple.com/thread/7783)を参照してください。

<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 と watchOS 1

アプリを実行またはデバッグする前に、*ウォッチ拡張機能プロジェクト*を**スタートアッププロジェクト**にする必要があります。 自体には、watch アプリを「起動」できないし、iOS アプリを選択する場合が起動します。 iOS シミュレーターでは通常どおりです。

既定では、watch アプリは、Visual Studio for Mac の**実行**コマンドまたは**デバッグ**コマンドから、通常の**アプリ**モード (概要、通知モードではありません) で開始されます。

Xcode 6 を使用している場合、iPhone 5、iPhone 5S、iPhone 6、および iPhone 6 Plus でのみ、 **Apple Watch-38mm**または**Apple Watch-42 mm** (Watch アプリケーションが表示される) の外部ディスプレイをアクティブにすることができます。

> [!NOTE]
> Xcode 6 を使用する場合、[ウォッチ] 画面が iOS シミュレーターに自動的に表示されないことに注意してください。
> **ハードウェア > 外部ディスプレイ** メニューを使用して、ウォッチ 画面を表示します。

<a name="custommodes" />

## <a name="launching-notification-mode"></a>通知モードを起動します。

コードで通知を処理する方法については、[[通知] ページ](~/ios/watchos/platform/notifications.md)を参照してください。

Visual Studio for Mac は、通知の通知_スタートアップモード_で watch アプリを起動できます。

Watch app プロジェクトを右クリックし、 **[> カスタム構成で実行]** を選択します。

[![](installation-images/runwith-customparams-sml.png "Running a Custom Configuration")](installation-images/runwith-customparams.png#lightbox)

これにより、 **[カスタムパラメーター]** ウィンドウが開きます。このウィンドウで**通知**を選択し、JSON ペイロードを指定できます。次に、 **[実行]** を押して、シミュレーターで watch アプリを起動します。

[![](installation-images/runwith-execargs-sml.png "Setting the Notification and Payload")](installation-images/runwith-execargs.png#lightbox)

## <a name="debugging"></a>デバッグ

デバッグは、Visual Studio for Mac と Visual Studio の両方でサポートされます。
通知モードでデバッグするときに、通知の JSON ファイルを指定してください。 このスクリーン ショットは、watch アプリで検出されるデバッグ ブレークポイントを示しています。

![](installation-images/debug-sml.png "This screenshot shows a debug breakpoint being hit in a watch app")

起動手順に従うと、 **IOS シミュレーター (watch)** で実行される watch アプリが作成されます。
通知モードでは、**デバッグ > システムログを開く** (**CMD +/** ) を選択し、コードで `Console.WriteLine` を使用できます。

### <a name="debugging-lifecycle-event-handlers"></a>ライフ サイクル イベント ハンドラーのデバッグ

<!--
To test the functionality in your  and 
  methods, use the **Hardware > Lock** command in the iOS Simulator.
  Locking will trigger the `DidDeactivate` method and the watch simulator
  will indicate that it has been locked. Swipe the iOS Simulator to unlock,
  which triggers the `WillActivate` method of the watch app.
-->

WatchOS テンプレートファイル (`InterfaceController`、`ExtensionDelegate`、`NotificationController`、`ComplicationController`など) には、既に実装されている必要なライフサイクルメソッドが付属しています。 イベントのライフサイクルを理解しやすくするために、`Console.WriteLine` 呼び出しを追加し、**アプリケーションの出力**を読み取ります。

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [最初のアプリの視聴に関するビデオ](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple の WatchKit のヒント](https://developer.apple.com/watchkit/tips/)
