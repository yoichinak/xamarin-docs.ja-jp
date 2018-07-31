---
title: Visual Studio から .xcarchive アーカイブを作成することはできますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 1c20534e1d4476ce7ff85d9ffd5ae8742c445983
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350211"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio から .xcarchive アーカイブを作成することはできますか。

## <a name="for-xamarin-4"></a>Xamarin 4

Xamarin の時点で 4.x に今すぐ作成することは、 `.xcarchive` Windows を設定してから、`ArchiveOnBuild`プロパティを`true`します。 たとえばを使用して`MSBuild`コマンドラインで。

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive`に配置されます、`$HOME/Library/Developer/Xcode/Archives`ディレクトリ Xcode と Xamarin Studio の両方が既にビルドされて表示アーカイブに検索を Mac ビルド ホストにします。

これを参照してください[Xamarin のフォーラム投稿](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635)に関するその他のメモの簡単ないくつかの`ArchiveOnBuild`プロパティ。 のドキュメントを参照して[Windows 上の Xamarin.iOS コマンド ライン ビルド](~/ios/get-started/installation/windows/connecting-to-mac/index.md)の詳細については、`ServerAddress`と`ServerUser`プロパティ。

* * *

## <a name="for-xamarin-3-and-earlier"></a>3 およびそれ以前の Xamarin 用

Xamarin 3.x の Visual Studio 拡張機能が生成するためのメカニズムを提供していない`.xcarchive`をアーカイブします。 ただし、作成に使用されるロジック`.xcarchive`Mac で Xamarin Studio でアーカイブ[ここに記載されて](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5)で独自に作成することがおそらくあるため、`.xcarchive`を希望する場合に手動でします。

注意する必要はありませんが、`.xcarchive`アプリ ストアに送信します。 App Store 配布プロファイル (、アドホック配布プロファイルではなく) で署名されている限り、IPA ファイルを送信できます。

Zip だけは実際には、`.app`つまり署名されて、アプリ ストアの配布プロファイル) は、バンドル、および送信する`.zip`app store へのファイル。

どちらの場合、アプリ (なく Xcode) を送信する、アプリケーション ローダー アプリを使用できます。

