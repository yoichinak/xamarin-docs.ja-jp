---
title: Xcode を使用したユーザーインターフェイスの設計
description: Mac で Xcode を使用して、iOS ユーザーインターフェイスを直接作成するための推奨される方法について説明します。
ms.prod: xamarin
ms.assetid: af9f95db-5cd6-475d-867d-f73e1574e8fc
ms.technology: xamarin-ios
author: joshspicer
ms.author: jospicer
ms.date: 10/27/2020
ms.openlocfilehash: b25245250bd9ea17034ea8f6b11388a12fc13241
ms.sourcegitcommit: d1f0e0a9100548cfe0960ed2225b979cc1d7c28f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96439575"
---
# <a name="designing-user-interfaces-with-xcode"></a>Xcode を使用したユーザーインターフェイスの設計

Visual Studio 2019 バージョン16.8 および Visual Studio for Mac バージョン8.8 以降では、nib ファイルを編集するために、Mac 上の Xcode Interface Builder で編集することをお勧めします。

> [!NOTE]
> Visual Studio 2019 バージョン16.9 以降では、Windows で iOS のストーリーボードを編集する方法はサポートされていません。 Visual Studio for Mac と Xcode Interface Builder を使用して、Xamarin のユーザーインターフェイスの作成を続行します。

この記事では、Xcode Interface Builder を使用してユーザーインターフェイスを構築するための一般的なソリューションについて説明します。  この記事は、以前に Xamarin の iOS デザイナーを使用して Ui を編集した場合に特に役立ちます。 

ストーリーボードの詳細なチュートリアルについては、「 [Xamarin. iOS のストーリーボード](./indepth-storyboard.md)」を参照してください。

## <a name="how-to-open-a-storyboard"></a>ストーリーボードを開く方法 

ストーリーボードファイルを右クリックし、[ **Xcode Interface Builder**] を選択して Visual Studio for Mac で iOS ユーザーインターフェイスファイルを開きます。

[![Interface Builder の選択](images/select-interface-builder.png)](images/select-interface-builder.png#lightbox)

Xcode ウィンドウが開いたことを確認します。 ここで保存した編集は、Visual Studio プロジェクトに反映されます。

[![Xcode ウィンドウ](images/xcode.png)](images/xcode.png#lightbox)

Xcode Interface Builder の詳細については、「 [組み込み Interface Builder](https://developer.apple.com/xcode/interface-builder/)」を参照してください。

## <a name="creating-a-new-control"></a>新しいコントロールの作成

Xcode Interface Builder で新しいコントロールを作成するには、まず、編集するストーリーボードを選択します。 次に、[Xcode library] ダイアログボックス ([**View**  >  **Show Library**]) を開き、コントロールをストーリーボードにドラッグします。

[![ライブラリピッカー](images/library-picker.png)](images/library-picker.png#lightbox)

次に、対応するビューコントローラーのヘッダーファイルを開きます。  空の "単一ビュー" Xamarin. iOS アプリの場合、既定のストーリーボードは **メインストーリーボード** と呼ばれます。 対応するビューコントローラーファイルは、Xcode から表示すると、対応する **viewcontroller .h** ヘッダーファイルと共に Visual Studio で **ViewController.cs** と呼ばれます。

Xcode Interface Builder から、ストーリーボードと対応するビューコントローラーのヘッダーファイルの両方を開きます。  **Control** キー ( **^** ) を押しながら、Xcode によってダイアログボックスが表示されるまで、ストーリーボードのコントロールをビューコントローラーファイルにドラッグします。

[![デモリンクコントロール](images/demo-link-control.gif)](images/demo-link-control.gif#lightbox)

上の図に示すように、対応する C# コードはビューコントローラーの分離コードファイルに自動的に生成されます。  これで、Xamarin. iOS プロジェクト内のこのコントロールにアクセスできるようになります。

## <a name="editing-an-existing-controls-name"></a>既存のコントロール名の編集

Xcode Interface Builder から既存のコントロールの名前を編集し、その変更が C# プロジェクトに反映されるようにするには、適切なビューコントローラーのヘッダーファイルに移動して右クリックし、[ **リファクター**] を選択します。   

[![リファクターコントロール](images/refactor-control.png)](images/refactor-control.png#lightbox)

分離コードファイルは新しい名前で再生成され、Visual Studio for Mac のコードを使用してコントロールにアクセスできるようになります。

## <a name="known-problems"></a>既知の問題

このセクションでは、既知の問題について説明します。

### <a name="visual-studio-could-not-communicate-with-xcode"></a>"Visual Studio は Xcode と通信できませんでした"

MacOS Catalina.properties 以降では、以下のエラーが発生する場合があります。

[![エラーを通知しない](images/could-not-communicate.png)](images/could-not-communicate.png#lightbox)

まず、Mac のシステム設定の [ **セキュリティ & プライバシー > Automation**] で、Visual Studio が表示され、 **Xcode** がオンになっていることを確認します。

[![macOS のセキュリティ](images/macos-security.png)](images/macos-security.png#lightbox)

**Xcode** がオンになっていて、エラーメッセージがまだ表示されている場合は、Visual Studio for Mac プライバシーアクセス許可のリセットが必要になることがあります。

これは、ターミナルウィンドウを起動し、次のコマンドを発行することで実現できます。

```bash
sudo tccutil reset All "com.microsoft.visual-studio"
```

上記の変更を確実に適用するには、Mac の PRAM をリセットします。 手順について [は、「Mac で NVRAM または PRAM をリセット](https://support.apple.com/HT204063)する」を参照してください。
