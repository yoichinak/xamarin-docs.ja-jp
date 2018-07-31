---
title: Google Play の統合サービス コンポーネントと NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 3f5c5f75ae1c7a44537afa59ff4a15d54b1df50b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351484"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Google Play の統合サービス コンポーネントと NuGet

## <a name="history"></a>履歴

いくつかの Google Play Services のコンポーネントと NuGet パッケージを使用してがあります。

-   Google play 開発者サービス (Froyo)
-   Google play 開発者サービス (Gingerbread)
-   Google play 開発者サービス (ICS)
-   Google play 開発者サービス (JellyBean)
-   Google play 開発者サービス (KitKat)

Google play 開発者サービス用の Google のみを実際には、2 つ .jar ファイルします。

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

このツールは紹介しませんでした正しくため不一致が存在して`aapt.exe`API レベルのリソースを最大限が、特定のアプリに使用します。 つまり、Gingerbread のような下位 API レベルの Google play 開発者サービス (KitKat) バインドを使用しようとすると、コンパイル エラーを受信しました。

## <a name="unifying-google-play-services"></a>Google Play Services の統合

最近のバージョンの Xamarin.Android では、ここで言って`aapt.exe`のため、この問題が私たちを使用するには、どのようなリソースの最大バージョン。

これは、ことはありません Gingerbread/ICS/JellyBean/KitKat (ただし行う必要があります、個別のバインド (Froyo 用) まったく異なる .jar ファイルであるため) 用に個別のパッケージを本当の理由。

わかりやすくするための開発者、おした統合され、コンポーネントと NuGet パッケージを 2 つに。

-   Google play 開発者サービス (Froyo) (バインド`google-play-services-froyo.jar`)
-   Google play 開発者サービス (バインド`google-play-services.jar`)

### <a name="which-one-should-be-used"></a>どれを使用する必要がありますか。

ほとんどの場合、Google play 開発者サービスを使用する必要があります。 (Froyo) パッケージを使用する唯一の理由は、Froyo は積極的に対象とするかどうかです。 Google からこの個別の .jar ファイルが存在する唯一の理由が Froyo がデバイスのごく一部であるので、自分にこの .jar ファイルが Google play 開発者サービスの固定された、サポートされていないスナップショットであるため、サポートを停止しました。

### <a name="note-about-gingerbread"></a>Gingerbread に関する注意事項

Gingerbread には、フラグメントが既定では、サポートはありません。 このため、一部のバインディングのクラスは役に立ちません Gingerbread デバイス上で実行時、アプリで. クラスのような`MapFragment`Gingerbread では動作しないと、サポートのバリアントを代わりに使用する必要があります`SupportMapFragment`します。 開発者が使用するバージョンの確認です。 この非互換性は、Google によって、Google Play Services のドキュメントに記録されます。

### <a name="what-happens-to-the-old-componentsnugets"></a>古いコンポーネント/NuGet の動作

必要がなくなったらためにある無効/Delisted 次のコンポーネント/NuGets:

-   Google play 開発者サービス (Gingerbread)
-   Google play 開発者サービス (JellyBean)
-   Google play 開発者サービス (KitKat)

既存の_Google Play サービス (ICS)_ /Nuget コンポーネントの名前は_Google play 開発者サービス_と最新将来的に保持されます。 この 1 つを使用する無効/Delisted パッケージの 1 つを参照するすべてのプロジェクトを更新する必要があります。

無効になっているコンポーネントがまだ存在し、それらを回避するために、参照がプロジェクトの復元を可能にする必要があります。 同様に、delisted NuGet パッケージは引き続き存在し、復元することができます。 これらは更新されません今後します。
