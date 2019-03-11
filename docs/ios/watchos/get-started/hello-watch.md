---
title: はじめての watchOS – チュートリアル
description: このドキュメントでは、Xamarin を使用して単純な watchOS アプリケーションを構築するためのチュートリアルを示します。 これには、for Mac に Visual Studio と Visual Studio の両方で動作し、ストーリー ボードを操作して、コードでイベントに応答する方法について説明します。
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/14/2016
ms.openlocfilehash: de04a2d7f42ec36c464c75ced73bf8f8029ec1da
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672613"
---
# <a name="hello-watchos--walkthrough"></a>はじめての watchOS – チュートリアル

次の手順では、ソリューションを作成した後に[セットアップとインストール](~/ios/watchos/get-started/installation.md)、3 つのプロジェクトが必要があります。

- セットアップまたはその他のデバイス上の管理タスクに使用する iOS 親アプリ。 (IOS の拡張機能の他の種類では、これは多くの場合と呼ばれるに「コンテナー」アプリです。)Watch アプリでは、なし、Watch アプリの実行を開始するユーザーのことは**これまで**親アプリケーションを実行しています。
- Watch アプリ; のプログラム コードを含むウォッチ拡張機能そして
- Watch アプリをウォッチ上に描画されるストーリー ボードとイメージのリソースを保持します。

いることを確認、[の参照が正しく](~/ios/watchos/get-started/project-references.md): 親アプリが、拡張機能への参照をしていること、および拡張機能が、Watch アプリへの参照。

バンドル識別子は、次のことを確認、 \*.watchkitextension \*.watchkitapp 規則、拡張機能の Info.plist ファイルがあるとは**WKApp バンドル ID**のバンドル識別子に値の設定Watch アプリ。

今すぐ、Watch アプリを実行できる必要がありますが、Watch アプリ内でストーリー ボード ファイルが空白であるために指示できないでしょう。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/projectstructure.png "ソリューション エクスプ ローラー")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-projectstructure.png "ソリューション エクスプ ローラー")

-----

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
Xamarin iOS デザイナーを起動、Watch アプリの Interface.storyboard をダブルクリックします (Mac を使用している場合もを右クリックし、**プログラムから開く > Xcode の Interface Builder**)


1.  確認、**ツールボックス**と**プロパティ**パッドが表示され、
1.  クリックして、インターフェイス コント ローラーを選択します。
1.  識別子とインターフェイスのコント ローラーのタイトルを設定**interfaceController**と**やあウォッチ**、
1.  確認、**クラス**に設定されている**InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "InterfaceController にやあウォッチ識別子と、インターフェイス コント ローラーのタイトルを設定します。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin iOS Visual Studio のデザイナーで編集する、Watch アプリの Interface.storyboard をダブルクリックします。

1.  プロパティ ウィンドウを開く
1.  クラスを変更**InterfaceController**;
1.  インターフェイス コント ローラーの; をクリックします。そして
1.  識別子とインターフェイスのコント ローラーのタイトルを設定**interfaceController**と**やあウォッチ**します。

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "InterfaceController にやあウォッチ識別子と、インターフェイス コント ローラーのタイトルを設定します。")

-----


UI を作成します。

1. **ツールボックス**パッド、
1. ドラッグ アンド ドロップ、**ボタン**と**ラベル**シーンにし、
1. テキストおよびに示すように、コントロールの属性を設定します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/draganddrop.png "テキストとに示すように、コントロールの属性を設定します。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-draganddrop.png "テキストとに示すように、コントロールの属性を設定します。")

-----

1. 設定、**名前**で、**プロパティ**各コントロールのパッド。 この例では使用した`myButton`と`myLabel`します。

1. ストーリー ボード上のボタンを選択しに移動、**プロパティ**パッドの**イベント**一覧表示し、

1. 新規作成**アクション**」と入力して`OnButtonPress`キーを押すと**Enter**します。
  アクションが、一覧に表示されでは、部分メソッドが自動的に作成されますC#します。

![](hello-watch-images/buttonaction.png "OnButtonPress アクション ボタンに追加")

ストーリー ボードを保存した後、 **InterfaceController.designer.cs**コントロール名とアクションで更新されます. 更新された後にこのファイルを開くことがわかる方法、`RegisterAttribute`コント ローラーとに UI コントロールの対応に対応するC#とマークされているインスタンス変数、 `OutletAttribute` 、でタグ付けされた部分メソッドにアクションがどのようにマップするか、`ActionAttribute`:

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

開くようになりました**InterfaceController.cs** (*いない*InterfaceController.designer.cs) し、次のコードを追加します。

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

このコードはかなり透明にする必要があります。 インスタンス変数`clickCount`は関数たびにインクリメント`OnButtonPress`が呼び出されます。 テキスト`myLabel`; この数を反映するように変更されました。`myLabel`、もちろん、XCode で作成した Outlet のいずれかの名前です。 `partial`関数は、指定したアクションの名前に関連付けられている関数の実装。

されていない、スタートアップ プロジェクトの場合

1. Watch Extension プロジェクトを右クリックし、選択**スタートアップ プロジェクトとして設定**、

1. IPhone 6 iOS 8.2) など、ウォッチ キットと互換性のあるシミュレーター イメージに配置ターゲットを設定します。

1. キーを押して、**デバッグ**をビルドし、シミュレーターの起動をトリガーするボタンをクリックします。

    [![](hello-watch-images/readytodebug-sml.png "Visual Studio のインターフェイス要素")](hello-watch-images/readytodebug.png#lightbox)

シミュレーターを起動すると、ラベルをインクリメントするボタンを押します。
これで、Watch アプリを等しくしました。

![](hello-watch-images/running.png "シミュレーターで実行されているアプリ")


## <a name="related-links"></a>関連リンク

- [作業の開始 (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [セットアップとインストール](~/ios/watchos/get-started/installation.md)
- [Watch アプリの最初のビデオ](https://blog.xamarin.com/your-first-watch-kit-app/)
