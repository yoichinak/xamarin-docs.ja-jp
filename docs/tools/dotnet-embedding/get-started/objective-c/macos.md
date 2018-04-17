---
title: MacOS の概要
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f75ced921cd240e280b5dd6f7366ccceefb5e40e
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
---
# <a name="getting-started-with-macos"></a>MacOS の概要


## <a name="what-you-will-need"></a>必要があります。

* 指示に従って、 [Objective C の概要](~/tools/dotnet-embedding/get-started/objective-c/index.md)ガイドです。

## <a name="hello-world"></a>Hello world

最初に、C# の場合は、単純な hello world の例をビルドします。

### <a name="create-c-sample"></a>C# のサンプルを作成します。

Mac 用 Visual Studio を開き、という名前の新しい Mac クラス ライブラリ プロジェクトを作成する**csharp からのこんにちは**、しに保存**~/Projects/hello-from-csharp**です。

コードで置き換え、`MyClass.cs`次のスニペットを持つファイル。

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

プロジェクトをビルドします。 結果として得られるアセンブリとして保存する**~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**です。

### <a name="bind-the-managed-assembly"></a>マネージ アセンブリをバインドします。

マネージ アセンブリのネイティブ フレームワークを作成する embeddinator を実行します。

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

配置されるフレームワーク**~/Projects/hello-from-csharp/output/hello-from-csharp.framework**です。

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力を使用します。

Xcode を開き、新しい Cocoa アプリケーションを作成します。 名前を付けます**csharp からのこんにちは**を選択し、 **OBJECTIVE-C**言語です。

開く、 **~/Projects/hello-from-csharp/output** Finder で、選択ディレクトリ**こんにちは-csharp.framework から**Xcode プロジェクトにドラッグ アンド ドロップすぐ上、 **csharp からのこんにちは**プロジェクト内のフォルダーです。

![ドラッグ アンド ドロップのフレームワーク](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

確認**ために必要な場合は、項目をコピー**がポップアップ表示されるダイアログ ボックスでチェックされ、をクリックして**完了**。

![必要な場合は、項目をコピーします。](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

選択、 **csharp からのこんにちは**プロジェクトに移動して、 **csharp からのこんにちは**ターゲットの**全般**タブです。**埋め込みバイナリ**セクションで、追加**こんにちは-csharp.framework から**です。

![埋め込まれたバイナリ](macos-images/hello-from-csharp-mac-embedded-binaries.png)

開いている**ViewController.m**の内容を置き換えます。

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

最後に、Xcode プロジェクトを実行し、次のように表示されます。

![シミュレーターで実行されている c# のサンプルからこんにちは](macos-images/hello-from-csharp-mac.png)

包括的かつ見栄えのよいサンプルは[ここ](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather)です。
