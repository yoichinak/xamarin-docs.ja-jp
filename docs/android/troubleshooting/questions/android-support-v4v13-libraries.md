---
title: "スマートな Xamarin Android v4 のサポート/v13 NuGet パッケージの管理"
ms.topic: article
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: ab5ec19498b3c61e988adc959e1751b96f60210d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>スマートな Xamarin Android v4 のサポート/v13 NuGet パッケージの管理

## <a name="about-the-android-support-libraries"></a>Android のサポート ライブラリについて

Google は、新しい機能を以前のバージョンの Android を使用できるようにサポート ライブラリを作成しました。 一般に、サポート ライブラリは、バージョン番号で指定と互換性が最下位の Android API レベルである、名前 (例: v4 のサポートは、API レベル 4 以降にのみ使用できます。 詳細についてはこの[スタック オーバーフローのディスカッション](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13))。 

2 つのサポート ライブラリ:`Support-v4`と`Support-v13`は使用できません同時に、同じアプリでは、これらは相互に排他的です。 これは、ため`Support-v13`実際にすべての型との実装を含む`Support-v4`です。 しようとすると、同じプロジェクトの両方を参照は、重複する型のエラーが発生します。

## <a name="problems-with-referencing"></a>参照の問題

`Support-v4`普及し、サード パーティ製ライブラリの多くは今すぐに依存してになりました。 でしたが、選択された代わりに、サポート v13 に依存するが、詳細に依存する一般的な_v4_これらサード パーティ製ライブラリを使用してすべてのアプリの API レベル 4 までをサポートするオプションを提供するためです。

Xamarin のサード パーティ製のライブラリを参照する場合、`Xamarin.Android.Support.v4.dll`へのバインド`Support-v4`、このライブラリを使用するすべてのアプリを参照する必要がありますも`Xamarin.Android.Support.v4.dll`します。 これが、同じアプリが必要とされるいくつかの機能の使用時に問題になります、`Xamarin.Android.Support.v13.dll`へのバインド`Support-v13`です。 どちらのバインディングを参照する場合は、重複する型のエラーが発生します。

## <a name="type-forwarded-v4-binding-assembly"></a>型転送 v4 バインド アセンブリ

この問題を回避する特殊なを作成しました`Xamarin.Android.Support.v4.dll`実装をするだけではないアセンブリ`[assembly: TypeForwardedTo (..)]`属性はすべての転送、`Support-v4`内で実装する型、`Xamarin.Android.Support.v13.dll`アセンブリ。

つまり、開発者はこれを参照できる_型転送_への参照を要求に対応する、アプリケーション内のアセンブリ`Xamarin.Android.Support.v4.dll`、サード パーティ製のライブラリ、状態のままで`Xamarin.Android.Support.v13.dll`アプリで使用します。

## <a name="nuget-assistance"></a>NuGet のサポート

NuGet を使用して適切なアセンブリを選択することが、開発者は、正しい参照必要を手動で追加でした、中に (通常のいずれか_v4_バインディングまたは型転送_v4_アセンブリ) とNuGet パッケージをインストールするとします。

そのため、 `Xamarin.Android.Support.v4` NuGet パッケージにはここで、次のロジックが含まれています。

かどうか、アプリが対象と API レベル 13 (ジンジャーブレッド 3.2 の場合) またはそれ以降。

*   `Xamarin.Android.Support.v13` NuGet は、依存関係として自動的に追加されます。
*   _型転送_`Xamarin.Android.Support.v4.dll`プロジェクトで参照されます。

API レベル 13 より低いものがターゲットとするアプリの場合、通常が表示されます`Xamarin.Android.Support.v4.dll`プロジェクトで参照されているバインディング。

## <a name="do-i-have-to-use-support-v13"></a>サポート v13 を使用する必要がありますは?

アプリが 13 またはそれ以降の API レベルを対象として、使用する場合、 `Xamarin Android Support-v4` NuGet パッケージ、 `Xamarin Android Support v13` NuGet パッケージが必要な依存関係。

互換性や少ない頭痛の原因になりますも価値があります (17 kb で 2 つの jar ファイルが異なる場合) アプリのサイズの非常に小さな引き上げだと考えています。

使用に関する adamant 場合`Support-v4`でアプリをターゲットにして API レベル 13 または以降では、常に手動でダウンロードできます、 `.nupkg`、抽出して、アセンブリを参照します。
