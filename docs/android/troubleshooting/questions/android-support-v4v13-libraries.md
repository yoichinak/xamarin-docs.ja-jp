---
title: より高度な Xamarin Android サポート v4/v13 NuGet パッケージ
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: c74cac6a6d669385855999a565711a3fdc85f3b7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019539"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>より高度な Xamarin Android サポート v4/v13 NuGet パッケージ

## <a name="about-the-android-support-libraries"></a>Android サポートライブラリについて

Google は、旧バージョンの Android で新機能を利用できるようにするためのサポートライブラリを作成しました。 一般に、サポートライブラリの名前にはバージョン番号が付けられます。これは、互換性のある最も低い Android API レベルです (例: Support-v4 は API レベル4以上でのみ使用できます)。 詳細については、この[Stack Overflow の説明](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)) を参照してください。 

2つのサポートライブラリ (`Support-v4` と `Support-v13` を同じアプリで同時に使用することはできません。つまり、相互に排他的です。 これは、`Support-v13` 実際に `Support-v4`のすべての型と実装が含まれているためです。 同じプロジェクト内で両方を参照しようとすると、重複する型エラーが発生します。

## <a name="problems-with-referencing"></a>参照に関する問題

`Support-v4` が広く普及しているため、多くのサードパーティのライブラリがそれに依存するようになりました。 代わりに、v13 に依存するように選択することもできますが、このようなサードパーティのライブラリを使用するすべてのアプリが、すべての API レベルを4までサポートするオプションを提供するため、 _v4_に依存するのが一般的です。

Xamarin サードパーティライブラリが `Support-v4`への `Xamarin.Android.Support.v4.dll` バインドを参照する場合、このライブラリを使用するすべてのアプリは `Xamarin.Android.Support.v4.dll`も参照する必要があります。 これは、同じアプリが `Xamarin.Android.Support.v13.dll` バインドの一部の機能を `Support-v13`に使用する必要がある場合に問題になります。 両方のバインドを参照すると、重複する型エラーが発生します。

## <a name="type-forwarded-v4-binding-assembly"></a>タイプ-転送済み v4 バインドアセンブリ

この問題を回避するために、実装のない特別な `Xamarin.Android.Support.v4.dll` アセンブリを作成しましたが、`Support-v4` のすべての型を `Xamarin.Android.Support.v13.dll` アセンブリ内の実装に転送する属性を単純に `[assembly: TypeForwardedTo (..)]` します。

つまり、開発者は、サードパーティのライブラリによって `Xamarin.Android.Support.v4.dll` への参照を満たすと同時に、アプリで `Xamarin.Android.Support.v13.dll` を使用できるようにする、この_タイプ転送_されたアセンブリをアプリで参照できます。

## <a name="nuget-assistance"></a>NuGet のサポート

開発者は、必要な正しい参照を手動で追加できますが、nuget パッケージのインストール時に、NuGet を使用して適切なアセンブリ (通常の_v4_バインドまたはタイプ転送済み_v4_アセンブリ) を選択することができます。

`Xamarin.Android.Support.v4` NuGet パッケージには、次のロジックが含まれています。

アプリが API レベル 13 (ジンジャー 3.2) 以上を対象としている場合:

* `Xamarin.Android.Support.v13` NuGet は依存関係として自動的に追加されます
* _型転送_された `Xamarin.Android.Support.v4.dll` はプロジェクト内で参照されます

アプリが API レベル13よりも低いものを対象としている場合は、プロジェクトで参照される通常の `Xamarin.Android.Support.v4.dll` バインドを取得します。

## <a name="do-i-have-to-use-support-v13"></a>V13 を使用する必要がありますか?

アプリが API レベル13以上を対象としており、`Xamarin Android Support-v4` NuGet パッケージを使用することを選択した場合は、`Xamarin Android Support v13` NuGet パッケージが必要な依存関係になります。

アプリのサイズは非常にわずかに増加しています (2 つの .jar ファイルは 17 kb で異なります)。互換性が十分であり、それによって生じる問題が少なくなります。

API レベル13以上を対象とするアプリで `Support-v4` を使用する方法について adamant ている場合は、いつでも手動で `.nupkg`をダウンロードして抽出し、アセンブリを参照することができます。
