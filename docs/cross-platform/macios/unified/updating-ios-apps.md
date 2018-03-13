---
title: "IOS アプリの既存の更新"
description: "アプリを更新する既存 Xamarin.iOS Unified API を使用してこれらの手順に従います。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3114b2abdc96bc7d122ca0b86e1c53977f6df1be
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="updating-existing-ios-apps"></a>IOS アプリの既存の更新

_アプリを更新する既存 Xamarin.iOS Unified API を使用してこれらの手順に従います。_

Unified API を使用して既存のアプリを更新すると、同様に、プロジェクト ファイル自体の名前空間と、アプリケーション コードで使用される Api への変更が必要です。

## <a name="the-road-to-64-bits"></a>64 ビットへの道

新しい Unified Api は、Xamarin.iOS モバイル アプリケーションからデバイスに 64 ビット アーキテクチャをサポートするために必要です。 、2015 年 2 月 1 日の時点では、Apple は、iTunes アプリ ストアへのすべての新しいアプリケーション送信が 64 ビット アーキテクチャをサポートする必要があります。

Xamarin Mac 用の Visual Studio および Visual Studio の両方を Classic API から Unified API への移行プロセスを自動化するためのツールを提供するか、プロジェクト ファイルを手動で変換することができます。 自動のツールを使用して、強くお勧め、中に、この記事の内容は両方の方法を説明します。

### <a name="before-you-start"></a>開始する前にしています.

Unified API に、既存のコードを更新する前にすべてを除去することを強くお勧め*コンパイルの警告*です。 多く*警告*Classic API ではエラーになる統合に移行するとします。 Classic API からのメッセージのコンパイラは、更新するものにヒントを提供する多くの場合、ために、開始する前にそれらを修正することが簡単です。

## <a name="automated-updating"></a>自動更新

Mac または Visual Studio の Visual Studio で既存の iOS プロジェクトを選択して、警告が修正され後、 **Xamarin.iOS Unified API への移行**から、**プロジェクト**メニュー。 例:

![](updating-ios-apps-images/beta-tool1.png "[プロジェクト] メニューから Xamarin.iOS Unified API への移行を選択します。")

この警告は、自動的に移行を実行する前にすることに同意する必要があります (当然ながらようにこの adventure に着手する前にバックアップ/ソース コントロールがある)。

![](updating-ios-apps-images/beta-tool2.png "自動移行を実行する前に、この警告に同意します。")

ツールは基本的に記載されているすべてのステップを自動化、**手動で更新**セクションでは、次に示すし、推奨される方法を Unified API に Xamarin.iOS の既存のプロジェクトを変換します。

## <a name="steps-to-update-manually"></a>手動で更新する手順

もう一度、したら、警告が解決されて、新しい Unified API を使用する Xamarin.iOS アプリを手動で更新するこれらの手順に従います。

### <a name="1-update-project-type--build-target"></a>1.プロジェクトの更新の種類およびビルドのターゲット

プロジェクト フレーバーの変更、 **csproj**ファイルから`6BC8ED88-2882-458C-8E55-DFD12B67127B`に`FEACFBD2-3405-455C-9665-78FE426C6842`です。 編集、 **csproj**ファイルの最初の項目を置き換えて、テキスト エディターで、`<ProjectTypeGuids>`要素を示すようにします。

![](updating-ios-apps-images/csproj.png "ように、ProjectTypeGuids 要素の最初の項目を置き換えて、テキスト エディターで、csproj ファイルを編集します。")

変更、**インポート**要素を含む`Xamarin.MonoTouch.CSharp.targets`に`Xamarin.iOS.CSharp.targets`ように。

![](updating-ios-apps-images/csproj2.png "ように、Xamarin.iOS.CSharp.targets に Xamarin.MonoTouch.CSharp.targets を含むインポート要素を変更します。")

### <a name="2-update-project-references"></a>2.プロジェクト参照を更新します。

IOS アプリケーション プロジェクトの展開**参照**ノード。 最初に表示、* 壊れた - **monotouch**参照 (プロジェクトの種類を変更した) ため、このスクリーン ショットに類似します。

![](updating-ios-apps-images/references.png "最初に表示されます分割 monotouch の参照をこのスクリーン ショットのようなプロジェクトの種類が変更されたため")

IOS アプリケーション プロジェクトを右クリックして**参照の編集**、順にクリックして、 **monotouch**を参照し、赤い"X"ボタンを使用して削除します。

![](updating-ios-apps-images/references-delete-monotouch-sml.png "編集のリファレンスへの iOS アプリケーション プロジェクトを右クリックし、monotouch リファレンスをクリックし、赤を使用して削除 X ボタン")

参照の一覧および目盛りの末尾までスクロールして、 **Xamarin.iOS**アセンブリ。

![](updating-ios-apps-images/references-add-xamarinios-sml.png "ティック Xamarin.iOS アセンブリと参照の一覧の末尾にスクロールします。")

キーを押して**OK**プロジェクト参照の変更を保存します。

### <a name="3-remove-monotouch-from-namespaces"></a>3.名前空間から MonoTouch を削除します。

削除、 **MonoTouch**プレフィックスで名前空間から`using`ステートメントまたは任意の場所、classname 完全認定されたなどです。 `MonoTouch.UIKit` 単なる`UIKit`)。

### <a name="4-remap-types"></a>4.型を再マップします。

[ネイティブ型](~/cross-platform/macios/nativetypes.md)されている導入以前、使用されていたのインスタンスなど一部の種類を置換する`System.Drawing.RectangleF`で`CoreGraphics.CGRect`(など)。 型の完全な一覧にあります、[ネイティブ型](~/cross-platform/macios/nativetypes.md)ページ。

### <a name="5-fix-method-overrides"></a>5.メソッドのオーバーライドを修正します。

いくつか`UIKit`メソッドの署名を使用して、新しい変更があった[ネイティブ型](~/cross-platform/macios/nativetypes.md)(など`nint`)。 カスタムのサブクラスは、これらのメソッドをオーバーライドする場合、署名と一致しなくなります、エラーが発生します。 ネイティブ型を使用して新しいシグネチャと一致するサブクラスを変更することで、これらのメソッドのオーバーライドを修正します。

例としては、変更する`public override int NumberOfSections (UITableView tableView)`を返す`nint`両方型の戻り値のパラメーターの型を変更して`public override int RowsInSection (UITableView tableView, int section)`に`nint`です。

## <a name="considerations"></a>注意事項

次の考慮事項は、Classic API から新しい Unified API コンポーネントまたは NuGet パッケージの 1 つまたは複数のアプリケーションが依存している場合で Xamarin.iOS の既存のプロジェクトを変換するときに考慮する必要があります。

### <a name="components"></a>コンポーネント

アプリケーションに含まれている任意のコンポーネントも Unified API に更新する必要があります。 またはコンパイルしようとすると、競合が発生します。 含まれるコンポーネントの Unified API をサポートする Xamarin コンポーネント ストアから新しいバージョンで現在のバージョンを置き換えるし、クリーン ビルドを実行します。 32 ビットの唯一のコンポーネント ストアで警告の作成者によって変換されていない任意のコンポーネントが表示されます。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API のサポートを使用する NuGet への変更の原因は、中にされていない、NuGet の新しいリリースを新しい Api を認識する NuGet を取得する方法を検討しているためです。

コンポーネントと同じように、その時点までには、Unified Api をサポートするバージョンに、プロジェクトに含めるすべての NuGet パッケージを切り替えるし、その後、クリーン ビルドを実行する必要があります。

> [!IMPORTANT]
> **注:**形式でエラーがあれば_"エラー 3 は、同じ Xamarin.iOS プロジェクトに 'monotouch.dll' と 'Xamarin.iOS.dll' の両方を含めることはできません - 'Xamarin.iOS.dll' は 'monotouch.dll' によって参照されますが、明示的に参照される ' xxx、バージョン = 0.0.000、Culture = neutral, PublicKeyToken = null'"_ Unified Api にアプリを変換した後は、通常、統合 API が更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージを持つためです。 削除する既存のコンポーネント/NuGet Unified Api をサポートするバージョンに更新して、クリーン ビルドを行う必要があります。

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS アプリのビルド時に 64 ビットの有効化

Unified API に変換された、Xamarin.iOS モバイル アプリケーションを開発者もする必要がありますを使用するアプリのオプションから 64 ビット コンピューターでアプリケーションの構築します。 参照してください、**を有効にする 64 ビット ビルドの Xamarin.iOS アプリ**の[32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64/index.md#enable-64)64 ビットを有効にする方法の詳細な手順についてはドキュメントを作成します。

## <a name="finishing-up"></a>作業の完了

メソッドを使用して、自動または手動 Xamarin.iOS アプリケーションをクラシックから Unified Api に変換することを選択するかどうかをさらの手動による介入が必要とするいくつかのインスタンスがあります。 参照してください、 [Unified API にコードを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)ドキュメントの既知の問題と回避策です。

## <a name="related-links"></a>関連リンク

- [コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [従来の vs Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Unified API (ビデオ) への移行](http://university.xamarin.com/lightninglectures/migrating-to-the-unified-api)
