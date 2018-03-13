---
title: "Mac プラットフォームのセットアップ"
description: "Xamarin.Forms ようになりました Mac プラットフォーム用のプレビューがサポート"
ms.topic: article
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: e8487dc06b3512a0ec0bb1b30393faeab506df60
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="mac-platform-setup"></a>Mac プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

始める前に、作成 (または既存の使用) Xamarin.Forms プロジェクト。
ファルダ for Visual Studio を使用して Mac アプリのみを追加することができます。

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Xamarin.forms、macOS プロジェクトを追加すると、によって[Xamarin 大学](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Mac アプリケーションを追加します。

Sierra および Mac OS X 許可されて macOS で実行する Mac アプリケーションを追加する手順に従います。

1. Mac 用 Visual Studio で既存の Xamarin.Forms ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.**

2. **新しいプロジェクト**ウィンドウを選択**Mac > アプリ > Cocoa アプリ**とキーを押します**次**です。

3. 型、**アプリ名**(および必要に応じて、ドッキング項目の別の名前を選択)、キーを押します**次**です。

4. 構成を確認し、キーを押して**作成**です。 次の手順は以下に示します。

  ![Cocoa アプリを追加する方法を示すアニメーションの手順](mac-images/add-macos-proj.gif)

5. Mac プロジェクトを右クリックして**パッケージ > パッケージを追加しています.**を追加する、 [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet です。 このバージョンへの他のプロジェクトを更新することも必要があります。

6. Mac プロジェクトを右クリックして**参照**Xamarin.Forms プロジェクト (共有プロジェクトまたは PCL) への参照を追加します。

  ![Xamarin.Forms 共有コード プロジェクトへの参照を追加します。](mac-images/references-sml.png)

7. 更新**Main.cs**初期化するために、 `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. 更新`AppDelegate`Xamarin.Forms を初期化するために、ウィンドウを作成し、Xamarin.Forms アプリケーションの読み込み (適切な設定を記述する`Title`)。 _初期化する必要がある他の依存関係があれば、ここでことがもできます。_

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. ダブルクリックして**Main.storyboard** Xcode で編集します。 選択、**ウィンドウ**と_をオフに_、**は最初のコント ローラー** (これは上記のコード ウィンドウを作成するため) チェック ボックス。

  [![Xcode では、最初のコント ローラー チェック ボックスをオフに](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  望ましくない項目を削除するストーリー ボードにメニュー システムを編集することができます。

10. 最後に、(ローカル リソースを追加します。 イメージ ファイル) に必要な既存のプラットフォーム プロジェクトからです。

11. Mac プロジェクトでは、macos Xamarin.Forms コードを実行する必要があります!

## <a name="next-steps"></a>次の手順

### <a name="styling"></a>[スタイル]

加えられた最近の変更と`OnPlatform`任意の数のプラットフォームを対象にできますようになりました。 MacOS が含まれます。

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

次のようなプラットフォームでも倍加可能性がありますに注意してください:`<On Platform="iOS, macOS" ...>`です。

### <a name="window-size-and-position"></a>ウィンドウのサイズと位置

初期サイズとウィンドウの位置を調整することができます、 `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>既知の問題

これは、プレビューわけではないが実稼働の準備完了であることが予想されます。 次には、macOS をプロジェクトに追加するときに発生する、いくつかの点を示します。

### <a name="not-all-nugets-are-ready-for-macos"></a>すべての NuGets の準備ができて macOS

パッケージは、macOS プロジェクトで作業を対象に"xamarinmac20"する必要があります。 MacOS をまだサポートしていないを使用するライブラリのいくつかあります。

この場合、追加するプロジェクトのメンテナンス ツールに要求を送信する必要があります。 サポートを取得するまでは、ソリューションを検索する必要があります。

### <a name="missing-xamarinforms-features"></a>Xamarin.Forms 機能がありません。

Xamarin.Forms のすべての機能はこのプレビューでは; で完了しましたまだ実装されていない機能の一部の一覧を次に示します。

* [フッター]
* イメージの側面
* ListView-も、UnevenRows サポート、更新、SeparatorColor、SeparatorVisibility
* MasterDetailPage – BackgroundColor
* ナビゲーション: InsertPageBefore
* OpenGLRenderer
* ピッカー-Bindable/観測可能なオブジェクトの実装
* TabbedPage – BarBackgroundColor、BarTextColor
* テーブル – UnevenRows
* ViewCell – IsEnabled、ForceUpdateSize
* WebView – ほとんど WebNavigationEvents


## <a name="related-links"></a>関連リンク

- [Xamarin.Mac](~/mac/index.yml)
