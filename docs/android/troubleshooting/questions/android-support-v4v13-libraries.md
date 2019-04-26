---
title: より高度な Xamarin Android サポート v4/v13 NuGet パッケージ
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: a990d933c258812b2b3d3374fb6435af06f729ea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227053"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>より高度な Xamarin Android サポート v4/v13 NuGet パッケージ

## <a name="about-the-android-support-libraries"></a>Android サポート ライブラリについて

Google では、新しい機能を以前のバージョンの Android を使用できるようにサポート ライブラリを作成しました。 一般に、サポート ライブラリは、その名前と互換性がある最小の Android API レベルでのバージョン番号を与えられます (例。サポート v4 は、API レベル 4 以降にのみ使用できます。 詳細についてはこの[スタック オーバーフローのディスカッション](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13))。 

2 つのサポート ライブラリ:`Support-v4`と`Support-v13`は使用できませんまとめて、同じアプリでは、これらは相互に排他的です。 これは、ため`Support-v13`実際にすべての型およびの実装が含まれています`Support-v4`します。 しようとして、同じプロジェクト内の両方を参照する場合は、重複する型のエラーが発生します。

## <a name="problems-with-referencing"></a>参照に関する問題

`Support-v4`サード パーティ製ライブラリの多くが今すぐに依存して、これほどまで普及になりました。 選択したサポート v13 を代わりに、依存する可能性がありますが、むしろに依存する一般的な_v4_これらのサード パーティ製ライブラリを使用してすべてのアプリの API レベル 4 までをサポートしているオプションを提供するためです。

Xamarin のサード パーティ製のライブラリを参照する場合、`Xamarin.Android.Support.v4.dll`へのバインド`Support-v4`、このライブラリを使用するすべてのアプリを参照する必要がありますも`Xamarin.Android.Support.v4.dll`します。 また、の機能の一部を使用する必要も、同じアプリときに問題が、`Xamarin.Android.Support.v13.dll`へのバインド`Support-v13`します。 どちらのバインドを参照する場合は、重複する型のエラーが発生します。

## <a name="type-forwarded-v4-binding-assembly"></a>型転送 v4 バインド アセンブリ

この問題を回避するには、特別なを作成しました`Xamarin.Android.Support.v4.dll`の実装をするだけではないアセンブリ`[assembly: TypeForwardedTo (..)]`属性はすべての転送、`Support-v4`内で実装する型、`Xamarin.Android.Support.v13.dll`アセンブリ。

つまり、開発者はこれを参照できる_型転送_アセンブリへの参照を要求に対応するアプリで`Xamarin.Android.Support.v4.dll`、サード パーティ製のライブラリ、状態のままで`Xamarin.Android.Support.v13.dll`アプリで使用します。

## <a name="nuget-assistance"></a>NuGet のサポート

NuGet を使用して、適切なアセンブリを選択することが、開発者は、ために必要な適切な参照を手動で追加でした、中に (通常のいずれか_v4_バインディングまたは型転送_v4_アセンブリ) とNuGet パッケージをインストールします。

そのため、 `Xamarin.Android.Support.v4` NuGet パッケージにはここで、次のロジックが含まれています。

アプリが API レベル 13 (Gingerbread 3.2) を対象とするかどうかまたはそれ以降。

*   `Xamarin.Android.Support.v13` NuGet は依存関係として自動的に追加されます。
*   _型転送_`Xamarin.Android.Support.v4.dll`プロジェクトで参照されます

API レベル 13 よりも低いものが対象とするアプリの場合、通常になります`Xamarin.Android.Support.v4.dll`プロジェクトで参照されているバインディング。

## <a name="do-i-have-to-use-support-v13"></a>サポート v13 を使用する必要はでしょうか。

アプリが API レベル 13 以上を対象として、使用する場合、 `Xamarin Android Support-v4` NuGet パッケージを`Xamarin Android Support v13`NuGet パッケージが必要な依存関係。

感じる (17 kb が異なる 2 つの .jar ファイル) アプリのサイズの非常に小さな増加価値との互換性と少ない頭痛の種になります。

使用に関する adamant 場合`Support-v4`API レベル 13 を対象とするアプリで、または以降では、常に手動でダウンロードできます、 `.nupkg`、抽出して、およびアセンブリを参照します。
