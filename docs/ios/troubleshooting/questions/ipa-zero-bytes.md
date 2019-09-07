---
title: IPA ファイルが 0 バイトです
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: ba86567213fe76847459df3df9b4450881e0651e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769232"
---
# <a name="ipa-file-is-0-bytes"></a>IPA ファイルが 0 バイトです

> [!IMPORTANT]
> この問題は、最新バージョンの Xamarin で解決されました。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。

以前のバージョンの Xamarin では、Windows 上の IPA ファイルが0バイトになる可能性があるという既知の問題がいくつかありました。 

### <a name="fixed-in-xamarin-for-visual-studio-311584"></a>Xamarin for Visual Studio 3.11.584 で修正済み 
- [バグ 24416-コマンドラインから "アドホック" 構成をビルドしても、IPA ファイルが Windows にコピーされない](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [バグ 24417-"プロジェクトのプロパティ-> iOS IPA のオプション-> パッケージ名" を変更すると、IPA が Windows にコピーされないようにする](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [バグ 29822-[XVS. iOS 3.11] 設定 "Build" の番号が "Version" の番号と異なると、IPA が Windows にコピーされません](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### <a name="fixed-in-xamarin-for-visual-studio-410496"></a>Xamarin for Visual Studio 4.1.0.496 で修正済み
- [バグ 27989-ビルドサーバーでの ipa ファイルの表示が、アセンブリ名がプロジェクト名と一致しない場合に失敗する](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)
