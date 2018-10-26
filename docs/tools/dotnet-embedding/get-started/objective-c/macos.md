---
title: MacOS の概要
description: このドキュメントの説明方法 macOS で .NET の埋め込みの使用を開始します。 要件について説明し、マネージ アセンブリをバインドし、Xcode プロジェクトで生成された出力を使用する方法を示すサンプル アプリケーションを表示します。
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 3de47aa57df29f52f71508977ae017dff009c282
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121569"
---
# <a name="getting-started-with-macos"></a>MacOS の概要

## <a name="what-you-will-need"></a>必要があります。

* 手順については、 [Objective C の概要](~/tools/dotnet-embedding/get-started/objective-c/index.md)ガイド。

## <a name="hello-world"></a>Hello world

まず、c# では、単純な hello world の例を構築します。

### <a name="create-c-sample"></a>C# のサンプルを作成します。

Mac を Visual Studio を開き、という名前の新しい Mac クラス ライブラリ プロジェクトを作成**csharp からのこんにちは**、保存して **~/Projects/hello-from-csharp**します。

コードに置き換えます、 **MyClass.cs**を次のスニペット ファイル。

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

プロジェクトをビルドします。 結果として得られるアセンブリとして保存する **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**します。

### <a name="bind-the-managed-assembly"></a>マネージ アセンブリをバインドします。

マネージ アセンブリを作成したら、.NET の埋め込みを呼び出すことによってバインドします。

」の説明に従って、[インストール](~/tools/dotnet-embedding/get-started/install/install.md)ガイド、これで、プロジェクトのビルド後のステップとしてカスタム MSBuild ターゲット、または手動で。

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

フレームワークに置かれる **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**します。

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力を使用します。

Xcode を開き、新しい Cocoa アプリケーションを作成します。 名前を付けます**csharp からのこんにちは**を選択し、 **Objective C**言語。

開く、 **~/Projects/hello-from-csharp/output** finder で、選択ディレクトリ**こんにちは-csharp.framework から**Xcode プロジェクトにドラッグし、ドロップ、すぐ上、 **csharp からのこんにちは**プロジェクト内のフォルダー。

![ドラッグ アンド ドロップのフレームワーク](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

確認します**必要な場合は、項目をコピー**がポップアップ表示されるダイアログ ボックスがオンにして**完了**します。

![必要な場合は、項目をコピーします。](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

選択、 **csharp からのこんにちは**プロジェクトに移動して、 **csharp からのこんにちは**ターゲットの**全般**タブ。**埋め込みバイナリ**セクションで、追加**こんにちは-csharp.framework から**します。

![埋め込みバイナリ](macos-images/hello-from-csharp-mac-embedded-binaries.png)

開いている**ViewController.m**、開き、内容に置き換えます。

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

包括的かつ見栄えのよいサンプル[は](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather)します。
