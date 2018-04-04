---
title: Visual Studio から .xcarchive アーカイブを作成することは?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 23af3bf277ab68ffe93df2e1ee8ee64afc01828d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio から .xcarchive アーカイブを作成することは?

## <a name="for-xamarin-4"></a>Xamarin 4 の

Xamarin の時点で 4.x では、これで作成すること、 `.xcarchive` Windows を設定してから、`ArchiveOnBuild`プロパティを`true`です。 たとえばを使用して`MSBuild`コマンド ラインで。

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive`に配置される、 `$HOME/Library/Developer/Xcode/Archives` Mac ビルド ホスト Xcode と Xamarin Studio の両方が以前に構築されて表示アーカイブに検索されるディレクトリ。

これを参照してください[Xamarin フォーラム投稿](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635)に関する追加情報を簡単ないくつかの`ArchiveOnBuild`プロパティです。 ドキュメントを参照してください[Xamarin.iOS コマンド ラインが Windows でビルド](~/ios/get-started/installation/windows/connecting-to-mac/index.md)の詳細については、`ServerAddress`と`ServerUser`プロパティです。

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin 3 およびそれ以前の

3.x Visual Studio の Xamarin 拡張機能が生成するメカニズムを提供しません`.xcarchive`をアーカイブします。 ただし、作成に使用されるロジック`.xcarchive`Mac で Xamarin Studio でアーカイブ[ここに記載されて](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5)独自に作成することが可能性がありますので、`.xcarchive`する場合に手動でします。

注意する必要はありませんが、`.xcarchive`アプリ ストアに送信します。 アプリ ストア ディストリビューション プロファイル (、アドホック ディストリビューション プロファイルではなく) で署名されている限り、IPA ファイルを送信することができます。

Zip だけが実際には、`.app`バンドル (App ストアのディストリビューション プロファイルを使用して署名)、および送信する`.zip`アプリ ストアへのファイルです。

どちらの場合、アプリ (なく Xcode) を送信するアプリケーション ローダー アプリを使用できます。

