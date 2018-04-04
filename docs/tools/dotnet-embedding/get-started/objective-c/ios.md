---
title: IOS の概要
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 34afdd9e91ebfbe7ad57c7eec6ba7f05fff1a2aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-ios"></a>IOS の概要


## <a name="requirements"></a>要件

要件に加え、 [Objective C の概要](~/tools/dotnet-embedding/get-started/objective-c/index.md)ガイドも必要です。

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/)以降

## <a name="hello-world"></a>Hello world

最初は単純な hello world の例 (C#) を構築してみましょう。

### <a name="create-c-sample"></a>C# のサンプルを作成します。

Mac 用 Visual Studio を開き、新しい iOS クラス ライブラリ プロジェクトを作成、名前を付けます`hello-from-csharp`、しに保存`~/Projects/hello-from-csharp`です。

コードで置き換え、`MyClass.cs`次のスニペットを持つファイル。

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

プロジェクトをビルド、結果として得られるアセンブリとして保存する`~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`です。

### <a name="bind-the-managed-assembly"></a>マネージ アセンブリをバインドします。

マネージ アセンブリのネイティブ フレームワークを作成する embeddinator を実行します。

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

配置されるフレームワーク`~/Projects/hello-from-csharp/output/hello-from-csharp.framework`です。

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力を使用します。

Xcode を開き、新しい iOS ビューの 1 つのアプリケーションを作成し、名前を付けます`hello-from-csharp`を選択し、 **OBJECTIVE-C**言語です。

開く、 `~/Projects/hello-from-csharp/output` Finder で、選択ディレクトリ`hello-from-csharp.framework`Xcode プロジェクトにドラッグ アンド ドロップすぐ上、`hello-from-csharp`プロジェクト内のフォルダーです。

![ドラッグ アンド ドロップ framework]Images/hello-from-csharp-ios-drag-drop-framework.png)

確認`Copy items if needed`がポップアップ表示されるダイアログ ボックスでチェックされ、クリックして`Finish`です。

![必要な場合は、項目をコピーします。](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

選択、`hello-from-csharp`プロジェクトに移動して、`hello-from-csharp`ターゲットの**[全般] タブ**です。**埋め込みバイナリ**セクションで、追加`hello-from-csharp.framework`です。

![埋め込まれたバイナリ](ios-images/hello-from-csharp-ios-embedded-binaries.png)

ViewController.m を開き、内容を置き換えます。

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

最後に、Xcode プロジェクトを実行し、次のように表示されます。

![シミュレーターで実行されている c# のサンプルからこんにちは](ios-images/hello-from-csharp-ios.png)
