---
title: より高度な Xamarin Android サポート v4/v13 NuGet パッケージ
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 64ceacc37a303e69b0dd7073ec6547ed344af51d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523430"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>より高度な Xamarin Android サポート v4/v13 NuGet パッケージ

## <a name="about-the-android-support-libraries"></a>Android サポートライブラリについて

Google は、旧バージョンの Android で新機能を利用できるようにするためのサポートライブラリを作成しました。 一般に、サポートライブラリの名前にはバージョン番号が付けられます。これは、互換性のある最も低い Android API レベルです (例:サポート-v4 は API レベル4以上でのみ使用できます。 詳細については、この[Stack Overflow の説明](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)) を参照してください。 

2つのサポートライブラリ: `Support-v4`と`Support-v13`は、同じアプリで一緒に使用することはできません。つまり、相互に排他的です。 これは、 `Support-v13`実際には、の`Support-v4`すべての型と実装が含まれているためです。 同じプロジェクト内で両方を参照しようとすると、重複する型エラーが発生します。

## <a name="problems-with-referencing"></a>参照に関する問題

が`Support-v4`広く普及しているため、サードパーティ製のライブラリが多く、それに依存するようになりました。 代わりに、v13 に依存するように選択することもできますが、このようなサードパーティのライブラリを使用するすべてのアプリが、すべての API レベルを4までサポートするオプションを提供するため、 _v4_に依存するのが一般的です。

Xamarin サードパーティライブラリがへの`Xamarin.Android.Support.v4.dll`バインドを参照する場合、このライブラリを使用するすべてのアプリもを`Support-v4`参照`Xamarin.Android.Support.v4.dll`する必要があります。 これは、同じアプリがの`Xamarin.Android.Support.v13.dll` `Support-v13`バインディングからの一部の機能を使用する必要がある場合に問題になります。 両方のバインドを参照すると、重複する型エラーが発生します。

## <a name="type-forwarded-v4-binding-assembly"></a>タイプ-転送済み v4 バインドアセンブリ

この問題を回避するために、実装のない`Xamarin.Android.Support.v4.dll`特殊なアセンブリを作成しました`[assembly: TypeForwardedTo (..)]`が、単純に、 `Support-v4`すべての型を`Xamarin.Android.Support.v13.dll`アセンブリ内の実装に転送する属性を作成しました。

つまり、開発者はアプリ内でこの_タイプ転送_されたアセンブリを参照できます。 `Xamarin.Android.Support.v4.dll`このアセンブリは、サードパーティのライブラリに`Xamarin.Android.Support.v13.dll`よるへの参照を満たしていても、アプリで使用することができます。

## <a name="nuget-assistance"></a>NuGet のサポート

開発者は、必要な正しい参照を手動で追加できますが、nuget パッケージのインストール時に、NuGet を使用して適切なアセンブリ (通常の_v4_バインドまたはタイプ転送済み_v4_アセンブリ) を選択することができます。

`Xamarin.Android.Support.v4` NuGet パッケージには、次のロジックが含まれるようになりました。

アプリが API レベル 13 (ジンジャー 3.2) 以上を対象としている場合:

* `Xamarin.Android.Support.v13`NuGet は自動的に依存関係として追加されます
* 転送`Xamarin.Android.Support.v4.dll`される型はプロジェクトで参照されます

アプリが API レベル13よりも低いものを対象としている場合は`Xamarin.Android.Support.v4.dll` 、プロジェクトで通常のバインドが参照されます。

## <a name="do-i-have-to-use-support-v13"></a>V13 を使用する必要がありますか?

アプリが API レベル13以上を対象としていて、 `Xamarin Android Support-v4` nuget パッケージ`Xamarin Android Support v13`を使用することを選択した場合は、nuget パッケージが必要な依存関係になります。

アプリのサイズは非常にわずかに増加しています (2 つの .jar ファイルは 17 kb で異なります)。互換性が十分であり、それによって生じる問題が少なくなります。

API レベル13以上を対象`Support-v4`とするアプリでを使用する方法については、をいつでも`.nupkg`手動でダウンロードして抽出し、アセンブリを参照することができます。
