---
title: Hello, watchOS –チュートリアル
description: このドキュメントでは、Xamarin を使用した単純な watchOS アプリケーションの構築に関するチュートリアルを提供します。 また、Visual Studio と Visual Studio for Mac の両方で作業する方法、ストーリーボードを操作する方法、およびコード内のイベントに応答する方法についても説明します。
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/14/2016
ms.openlocfilehash: 3f69f10274c413a107a40b2f404b3227cfee67cf
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936743"
---
# <a name="hello-watchos--walkthrough"></a>Hello, watchOS –チュートリアル

「[セットアップとインストール](~/ios/watchos/get-started/installation.md)」の手順に従ってソリューションを作成すると、次の3つのプロジェクトが作成されます。

- セットアップまたはその他のデバイス上の管理タスクに使用される iOS 親アプリ。 (他の種類の iOS 拡張機能では、これは "コンテナー" アプリと呼ばれることがよくあります)。Watch アプリでは、ユーザーは親アプリを実行**しなくても、** watch アプリの実行を開始できます。
- Watch アプリのプログラムコードを含む Watch の拡張機能。そして
- ウォッチアプリ。ウォッチにレンダリングされるストーリーボードとイメージリソースを保持します。

[参照が正しい](~/ios/watchos/get-started/project-references.md)ことを確認します。親アプリには拡張機能への参照があり、拡張機能には Watch アプリへの参照が含まれていることを確認します。

バンドルの識別子が \* . watchkitextension. watchkitapp 規則に従っていること \* 、および拡張機能の情報の plist ファイルに**WKApp バンドル ID**値がウォッチアプリのバンドル id に設定されていることを確認します。

ここで Watch アプリを実行できるようになりますが、Watch アプリ内のストーリーボードファイルは空であるため、通知することはできません。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![ソリューションエクスプローラー](hello-watch-images/projectstructure.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![ソリューションエクスプローラー](hello-watch-images/vs-projectstructure.png)

-----

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Watch アプリで Xcode をダブルクリックして Xamarin iOS Designer を起動します (Mac を使用している場合は、右クリックして、 **> Interface Builder で開く**こともできます)

1. **ツールボックス**と**プロパティ**パッドが表示されていることを確認します。
1. インターフェイスコントローラーをクリックして選択します。
1. インターフェイスコントローラーの識別子とタイトルを**interfaceController**と**Hi Watch**に設定します。
1. **クラス**が**InterfaceController**に設定されていることを確認します。

    ![インターフェイスコントローラーの識別子とタイトルを interfaceController と Hi Watch に設定します。](hello-watch-images/interfacecontrollerattributes.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio で Xamarin iOS Designer を使用して編集するには、Watch アプリで [] というインターフェイスをダブルクリックします。

1. プロパティペインを開きます。
1. クラスを**InterfaceController**に変更します。
1. インターフェイスコントローラーをクリックします。そして
1. インターフェイスコントローラーの識別子とタイトルを**interfaceController**および**Hi Watch**に設定します。

    ![インターフェイスコントローラーの識別子とタイトルを interfaceController と Hi Watch に設定します。](hello-watch-images/vs-interfacecontrollerattributes.png)

-----

UI を作成します。

1. [**ツールボックス**] パッドから
1. **ボタン**と**ラベル**をシーンにドラッグアンドドロップします。
1. 次に示すように、コントロールのテキストと属性を設定します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![以下に示すように、コントロールのテキストと属性を設定します。](hello-watch-images/draganddrop.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![以下に示すように、コントロールのテキストと属性を設定します。](hello-watch-images/vs-draganddrop.png)

-----

1. 各コントロールの [**プロパティ**] パッドで**名前**を設定します。 この例では、とを使用しました `myButton` `myLabel` 。

1. ストーリーボード上のボタンを選択し、[**プロパティ**] パッドの [**イベント**] の一覧にアクセスします。

1. 「」と入力し、Enter キーを押して、新しい**アクション**を作成し `OnButtonPress` ます。 **Enter**
  アクションが一覧に表示され、C# で部分メソッドが自動的に作成されます。

![ボタンに追加された OnButtonPress アクション](hello-watch-images/buttonaction.png)

ストーリーボードを保存すると、 **InterfaceController.designer.cs**はコントロール名とアクションを使用して更新されます。 更新後にこのファイルを開くと、がコントローラーにどのように対応しているか、およびでマークされた C# インスタンス変数と UI コントロールがどのように対応しているかを確認でき `RegisterAttribute` `OutletAttribute` ます。また、アクションは、でタグ付けされた部分メソッドにマップされ `ActionAttribute` ます。

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

ここで、 **InterfaceController.cs** (*not* InterfaceController.designer.cs) を開き、次のコードを追加します。

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

このコードは非常に透過的である必要があります。インスタンス変数は、 `clickCount` 関数 `OnButtonPress` が呼び出されるたびにインクリメントされます。 のテキストは、 `myLabel` この数を反映するように変更されています。 `myLabel` もちろん、は、XCode で作成したアウトレットの1つの名前です。 `partial`関数は、指定したアクションの名前に関連付けられている関数の実装です。

まだスタートアッププロジェクトでない場合は、

1. Watch 拡張機能プロジェクトを右クリックし、[**スタートアッププロジェクトに設定**] を選択します。

1. 配置ターゲットを Watch Kit と互換性のあるシミュレーターイメージ (iPhone 6 iOS 8.2 など) に設定します。

1. ビルドとシミュレーターの起動をトリガーするには、[**デバッグ**] ボタンをクリックします。

    [![Visual Studio のインターフェイス要素](hello-watch-images/readytodebug-sml.png)](hello-watch-images/readytodebug.png#lightbox)

シミュレーターが起動したら、ボタンを押してラベルをインクリメントします。
これで、ウォッチアプリを作成できました。

![シミュレーターで実行されているアプリ](hello-watch-images/running.png)

## <a name="related-links"></a>関連リンク

- [セットアップとインストール](~/ios/watchos/get-started/installation.md)
- [最初のアプリの視聴に関するビデオ](https://blog.xamarin.com/your-first-watch-kit-app/)
