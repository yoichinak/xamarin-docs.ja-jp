---
title: Xcode で Xamarin.iOS アプリのデバッグ
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5FDDEDB3-AEB9-4D9C-9F7B-FEFAA9AF0031
ms.technology: xamarin-ios
author: migueldeicaza
ms.author: miguel
ms.date: 02/22/2018
ms.openlocfilehash: e0127d4b24236d350e5fa967110316544c320d0f
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514709"
---
# <a name="debugging-xamarinios-apps-with-xcode"></a>Xcode で Xamarin.iOS アプリのデバッグ

Xamarin.iOS アプリケーションの一部のデバッグに Xcode を使用するシナリオである可能性があります。 さらに、.NET コードをデバッグすることはありませんが、ネイティブ コードをデバッグし、Xcode でネイティブのビジュアライザーの一部を使用できます。

## <a name="walkthrough"></a>チュートリアル

Xcode でのデバッグに Visual Studio for Mac の組み込みのサポートはありませんが、これを実現するために、次の手順を使用できます。

1. Xamarin アプリで 1 つとして同じバンドル ID を持つには、Xcode の iOS アプリを作成します。
   
    - 開き、Xamarin.iOS プロジェクトのバンドル id を見つけることができます、 **Info.plist**ファイル。

        ![Info.plist を編集](debugging-with-xcode-images/vsmac-infoplist.png "Info.list の編集")

    - Xcode でプロジェクトを作成するときに、またはプロジェクトのターゲットを選択して、バンドル id を設定します。

        ![Xcode でバンドル Id の設定](debugging-with-xcode-images/xcode-bundle.png "Xcode でバンドル Id の設定")

2. アプリを自動的に起動するのではなく起動を待機する Xcode プロジェクトを変更します。

    - オープン、**スキームの編集パネル**を選択して**製品 > スキーム > 編集スキーム**またはを使用して、 **cmd⌘ + <** キーボード ショートカット。

    - 選択、**実行**スキーム、および右のパネルが表示**起動**オプション。 選択**実行可能ファイルを起動するまで待ちます** をクリック**閉じる**します。

        ![実行可能ファイルを起動するまで待ちます](debugging-with-xcode-images/xcode-schemes.png "実行可能ファイルを起動するまで待ちます")

3. Xcode プロジェクトを実行します。

    これにより、デバイスで、ダミーの Xcode のアプリがインストールされますが、起動はしません。

4. Xamarin アプリを実行します。

    Xcode を起動するとき、Xamarin アプリにアタッチする必要があります。

### <a name="caveats"></a>注意事項

起動するたびに、Xamarin.iOS アプリに少し変更を作成する必要があります。 それ以外の場合、Visual Studio for Mac がアプリを構築する必要があることを検出するは*と*が既にインストールされている Xcode のダミー アプリ経由で再インストールことはありません。

## <a name="alternative---using-lldb"></a>代替 - lldb を使用します。

使用してに慣れている場合**lldb** 、コマンドラインより簡単に解決します。

シェルでは、次のコマンドを入力します。

```bash
touch ~/.mtouch-launch-with-lldb
```

手順が表示されます、**アプリケーション出力**ウィンドウ、アプリケーションを実行するときに、基本的には、何ができますを使用する**lldb**アプリケーションをデバッグするためのコマンドラインから。
