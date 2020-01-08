---
title: 既存の iOS アプリの更新
description: このドキュメントでは、Classic API から Unified API に Xamarin iOS アプリを更新するために従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 9b531bd095781c80c5f3418725d57f8f6bbb06fd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015026"
---
# <a name="updating-existing-ios-apps"></a>既存の iOS アプリの更新

_Unified API を使用するように既存の Xamarin iOS アプリを更新するには、次の手順に従います。_

Unified API を使用するように既存のアプリを更新するには、アプリケーションコードで使用される名前空間と API に加えて、プロジェクトファイル自体を変更する必要があります。

## <a name="the-road-to-64-bits"></a>64 ビットへの道のり

Xamarin.iOS モバイルアプリケーションから64ビットのデバイスアーキテクチャをサポートするには、統合された新しい API が必要です。 2015年2月1日の時点で、iTunes App Store への新しいアプリの送信はすべて64ビットアーキテクチャをサポートしている必要があります。

Xamarin には、Visual Studio for Mac と Visual Studio の両方のツールが用意されており、Classic API から Unified API への移行プロセスを自動化できます。また、プロジェクトファイルを手動で変換することもできます。 自動ツールを使用することを強くお勧めしますが、この記事では両方の方法について説明します。

### <a name="before-you-start"></a>開始する前に...

既存のコードを Unified API に更新する前に、すべての*コンパイル警告*を除去することを強くお勧めします。 統合に移行すると、Classic API の多くの*警告*がエラーになります。 Classic API のコンパイラメッセージは、多くの場合、更新対象のヒントを提供するため、作業を開始する前に修正する方が簡単です。

## <a name="automated-updating"></a>自動更新

警告が修正されたら、Visual Studio for Mac または Visual Studio で既存の iOS プロジェクトを選択し、 **[プロジェクト]** メニューから **[Xamarin. IOS Unified API に移行]** を選択します。 (例:

![](updating-ios-apps-images/beta-tool1.png "Choose Migrate to Xamarin.iOS Unified API from the Project menu")

自動移行を実行する前に、この警告に同意する必要があります (当然、この大胆な作業の着手の前にバックアップとソース管理があることを確認する必要があります)。

![](updating-ios-apps-images/beta-tool2.png "Agree to this warning before the automated migration will run")

このツールでは、次に示す「**手動で更新**する」セクションで説明されているすべての手順が基本的に自動化されており、既存の Xamarin.iOS プロジェクトを Unified API に変換するための推奨される方法です。

## <a name="steps-to-update-manually"></a>手動で更新するための手順

警告が修正されたら、次の手順に従って、新しい Unified API を使用するように Xamarin.iOS アプリを手動で更新します。

### <a name="1-update-project-type--build-target"></a>1. ビルドターゲット & プロジェクトの種類を更新します

**.Csproj**ファイルのプロジェクトフレーバーを `6BC8ED88-2882-458C-8E55-DFD12B67127B` から `FEACFBD2-3405-455C-9665-78FE426C6842`に変更します。 テキストエディターで **.csproj**ファイルを編集し、次に示すように `<ProjectTypeGuids>` 要素の最初の項目を置き換えます。

![](updating-ios-apps-images/csproj.png "Edit the csproj file in a text editor, replacing the first item in the ProjectTypeGuids element as shown")

次に示すように、`Xamarin.MonoTouch.CSharp.targets` を含む**Import**要素を `Xamarin.iOS.CSharp.targets` に変更します。

![](updating-ios-apps-images/csproj2.png "Change the Import element that contains Xamarin.MonoTouch.CSharp.targets to Xamarin.iOS.CSharp.targets as shown")

### <a name="2-update-project-references"></a>2. プロジェクト参照を更新する

IOS アプリケーションプロジェクトの **[参照設定]** ノードを展開します。 最初に、このスクリーンショットのような * **monotouch.dialog**の参照が表示されます (プロジェクトの種類を変更しただけなので)。

![](updating-ios-apps-images/references.png "It will initially show a broken- monotouch reference similar to this screenshot because the project type changed")

IOS アプリケーションプロジェクトを右クリックして**参照を編集**し、 **[monotouch.dialog]** 参照をクリックして、赤い [X] ボタンを使用して削除します。

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Right-click on the iOS application project to Edit References, then click on the monotouch reference and delete it using the red X button")

次に、[参照] 一覧の末尾までスクロールし、 **Xamarin. iOS**アセンブリをティックします。

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Now scroll to the end of the references list and tick the Xamarin.iOS assembly")

**[OK]** をクリックして、プロジェクト参照の変更を保存します。

### <a name="3-remove-monotouch-from-namespaces"></a>3. 名前空間から Monotouch.dialog を削除する

`using` ステートメント内の名前空間から、または classname が完全に修飾されている場所 (例を含む) から、 **monotouch.dialog**プレフィックスを削除します。 `MonoTouch.UIKit` が `UIKit`) になります。

### <a name="4-remap-types"></a>4. リマップ型

以前に使用されていたいくつかの型を置き換える[ネイティブ型](~/cross-platform/macios/nativetypes.md)が導入されました。たとえば、`System.Drawing.RectangleF` のインスタンス (など) `CoreGraphics.CGRect` に置き換えられます。 型の完全な一覧については、「[ネイティブ型](~/cross-platform/macios/nativetypes.md)」ページを参照してください。

### <a name="5-fix-method-overrides"></a>5. メソッドのオーバーライドを修正する

一部の `UIKit` メソッドでは、新しい[ネイティブ型](~/cross-platform/macios/nativetypes.md)(`nint`など) を使用するようにシグネチャが変更されています。 カスタムサブクラスでこれらのメソッドをオーバーライドすると、署名が一致しなくなり、エラーが発生します。 ネイティブ型を使用して新しいシグネチャに一致するようにサブクラスを変更することで、これらのメソッドのオーバーライドを修正します。

たとえば、`public override int NumberOfSections (UITableView tableView)` を変更して `nint` を返し、`public override int RowsInSection (UITableView tableView, int section)` の戻り値の型とパラメーターの型の両方を `nint`に変更することができます。

## <a name="considerations"></a>注意事項

アプリが1つ以上のコンポーネントまたは NuGet パッケージに依存している場合は、既存の Xamarin の iOS プロジェクトを Classic API から新しい Unified API に変換する際には、次の点に注意する必要があります。

### <a name="components"></a>コンポーネント

アプリケーションに含まれているコンポーネントも Unified API に更新する必要があります。または、コンパイルしようとすると競合が発生します。 含まれているコンポーネントについて、現在のバージョンを Xamarin コンポーネントストアの新しいバージョンに置き換えて、Unified API をサポートし、クリーンビルドを実行します。 作成者によってまだ変換されていないコンポーネントは、コンポーネントストアに32ビットのみの警告を表示します。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API サポートを利用するために NuGet に変更が加えられましたが、NuGet の新しいリリースはありませんでした。そのため、NuGet を入手して新しい Api を認識する方法を評価しています。

この時間が経過するまでは、コンポーネントと同じように、プロジェクトに含まれているすべての NuGet パッケージを、 Unified API をサポートするバージョンに切り替え、後でクリーン ビルドを実行する必要があります。

> [!IMPORTANT]
> _"エラー3に ' monotouch.dialog ' と ' 0.0.000 ' の両方を同じ Xamarin に含めることはできません" という形式のエラーが発生した場合は、' monotouch.dialog ' が明示的に参照されていますが、' ' は ' xxx, Version =, Culture = によって参照されています。ニュートラル, PublicKeyToken = null ' "_ アプリケーションを統合 api に変換した後、通常は、Unified API に更新されていないコンポーネントまたは NuGet パッケージがプロジェクトにあることが原因です。 既存のコンポーネントまたは NuGet を削除し、統合された Api をサポートし、クリーンビルドを実行するバージョンに更新する必要があります。

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin iOS アプリの64ビットビルドを有効にする

Unified API に変換された Xamarin.iOS モバイルアプリケーションでは、開発者は引き続き、アプリのオプションから64ビットコンピューター用のアプリケーションのビルドを有効にする必要があります。 64ビットビルドを有効にするための詳細な手順については、 [32/64 ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md#enable-64)の**64 ビットビルドの有効化**に関するドキュメントを参照してください。

## <a name="finishing-up"></a>終了しています

自動または手動のいずれかの方法を使用して、Xamarin.iOS アプリケーションを Classic API から Unified API に変換することを選択したかどうかにかかわらず、手動での介入が必要になるインスタンスがいくつかあります。 既知の問題と回避策については「[コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
