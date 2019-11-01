---
title: Visual Studio から .xcarchive アーカイブを作成することはできますか?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 27697223735c4074dd508d3f603ef6aa4e47b1be
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031168"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio から .xcarchive アーカイブを作成することはできますか?

## <a name="for-xamarin-4"></a>Xamarin 4 の場合

Xamarin 4.x の時点では、`ArchiveOnBuild` プロパティを `true`に設定して、Windows から `.xcarchive` を作成できるようになりました。 たとえば、コマンドラインで `MSBuild` を使用すると、次のようになります。

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` は、以前に作成したアーカイブを表示するために、Xcode と Xamarin Studio 検索の両方を行う Mac ビルドホストの `$HOME/Library/Developer/Xcode/Archives` ディレクトリに配置されます。

`ArchiveOnBuild` プロパティに関するその他の注意事項については、この[Xamarin フォーラムの投稿](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635)を参照してください。 `ServerAddress` と `ServerUser` プロパティの詳細については、 [Windows での Xamarin のコマンドラインビルド](~/ios/get-started/installation/windows/connecting-to-mac/index.md)に関するドキュメントを参照してください。

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin 3 以前の場合

Xamarin 3.x Visual Studio 拡張機能には `.xcarchive` アーカイブを作成するためのメカニズムが用意されていません。 ただし、Mac で Xamarin Studio に `.xcarchive` アーカイブを作成するために使用されるロジック[はここで説明され](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5)ているので、必要に応じて独自の `.xcarchive` を手動で作成することもできます。

ただし、アプリストアに送信する `.xcarchive` は必要ないことに注意してください。 IPA ファイルは、アプリストア配布プロファイル (アドホック配布プロファイルではありません) で署名されている限り送信できます。

実際には、`.app` バンドル (App Store 配布プロファイルで署名されている) を zip 形式にして、その `.zip` ファイルを app store に送信するだけでもかまいません。

どちらの場合も、アプリケーションローダーアプリを使用して、(Xcode ではなく) アプリを送信できます。
