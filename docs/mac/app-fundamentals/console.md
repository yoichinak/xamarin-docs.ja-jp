---
title: Xamarin.Mac のバインドを使用して、コンソール アプリ向け
description: Xamarin.Mac を使用してネイティブ macOS Api にアクセスするヘッドレスのコンソール アプリを作成します。
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: 135ef06cd044235023c3acc970c8e7a33f144b47
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320827"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>コンソール アプリケーションで Xamarin.Mac バインド

ヘッドレス アプリケーションを構築する (C#) Apple のネイティブ Api の一部を使用するいくつかのシナリオがある&ndash;いずれかのユーザー インターフェイスがない&ndash;c# を使用します。

Mac アプリケーションのプロジェクト テンプレートへの呼び出しを含める`NSApplication.Init()`への呼び出し後に`NSApplication.Main(args)`、ように、通常は表示されます。

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

呼び出し`Init`、Xamarin.Mac のランタイムへの呼び出しを準備します`Main(args)`Cocoa のアプリケーション メイン ループには、キーボードとマウスのイベントを受信し、アプリケーションのメイン ウィンドウを表示するアプリケーションの準備を開始します。   呼び出し`Main`試みます Cocoa リソースを見つけ、最上位ウィンドウを準備して、アプリケーション バンドルの一部として、プログラムが必要です (ディレクトリに分散プログラム、`.app`拡張機能と非常に特定のレイアウト)。

ヘッドレス アプリケーションでは、ユーザーは必要はありません、インターフェイスし、アプリケーション バンドルの一部として実行する必要はありません。

## <a name="creating-the-console-app"></a>コンソール アプリケーションの作成

したがって、通常の .NET コンソール プロジェクトの種類を開始することをお勧めします。

いくつかの作業を行う必要があります。

- 空のプロジェクトを作成します。
- Xamarin.Mac.dll ライブラリを参照します。
- プロジェクトには、非管理対象の依存関係を表示します。

次の手順については、以下で詳しく説明します。

### <a name="create-an-empty-console-project"></a>空のコンソール プロジェクトを作成します。

新しい .NET コンソール プロジェクトを作成、.NET と .NET Core ではなく、Xamarin.Mac.dll としては、.NET Core ランタイムで実行されません、Mono ランタイムで実行のみであることを確認します。

### <a name="reference-the-xamarinmac-library"></a>Xamarin.Mac のライブラリを参照します。

コードをコンパイルするを参照するが、`Xamarin.Mac.dll`このディレクトリからアセンブリ。 `/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

これを行うには、プロジェクト間参照移動、 **.NET アセンブリ**タブをクリックし、をクリックして、**参照**ファイル システム上のファイルを検索するボタンをクリックします。  上記のパスに移動して選択し、 **Xamarin.Mac.dll**そのディレクトリから。

Cocoa Api へアクセスを付与すると、コンパイル時にこれは。   この時点では、追加することができます`using AppKit`、ファイル、および呼び出しの先頭に、`NSApplication.Init()`メソッド。   1 つ以上のステップがあるアプリケーションを実行する前にします。

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>非管理対象のサポート ライブラリをプロジェクトに取り込む

アプリケーションを実行すると、前にする必要がある、`Xamarin.Mac`をプロジェクトにライブラリをサポートします。   これを行うには、プロジェクトに新しいファイルを追加 (プロジェクトのオプションを選択**追加**、し**既存ファイルの追加**) このディレクトリに移動します。

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

ここでは、ファイルを選択します。 **libxammac.dylib**します。   コピー、移動、およびリンクの選択肢が提供されます。   リンク、個人的に好きですが、同様に機能をコピーします。    ファイルを選択する必要がありますとプロパティ パッド (選択**ビュー > パッド > プロパティ**プロパティ パッドが表示されない場合) に移動、**ビルド**セクションし、設定、**出力ディレクトリにコピーディレクトリ**設定**新しい場合はコピー**します。

Xamarin.Mac アプリケーションを実行できます。

結果、bin ディレクトリでは、次のようになります。

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

このアプリを実行するには、これらのファイルを同じディレクトリにする必要があります。

## <a name="building-a-standalone-application-for-distribution"></a>配布用のスタンドアロン アプリケーションの構築

ユーザーに 1 つの実行可能ファイルを配布する場合があります。  これを行うには、使用することができます、`mkbundle`自己完結型の実行可能ファイルに、さまざまなファイルを有効にするツール。

まず、アプリケーションをコンパイルして実行することを確認してください。   結果に満足したら後、は、コマンドラインから次のコマンドを実行できます。

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

上記のコマンドライン呼び出し、オプションで`-o`が生成された出力を指定するために、この場合は、渡された`/tmp/consoleapp`します。   これは、ここで配布できるスタンドアロン アプリケーションを完全に自己完結型の実行可能ファイルは、Mono または Xamarin.Mac で外部の依存関係がありません。

手動で指定されたコマンドライン、 **machine.config**ファイルを使用して、システム全体のライブラリのマッピングの構成ファイル。   これらは、すべてのアプリケーションは必要ありませんが、.NET の多くの機能を使用するときに使用すると、それらをバンドルすると便利です。

## <a name="project-less-builds"></a>プロジェクトのないビルド

自己完結型の Xamarin.Mac アプリケーションを作成する完全なプロジェクトを必要としない、ジョブの実行を取得する単純な Unix メイクファイルを使用することもできます。   次の例では、シンプルなコマンド ライン アプリケーションのメイクファイルをセットアップする方法を示しています。

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

上記`Makefile`3 つのターゲットを提供します。

- `make` プログラムをビルドします。
- `make run` ビルドし、現在のディレクトリでプログラムを実行
- `make bundle` 自己完結型の実行可能ファイルが作成されます。
