---
title: "watchOS トラブルシューティング"
description: "既知の問題の解決策、watchOS 開発上の問題です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4a6b916f991b337d8a28764f1482ddd837bad460
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="watchos-troubleshooting"></a>watchOS トラブルシューティング

このページには、追加情報の解決策、開発中の機能が含まれています。 これらの回避策の一部は、このプレビュー リリースにのみ適用されます。

- [既知の問題](#knownissues)

- [アイコン イメージのアルファ チャンネルを削除します。](#noalpha)

- [インターフェイス コント ローラーのファイルを手動で追加する](#add)Xcode インターフェイスのビルダー。

- [コマンドラインから WatchApp を起動する](#command_line)です。

<a name="knownissues" />

## <a name="known-issues"></a>既知の問題

### <a name="general"></a>全般

<a name="deploy" />

- 以前のリリースの Visual Studio for Mac が正しく示されていないのいずれかの、 **AppleCompanionSettings** 88 x 88 (ピクセル) ですその結果として、アイコン、**アイコン エラーのない**アプリに送信しようとする場合。ストア。
    このアイコンは 87 x 87 ピクセルである必要があります (29 単位 **@3x**  Retina 画面)。 Mac で Xcode でのイメージ アセットを編集いずれかの Visual Studio でこれを修正または手動で編集することはできません、 **Contents.json**ファイル (一致するように[このサンプル](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132))。

- 場合ウォッチ拡張機能プロジェクトの**Info.plist > WKApp バンドル ID**は[正しく設定されている](~/ios/watchos/get-started/project-references.md)Watch アプリの一致するように**バンドル ID**デバッガーは接続に失敗し、VisualMac 用 studio が、メッセージで待機*「デバッガーの接続を待機している」*です。

- デバッグはサポートされて**通知**モードですが信頼できることができます。 再試行は動作場合があります。 いることを確認、Watch アプリの**Info.plist** `WKCompanionAppBundleIdentifier`親/コンテナーの iOS アプリのバンドル id が一致するように設定されている (ie。 iPhone で実行されている 1 つ)。

- iOS デザイナーでは、一目または通知インターフェイス コント ローラーのエントリ ポイントに表示する矢印は表示されません。

- 2 つ追加することはできません`WKNotificationControllers`ストーリー ボードにします。
    回避策:`notificationCategory`ストーリー ボード XML 内の要素は必ず同じ挿入`id`です。 2 つ (以上) の通知コント ローラーを追加、テキスト エディターで、ストーリー ボード ファイルを開くし、手動で変更は、この問題を回避する、`id`一意である要素。

    [![](troubleshooting-images/duplicate-id-sml.png "テキスト エディターでファイルを手動で一意である id 要素を変更、ストーリー ボードを開く")](troubleshooting-images/duplicate-id.png#lightbox)

- エラーが表示、"アプリケーションがビルドされていない"アプリを起動しようとしています。 これが発生した後、**クリーン**ウォッチ拡張機能プロジェクトをスタートアップ プロジェクトを設定するとします。
    修正プログラムは、選択**ビルド > すべてリビルド**し、再度アプリを起動します。

### <a name="visual-studio"></a>Visual Studio

ウォッチ キット用の iOS デザイナー サポート*必要があります*ソリューションを正しく構成されています。 プロジェクト参照が設定されていない場合 (を参照してください[の参照を設定する方法](~/ios/watchos/get-started/project-references.md)) デザイン サーフェイスが正しく機能しません。

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>アイコン イメージのアルファ チャンネルを削除します。

アイコンはアルファ チャネル (アルファ チャネルは、イメージの透明な領域を定義) を含めることはできません、それ以外の場合、次のようなエラーでアプリ ストアの送信中に、アプリが拒否されます。

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Mac OS X でアルファ チャネルを削除するは簡単、**プレビュー**アプリ。

1. アイコン イメージを開きます**プレビュー**を選択し**ファイル > エクスポート**です。

2. 表示されるダイアログ ボックスが含まれます、**アルファ**アルファ チャネルが存在する場合にチェック ボックスをオンします。

    ![](troubleshooting-images/remove-alpha-sml.png "表示されるダイアログ ボックスは、アルファ チャネルが存在する場合、アルファのチェック ボックスをオンに含まれます")

3. *Untick* 、**アルファ** チェック ボックスと**保存**正しい場所にファイル。

4. アイコン イメージでは、Apple の検証チェックが合格しました必要があります。


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>インターフェイス コント ローラーのファイルを手動で追加します。

> [!IMPORTANT]
> Xamarin の WatchKit サポートには、次の手順を必要としない iOS デザイナー (Visual Studio for Mac と Visual Studio の両方でウォッチ ストーリー ボードのデザインが含まれています。 単にインターフェイスのコント ローラー、Visual Studio for Mac プロパティ パッドと自動的に作成されるファイルの c# コードでクラス名を付けます。


*場合*Xcode インターフェイスのビルダーを使用して、新しいインターフェイスのコント ローラーをウォッチ アプリを作成し、コンセントとアクションは、c# で使用できるように、Xcode との同期を有効にするには、次の手順に従います。

1. アプリを開き、ウォッチの**Interface.storyboard**で**Xcode インターフェイス ビルダー**です。
    
    ![](troubleshooting-images/add-6.png "Xcode インターフェイス ビルダーで、ストーリー ボードを開く")

2. 新しいドラッグ`InterfaceController`ストーリー ボード上に。

    ![](troubleshooting-images/add-1.png "InterfaceController")

3. 今すぐにドラッグできるコントロール インターフェイス コント ローラー ( ラベルやボタンなど) は作成できませんコンセントまたはアクション、まだがあるためありません**.h**ヘッダー ファイルです。 次の手順と、必要な**.h**ヘッダー ファイルを作成します。

    ![](troubleshooting-images/add-2.png "レイアウト内のボタン")

4. ストーリー ボードを閉じて、for mac を Visual Studio に戻る 新しい c# ファイルを作成**MyInterfaceController.cs** (または任意の名前) で、**アプリ拡張機能を見る**プロジェクト (、ウォッチ アプリケーションではなく、ストーリー ボードは、ここで自体)。 (名前空間、クラス名、およびコンス トラクターの名前を更新)、次のコードを追加します。

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

5. 別の新しい c# ファイルを作成する**MyInterfaceController.designer.cs**で、**アプリ拡張機能を見る**プロジェクトし、次のコードを追加します。 名前空間、クラス名を更新することを確認して、`Register`属性。

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
    
    ヒント: することができます (省略可能) このファイルの最初のファイルの子ノードをドラッグして、その他の c#、Visual Studio でファイル Mac ソリューション パッド用です。 次のように表示されます。
    
    ![](troubleshooting-images/add-5.png "ソリューションのパッド")

6. 選択**ビルド > すべてビルド**Xcode 同期は、新しいクラスを認識できるように (を使用して、`Register`属性) を使用しています。

7. Watch アプリのストーリー ボード ファイルを右クリックしを選択すると、ストーリー ボードを再度開いて**ファイルを開く > Xcode インターフェイス ビルダー**:

    ![](troubleshooting-images/add-6.png "インターフェイスのビルダーで、ストーリー ボードを開く")

8. 新しいインターフェイス コント ローラーを選択し、上記で定義したなどのクラス名を付けます。 `MyInterfaceController`。
すべてが正しく機能場合、表示されるはずで自動的に、**クラス:**ドロップ ダウン リストと、そこから選択することができます。

    ![](troubleshooting-images/add-4.png "カスタム クラスを設定")

9. 選択、**アシスタント エディター**ストーリー ボードと、コード サイド バイ サイドを認識できるように、Xcode (2 つの重なり合う円のアイコン) で表示します。

    ![](troubleshooting-images/add-7.png "アシスタント エディターのツールバー項目")

    コード ペインで、フォーカスがある場合は、見ていることを確認、 **.h**ヘッダー ファイル、および場合は、階層リンク バーを右クリックし、正しいファイルを選択できません (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "MyInterfaceController を選択します。")

10. コンセントと別のアクションを作成できます**ctrl キーを押しながらドラッグ**にストーリー ボードから、 **.h**ヘッダー ファイルです。

    ![](troubleshooting-images/add-9.png "コンセントとアクションの作成")

    ドラッグを解放するときに、コンセントまたはアクションを作成するかどうかを選択するように求められ、その名前を選択します。

    ![](troubleshooting-images/add-a.png "コンセントとアクションのダイアログ ボックス")

11. ストーリー ボード変更が保存され、Xcode を閉じて後、for mac Visual Studio に戻る ヘッダー ファイルの変更を検出して自動的にコードを追加して、 **. designer.cs**ファイル。


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


これでコントロールを参照 (したりアクションを実装) (C#) です。


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>コマンドラインから Watch アプリを起動します。

> [!IMPORTANT]
> 既定とでは、通常のアプリのモードで Watch アプリを起動できます**Glance**または**通知**モードを使用して[カスタム実行パラメーター](~/ios/watchos/get-started/installation.md#custommodes) Mac 用の Visual Studio で、Visual Studio です。


また、iOS シミュレーターを制御するのにコマンドラインを使用することができます。 Watch アプリの起動に使ったコマンド ライン ツールは**mtouch**です。

(端末の 1 つの行として実行) 完全な例を次に示します。

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

アプリを反映するように更新する必要がありますパラメーターは`launchsimwatch`:。

### <a name="--launchsimwatch"></a>--launchsimwatch

メインのアプリ バンドルの完全パス*watch アプリと拡張機能を含む iOS アプリの*します。

> [!NOTE]
> パスを指定する必要がありますが、 *iPhone アプリケーション .app ファイル*、つまり、1 つ、iOS シミュレーターにデプロイされ、ウォッチ拡張機能と watch アプリの両方を格納しています。

例:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>通知モード

アプリのテストに[**通知**モード](~/ios/watchos/platform/notifications.md)設定してください、`watchlaunchmode`パラメーターを`Notification`し、テスト通知のペイロードを含む JSON ファイルへのパスを指定します。

ペイロード パラメーターは*必要*通知モードにします。

たとえば、これらの引数を mtouch コマンドに追加します。

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>その他の引数

残りの引数は以下について説明します。

### <a name="--sdkroot"></a>--sdkroot

必須。 Xcode (6.2 以降) へのパスを指定します。

例:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--device

実行するシミュレーター デバイスです。 これは、特定のデバイスの udid を使用するか、ランタイムとデバイスの種類の組み合わせを使用して 2 つの方法で指定できます。

正確な値は、マシン間で変化し、Apple を使用して照会できます**simctl**ツール。

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

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (サンプル)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
