---
title: こんにちは, ウォッチ
description: Xamarin と watchOS の概要
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 2281fa801d32e8d8934767ae090503ca523d7eff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="hello-watch"></a>こんにちは, ウォッチ

」の手順に従って、ソリューションを作成した後[セットアップとインストール](~/ios/watchos/get-started/installation.md)、3 つのプロジェクトになります。

- セットアップまたは他のデバイス上の管理タスクに使用する iOS 親アプリ。 (IOS の拡張機能の他の種類では、これは多くの場合と呼びます"Container"アプリです。)Watch アプリを使用した可能性がありますせず Watch アプリの実行を開始するユーザーの**れた**; 親アプリケーションの実行
- Watch アプリ; のプログラム コードが含まれ、ウォッチ拡張機能そして
- Watch アプリをウォッチ上に描画されるストーリー ボードとイメージのリソースを保持します。

確認、[の参照が正しく](~/ios/watchos/get-started/project-references.md): 親アプリケーションが、拡張機能への参照であると、拡張機能が、Watch アプリへの参照であります。

バンドル識別子は、次のことを確認、 \*.watchkitextension \*.watchkitapp 規約こと、および拡張機能の Info.plist ファイルがそのが**WKApp バンドル ID**値のバンドル Id に設定Watch アプリ。

ここで、ウォッチ アプリを実行することができますへの通知できる場合、Watch アプリ内でのストーリー ボード ファイルは空白であるためにはありません。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/projectstructure.png "ソリューション エクスプ ローラー")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "ソリューション エクスプ ローラー")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
Xamarin iOS デザイナーを起動する Watch アプリで Interface.storyboard をダブルクリックして (Mac の場合もを右クリックし、**ファイルを開く > Xcode インターフェイス ビルダー**)


1.  確認してください、**ツールボックス**と**プロパティ**パッドは、表示
1.  クリックして、インターフェイスのコント ローラーを選択
1.  識別子とインターフェイスのコント ローラーのタイトルを設定**interfaceController**と**やあウォッチ**、
1.  確認、**クラス**に設定されている**InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "InterfaceController とやあウォッチに識別子とインターフェイス コント ローラーのタイトルを設定します。")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin iOS Visual Studio のデザイナーで編集する Watch アプリで Interface.storyboard 上をダブルクリックします。

1.  プロパティ ペインを開く
1.  クラスを変更**InterfaceController**です。
1.  インターフェイス コント ローラーの; をクリックします。そして
1.  識別子とインターフェイスのコント ローラーのタイトルを設定**interfaceController**と**やあウォッチ**です。

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "InterfaceController とやあウォッチに識別子とインターフェイス コント ローラーのタイトルを設定します。")

-----


UI を作成します。

1. **ツールボックス**パッド、
1. ドラッグ アンド ドロップ、**ボタン**と**ラベル**シーンにし、
1. テキストおよびように、コントロールの属性を設定します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/draganddrop.png "テキストとように、コントロールの属性を設定します。")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "テキストとように、コントロールの属性を設定します。")

-----

1. 設定、**名前**で、**プロパティ**各コントロールのパッド。 この例では、使用した`myButton`と`myLabel`です。

1. ストーリー ボード上のボタンをオンにして、**プロパティ**パッドの**イベント**一覧表示し、

1. 新規作成**アクション**」と入力して`OnButtonPress`キーを押して**Enter**です。
  一覧で、アクションが表示され、C# の場合は、部分メソッドが自動的に作成されます。

![](hello-watch-images/buttonaction.png "OnButtonPress アクション ボタンを追加")

ストーリー ボードを保存した後、 **InterfaceController.designer.cs**コントロール名とアクションが更新されます. 更新された後にこのファイルを開くことがわかりますが、どのように`RegisterAttribute`コント ローラーと UI コントロールが付いている c# インスタンス変数に対応する方法に対応する、`OutletAttribute`でタグ付けの操作が部分メソッドにマップする方法と、 `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

開くように**InterfaceController.cs** (*いない*InterfaceController.designer.cs) し、次のコードを追加します。

```csharp
int clickCount = 0;

partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}

```

このコードはきわめて透過的にする必要があります。 インスタンス変数`clickCount`は関数をたびにインクリメントされます`OnButtonPress`と呼びます。 テキスト`myLabel`; この数を反映するように変更されました`myLabel`、もちろんは XCode で作成したコンセントのいずれかの名前。 `partial`関数は、指定したアクションの名前に関連付けられている関数の実装です。

既ににはそれがスタートアップ プロジェクトになることがない場合

1. ウォッチ拡張機能プロジェクトを右クリックし、選択**スタートアップ プロジェクトとして設定**、

1. IPhone 6 iOS 8.2) など、ウォッチ キットと互換性のあるシミュレーター イメージに、配置ターゲットを設定します。

1. キーを押して、**デバッグ**をビルドし、シミュレーターの起動をトリガーするボタンをクリックします。

    [![](hello-watch-images/readytodebug-sml.png "Visual Studio のインターフェイス要素")](hello-watch-images/readytodebug.png#lightbox)

シミュレーターを起動すると、ラベルをインクリメントするボタンを押します。
おめでとう、思います自分で Watch アプリです。

![](hello-watch-images/running.png "アプリをシミュレーターで実行されています。")


## <a name="related-links"></a>関連リンク

- [作業の開始 (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [セットアップとインストール](~/ios/watchos/get-started/installation.md)
- [最初の Watch アプリ ビデオ](http://blog.xamarin.com/your-first-watch-kit-app/)
