---
title: 既存の Mac アプリを更新します。
description: このドキュメントでは、Unified API にクラシック API から Xamarin.Mac アプリの更新に従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 5e6034b079bba5e884872e4f2096d677fd3641d0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61213563"
---
# <a name="updating-existing-mac-apps"></a>既存の Mac アプリを更新します。

Unified API を使用して既存のアプリを更新するには、プロジェクト ファイル自体にも名前空間と、アプリケーション コードで使用される Api への変更が必要です。

## <a name="the-road-to-64-bits"></a>64 ビットへの道

新しい Unified Api は、Xamarin.Mac アプリケーションからデバイスに 64 ビット アーキテクチャをサポートするために必要です。 2015 年 2 月 1 日の時点では、Apple は、Mac App Store に新しいアプリを送信するすべてが 64 ビット アーキテクチャをサポートすることが必要です。

Xamarin は Visual Studio for Mac と Visual Studio の両方がクラシック API から Unified API への移行プロセスを自動化するためのツールを提供します。 または、プロジェクト ファイルを手動で変換することができます。 自動ツールを使用して、ことを強くお勧め、この記事では両方の方法を説明します。

### <a name="before-you-start"></a>開始する前にしています.

既存のコードを Unified API に更新する前にすべてを除去することは高推奨される*コンパイル警告*します。 多く*警告*Classic API ではエラーになる統合に移行するとします。 クラシック API からコンパイラ メッセージは多くの場合、更新についてのヒントを提供するため、開始する前にそれらの修正が簡単です。

## <a name="automated-updating"></a>自動更新

Mac または Visual Studio の Visual Studio で既存の Mac プロジェクトを選択して、後、警告が修正されました**Xamarin.Mac Unified API への移行**から、**プロジェクト**メニュー。 例:

![](updating-mac-apps-images/beta-tool1.png "[プロジェクト] メニューから Xamarin.Mac Unified API に移行を選択します。")

自動移行を実行する前に、この警告に同意する必要があります (明らかに必ずこの冒険に着手する前にバックアップ/ソース コントロールがある)。

![](updating-mac-apps-images/migrate01.png "自動移行を実行する前に、この警告に同意します。")

Xamarin.Mac アプリケーションで、Unified API を使用する場合に選択できる 2 つのサポートされているターゲット フレームワーク種類があります。

- **Xamarin.Mac Mobile Framework** -これは、Xamarin.iOS と Xamarin.Android の完全なサブセットをサポートで使用される同じチューニングされた .NET framework**デスクトップ**フレームワーク。 これは、優れたリンク動作によるものより小さな平均バイナリを提供するために、推奨されたフレームワークです。
- **Xamarin.Mac .NET 4.5 Framework** -このフレームワークは、のサブセットでは、もう一度、**デスクトップ**フレームワーク。 削除し、ただし、はるかに少ないの完全な**デスクトップ**フレームワークよりも、 **Mobile** framework する必要があります _「機能」_ でほとんどの NuGet パッケージやサード パーティ製のライブラリです。 これにより、標準を使用する開発者**デスクトップ**アセンブリ中に、まだサポートされているフレームワークが、このオプションを使用してより大きなアプリケーション バンドルを生成します。 これは、サード パーティ製の .NET アセンブリが使用されていると互換性がないことをお勧めのフレームワーク、 **Xamarin.Mac Mobile Framework**します。 サポートされているアセンブリの一覧を参照してください、[アセンブリ](~/cross-platform/internals/available-assemblies.md)ドキュメント。

Xamarin.Mac アプリケーションの特定のターゲットを選択した場合の影響とターゲット フレームワークの詳細についてを参照してください、[ターゲット フレームワーク](~/mac/platform/target-framework.md)ドキュメント。 

ツールは、基本的に記載されているすべての手順を自動化、**手動で更新**セクションの次に示すし、は Unified API に既存の Xamarin.Mac プロジェクトを変換する、推奨される方法です。

## <a name="steps-to-update-manually"></a>手動で更新する手順

ここでも、したら、警告が解決されて、新しい Unified API を使用して Xamarin.Mac アプリを手動で更新するこれらの手順に従います。

### <a name="1-update-project-type--build-target"></a>1.ビルドのターゲット (&)、プロジェクトの更新の種類

プロジェクト フレーバーの変更、 **csproj**ファイルから`42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23`に`A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`します。 編集、 **csproj**ファイルの最初の項目を置き換え、テキスト エディターで、`<ProjectTypeGuids>`よう要素。

![](updating-mac-apps-images/csproj.png "ProjectTypeGuids 要素の最初の項目を置き換えるよう、テキスト エディターで、csproj ファイルを編集します。")

変更、**インポート**要素を含む`Xamarin.Mac.targets`に`Xamarin.Mac.CSharp.targets`よう。

![](updating-mac-apps-images/csproj2.png "示すように Xamarin.Mac.CSharp.targets Xamarin.Mac.targets を含むインポート要素を変更します。")

次の行の後にコードを追加、`<AssemblyName>`要素。

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

例:

![](updating-mac-apps-images/csproj3.png "< AssemblyName > 要素の後に次のコード行を追加します。")

### <a name="2-update-project-references"></a>2.プロジェクト参照を更新します。

Mac アプリケーション プロジェクトの展開**参照**ノード。 最初に表示されます、* 壊れた - **XamMac**参照 (プロジェクトの種類を変更した) ため、このスクリーン ショット。

![](updating-mac-apps-images/references.png "最初に表示されます XamMac が壊れている参照このスクリーン ショット")

をクリックして、**歯車アイコン**の横にある、 **XamMac**エントリと選択**削除**壊れている参照を削除します。

次に、右クリックし、**参照**フォルダー、**ソリューション エクスプ ローラー**選択**参照の編集**します。 参照の一覧の一番下までスクロールして、チェックだけでなく**Xamarin.Mac**します。

![](updating-mac-apps-images/references2.png "参照の一覧の一番下までスクロールして Xamarin.Mac 以外のチェック")

キーを押して**OK**プロジェクト参照の変更を保存します。

### <a name="3-remove-monomac-from-namespaces"></a>3.名前空間から MonoMac を削除します。

削除、 **MonoMac**プレフィックスで名前空間から`using`ステートメントまたは任意の場所、クラス名がされて完全修飾 (例。 `MonoMac.AppKit` 単なる`AppKit`)。

### <a name="4-remap-types"></a>4.型を再マップします。

[ネイティブ型](~/cross-platform/macios/nativetypes.md)されている使用されていたなどのインスタンスをいくつかの型に置き換わるものですが導入された`System.Drawing.RectangleF`で`CoreGraphics.CGRect`(など)。 型の完全な一覧で確認できます、[ネイティブ型](~/cross-platform/macios/nativetypes.md)ページ。

### <a name="5-fix-method-overrides"></a>5.メソッドのオーバーライドを修正します。

いくつか`AppKit`メソッド シグネチャに、新しい変更があった[ネイティブ型](~/cross-platform/macios/nativetypes.md)(など`nint`)。 カスタムのサブクラスは、これらのメソッドをオーバーライドする場合、署名と一致しなくなり、エラーが発生します。 ネイティブ型を使用して、新しいシグネチャに一致するサブクラスを変更することで、これらのメソッド オーバーライドを修正します。 

## <a name="considerations"></a>注意事項

次の考慮事項は、そのアプリが 1 つ以上のコンポーネントまたは NuGet パッケージに依存する場合は、新しい Unified API にクラシック API から既存の Xamarin.Mac プロジェクトを変換するときに考慮する必要があります。 

### <a name="components"></a>コンポーネント

アプリケーションに含める任意のコンポーネントは、Unified API に更新する必要があります。 またはコンパイルしようとすると、競合が表示されます。 含まれるコンポーネントの現在のバージョン、Unified API をサポートする Xamarin コンポーネント ストアから新しいバージョンに置き換えるし、クリーン ビルドを実行します。 作成者によって変換されていない任意のコンポーネントでは、32 ビットのコンポーネント ストアにのみ警告を表示します。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API サポートを使用する NuGet への変更を投稿しました中がありますが、NuGet の新しいリリースのため、新しい Api を認識する NuGet を取得する方法を評価します。 

コンポーネントと同様、その時点までには、Unified Api をサポートするバージョンをプロジェクトに含めるすべての NuGet パッケージを切り替えて、その後、クリーン ビルドを実行する必要があります。

> [!IMPORTANT]
> フォームでエラーがあれば _"エラー 3 は、同じ Xamarin.Mac プロジェクトに 'monomac.dll' と 'Xamarin.Mac.dll' の両方を含めることはできません - 'Xamarin.Mac.dll' は 'monomac.dll' によって参照されますが、明示的に参照される ' xxx、バージョン 0.0.000、カルチャを = =neutral, PublicKeyToken = null'"_ Unified Api にアプリケーションを変換した後は通常、Unified API に更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージすることが原因です。 既存のコンポーネント/NuGet の削除を許可し、Unified Api をサポートするバージョンに更新し、クリーン ビルドを実行する必要があります。

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin.Mac アプリのビルドを 64 ビットの有効化

Unified API に変換された、Xamarin.Mac モバイル アプリケーション、開発者もに、アプリのオプションから 64 ビット コンピューターでアプリケーションのビルドを有効にする必要あります。 参照してください、**を有効にする 64 ビット ビルドの Xamarin.Mac アプリ**の[32/64 ビットのプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)64 ビットの有効化の詳細な手順についてはドキュメントを作成します。
    
## <a name="finishing-up"></a>最後の仕上げ

クラシックから Unified Api に、Xamarin.Mac アプリケーションを変換する自動または手動のメソッドを使用することを選択するかどうかをさらの手動による介入が必要とするいくつかのインスタンスがあります。 参照してください、 [Unified API にコードを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)ドキュメントの既知の問題と回避策。

## <a name="related-links"></a>関連リンク

- [コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
