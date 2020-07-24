---
title: watchOS のトラブルシューティング
description: このドキュメントでは、Xamarin を使用した watchOS 開発の既知の問題と回避策について説明します。 問題のあるイメージについて説明し、手動でインターフェイスコントローラーファイルを追加し、コマンドラインから watch アプリを起動するなどの操作を行います。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 497096e7f422e8337498339737ab304b0d896dfe
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938997"
---
# <a name="watchos-troubleshooting"></a>watchOS のトラブルシューティング

このページには、発生する可能性のある問題の追加情報と回避策が記載されています。

- [既知の問題](#knownissues)

- [アイコンイメージからアルファチャネルを削除する](#noalpha)

- Xcode Interface Builder の[インターフェイスコントローラーファイルを手動で追加](#add)します。

- [コマンドラインから WatchApp を起動](#command_line)します。

<a name="knownissues"></a>

## <a name="known-issues"></a>の既知の問題

### <a name="general"></a>全般

<a name="deploy"></a>

- 以前のリリースの Visual Studio for Mac では、 **AppleCompanionSettings**アイコンの1つが正しくないとして表示されます。これにより、App Store に送信しようとするとアイコンが表示されないという**エラーが発生**します。
    このアイコンは87x87 ピクセル (Retina 画面の場合は29単位) にする必要があり **@3x** ます。 Xcode のイメージ資産を編集するか、ファイル**のContents.js**を手動で編集する Visual Studio for Mac では、この問題を修正することはできません。

- Watch 拡張機能プロジェクト > の**WKApp バンドル id**が watch アプリの**バンドル id**と一致するように[正しく設定](~/ios/watchos/get-started/project-references.md)されていない場合、デバッガーは接続に失敗し、Visual Studio for Mac は *"デバッガーの接続を待機*しています" というメッセージを表示して待機します。

- デバッグは**通知**モードでサポートされていますが、信頼性が低い可能性があります。 再試行は機能する場合があります。 Watch アプリの**情報** `WKCompanionAppBundleIdentifier` が、iOS の親/コンテナーアプリのバンドル識別子 (iPhone で実行されるもの) に一致するように設定されていることを確認します。

- iOS デザイナーでは、表示や通知インターフェイスコントローラーのエントリポイント矢印は表示されません。

- ストーリーボードに2つを追加することはできません `WKNotificationControllers` 。
    回避策: `notificationCategory` ストーリーボード XML 内の要素は、常に同じで挿入されます `id` 。 この問題を回避するには、2つ (以上) の通知コントローラーを追加し、ストーリーボードファイルをテキストエディターで開き、 `id` 要素を一意になるように手動で変更します。

    [![ストーリーボードファイルをテキストエディターで開き、id 要素を手動で変更して一意にする](troubleshooting-images/duplicate-id-sml.png)](troubleshooting-images/duplicate-id.png#lightbox)

- アプリを起動しようとすると、"アプリケーションがビルドされていません" というエラーが表示されることがあります。 このエラーは、スタートアッププロジェクトが watch 拡張機能プロジェクトに設定されている場合、**クリーン**後に発生します。
    この問題を解決するには、[**ビルド > リビルド**] を選択し、アプリを再起動します。

### <a name="visual-studio"></a>Visual Studio

IOS Designer support for Watch Kit を使用するには、ソリューションが正しく構成されている*必要があり*ます。 プロジェクト参照が設定されていない場合 (「[参照の設定方法」を](~/ios/watchos/get-started/project-references.md)参照)、デザインサーフェイスは正しく機能しません。

<a name="noalpha"></a>

## <a name="removing-the-alpha-channel-from-icon-images"></a>アイコンイメージからアルファチャネルを削除する

アイコンには、アルファチャネルを含めることはできません (アルファチャネルはイメージの透明な領域を定義します)。それ以外の場合、アプリはアプリストアの送信中に拒否され、次のようなエラーが発生します。

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

**プレビュー**アプリを使用して Mac OS X のアルファチャネルを簡単に削除できます。

1. **プレビュー**でアイコン画像を開き、[**ファイル > エクスポート**] を選択します。

2. アルファチャネルが存在する場合、表示されるダイアログには [**アルファ**] チェックボックスが表示されます。

    ![アルファチャネルが存在する場合、表示されるダイアログには [アルファ] チェックボックスが表示されます。](troubleshooting-images/remove-alpha-sml.png)

3. **アルファ**チェックボックスを*オフにし*、正しい場所にファイルを**保存**します。

4. これで、アイコンイメージは Apple の検証チェックに合格します。

<a name="add"></a>

## <a name="manually-adding-interface-controller-files"></a>手動によるインターフェイスコントローラーファイルの追加

> [!IMPORTANT]
> Xamarin の WatchKit サポートには、iOS デザイナー (Visual Studio for Mac と Visual Studio の両方) で watch のストーリーボードをデザインする機能が含まれています。これについては、以下に説明する手順は必要ありません。 [Visual Studio for Mac Properties] パッドでインターフェイスコントローラーにクラス名を指定するだけで、C# コードファイルが自動的に作成されます。

Xcode Interface Builder を使用して*いる場合*は、次の手順に従って、watch アプリ用の新しいインターフェイスコントローラーを作成し、Xcode との同期を有効にすることで、C# でアウトレットとアクションを使用できるようにします。

1. **Xcode Interface Builder**で watch アプリのインターフェイスを開きます **。**

    ![Xcode Interface Builder でストーリーボードを開く](troubleshooting-images/add-6.png)

2. 新しいを `InterfaceController` ストーリーボードにドラッグします。

    ![InterfaceController](troubleshooting-images/add-1.png)

3. コントロールをインターフェイスコントローラーにドラッグできるようになりました (例 ラベルとボタン) を作成することはできません **。 h**ヘッダーファイルがないためです。 次の手順を実行すると、必要な **.h**ヘッダーファイルが作成されます。

    ![レイアウト内のボタン](troubleshooting-images/add-2.png)

4. ストーリーボードを閉じて Visual Studio for Mac に戻ります。 **Watch アプリ拡張機能**プロジェクトに新しい C# ファイル**MyInterfaceController.cs** (または好きな名前) を作成します (ストーリーボードがある watch アプリ自体ではありません)。 次のコードを追加します (名前空間、classname、コンストラクター名を更新します)。

    ```csharp
    using System;
    using WatchKit;
    using Foundation;

    namespace WatchAppExtension  // remember to update this
    {
        public partial class MyInterfaceController // remember to update this
        : WKInterfaceController
        {
            public MyInterfaceController // remember to update this
            (IntPtr handle) : base (handle)
            {
            }
            public override void Awake (NSObject context)
            {
                base.Awake (context);
                // Configure interface objects here.
                Console.WriteLine ("{0} awake with context", this);
            }
            public override void WillActivate ()
            {
                // This method is called when the watch view controller is about to be visible to the user.
                Console.WriteLine ("{0} will activate", this);
            }
            public override void DidDeactivate ()
            {
                // This method is called when the watch view controller is no longer visible to the user.
                Console.WriteLine ("{0} did deactivate", this);
            }
        }
    }
    ```

5. **Watch アプリ拡張機能**プロジェクトに別の新しい C# ファイル**MyInterfaceController.designer.cs**を作成し、次のコードを追加します。 名前空間、クラス名、および属性を必ず更新してください `Register` 。

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;

    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```

    > [!TIP]
    > このファイルを最初のファイルの子ノードにする (必要に応じて) Visual Studio for Mac Solution Pad 内の他の C# ファイルにドラッグすることもできます。 その後、次のように表示されます。

    ![ソリューションパッド](troubleshooting-images/add-5.png)

6. Xcode 同期で、使用した新しいクラス (属性を使用) が認識されるように、[**ビルド >** ビルド] を選択し `Register` ます。

7. [ウォッチ] アプリのストーリーボードファイルを右クリックし、[ **Open With > Xcode Interface Builder**] を選択して、ストーリーボードを再度開きます。

    ![Interface Builder でストーリーボードを開く](troubleshooting-images/add-6.png)

8. 新しいインターフェイスコントローラーを選択し、前の手順で定義したクラス名を指定します。例を示します。 `MyInterfaceController`.
    すべてが正常に動作している場合は、[**クラス:** ] ドロップダウンリストに自動的に表示され、そこから選択できます。

    ![カスタムクラスの設定](troubleshooting-images/add-4.png)

9. ストーリーボードとコードをサイドバイサイドで表示できるように、Xcode (2 つの重なり合う円があるアイコン) で**アシスタントエディター**ビューを選択します。

    ![アシスタントエディターのツールバー項目](troubleshooting-images/add-7.png)

    コードペインにフォーカスがある場合は、 **.h**ヘッダーファイルを参照していることを確認し、階層リンクバーを右クリックして適切なファイル (**MyInterfaceController**) を選択します。

    ![MyInterfaceController の選択](troubleshooting-images/add-8.png)

10. **Ctrl キーを押しながら**、ストーリーボードから **.h**ヘッダーファイルにドラッグして、コンセントとアクションを作成できるようになりました。

    ![アウトレットとアクションの作成](troubleshooting-images/add-9.png)

    ドラッグを解除すると、アウトレットまたはアクションを作成するかどうかを選択し、その名前を選択するように求められます。

    ![アウトレットとアクションダイアログ](troubleshooting-images/add-a.png)

11. ストーリーボードの変更が保存され、Xcode が閉じられたら、Visual Studio for Mac に戻ります。 このメソッドは、ヘッダーファイルの変更を検出し、 **designer.cs**ファイルにコードを自動的に追加します。

    ```csharp
    [Register ("MyInterfaceController")]
    partial class MyInterfaceController
    {
        [Outlet]
        WatchKit.WKInterfaceButton myButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (myButton != null) {
                myButton.Dispose ();
                myButton = null;
            }
        }
    }
    ```

C# では、コントロールを参照する (またはアクションを実装する) ことができるようになりました。

<a name="command_line"></a>

## <a name="launching-the-watch-app-from-the-command-line"></a>コマンドラインからの Watch アプリの起動

> [!IMPORTANT]
> 既定では、Watch アプリは通常のアプリモードで起動できます。また、Visual Studio for Mac と Visual Studio の[カスタム実行パラメーター](~/ios/watchos/get-started/installation.md#custommodes)を使用して、**ひとめ**でも**通知**モードでもかまいません。

コマンドラインを使用して iOS シミュレーターを制御することもできます。 Watch アプリを起動するために使用されるコマンドラインツールは**mtouch**です。

完全な例を次に示します (ターミナルで単一行として実行)。

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

アプリを反映するために更新する必要があるパラメーターは `launchsimwatch` 次のとおりです。

### <a name="--launchsimwatch"></a>--launchシム watch

*Watch アプリと拡張機能を含む iOS アプリの*メインアプリバンドルへの完全パスです。

> [!NOTE]
> 指定する必要のあるパスは、 *iPhone アプリケーションのアプリファイル*(iOS シミュレーターにデプロイされ、watch 拡張機能と watch アプリの両方が含まれているファイル) 用です。

例:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

## <a name="notification-mode"></a>通知モード

アプリの[**通知**モード](~/ios/watchos/platform/notifications.md)をテストするには、 `watchlaunchmode` パラメーターをに設定 `Notification` し、テスト通知ペイロードを含む JSON ファイルへのパスを指定します。

通知モードでは、ペイロードパラメーターが*必要*です。

たとえば、次の引数を mtouch コマンドに追加します。

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```

## <a name="other-arguments"></a>他の引数

残りの引数については、以下で説明します。

### <a name="--sdkroot"></a>--sdkroot

必須。 Xcode (6.2 以降) へのパスを指定します。

例:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--デバイス

実行するシミュレーターデバイス。 これは、特定のデバイスの udid を使用するか、またはランタイムとデバイスの種類の組み合わせを使用して、2つの方法で指定できます。

正確な値はコンピューターによって異なり、Apple の簡単な**ctl**ツールを使用してクエリを実行できます。

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

例:

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**ランタイムとデバイスの種類**

例:

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```

## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WatchTables (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
