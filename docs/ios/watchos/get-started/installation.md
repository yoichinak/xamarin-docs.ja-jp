---
title: Xamarin での watchOS のインストールと使用
description: このドキュメントでは、Xamarin で watchOS をインストールして使用する方法について説明します。 インストール、watchOS プロジェクトの構造、iOS デザイナーの使用方法、Xcode の統合について説明し、トラブルシューティングのヒントを提供します。
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/05/2017
ms.openlocfilehash: 4cc321f44238a7b738e40c02656b42f1eda1155a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938763"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>Xamarin での watchOS のインストールと使用

watchOS 4 では、Xcode 9 の macOS Sierra (10.12) が必要です。

watchOS 1 では、Xcode 7 で OS X ヨーク Semite (10.10) が必要でした。

> [!WARNING]
> [2018 年4月1日以降、watchOS 1 の更新は受け付けられません](https://developer.apple.com/news/?id=11162017a)。 今後の更新では、watchOS 2 SDK 以降を使用する必要があります。watchOS 4 SDK を使用してビルドすることをお勧めします。

## <a name="project-structure"></a>プロジェクト構造

Watch アプリは、次の3つのプロジェクトで構成されています。

- **Xamarin ios アプリプロジェクト**-これは通常の iphone プロジェクトであり、Xamarin の ios テンプレートのいずれでもかまいません。 Watch アプリとその拡張機能は、このメインプロジェクト内にバンドルされます。

- **Watch Extension プロジェクト**-watch アプリのコード (コントローラークラスなど) が含まれています。

- **アプリプロジェクトを見る**: これには、watch アプリのすべての UI リソースを含むユーザーインターフェイスストーリーボードファイルが含まれています。

[Watch Kit Catalog のサンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)ソリューションは、Xamarin. Studio で次のようになります。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![Visual Studio のソリューション](installation-images/catalog-solution.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Visual Studio のソリューション](installation-images/catalog-solution-vs.png)

-----

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルをダウンロードして実行し、作業を開始します。
サンプルの画面は、[[コントロール](~/ios/watchos/user-interface/index.md)] ページにあります。

## <a name="creating-a-new-project"></a>新規プロジェクトの作成

新しい "ウォッチソリューション" を作成することはできません...代わりに、既存の iOS アプリケーションに Watch アプリを追加できます。 Watch アプリを作成するには、次の手順に従います。

1. 既存のプロジェクトがない場合は、まず [**ファイル > 新しいソリューション**] を選択し、iOS アプリを作成します ( **1 つのビューアプリ**など)。

    [![[ファイル > 新しいソリューションを選択し、iOS アプリを作成する]](installation-images/cycle8-2-sml.png)](installation-images/cycle8-2.png#lightbox)

2. IOS アプリが作成されたら (または、既存の iOS アプリを使用する予定の場合)、ソリューションを右クリックし、[追加]、[**新しいプロジェクトの追加 >**] の順に選択します。[**新しいプロジェクト**] ウィンドウで、[ **watchOS > App > WatchKit app**] を選択します。

    [![[WatchOS > App > WatchKit App] を選択します。](installation-images/cycle8-6-sml.png)](installation-images/cycle8-6.png#lightbox)

3. 次の画面では、ウォッチアプリを含める iOS アプリプロジェクトを選択できます。

    [![ウォッチアプリを含める iOS アプリプロジェクトを選択します](installation-images/cycle8-7-sml.png)](installation-images/cycle8-7.png#lightbox)

4. 最後に、プロジェクトを保存する場所 (および必要に応じてソース管理を有効にする) を選択します。

    [![プロジェクトを保存する場所を選択します](installation-images/cycle8-8-sml.png)](installation-images/cycle8-8.png#lightbox)

5. Visual Studio for Mac により、[プロジェクト参照と**情報 plist**設定](~/ios/watchos/get-started/project-references.md)が自動的に構成されます。

## <a name="creating-the-watch-user-interface"></a>Watch ユーザーインターフェイスの作成

<a name="designer"></a>

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS Designer の使用

[ウォッチ] アプリの [**インターフェイス**] をダブルクリックして、iOS Designer を使用して編集します。 [**ツールボックス**] から、[インターフェイスコントローラー] と [UI コントロール] をストーリーボードにドラッグして、[**プロパティ**] パッドを使用して構成することができます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![デザイナーのストーリーボード](installation-images/iosdesigner-sml.png)](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![デザイナーのストーリーボード](installation-images/iosdesigner-sml-vs.png)](installation-images/iosdesigner-vs.png#lightbox)

-----

新しい各インターフェイスコントローラーを選択し、[**プロパティ**] パッドに名前を入力することで、**クラス**を指定する必要があります (これにより、必要な C# 分離コードファイルが自動的に作成されます)。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![各新しいインターフェイスコントローラーにクラスを指定する](installation-images/iosdesigner-classname.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![各新しいインターフェイスコントローラーにクラスを指定する](installation-images/iosdesigner-classname-vs.png)

-----

**Ctrl キーを押しながら**、ボタン、テーブル、またはインターフェイスコントローラーから別のインターフェイスコントローラーにドラッグして、セグエを作成します。

### <a name="using-xcode-on-the-mac"></a>Mac での Xcode の使用

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Xcode を使用してユーザーインターフェイスを作成し続けるには、Xcode ファイルを右クリックし、[ **Open With >] \ (Interface Builder を使用**して開く \) を選択します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

また、Visual Studio ユーザーは、Xcode を使用して、Mac ビルドホストを直接使用するように切り替えてユーザーインターフェイスを構築することもできます。
Visual Studio for Mac でソリューションを開き、Xcode ファイルを右クリックして、[ **Open With > Interface Builder**] を選択します。

-----

![Xcode で Interface Builder を開きます。](installation-images/openwith-xcode.png)

Xcode を使用する場合は、通常の[iOS アプリのストーリーボード](~/ios/user-interface/storyboards/index.md)の場合と同じ手順に従う必要があります (たとえば、 **Ctrl キーを押しながら** **.h**ヘッダーファイルにドラッグして、コンセントとアクションを作成するなど)。

ストーリーボードを Interface Builder Xcode に保存すると、作成したアウトレットとアクションが watch 拡張プロジェクトの**designer.cs**ファイルに自動的に追加されます。

### <a name="adding-additional-screens-in-xcode"></a>Xcode でのその他の画面の追加

Xcode を使用してストーリーボードに追加の画面 (既定では、テンプレートに含まれているもの以外) を追加する場合は、新しいインターフェイスコントローラーごとに**C# コードファイルを手動で追加する必要があり**Interface Builder。

[新しいインターフェイスコントローラーをストーリーボードに追加する方法](~/ios/watchos/troubleshooting.md#add)については、詳細な手順を参照してください。

*Xamarin iOS Designer はこれを自動的に行いますが、手動の操作は必要ありません。*

## <a name="building"></a>ビルド

Watch アプリを含むプロジェクトは、他の iOS プロジェクトと同様にビルドされます。 ビルドプロセスによって、ウォッチ拡張機能 (appex) を含む iPhone アプリケーション (app.config) が作成されます。このアプリケーションには、コードのない watch アプリケーション (app.config) が含まれています。

## <a name="launching"></a>起動

Visual Studio for Mac または Visual Studio (Mac ビルドホストで開始) を使用して、シミュレーターで watch アプリを起動できます。

WatchKit アプリを起動するには、次の2つのモードがあります。

- 通常のアプリモード (既定)、および
- [通知](~/ios/watchos/platform/notifications.md)(JSON 形式のテスト通知ペイロードを必要とします)。

### <a name="xcode-8-support"></a>Xcode 8 のサポート

Xcode 8 (またはそれ以降) がインストールされると Apple Watch シミュレーターは iOS シミュレーターとは別になります (*外部ディスプレイ*として表示される[Xcode 6](#xcode6)とは異なります)。
Watch App プロジェクトを選択してスタートアッププロジェクトにすると、シミュレーターの一覧に*iOS*シミュレーターが表示され、次のように選択できます。

[![シミュレーターの種類の選択](installation-images/xs-xcode8-watchos3-sml.png)](installation-images/xs-xcode8-watchos3.png#lightbox)

デバッグを開始すると、iOS シミュレーター*と*Apple Watch シミュレーターの*2 つ*のシミュレーターが起動します。 **コマンド + Shift + H**を使用して、[ウォッチ] メニューと時計の表面に移動します。また、[**ハードウェア**] メニューを使用して、 **Force Touch の負荷**を設定します。 トラックパッドまたはマウスをスクロールすると、Digital Crown の使用がシミュレートされます。

#### <a name="troubleshooting"></a>トラブルシューティング

対になっているウォッチがないシミュレーターを起動しようとすると、**アプリケーションの出力**に次のエラーが表示されます。

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

既定値が機能しない場合、シミュレーターの構成手順については、 [Apple のフォーラム](https://forums.developer.apple.com/thread/7783)を参照してください。

<a name="xcode6"></a>

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 と watchOS 1

アプリを実行またはデバッグする前に、*ウォッチ拡張機能プロジェクト*を**スタートアッププロジェクト**にする必要があります。 Watch アプリ自体を "起動" することはできません。 iOS アプリを選択すると、iOS シミュレーターでは通常どおり起動します。

既定では、watch アプリは、Visual Studio for Mac の**実行**コマンドまたは**デバッグ**コマンドから、通常の**アプリ**モード (概要、通知モードではありません) で開始されます。

Xcode 6 を使用している場合、iPhone 5、iPhone 5S、iPhone 6、および iPhone 6 Plus でのみ、 **Apple Watch-38mm**または**Apple Watch-42 mm** (Watch アプリケーションが表示される) の外部ディスプレイをアクティブにすることができます。

> [!NOTE]
> Xcode 6 を使用する場合、[ウォッチ] 画面が iOS シミュレーターに自動的に表示されないことに注意してください。
> [**ハードウェア > 外部ディスプレイ**] メニューを使用して、[ウォッチ] 画面を表示します。

<a name="custommodes"></a>

## <a name="launching-notification-mode"></a>通知モードの起動

コードで通知を処理する方法については、[[通知] ページ](~/ios/watchos/platform/notifications.md)を参照してください。

Visual Studio for Mac は、通知の通知_スタートアップモード_で watch アプリを起動できます。

Watch app プロジェクトを右クリックし、[ **> カスタム構成で実行**] を選択します。

[![カスタム構成の実行](installation-images/runwith-customparams-sml.png)](installation-images/runwith-customparams.png#lightbox)

これにより、[**カスタムパラメーター** ] ウィンドウが開きます。このウィンドウで**通知**を選択し、JSON ペイロードを指定できます。次に、[**実行**] を押して、シミュレーターで watch アプリを起動します。

[![通知とペイロードの設定](installation-images/runwith-execargs-sml.png)](installation-images/runwith-execargs.png#lightbox)

## <a name="debugging"></a>デバッグ

デバッグは Visual Studio for Mac と Visual Studio の両方でサポートされています。
通知モードでデバッグする場合は、通知 JSON ファイルを必ず指定してください。 このスクリーンショットは、ウォッチアプリでヒットしているデバッグブレークポイントを示しています。

![このスクリーンショットは、ウォッチアプリでヒットしているデバッグブレークポイントを示しています。](installation-images/debug-sml.png)

起動手順に従うと、 **IOS シミュレーター (watch)** で実行される watch アプリが作成されます。
通知モードでは、[デバッグ] を選択して**システムログ**(**CMD +/**) を開き、コードでを使用でき > `Console.WriteLine` ます。

### <a name="debugging-lifecycle-event-handlers"></a>デバッグライフサイクルイベントハンドラー

<!--
To test the functionality in your  and 
  methods, use the **Hardware > Lock** command in the iOS Simulator.
  Locking will trigger the `DidDeactivate` method and the watch simulator
  will indicate that it has been locked. Swipe the iOS Simulator to unlock,
  which triggers the `WillActivate` method of the watch app.
-->

WatchOS テンプレートファイル (、、、など) には、 `InterfaceController` `ExtensionDelegate` `NotificationController` `ComplicationController` 既に実装されている必要なライフサイクルメソッドが付属しています。 `Console.WriteLine`イベントのライフサイクルを理解しやすくするために、呼び出しを追加し、**アプリケーションの出力**を読み取ります。

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [最初のアプリの視聴に関するビデオ](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple の WatchKit のヒント](https://developer.apple.com/watchkit/tips/)
