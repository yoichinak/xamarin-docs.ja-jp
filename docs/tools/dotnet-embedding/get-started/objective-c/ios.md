---
title: IOS の概要
description: このドキュメントでは、iOS での .NET の埋め込みの使用を開始する方法について説明します。 要件について説明し、マネージ アセンブリをバインドし、Xcode プロジェクトの出力を使用する方法を示すサンプル アプリを表示します。
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 009772ac88ad57bab53fb71c9705b71f0f8acc8b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61318701"
---
# <a name="getting-started-with-ios"></a>IOS の概要

## <a name="requirements"></a>必要条件

要件に加え、 [Objective C の概要](~/tools/dotnet-embedding/get-started/objective-c/index.md)ガイドも必要になります。

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/)またはそれ以降

## <a name="hello-world"></a>Hello world

まず、c# では、単純な hello world の例を構築します。

### <a name="create-c-sample"></a>C# のサンプルを作成します。

Mac 用の Visual Studio を開き、新しい iOS クラス ライブラリ プロジェクトを作成、名前を付けます**csharp からのこんにちは**、保存して **~/Projects/hello-from-csharp**します。

コードに置き換えます、 **MyClass.cs**を次のスニペット ファイル。

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

プロジェクトをビルドし、生成されたアセンブリとして保存されます **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**します。

### <a name="bind-the-managed-assembly"></a>マネージ アセンブリをバインドします。

マネージ アセンブリを作成したら、.NET の埋め込みを呼び出すことによってバインドします。

」の説明に従って、[インストール](~/tools/dotnet-embedding/get-started/install/install.md)ガイド、これで、プロジェクトのビルド後のステップとしてカスタム MSBuild ターゲット、または手動で。

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

フレームワークに置かれる **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**します。

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力を使用します。

Xcode を開き、新しい iOS 単一ビュー アプリケーションの作成、名前を付けます**csharp からのこんにちは**を選択し、 **Objective C**言語。

開く、 **~/Projects/hello-from-csharp/output** finder で、選択ディレクトリ**こんにちは-csharp.framework から**Xcode プロジェクトにドラッグし、ドロップ、すぐ上、 **csharp からのこんにちは**プロジェクト内のフォルダー。

![ドラッグ アンド ドロップのフレームワーク](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

確認します**必要な場合は、項目をコピー**がポップアップ表示されるダイアログ ボックスがオンにして**完了**します。

![必要な場合は、項目をコピーします。](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

選択、 **csharp からのこんにちは**プロジェクトに移動して、 **csharp からのこんにちは**ターゲットの **[全般] タブ**します。**埋め込みバイナリ**セクションで、追加**こんにちは-csharp.framework から**します。

![埋め込みバイナリ](ios-images/hello-from-csharp-ios-embedded-binaries.png)

開いている**ViewController.m**、開き、内容に置き換えます。

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

.NET の埋め込みは現在サポートしていません bitcode ios では、いくつかの Xcode プロジェクト テンプレートを有効になっています。 

プロジェクトの設定で無効にします。

![Bitcode オプション](../../images/ios-bitcode-option.png)

最後に、Xcode プロジェクトを実行し、次のように表示されます。

![シミュレーターで実行されている c# のサンプルからこんにちは](ios-images/hello-from-csharp-ios.png)
