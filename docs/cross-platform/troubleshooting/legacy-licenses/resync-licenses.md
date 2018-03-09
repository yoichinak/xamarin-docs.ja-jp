---
title: "どのように手動で再同期 Xamarin のライセンスですか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 2413b6b7563a6ed1e17a8db61d2d61ddc85e71ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>どのように手動で再同期 Xamarin のライセンスですか。

> [!IMPORTANT]
> 所有またはを使用していない限り、Xamarin アカウントにログインする必要がないために、このガイドはほとんどの MSDN ユーザーに適用されません、 [Xamarin コンポーネント ストア](https://components.xamarin.com/)または[Visual Studio for Mac (Mac)](~/cross-platform/get-started/requirements.md)です。




## <a name="overview"></a>概要

通常、ライセンス情報は再同期化する Xamarin ライセンス サーバーと自動的に IDE を起動するとき。 たとえば、ライセンスのアップグレード、ダウン グレード、および更新は通常自動的に表示されます、IDE の再起動後にします。 IDE に表示ライセンス情報が上に表示される情報と一致する必要があります、[アカウント ページで](https://store.xamarin.com/account/my/subscription/computers)です。 情報がまだ一致していない場合は、ストア アカウント情報が正しい、および、ライセンス再ログアウト、ディスク上のライセンス ファイルを削除およびに戻ってログインし、手動で同期最初再確認できます。

### <a name="note-for-ios-developers-on-windows"></a>Windows 上の iOS 開発者向けの注記

Windows 上の iOS 開発者可能性があります。 も for Visual Studio for Mac; これらの手順を実行する必要があります。場合でも、ペア Mac をビルド ホストを開発しない直接です。

## <a name="quick-manual-refresh-steps"></a>クイックの手動更新手順

このクイック プロセスでは、場合によっては十分な可能性のあるディスク上のライセンス ファイルを削除する手順をスキップします。 

1.  IDE を使用して、Xamarin アカウントからログアウトします。
    -   Visual Studio for Mac
        -   ようこそ画面の右上隅
        -   **Visual Studio for Mac > アカウント**(Mac)
        -   **ツール > アカウント**(Windows)
    -   Visual Studio
        -   **ツール > Xamarin アカウント**
2.  IDE で Xamarin アカウントにログインします。

## <a name="extended-manual-license-refresh-steps"></a>手動でのライセンスの更新手順の拡張

1.  IDE を使用して、Xamarin アカウントからログアウトします。 
2.  IDE を終了します。
3.  ストアのアカウント情報が正しいことを確認してください。 具体的には重複したコンピューター名に確認、[コンピューター ページ](https://store.xamarin.com/account/my/subscription/computers)です。

4.  重複したコンピューター名のペアがある場合を使用して、 **Deactivate**ドロップ ダウン メニュー項目を削除する_両方_の組み合わせのメンバー。
    
    ![ライセンスの https://store.xamarin.com/account/my/subscription/computers で非アクティブ化->](resync-licenses-images/deactivate.png "非アクティブ化するドロップダウン メニュー項目を使用して、ペアの両方のメンバーを削除するには")

5.  ディスク上にまだ存在しているライセンス ファイルの残りのコピーを削除します。
    -   Windows

        次のフォルダーのすべてを削除します。
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        Windows の既定のインストール。
        -   `%PROGRAMDATA%` を展開されます。 `C:\ProgramData`
        -   `%LOCALAPPDATA%` を展開されます。 `C:\Users\%USERNAME%\AppData\Local`

        `VirtualStore`ライセンスのコピー Windows によって特定のシナリオでは、自動的に作成します。 VirtualStore コピーが存在しない場合が読み取ら_代わりに_中のライセンスの`%PROGRAMDATA%`します。

    -   Mac

        次のフォルダーのすべてを削除します。

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        これらのパスにアクセスすることができます**Finder**を選択して、**移動 > フォルダーに移動**メニューとダイアログ ウィンドウに貼り付けること。 Finder が自動的に置き換えられます、~ ユーザー フォルダーとチルダ文字です。

6.  Mac 用の Visual Studio または Visual Studio とログを開くには、Xamarin アカウントにバックアップします。

## <a name="supplementary-information-individual-license-file-locations"></a>補足情報: 個別のライセンス ファイルの場所

### <a name="windows"></a>Windows

Windows では、個別のライセンス ファイルは、次の場所に格納されます。

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

用のライセンス*試用*別のサブスクリプションの名前します。

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

Mac では、個別のライセンス ファイルは、次の場所に格納されます。

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

用のライセンス*試用*別のサブスクリプションの名前します。

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>追加のトラブルシューティング手順

-   ユーザー名、コンピューター名、または完全に展開されたライセンス ファイルのパスのいずれかで、すべての非 ASCII 文字があるかどうかを確認してください。

-   [Xamarin の開発者向けのサポートに問い合わせてください。](http://xamarin.com/support)
