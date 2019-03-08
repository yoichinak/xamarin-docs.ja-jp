---
title: watchOS トラブルシューティング
description: このドキュメントでは、既知の問題と Xamarin で watchOS の開発の回避策について説明します。 コマンドラインで watch アプリの起動、インターフェイス コント ローラーのファイルを手動で追加の問題などを使用したイメージがについて説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 70ef341c066c77e214761d75c173faef00266e4c
ms.sourcegitcommit: 2713f2c1d74e3582704c3d0ca65b6651119ed489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/15/2019
ms.locfileid: "56321156"
---
# <a name="watchos-troubleshooting"></a>watchOS トラブルシューティング

このページには、追加情報と発生する問題の回避策が含まれています。

- [既知の問題](#knownissues)

- [アイコン イメージのアルファ チャネルを削除します。](#noalpha)

- [インターフェイス コント ローラーのファイルを手動で追加](#add)Xcode の Interface Builder の。

- [コマンドラインから WatchApp を起動する](#command_line)します。

<a name="knownissues" />

## <a name="known-issues"></a>既知の問題

### <a name="general"></a>全般

<a name="deploy" />

- 以前のリリースの Visual Studio for Mac が正しく示されるしないのいずれか、 **AppleCompanionSettings**アイコン 88 x 88 ピクセルです。 その結果として、**アイコン エラーのない**App Store に送信しようとした場合。
    このアイコンは 87 x 87 ピクセルである必要があります (29 単位 **@3x** Retina 画面)。 Visual studio for Mac の Xcode 内のイメージ アセットを編集、これを修正または手動で編集することはできません、 **Contents.json**ファイル (一致するように[このサンプル](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132))。

- 場合ウォッチ拡張機能プロジェクトの**Info.plist > WKApp バンドル ID**でない[正しく設定されている](~/ios/watchos/get-started/project-references.md)Watch アプリの一致するように**バンドル ID**デバッガーは、接続に失敗し、VisualStudio for Mac は、メッセージを待機 *「デバッガーの接続を待機している」* します。

- デバッグがサポートされている**通知**モードが信頼できることができます。 再試行は機能場合があります。 確認します、Watch アプリの**Info.plist** `WKCompanionAppBundleIdentifier` iOS 親/コンテナー アプリのバンドル識別子が一致するように設定されている (つまり。 iPhone で実行されている 1 つ)。

- iOS Designer では、entrypoint 概要または通知インターフェイス コント ローラーに表示する矢印は表示されません。

- 2 つ追加することはできません`WKNotificationControllers`ストーリー ボードにします。
    回避策:`notificationCategory`ストーリー ボードの XML 内の要素は常に同じ挿入`id`します。 2 つ (以上) の通知コント ローラーを追加、テキスト エディターで、ストーリー ボード ファイルを開くし、手動で変更は、この問題を回避する、`id`一意である要素。

    [![](troubleshooting-images/duplicate-id-sml.png "ファイルはテキスト エディターで、ストーリー ボードを開くと、一意である id 要素を手動で変更")](troubleshooting-images/duplicate-id.png#lightbox)

- "アプリケーションがビルドされていない"、エラーが表示、アプリを起動しようとしています。 これが発生した後、**クリーン**ウォッチ拡張機能プロジェクトをスタートアップ プロジェクトを設定するとします。
    修正を選択するには**ビルド > すべてリビルド**と、アプリを再度起動します。

### <a name="visual-studio"></a>Visual Studio

IOS Designer をウォッチ キット サポート*必要があります*を正しく構成するソリューション。 プロジェクト参照が設定されていない場合 (を参照してください[セットへの参照方法](~/ios/watchos/get-started/project-references.md)) デザイン画面が正しく機能しません。

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>アイコン イメージのアルファ チャネルを削除します。

アイコンがアルファ チャネルが (アルファ チャネルでは、イメージの透明な領域を定義します) を含めることはできません、それ以外の場合、アプリは次のようなエラーでのアプリ ストアの送信中に拒否されます。

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Mac OS X を使用してアルファ チャネルを削除するは簡単、**プレビュー**アプリ。

1. アイコン イメージを開きます**プレビュー**選び、**ファイル > エクスポート**します。

2. 表示されるダイアログ ボックスが含まれます、**アルファ**アルファ チャネルが存在する場合にチェック ボックスをオンします。

    ![](troubleshooting-images/remove-alpha-sml.png "表示されるダイアログ ボックスがアルファ チャネルが存在する場合にアルファのチェック ボックスが含まれます")

3. *Untick* 、**アルファ**チェック ボックスと**保存**を正しい場所にファイル。

4. アイコン イメージでは、Apple の検証チェックを渡す必要がありますようになりました。


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>インターフェイス コント ローラーのファイルを手動で追加します。

> [!IMPORTANT]
> Xamarin の WatchKit サポートには、以下の手順が必要としない iOS デザイナー (Visual Studio for Mac と Visual Studio の両方) でウォッチのストーリー ボードのデザインが含まれます。 単に、インターフェイス コント ローラーに名前を付けますクラス、Visual studio Mac プロパティ パッドのC#コード ファイルが自動的に作成されます。


*場合*Xcode の Interface Builder を使用している、watch アプリ用の新しいインターフェイス コント ローラーを作成し、outlet と action をで使用できるようには、Xcode との同期を有効にするには、この手順に従ってC#:

1. Watch アプリの開く**Interface.storyboard**で**Xcode の Interface Builder**します。
    
    ![](troubleshooting-images/add-6.png "Xcode の Interface Builder のストーリー ボードを開く")

2. 新しいドラッグ`InterfaceController`ストーリー ボード上に。

    ![](troubleshooting-images/add-1.png "InterfaceController")

3. (例: 今すぐインターフェイス コント ローラーにこのコントロールをドラッグすることができます。 ラベルやボタンなど) が作成できません outlet またはアクションが存在しない **.h**ヘッダー ファイル。 次の手順と、必要な **.h**ヘッダー ファイルを作成します。

    ![](troubleshooting-images/add-2.png "レイアウトのボタン")

4. ストーリー ボードを閉じて、Visual studio for mac を返す 新規作成C#ファイル**MyInterfaceController.cs** (または任意の名前) で、**アプリ拡張機能を見る**プロジェクト (ストーリー ボードは自体 watch アプリされません)。 (名前空間、クラス名、およびコンス トラクターの名前を更新しています)、次のコードを追加します。

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

5. 別の新しい作成C#ファイル**MyInterfaceController.designer.cs**で、**アプリ拡張機能を見る**プロジェクトし、次のコードを追加します。 名前空間、クラス名を更新してください、`Register`属性。

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
    
    ヒント :このファイルをもう一方にドラッグすることによって、最初のファイルの子ノードを (オプション) することができますC#Mac Solution Pad の Visual Studio でのファイル。 次のように表示されます。
    
    ![](troubleshooting-images/add-5.png "Solution pad")

6. 選択**ビルド > すべてビルド**Xcode 同期によって認識され、新しいクラス (を使用して、`Register`属性) を使用しています。

7. Watch アプリのストーリー ボード ファイルを右クリックして、ストーリー ボードを再び開く**プログラムから開く > Xcode の Interface Builder**:

    ![](troubleshooting-images/add-6.png "インターフェイス ビルダーで、ストーリー ボードを開く")

8. 新しいインターフェイス コント ローラーを選択し、例: 上記で定義したクラス名を付けます。 `MyInterfaceController`。
すべてが正しく機能場合、表示されるはずで自動的に、**クラス:** ドロップダウン リストと、そこから選択することができます。

    ![](troubleshooting-images/add-4.png "カスタム クラスの設定")

9. 選択、**アシスタント エディター** Xcode (2 つの重なり合う円のアイコン) で確認するため、ストーリー ボードとコードで並列に確認できます。

    ![](troubleshooting-images/add-7.png "アシスタント エディターのツールバー項目")

    コード ペインで、フォーカスがある場合を見ていることを確認、 **.h**ヘッダー ファイル、および階層リンク バーを右クリックし、正しいファイルを選択しない場合 (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "MyInterfaceController を選択します。")

10. Outlet と action で作成できます**Ctrl + ドラッグ**にストーリー ボードから、 **.h**ヘッダー ファイル。

    ![](troubleshooting-images/add-9.png "Outlet と action を作成します。")

    ドラッグをリリースするときに、アウトレットまたはアクションを作成するかどうかを選択するように求められ、その名前を選択します。

    ![](troubleshooting-images/add-a.png "コンセントとアクションのダイアログ ボックス")

11. ストーリー ボードの変更を保存し、Xcode が閉じている、Visual Studio for mac に戻ります ヘッダー ファイルの変更を検出するコードを自動的に追加し、 **. designer.cs**ファイル。


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


これでコントロールを参照 (したりアクションを実装) でC#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>コマンドラインから、Watch アプリを起動します。

> [!IMPORTANT]
> 既定とでも、通常のアプリのモードで、Watch アプリを起動できます**概要**または**通知**モードを使用して[カスタム実行パラメーター](~/ios/watchos/get-started/installation.md#custommodes) Visual Studio for Mac で、Visual Studio。


IOS シミュレーターを制御するのにコマンドラインを使用することもできます。 Watch アプリを起動するために使用するコマンド ライン ツールは**mtouch**します。

(ターミナルでの単一行として実行) の完全な例を次に示します。

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

アプリを反映するように更新する必要があるパラメーターが`launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

メイン アプリケーション バンドルへの完全パス*watch アプリと拡張機能を含む iOS アプリの*します。

> [!NOTE]
> パスを指定する必要がありますが、 *iPhone アプリケーションの .app ファイル*、iOS シミュレーターにデプロイされ、ウォッチ拡張機能と watch アプリの両方を格納しているものなどです。

例:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>通知モード

アプリのテストに[**通知**モード](~/ios/watchos/platform/notifications.md)設定、`watchlaunchmode`パラメーターを`Notification`し、テスト通知のペイロードを含む JSON ファイルへのパスを指定します。

ペイロードのパラメーターが*必要*通知モードにします。

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

### <a name="--device"></a>-デバイス

実行するシミュレーター デバイスです。 これは、特定のデバイスの udid を使用するか、ランタイムとデバイスの種類の組み合わせを使用して、2 つの方法で指定できます。

正確な値が、マシンの間で変化し、Apple を使用して照会できます**simctl**ツール。

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
