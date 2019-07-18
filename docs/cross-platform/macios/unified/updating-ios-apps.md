---
title: 既存の iOS アプリを更新しています
description: このドキュメントでは、Unified API にクラシック API からの Xamarin.iOS アプリを更新に従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 4e471aaca2a7a5f247b21dd1c1863a01b062a716
ms.sourcegitcommit: afe9d93373d66eb45d82cabefca83b5733969634
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855753"
---
# <a name="updating-existing-ios-apps"></a>既存の iOS アプリを更新しています

_Unified API を使用して既存の Xamarin.iOS アプリを更新する次の手順に従います。_

Unified API を使用して既存のアプリを更新するには、プロジェクト ファイル自体にも名前空間と、アプリケーション コードで使用される Api への変更が必要です。

## <a name="the-road-to-64-bits"></a>64 ビットへの道

新しい Unified Api Xamarin.iOS モバイル アプリケーションからデバイスに 64 ビット アーキテクチャをサポートするために必要です。 2015 年 2 月 1 日の時点では、Apple は、iTunes App Store に新しいアプリを送信するすべてが 64 ビット アーキテクチャをサポートすることが必要です。

Xamarin は Visual Studio for Mac と Visual Studio の両方がクラシック API から Unified API への移行プロセスを自動化するためのツールを提供します。 または、プロジェクト ファイルを手動で変換することができます。 自動ツールを使用して、ことを強くお勧め、この記事では両方の方法を説明します。

### <a name="before-you-start"></a>開始する前にしています.

既存のコードを Unified API に更新する前にすべてを除去することは高推奨される*コンパイル警告*します。 多く*警告*Classic API ではエラーになる統合に移行するとします。 クラシック API からコンパイラ メッセージは多くの場合、更新についてのヒントを提供するため、開始する前にそれらの修正が簡単です。

## <a name="automated-updating"></a>自動更新

For Mac または Visual Studio Visual Studio で既存の iOS プロジェクトを選択して、警告が解決されると後、 **Xamarin.iOS Unified API への移行**から、**プロジェクト**メニュー。 例えば:

![](updating-ios-apps-images/beta-tool1.png "[プロジェクト] メニューから Xamarin.iOS Unified API に移行を選択します。")

自動移行を実行する前に、この警告に同意する必要があります (明らかに必ずこの冒険に着手する前にバックアップ/ソース コントロールがある)。

![](updating-ios-apps-images/beta-tool2.png "自動移行を実行する前に、この警告に同意します。")

ツールは、基本的に記載されているすべての手順を自動化、**手動で更新**セクションの次に示すし、は Unified API に既存の Xamarin.iOS プロジェクトを変換する、推奨される方法です。

## <a name="steps-to-update-manually"></a>手動で更新する手順

ここでも、したら、警告が解決されて、新しい Unified API を使用して Xamarin.iOS アプリを手動で更新するこれらの手順に従います。

### <a name="1-update-project-type--build-target"></a>1. ビルドのターゲット (&)、プロジェクトの更新の種類

プロジェクト フレーバーの変更、 **csproj**ファイルから`6BC8ED88-2882-458C-8E55-DFD12B67127B`に`FEACFBD2-3405-455C-9665-78FE426C6842`します。 編集、 **csproj**ファイルの最初の項目を置き換え、テキスト エディターで、`<ProjectTypeGuids>`よう要素。

![](updating-ios-apps-images/csproj.png "ProjectTypeGuids 要素の最初の項目を置き換えるよう、テキスト エディターで、csproj ファイルを編集します。")

変更、**インポート**要素を含む`Xamarin.MonoTouch.CSharp.targets`に`Xamarin.iOS.CSharp.targets`よう。

![](updating-ios-apps-images/csproj2.png "示すように Xamarin.iOS.CSharp.targets Xamarin.MonoTouch.CSharp.targets を含むインポート要素を変更します。")

### <a name="2-update-project-references"></a>2. プロジェクト参照を更新します。

IOS アプリケーション プロジェクトの展開**参照**ノード。 最初に表示されます、* 壊れた - **monotouch**参照 (プロジェクトの種類を変更した) ため、このスクリーン ショット。

![](updating-ios-apps-images/references.png "最初に表示されます分割 monotouch の参照をこのスクリーン ショットのようなプロジェクトの種類が変更されたため")

IOS アプリケーション プロジェクトを右クリックして**参照の編集**、をクリックして、 **monotouch**を参照し、赤い"X"ボタンを使用して削除します。

![](updating-ios-apps-images/references-delete-monotouch-sml.png "参照の編集に iOS アプリケーション プロジェクトを右クリックし、monotouch 参照をクリックし、赤を使用して削除 X ボタン")

参照の一覧とティックの末尾にスクロールして、 **Xamarin.iOS**アセンブリ。

![](updating-ios-apps-images/references-add-xamarinios-sml.png "今すぐティック Xamarin.iOS アセンブリ、参照の一覧の末尾までスクロールします。")

キーを押して**OK**プロジェクト参照の変更を保存します。

### <a name="3-remove-monotouch-from-namespaces"></a>3.名前空間から MonoTouch を削除します。

削除、 **MonoTouch**プレフィックスで名前空間から`using`ステートメントまたは任意の場所、クラス名がされて完全修飾 (例。 `MonoTouch.UIKit` 単なる`UIKit`)。

### <a name="4-remap-types"></a>4。型を再マップします。

[ネイティブ型](~/cross-platform/macios/nativetypes.md)されている使用されていたなどのインスタンスをいくつかの型に置き換わるものですが導入された`System.Drawing.RectangleF`で`CoreGraphics.CGRect`(など)。 型の完全な一覧で確認できます、[ネイティブ型](~/cross-platform/macios/nativetypes.md)ページ。

### <a name="5-fix-method-overrides"></a>5。メソッドのオーバーライドを修正します。

いくつか`UIKit`メソッド シグネチャに、新しい変更があった[ネイティブ型](~/cross-platform/macios/nativetypes.md)(など`nint`)。 カスタムのサブクラスは、これらのメソッドをオーバーライドする場合、署名と一致しなくなり、エラーが発生します。 ネイティブ型を使用して、新しいシグネチャに一致するサブクラスを変更することで、これらのメソッド オーバーライドを修正します。

例としては、変更する`public override int NumberOfSections (UITableView tableView)`を返す`nint`型とパラメーターの型を返す両方を変更して`public override int RowsInSection (UITableView tableView, int section)`に`nint`します。

## <a name="considerations"></a>考慮事項

次の考慮事項は、そのアプリが 1 つ以上のコンポーネントまたは NuGet パッケージに依存する場合は、新しい Unified API にクラシック API から既存の Xamarin.iOS プロジェクトを変換するときに考慮する必要があります。

### <a name="components"></a>コンポーネント

アプリケーションに含める任意のコンポーネントは、Unified API に更新する必要があります。 またはコンパイルしようとすると、競合が表示されます。 含まれるコンポーネントの現在のバージョン、Unified API をサポートする Xamarin コンポーネント ストアから新しいバージョンに置き換えるし、クリーン ビルドを実行します。 作成者によって変換されていない任意のコンポーネントでは、32 ビットのコンポーネント ストアにのみ警告を表示します。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API サポートを使用する NuGet への変更を投稿しました中がありますが、NuGet の新しいリリースのため、新しい Api を認識する NuGet を取得する方法を評価します。

コンポーネントと同様、その時点までには、Unified Api をサポートするバージョンをプロジェクトに含めるすべての NuGet パッケージを切り替えて、その後、クリーン ビルドを実行する必要があります。

> [!IMPORTANT]
> フォームでエラーがあれば _"エラー 3 は、同じ Xamarin.iOS プロジェクトで 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません - 'Xamarin.iOS.dll' は 'monotouch.dll' によって参照されますが、明示的に参照される ' xxx、バージョン = 0.0.000、Culture = neutral, PublicKeyToken = null'"_ Unified Api にアプリケーションを変換した後は通常、Unified API に更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージすることが原因です。 既存のコンポーネント/NuGet の削除を許可し、Unified Api をサポートするバージョンに更新し、クリーン ビルドを実行する必要があります。

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS アプリのビルドを 64 ビットの有効化

Unified API に変換されている Xamarin.iOS モバイル アプリケーション、開発者もに、アプリのオプションから 64 ビット コンピューターでアプリケーションのビルドを有効にする必要あります。 参照してください、**を有効にする 64 ビット ビルドの Xamarin.iOS アプリ**の[32/64 ビットのプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md#enable-64)64 ビットの有効化の詳細な手順についてはドキュメントを作成します。

## <a name="finishing-up"></a>最後の仕上げ

クラシックから Unified Api に、Xamarin.iOS アプリケーションを変換する自動または手動のメソッドを使用することを選択するかどうかをさらの手動による介入が必要とするいくつかのインスタンスがあります。 参照してください、 [Unified API にコードを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)ドキュメントの既知の問題と回避策。

## <a name="related-links"></a>関連リンク

- [コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
