---
title: MacOS の概要
description: このドキュメントでは、macOS での .NET 埋め込みの使用方法について説明します。 要件について説明し、マネージアセンブリをバインドし、生成された出力を Xcode プロジェクトで使用する方法を示すサンプルアプリケーションを示します。
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: ee40a5ef3504e5d274a34ec2d9569026e5d40551
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199863"
---
# <a name="getting-started-with-macos"></a>MacOS の概要

## <a name="what-you-will-need"></a>必要なもの

* 『[目標](~/tools/dotnet-embedding/get-started/objective-c/index.md)の概要』のガイドの手順に従ってください。

## <a name="hello-world"></a>Hello world

まず、「」でC#簡単な hello world の例を作成します。

### <a name="create-c-sample"></a>サンプルC#の作成

Visual Studio for Mac を開き、 **hello-から csharp**という名前の新しい Mac クラスライブラリプロジェクトを作成し、 **~/Projects/hello-from-csharp**に保存します。

**MyClass.cs**ファイルのコードを次のスニペットに置き換えます。

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

プロジェクトをビルドします。 結果として得られるアセンブリは、 **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**として保存されます。

### <a name="bind-the-managed-assembly"></a>マネージアセンブリをバインドする

マネージアセンブリを作成したら、.NET 埋め込みを呼び出してバインドします。

[インストール](~/tools/dotnet-embedding/get-started/install/install.md)ガイドで説明されているように、これは、プロジェクトのビルド後の手順として、カスタム MSBuild ターゲットを使用するか、手動で行うことができます。

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

フレームワークは **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**に配置されます。

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力を使用する

Xcode を開き、新しい Cocoa アプリケーションを作成します。 **Csharp から hello**という名前を指定し、**目的の C**言語を選択します。

Finder で **~/Projects/hello-from-csharp/output**ディレクトリを開き、 **[hello-csharp]** を選択します。フレームワークを Xcode プロジェクトにドラッグし、プロジェクトの **[hello from-csharp]** フォルダーのすぐ上にドロップします。

![ドラッグアンドドロップフレームワーク](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

ポップアップ表示されるダイアログで **[必要に応じて項目をコピー]** する チェックボックスがオンになっていることを確認し、 **[完了]** をクリックします。

![必要に応じて項目をコピーする](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

**[Hello from-csharp]** プロジェクトを選択し、 **[hello from-csharp]** ターゲットの **[全般**] タブに移動します。 **[埋め込ま]** れたバイナリ セクションで、 **hello-csharp. フレームワーク**を追加します。

![埋め込みバイナリ](macos-images/hello-from-csharp-mac-embedded-binaries.png)

**Viewcontroller. m**を開き、内容を次の内容に置き換えます。

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

最後に、Xcode プロジェクトを実行すると、次のような内容が表示されます。

![シミュレーターでC#実行されているサンプルからの Hello](macos-images/hello-from-csharp-mac.png)

より完全で見栄えの良いサンプルについて[は、こちら](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather)を参照してください。
