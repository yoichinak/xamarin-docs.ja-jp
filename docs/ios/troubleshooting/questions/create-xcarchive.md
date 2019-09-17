---
title: Visual Studio から .xcarchive アーカイブを作成することはできますか?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 1b078b8cb4d1129127997e9fabdd0b128e09c90f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769368"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio から .xcarchive アーカイブを作成することはできますか?

## <a name="for-xamarin-4"></a>Xamarin 4 の場合

Xamarin 4.x の時点では、 `.xcarchive` `ArchiveOnBuild`プロパティをに`true`設定することによって、Windows からを作成できるようになりました。 たとえば、コマンドライン`MSBuild`でを使用すると、次のようになります。

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

は`.xcarchive` 、以前に作成し`$HOME/Library/Developer/Xcode/Archives`たアーカイブを表示するために Xcode と Xamarin Studio 検索の両方を実行する Mac ビルドホスト上のディレクトリに配置されます。

プロパティに関するその他の注意事項については`ArchiveOnBuild` 、この[Xamarin フォーラムの投稿](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635)を参照してください。 および`ServerAddress` プロパティの詳細については、[Windows上のXamarinのコマンドラインビルド](~/ios/get-started/installation/windows/connecting-to-mac/index.md)に関するドキュメントを参照して`ServerUser`ください。

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin 3 以前の場合

Xamarin 3.x Visual Studio 拡張機能には、アーカイブを作成`.xcarchive`するためのメカニズムが用意されていません。 ただし、ここでは、Mac 上`.xcarchive`の Xamarin Studio にアーカイブを作成するために使用するロジックに[つい](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5)て説明`.xcarchive`します。そのため、必要に応じて手動で作成することもできます。

しかし、アプリストアに送信するための`.xcarchive`が不要であることに注意してください。 IPA ファイルは、アプリストア配布プロファイル (アドホック配布プロファイルではありません) で署名されている限り送信できます。

実際には、バンドルを (アプリストア配布`.app`プロファイルで署名された) zip 形式で圧縮し、その`.zip`ファイルを app store に送信することもできます。

どちらの場合も、アプリケーションローダーアプリを使用して、(Xcode ではなく) アプリを送信できます。
