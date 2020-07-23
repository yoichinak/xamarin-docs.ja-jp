---
title: 既存の iOS アプリを更新しています
description: このドキュメントでは、Classic API から Unified API に Xamarin iOS アプリを更新するために従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 1cfc2a955ffbe3f1416c291b7d644392d51e0b8f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930482"
---
# <a name="updating-existing-ios-apps"></a>既存の iOS アプリを更新しています

_Unified API を使用するように既存の Xamarin iOS アプリを更新するには、次の手順に従います。_

Unified API を使用するように既存のアプリを更新するには、アプリケーションコードで使用される名前空間と Api に加えて、プロジェクトファイル自体を変更する必要があります。

## <a name="the-road-to-64-bits"></a>64ビットへの道路

Xamarin iOS モバイルアプリケーションから64ビットのデバイスアーキテクチャをサポートするには、統合された新しい Api が必要です。 2015年2月1日の時点で、iTunes App Store への新しいアプリの送信はすべて64ビットアーキテクチャをサポートしている必要があります。

Xamarin には、Visual Studio for Mac と Visual Studio の両方のツールが用意されており、Classic API から Unified API への移行プロセスを自動化できます。また、プロジェクトファイルを手動で変換することもできます。 自動ツールを使用することを強くお勧めしますが、この記事では両方の方法について説明します。

### <a name="before-you-start"></a>開始する前に...

既存のコードを Unified API に更新する前に、すべての*コンパイル警告*を除去することを強くお勧めします。 統合に移行すると、Classic API の多くの*警告*がエラーになります。 Classic API のコンパイラメッセージは、多くの場合、更新対象のヒントを提供するため、作業を開始する前に修正する方が簡単です。

## <a name="automated-updating"></a>自動更新

警告が修正されたら、Visual Studio for Mac または Visual Studio で既存の iOS プロジェクトを選択し、[**プロジェクト**] メニューから [ **Xamarin. IOS Unified API に移行**] を選択します。 次に例を示します。

![[プロジェクト] メニューから [Xamarin. iOS Unified API に移行する] を選択します。](updating-ios-apps-images/beta-tool1.png)

自動移行を実行する前に、この警告に同意する必要があります (当然、この adventure 着手の前にバックアップとソース管理があることを確認する必要があります)。

![自動移行を実行する前に、この警告に同意します](updating-ios-apps-images/beta-tool2.png)

このツールでは、次に示す「**手動で更新**する」セクションで説明されているすべての手順が基本的に自動化されており、既存の Xamarin iOS プロジェクトを Unified API に変換するための推奨される方法です。

## <a name="steps-to-update-manually"></a>手動で更新するための手順

警告が修正されたら、次の手順に従って、新しい Unified API を使用するように Xamarin iOS アプリを手動で更新します。

### <a name="1-update-project-type--build-target"></a>1. ビルドターゲット & プロジェクトの種類を更新します

**.Csproj**ファイルのプロジェクトフレーバーをからに変更 `6BC8ED88-2882-458C-8E55-DFD12B67127B` し `FEACFBD2-3405-455C-9665-78FE426C6842` ます。 テキストエディターで **.csproj**ファイルを編集し、 `<ProjectTypeGuids>` 次に示すように、要素の最初の項目を置き換えます。

![テキストエディターで .csproj ファイルを編集し、表示されているように ProjectTypeGuids 要素の最初の項目を置き換えます。](updating-ios-apps-images/csproj.png)

**Import** `Xamarin.MonoTouch.CSharp.targets` 次に示すように、を含む Import 要素をに変更 `Xamarin.iOS.CSharp.targets` します。

![Monotouch.dialog が含まれている Import 要素を、図に示すように変更します。](updating-ios-apps-images/csproj2.png)

### <a name="2-update-project-references"></a>2. プロジェクト参照を更新する

IOS アプリケーションプロジェクトの [**参照設定**] ノードを展開します。 最初に、このスクリーンショットのような * **monotouch.dialog**の参照が表示されます (プロジェクトの種類を変更しただけなので)。

![プロジェクトの種類が変更されたため、このスクリーンショットのような monotouch.dialog 参照が最初に表示されます。](updating-ios-apps-images/references.png)

IOS アプリケーションプロジェクトを右クリックして**参照を編集**し、[ **monotouch.dialog** ] 参照をクリックして、赤い [X] ボタンを使用して削除します。

![IOS アプリケーションプロジェクトを右クリックして参照を編集し、[monotouch.dialog] 参照をクリックして、赤い X ボタンを使用して削除します。](updating-ios-apps-images/references-delete-monotouch-sml.png)

次に、[参照] 一覧の末尾までスクロールし、 **Xamarin. iOS**アセンブリをティックします。

![次に、[参照] の一覧の最後までスクロールし、Xamarin. iOS アセンブリをティックします。](updating-ios-apps-images/references-add-xamarinios-sml.png)

**[OK]** をクリックして、プロジェクト参照の変更を保存します。

### <a name="3-remove-monotouch-from-namespaces"></a>3. 名前空間から Monotouch.dialog を削除する

ステートメント内**MonoTouch**の名前空間から、 `using` または classname が完全に修飾されている場所 (例を含む) から monotouch.dialog プレフィックスを削除します。 `MonoTouch.UIKit`がだけになり `UIKit` ます)。

### <a name="4-remap-types"></a>4. リマップ型

以前に使用されていたいくつかの型 (たとえば、のインスタンス) を置き換える[ネイティブ型](~/cross-platform/macios/nativetypes.md)が導入されました `System.Drawing.RectangleF` `CoreGraphics.CGRect` 。 型の完全な一覧については、「[ネイティブ型](~/cross-platform/macios/nativetypes.md)」ページを参照してください。

### <a name="5-fix-method-overrides"></a>5. メソッドのオーバーライドを修正する

一部 `UIKit` のメソッドでは、新しい[ネイティブ型](~/cross-platform/macios/nativetypes.md)(など) を使用するようにシグネチャが変更されてい `nint` ます。 カスタムサブクラスでこれらのメソッドをオーバーライドすると、署名が一致しなくなり、エラーが発生します。 ネイティブ型を使用して新しいシグネチャに一致するようにサブクラスを変更することで、これらのメソッドのオーバーライドを修正します。

例と `public override int NumberOfSections (UITableView tableView)` して、戻り値の変更 `nint` と、の戻り値の型とパラメーターの型の両方をに変更する方法があり `public override int RowsInSection (UITableView tableView, int section)` `nint` ます。

## <a name="considerations"></a>考慮事項

アプリが1つ以上のコンポーネントまたは NuGet パッケージに依存している場合は、既存の Xamarin の iOS プロジェクトを Classic API から新しい Unified API に変換する際には、次の点に注意する必要があります。

### <a name="components"></a>コンポーネント

アプリケーションに含まれているコンポーネントも Unified API に更新する必要があります。または、コンパイルしようとすると競合が発生します。 含まれているコンポーネントについて、現在のバージョンを Xamarin コンポーネントストアの新しいバージョンに置き換えて、Unified API をサポートし、クリーンビルドを実行します。 作成者によってまだ変換されていないコンポーネントは、コンポーネントストアに32ビットのみの警告を表示します。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API サポートを利用するために NuGet に変更が加えられましたが、NuGet の新しいリリースはありませんでした。そのため、NuGet を入手して新しい Api を認識する方法を評価しています。

この時間が経過するまでは、コンポーネントと同じように、プロジェクトに含まれているすべての NuGet パッケージを、統合された Api をサポートするバージョンに切り替え、後でクリーンビルドを実行する必要があります。

> [!IMPORTANT]
> "エラー 3" という形式のエラーが発生した場合は _、同じ Xamarin に "monotouch.dll" と "Xamarin.iOS.dll" の両方を含めることはできません。 iOS プロジェクト-' Xamarin.iOS.dll ' は明示的に参照されていますが、' monotouch.dll ' は ' xxx, Version = 0.0.000, Culture = 中立的, PublicKeyToken = null ' によって参照さ_れています。アプリケーションを統合 api に変換した後は、通常、Unified API に更新されていないコンポーネントまたは NuGet パッケージがプロジェクトにあることが原因 既存のコンポーネントまたは NuGet を削除し、統合された Api をサポートし、クリーンビルドを実行するバージョンに更新する必要があります。

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin iOS アプリの64ビットビルドを有効にする

Unified API に変換された Xamarin iOS モバイルアプリケーションでは、開発者は引き続き、アプリのオプションから64ビットコンピューター用のアプリケーションのビルドを有効にする必要があります。 64ビットビルドを有効にするための詳細な手順については、 [32/64 ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md#enable-64)の**64 ビットビルドの有効化**に関するドキュメントを参照してください。

## <a name="finishing-up"></a>終了しています

自動または手動のいずれかの方法を使用して、Xamarin アプリケーションをクラシック Api から統合 Api に変換することを選択したかどうかにかかわらず、手動での介入が必要になるインスタンスがいくつかあります。 既知の問題と回避策については[、Unified API ドキュメントにコードを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
