---
title: IOS の概要
description: このドキュメントでは、iOS での .NET 埋め込みの使用を開始する方法について説明します。 要件について説明し、マネージアセンブリをバインドし、その出力を Xcode プロジェクトで使用する方法を示すサンプルアプリを示します。
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: d5bde89ed90e55724fbc25fc473e265affa9ce2f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292938"
---
# <a name="getting-started-with-ios"></a>IOS の概要

## <a name="requirements"></a>必要条件

「[目的 C の](~/tools/dotnet-embedding/get-started/objective-c/index.md)概要」ガイドの要件に加えて、以下も必要になります。

* [Xamarin. iOS 10.11](https://visualstudio.microsoft.com/xamarin/)以降

## <a name="hello-world"></a>Hello world

まず、「」でC#簡単な hello world の例を作成します。

### <a name="create-c-sample"></a>サンプルC#の作成

Visual Studio for Mac を開き、新しい iOS クラスライブラリプロジェクトを作成します。このプロジェクトには、 **csharp から**名前を付け、 **~/Projects/hello-from-csharp**に保存します。

**MyClass.cs**ファイルのコードを次のスニペットに置き換えます。

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

プロジェクトをビルドします。作成したアセンブリは、 **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**として保存されます。

### <a name="bind-the-managed-assembly"></a>マネージアセンブリをバインドする

マネージアセンブリを作成したら、.NET 埋め込みを呼び出してバインドします。

[インストール](~/tools/dotnet-embedding/get-started/install/install.md)ガイドで説明されているように、これは、プロジェクトのビルド後の手順として、カスタム MSBuild ターゲットを使用するか、手動で行うことができます。

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

フレームワークは **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**に配置されます。

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode プロジェクトで生成された出力を使用する

Xcode を開き、新しい iOS シングルビューアプリケーションを作成します。これには、 **csharp から hello**という名前を指定し、 **C**言語を選択します。

Finder で **~/Projects/hello-from-csharp/output**ディレクトリを開き、 **[hello-csharp]** を選択します。フレームワークを Xcode プロジェクトにドラッグし、プロジェクトの **[hello from-csharp]** フォルダーのすぐ上にドロップします。

![ドラッグアンドドロップフレームワーク](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

ポップアップ表示されるダイアログで **[必要に応じて項目をコピー]** する チェックボックスがオンになっていることを確認し、 **[完了]** をクリックします。

![必要に応じて項目をコピーする](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

**[Hello from-csharp]** プロジェクトを選択し、 **[hello from-csharp]** ターゲットの **[全般] タブ**に移動します。 **[埋め込ま]** れたバイナリ セクションで、 **hello-csharp. フレームワーク**を追加します。

![埋め込みバイナリ](ios-images/hello-from-csharp-ios-embedded-binaries.png)

**Viewcontroller. m**を開き、内容を次の内容に置き換えます。

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

.NET 埋め込みでは、現在、一部の Xcode プロジェクトテンプレートに対して有効になっている iOS での bitcode はサポートされていません。 

プロジェクト設定で無効にします。

![Bitcode オプション](../../images/ios-bitcode-option.png)

最後に、Xcode プロジェクトを実行すると、次のような内容が表示されます。

![シミュレーターでC#実行されているサンプルからの Hello](ios-images/hello-from-csharp-ios.png)
