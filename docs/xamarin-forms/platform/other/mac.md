---
title: ''
description: この記事では、Mac プロジェクトをプロジェクトに追加する方法について説明し Xamarin.Forms ます。これにより、macOS Sierra と MacOS El Capitan で実行できるアプリが生成されます。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
ms.custom: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 11897d2d3b8b7ba0a62956f1dbe4d8b873352e7a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139556"
---
# <a name="mac-platform-setup"></a>Mac プラットフォームのセットアップ

![プレビュー](~/media/shared/preview.png)

開始する前に、プロジェクトを作成するか、既存のプロジェクトを使用 Xamarin.Forms します。 Visual Studio for Mac を使用して追加できるのは Mac アプリのみです。

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**MacOS プロジェクトをビデオに追加する Xamarin.Forms**

## <a name="adding-a-mac-app"></a>Mac アプリの追加

次の手順に従って、macOS Sierra および macOS El Capitan で実行される Mac アプリを追加します。

1. Visual Studio for Mac で、既存のソリューションを右クリックし、 Xamarin.Forms [**追加] > [新しいプロジェクトの追加**] の順に選択します。

2. [**新しいプロジェクト**] ウィンドウで、[ **Mac > アプリ > cocoa アプリ**] を選択し、[**次へ**] をクリックします。

3. **アプリ名**を入力し (必要に応じて、Dock 項目に別の名前を選択します)、[**次へ**] をクリックします。

4. 構成を確認し、[**作成**] を押します。 これらの手順を次に示します。

    ![Cocoa アプリを追加する方法を示すアニメーション化された手順](mac-images/add-macos-proj.gif)

5. Mac プロジェクトで、[パッケージ] を右クリックし、[**パッケージの追加**] を > て NuGet を追加します。 [Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/) また、同じバージョンの NuGet パッケージを使用するように、他のプロジェクトを更新する必要があり Xamarin.Forms ます。

6. Mac プロジェクトで、[**参照**] を右クリックし、プロジェクトへの参照を追加し Xamarin.Forms ます ([共有プロジェクト] または [.NET Standard ライブラリプロジェクト])。

    ![共有コードプロジェクトへの参照を追加する Xamarin.Forms](mac-images/references-sml.png)

7. **Main.cs**を更新してを初期化し `AppDelegate` ます。

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

8. [ `AppDelegate` 初期化] に更新し Xamarin.Forms 、ウィンドウを作成して、アプリケーションを読み込み Xamarin.Forms ます (適切なが設定されていることを覚えて `Title` います)。 _他に初期化する必要がある依存関係がある場合は、ここでも実行します。_

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

9. Xcode で [**メインのストーリーボード**] をダブルクリックして編集します。 **ウィンドウ**を選択し、[**初期コントローラーがある**] チェックボックスを_オフ_にします (これは、上記のコードによってウィンドウが作成されるためです)。

    [![Xcode の [Initial Controller] チェックボックスをオフにします。](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

    ストーリーボードでメニューシステムを編集して、不要な項目を削除することができます。

10. 最後に、すべてのローカルリソースを追加します ( 必要な既存のプラットフォームプロジェクトからのイメージファイル)。

11. Mac プロジェクトでは、macOS でコードを実行できるようになりました Xamarin.Forms 。

## <a name="next-steps"></a>次の手順

### <a name="styling"></a>スタイル

に加えられた最近の変更により `OnPlatform` 、任意の数のプラットフォームを対象にすることができるようになりました。 これには macOS が含まれます。

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

また、次のようなプラットフォームを使用することもできます `<On Platform="iOS, macOS" ...>` 。

### <a name="window-size-and-position"></a>ウィンドウのサイズと位置

でウィンドウの初期サイズと位置を調整でき `AppDelegate` ます。

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>既知の問題

これはプレビューなので、運用環境の準備ができているわけではありません。 MacOS をプロジェクトに追加するときに発生する可能性があるいくつかの事項を次に示します。

### <a name="not-all-nugets-are-ready-for-macos"></a>すべての Nuget が macOS 用に準備されているわけではありません

使用するライブラリによっては、まだ macOS がサポートされていないことがあります。 この場合は、プロジェクトのメンテナンスツールに要求を送信して、それを追加する必要があります。 サポートが必要になるまでは、代替手段を探す必要があります。

### <a name="missing-xamarinforms-features"></a>不足している Xamarin.Forms 機能

Xamarin.Formsこのプレビューでは、すべての機能が完了しているわけではありません。 詳細については、GitHub リポジトリの「 [Platform Support MacOS Status](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support-macOS-Status) 」を参照してください [Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms) 。

## <a name="related-links"></a>関連リンク

- [Xamarin.Mac](~/mac/index.yml)
