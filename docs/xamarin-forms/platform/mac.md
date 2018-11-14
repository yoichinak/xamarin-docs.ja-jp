---
title: Mac プラットフォームのセットアップ
description: この記事では、Mac のプロジェクトを macOS Sierra および El Capitan を macOS で実行できるアプリを生成する Xamarin.Forms プロジェクトに追加する方法について説明します。
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: 1aa21a416f4abca0440e96e25aebe5f834a717ce
ms.sourcegitcommit: f3f28722198e172d81c16bdeab0cb0a581a08dd0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2018
ms.locfileid: "51598874"
---
# <a name="mac-platform-setup"></a>Mac プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

開始する前に作成 (または、既存の使用) Xamarin.Forms プロジェクト。
Visual Studio for mac を使用する Mac アプリを追加することができますのみ

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Xamarin.Forms での macOS プロジェクトを追加すると、によって[Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Mac アプリを追加します。

MacOS Sierra および El Capitan を macOS で実行される Mac アプリを追加するこれらの手順に従います。

1. Visual studio for Mac では、既存の Xamarin.Forms ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.**

2. **新しいプロジェクト**ウィンドウを選択して**Mac > アプリ > Cocoa アプリ**キーを押します**次**します。

3. 型、**アプリ名**(および必要に応じて、Dock 項目の別の名前を選択)、キーを押します**次**します。

4. 構成とキーを押して確認**作成**です。 次の手順で以下に示します。

  ![Cocoa アプリを追加する方法を示すアニメーション化された手順](mac-images/add-macos-proj.gif)

5. 右クリックし、Mac のプロジェクトで**パッケージ > パッケージを追加しています.** を追加する、 [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet です。 他のプロジェクトは、このバージョンにも更新する必要があります。

6. 右クリックし、Mac のプロジェクトで**参照**Xamarin.Forms プロジェクト (共有プロジェクトまたは .NET Standard ライブラリ プロジェクト) への参照を追加します。

  ![Xamarin.Forms の共有コード プロジェクトへの参照を追加します。](mac-images/references-sml.png)

7. Update **Main.cs**初期化するために、 `AppDelegate`:

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

8. Update `AppDelegate` Xamarin.Forms を初期化するために、ウィンドウを作成し、Xamarin.Forms アプリケーションの読み込み (適切な設定を記憶`Title`)。 _初期化する必要がある他の依存関係があれば、ここでことがもできます。_

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

9. ダブルクリック**Main.storyboard** Xcode で編集します。 選択、**ウィンドウ**と_をオフに_、**は最初のコント ローラー** (これは上記のコード ウィンドウを作成するため) チェック ボックス。

  [![Xcode では、最初のコント ローラーのチェック ボックスをオフにします。](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  望ましくない項目を削除するストーリー ボードのメニュー システムを編集することができます。

10. 最後に、追加、ローカル リソースを (例。 イメージ ファイル) に必要な既存のプラットフォーム プロジェクトから。

11. Mac のプロジェクトでは、macOS で Xamarin.Forms コードを実行する必要があります!

## <a name="next-steps"></a>次の手順

### <a name="styling"></a>[スタイル]

加えられた最近の変更と`OnPlatform`さまざまなプラットフォームを対象にできますようになりました。 MacOS が含まれます。

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

このようなプラットフォームでも倍加可能性がありますに注意してください:`<On Platform="iOS, macOS" ...>`します。

### <a name="window-size-and-position"></a>ウィンドウのサイズと位置

初期サイズとウィンドウの位置を調整することができます、 `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>既知の問題

これは、その運用環境の準備がすべてではないことを想定する必要がありますのプレビューです。 MacOS をプロジェクトに追加するときに発生するいくつかの点を次に示します。

### <a name="not-all-nugets-are-ready-for-macos"></a>MacOS のすべての NuGets 準備が整いました

パッケージは、macOS project で作業する"xamarinmac20"をターゲットする必要があります。 MacOS をまだサポートしていない一部のライブラリを使用することがわかります。

この場合、追加するプロジェクトのメンテナンス ツールに要求を送信する必要があります。 サポートがインストールされるまでは、代わりとなるものを検索する必要があります。

### <a name="missing-xamarinforms-features"></a>不足している Xamarin.Forms の機能

Xamarin.Forms のすべての機能がこのプレビューで完了まだ実装されていない機能の一部を次に示します。

* [フッター]
* イメージの側面
* ListView – ScrollTo、UnevenRows サポート、更新、SeparatorColor、SeparatorVisibility
* MasterDetailPage – BackgroundColor
* ナビゲーション: InsertPageBefore
* OpenGLRenderer
* ピッカー-Bindable/観測可能なオブジェクトの実装
* TabbedPage – BarBackgroundColor、BarTextColor
* テーブル-UnevenRows
* ViewCell – IsEnabled ForceUpdateSize
* WebView – ほとんど WebNavigationEvents


## <a name="related-links"></a>関連リンク

- [Xamarin.Mac](~/mac/index.yml)
