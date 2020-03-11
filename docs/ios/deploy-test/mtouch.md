---
title: mtouch を使用する Xamarin.iOS アプリのバンドル
description: このドキュメントでは mtouch について説明します。このツールでは、Xamarin.iOS アプリケーションのバンドルへの変換、シミュレーターでの起動、および物理デバイスへの展開に必要な多くの手順を実行します。
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 2a0f9d063b319c0f412f6e8f47a59f0f994678ae
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026281"
---
# <a name="using-mtouch-to-bundle-xamarinios-apps"></a>mtouch を使用する Xamarin.iOS アプリのバンドル

iPhone アプリケーションはアプリケーション バンドルとして出荷されています。 これは拡張子 `.app` を持つディレクトリであり、コード、データ、構成ファイル、ユーザーのアプリケーションを iPhone が認識するためのマニフェストが含まれます。

.NET 実行可能ファイルをアプリケーションに変換するプロセスは主に `mtouch` コマンドで行われます。これは、アプリケーションをバンドルに変換するために必要なさまざまな手順を統合するツールです。 このツールは、シミュレーターのアプリケーションを起動するときや実際の iPhone または iPod Touch デバイスにソフトウェアを展開するときにも利用されます。

## <a name="detailed-instructions"></a>詳しい手順

[mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) のマニュアル ページをご覧ください。mtouch ツールのあらゆる利用方法が紹介されています。

## <a name="installation"></a>インストール

Mac では、`mtouch` は Xamarin.iOS にバンドルされています。 次のディレクトリにあります。

**/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**

`mtouch` を使いやすくするには、その親ディレクトリをシステムの `PATH` 環境変数に追加します。  

たとえば、Bash でこれを行うには、 **~/.bash_profile** ファイルの末尾に次の行を追加します。

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> `mtouch` を使う場合、 **/Developer/MonoTouch/usr/bin** ( **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin** を示すシンボリック リンク) が存在することに依存しないでください。 このシンボリック リンクは、 **/Library/Frameworks/...** にインストールされなかった古い MonoTouch リリースとの互換性を維持するためにのみ存在し、将来のリリースでは存在しなくなる可能性があります。

## <a name="building"></a>ビルド

`mtouch` コマンドは 3 とおりの方法でコードをコンパイルできます。

- シミュレーター テストのためにコンパイルします。
- デバイス配置のためにコンパイルします。
- 実行可能ファイルをデバイスに配置します。

### <a name="building-for-the-simulator"></a>シミュレーターのビルド

使用を開始するときに、最も一般的に利用されるシナリオは、シミュレーターでアプリケーションを試すことです。`mtouch -sim` を利用し、コードをシミュレーター パッケージにコンパイルします。 これは次のように行われます。

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>デバイスのビルド

デバイスのソフトウェアをビルドするには、`mtouch -dev` オプションを利用してアプリケーションをビルドします。また、アプリケーションの署名に利用する証明書の名前を指定する必要があります。 次は、デバイス用のアプリケーションをビルドする方法を示したものです。

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

この特別なケースでは、"iPhone Developer: Miguel de Icaza" 証明書を使用してアプリケーションに署名します。 この手順は非常に重要です。この手順がなければ、物理デバイスはアプリケーションの読み込みを拒否します。

 <a name="Running_your_Application" />

## <a name="running-your-application"></a>アプリケーションを実行する

### <a name="launching-on-the-simulator"></a>シミュレーターで起動する

アプリケーション バンドルがあれば、シミュレーターでの起動は非常に簡単です。

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

`--sdkroot` フラグが設定されていない場合、既定で xcode-select パスに設定され、次の警告が表示されます。

> 例: 警告 MT0061: Xcode.app が (--sdkroot を使用して) 指定されていません。'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer によってレポートされたようなシステム Xcode を使用 

このコマンド ラインで次のような出力が生成されます。

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```

標準出力のログと標準エラー ファイルも保存しておくことを強くお勧めします。デバッグに役に立ちます。 `Console.WriteLine` は `stdout` に出力され、`Console.Error.WriteLine` とその他の実行時エラー メッセージは `stderr` に出力されます。

これを行うには、`--stdout` フラグと `--stderr` フラグを使用します。

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

アプリケーションにエラーが発生した場合、出力とエラーを表示し、問題を診断できます。

### <a name="deploying-to-a-device"></a>デバイスへの展開

デバイスに配置するには、Apple の[デバイスの管理](https://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html)に関するドキュメントに基づいてデバイスをプロビジョニングする必要があります。 デバイスが適切にプロビジョニングされると、mtouch コマンドを利用し、コンパイル済みの ".app" をデバイスに配置できます。 これは次のコマンドで行います。

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

`--sdkroot` フラグが設定されていない場合、既定で xcode-select パスに設定され、次の警告が表示されます。

> 例: 警告 MT0061: Xcode.app が (--sdkroot を使用して) 指定されていません。'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer によってレポートされたようなシステム Xcode を使用 

以上の手順は通常、Visual Studio for Mac で実行されます。

## <a name="reference"></a>関連項目

その他のコマンド ライン オプションについては、[mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) マニュアル ページをご覧ください。

