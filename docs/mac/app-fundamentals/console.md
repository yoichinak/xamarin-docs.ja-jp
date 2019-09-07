---
title: コンソールアプリ用の Xamarin. Mac バインドの使用
description: Xamarin. Mac を使用して、ネイティブ macOS Api にアクセスするヘッドレスコンソールアプリを作成します。
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: a38543d575f655a3b1b70ff94eece7fef1bf2d40
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769891"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>コンソールアプリの Xamarin. Mac バインド

でC#一部の Apple ネイティブ api を使用して、を使用&ndash; C#するユーザーインターフェイス&ndash;を持たないヘッドレスアプリケーションを作成する場合があります。

Mac アプリケーション用のプロジェクトテンプレートには、の`NSApplication.Init()`呼び出しの後にの`NSApplication.Main(args)`呼び出しが含まれます。通常、次のようになります。

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

の呼び出し`Init`によって、Xamarin. Mac ランタイムが準備さ`Main(args)`れます。を呼び出すと、cocoa アプリケーションのメインループが開始されます。これにより、アプリケーションがキーボードイベントとマウスイベントを受信し、アプリケーションのメインウィンドウを表示するように準備されます。   の呼び出し`Main`では、cocoa リソースの検索、toplevel ウィンドウの準備、およびプログラムがアプリケーションバンドル ( `.app`拡張機能と非常に限定的なレイアウトのディレクトリで配布されたプログラム) の一部であることが要求されます。

ヘッドレスアプリケーションはユーザーインターフェイスを必要とせず、アプリケーションバンドルの一部として実行する必要もありません。

## <a name="creating-the-console-app"></a>コンソールアプリの作成

そのため、通常の .NET コンソールプロジェクトの種類から始めることをお勧めします。

次の操作を行う必要があります。

- 空のプロジェクトを作成します。
- Xamarin. Mac .dll ライブラリを参照します。
- アンマネージ依存関係をプロジェクトに取り込みます。

これらの手順については、以下で詳しく説明します。

### <a name="create-an-empty-console-project"></a>空のコンソールプロジェクトを作成する

新しい .NET コンソールプロジェクトを作成し、.net Core ではなく .NET であることを確認します。 Xamarin は .net core ランタイムでは実行されず、Mono ランタイムでのみ実行されます。

### <a name="reference-the-xamarinmac-library"></a>Xamarin. Mac ライブラリを参照する

コードをコンパイルするには、このディレクトリから`Xamarin.Mac.dll`アセンブリを参照します。`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

これを行うには、プロジェクト参照に移動し、 **[.Net アセンブリ]** タブを選択します。次に、 **[参照]** ボタンをクリックして、ファイルシステム上のファイルを指定します。  上のパスに移動し、そのディレクトリから**Xamarin. Mac .dll**を選択します。

これにより、コンパイル時に Cocoa Api にアクセスできるようになります。   この時点で、ファイルの先頭`using AppKit`にを追加し、 `NSApplication.Init()`メソッドを呼び出すことができます。   アプリケーションを実行する前に、もう1つの手順があります。

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>アンマネージドサポートライブラリをプロジェクトに取り込む

アプリケーションを実行する前に、 `Xamarin.Mac`サポートライブラリをプロジェクトに取り込む必要があります。   これを行うには、プロジェクトに新しいファイルを追加します (プロジェクトオプション で、**追加**、**既存ファイルの追加** の順に選択し、このディレクトリに移動します)。

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

ここで、 **libxammac**ファイルを選択します。   コピー、リンク、移動のいずれかを選択できます。   私はリンクを気に入っていますが、コピーも同様に機能します。    次に、ファイルを選択し、プロパティパッド (プロパティパッドが表示されていない場合は **[表示 > パッド > プロパティ]** を選択します) で、 **[ビルド]** セクションにアクセスし、 **[出力ディレクトリにコピー]** 設定を **[新しい場合はコピー]** する に設定します。

これで、Xamarin. Mac アプリケーションを実行できるようになりました。

Bin ディレクトリの結果は次のようになります。

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

このアプリを実行するには、すべてのファイルが同じディレクトリに存在する必要があります。

## <a name="building-a-standalone-application-for-distribution"></a>配布用のスタンドアロンアプリケーションを構築する

1つの実行可能ファイルをユーザーに配布することができます。  これを行うには、 `mkbundle`ツールを使用して、さまざまなファイルを自己完結型の実行可能ファイルに変換します。

まず、アプリケーションがコンパイルされて実行されていることを確認します。   結果に問題がなければ、コマンドラインから次のコマンドを実行できます。

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

上記のコマンドライン呼び出しでは、オプション`-o`を使用して、生成された出力を指定します`/tmp/consoleapp`。この例では、を渡しています。   これは、配布できるスタンドアロンアプリケーションであり、Mono または Xamarin. Mac に外部依存関係がありません。これは完全に完結している実行可能ファイルです。

コマンドラインでは、使用する**machine.config**ファイルと、システム全体のライブラリマッピング構成ファイルを手動で指定しました。   これらは、すべてのアプリケーションに必要なわけではありませんが、.NET の機能をより多く使用するときに使用されるため、バンドルすると便利です。

## <a name="project-less-builds"></a>プロジェクトのないビルド

自己完結型の Xamarin. Mac アプリケーションを作成するための完全なプロジェクトは必要ありませんが、単純な Unix makefile を使用してジョブを実行することもできます。   次の例は、単純なコマンドラインアプリケーションのメイクファイルを設定する方法を示しています。

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)

bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

上記`Makefile`は、3つのターゲットを提供します。

- `make`プログラムをビルドします
- `make run`現在のディレクトリにプログラムをビルドして実行します
- `make bundle`自己完結型の実行可能ファイルを作成します
