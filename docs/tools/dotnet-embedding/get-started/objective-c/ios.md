---
title: IOS の概要
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ab841c461356bd435db0c68e82c5e30d398d806a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-ios"></a>IOS の概要

## <a name="requirements"></a>要件

要件に加え、 [Objective C の概要](~/tools/dotnet-embedding/get-started/objective-c/index.md)ガイドも必要です。

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/)以降

## <a name="hello-world"></a>Hello world

最初に、C# の場合は、単純な hello world の例をビルドします。

### <a name="create-c-sample"></a>C# のサンプルを作成します。

Mac 用 Visual Studio を開き、新しい iOS クラス ライブラリ プロジェクトを作成、名前を付けます**csharp からのこんにちは**、しに保存 **~/Projects/hello-from-csharp**です。

コードで置き換え、**カスタム**次のスニペットを持つファイル。

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

プロジェクトをビルドし、生成されたアセンブリで保存されます **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**です。

### <a name="bind-the-managed-assembly"></a>マネージ アセンブリをバインドします。

マネージ アセンブリを作成したら、.NET の埋め込みを起動してバインドします。

」の説明に従って、[インストール](~/tools/dotnet-embedding/get-started/install/install.md)ガイド、そのため、プロジェクトのビルド後のステップとして、カスタム MSBuild ターゲットまたは手動で。

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

配置されるフレームワーク **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**です。

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力を使用します。

Xcode を開き、新しい iOS ビューの 1 つのアプリケーションを作成、名前を付けます**csharp からのこんにちは**を選択し、 **OBJECTIVE-C**言語です。

開く、 **~/Projects/hello-from-csharp/output** Finder で、選択ディレクトリ**こんにちは-csharp.framework から**Xcode プロジェクトにドラッグ アンド ドロップすぐ上、 **csharp からのこんにちは**プロジェクト内のフォルダーです。

![ドラッグ アンド ドロップ framework]Images/hello-from-csharp-ios-drag-drop-framework.png)

確認**ために必要な場合は、項目をコピー**がポップアップ表示されるダイアログ ボックスでチェックされ、をクリックして**完了**。

![必要な場合は、項目をコピーします。](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

選択、 **csharp からのこんにちは**プロジェクトに移動して、 **csharp からのこんにちは**ターゲットの **[全般] タブ**です。**埋め込みバイナリ**セクションで、追加**こんにちは-csharp.framework から**です。

![埋め込まれたバイナリ](ios-images/hello-from-csharp-ios-embedded-binaries.png)

開いている**ViewController.m**の内容を置き換えます。

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

.NET の埋め込みはサポートされていません bitcode ios で、一部の Xcode プロジェクト テンプレートが有効になっています。 

プロジェクトの設定でそれを無効にします。

![Bitcode オプション](../../images/ios-bitcode-option.png)

最後に、Xcode プロジェクトを実行し、次のように表示されます。

![シミュレーターで実行されている c# のサンプルからこんにちは](ios-images/hello-from-csharp-ios.png)
