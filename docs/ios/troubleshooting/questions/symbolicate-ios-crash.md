---
title: IOS クラッシュ ログを symbolicate .dSYM ファイルはどこで入手できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ce60c19ab0b680e00338f517e5a3f17f725ed329
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>IOS クラッシュ ログを symbolicate .dSYM ファイルはどこで入手できますか。

Visual studio から iOS アプリを構築するときに symbolicate クラッシュ レポートに使用できる .dSYM ファイル パスのビルド ホスト上で終了します。
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

注意してください、`~/Library`フォルダーが表示される Finder で既定では、ため必要がある使用 Finder の**移動 > フォルダーに移動**メニューを入力:`~/Library/Caches/Xamarin/mtbs/builds/`フォルダーを開きます。  

できます再表示する代わりに、`~/Library`フォルダーを使用して、**表示オプションを表示**ホーム フォルダーのパネルです。 Finder でサイド バーで、ホーム フォルダーを選択して、[検索] メニューを使用する場合**ビュー > 表示オプションを表示**(または cmd j) にあるチェック ボックスが表示されます、**ライブラリ フォルダーの表示**です。


### <a name="see-also"></a>関連項目
- IOS を symbolicating の拡張手順のクラッシュ レポート。 [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [IOS アプリケーションのクラッシュ ログを demystifying](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
