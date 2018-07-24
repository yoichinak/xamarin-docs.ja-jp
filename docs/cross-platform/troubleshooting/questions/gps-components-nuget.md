---
title: Google Play の統合サービスのコンポーネントと NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.openlocfilehash: cfd417f4fc01b07b4334259c45472eb24b73abd8
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919873"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Google Play の統合サービスのコンポーネントと NuGet

### <a name="history"></a>履歴

Google プレイ サービス コンポーネントと NuGet パッケージのいくつかにするために使用があります。

-   Google Play サービス (Froyo)
-   Google Play サービス (ジンジャーブレッド)
-   Google Play サービス (ICS)
-   Google Play サービス (JellyBean)
-   Google Play サービス (KitKat)

Google Play サービス用の Google 実際にのみ付属 2 .jar ファイルします。

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

このツールは紹介しませんでした正しくためでは、不一致が存在していた`aapt.exe`が特定のアプリをするための最大リソース API レベルです。 これは、ため、ジンジャーブレッドと同様より低い API レベルで Google Play サービス (KitKat) バインドを使用しようとした場合は、コンパイル エラーを受け取りました。

### <a name="unifying-google-play-services"></a>Google Play サービスの統合

Xamarin.Android のより新しいバージョンは、今すぐ通知`aapt.exe`うえでこの問題がなくなるため、使用するリソースの最大バージョン。

つまりはありませんジンジャーブレッド/ICS/JellyBean/KitKat (ただし行う必要があります、個別のバインディング Froyo のまったく異なる .jar ファイルであるため) 用に別々 のパッケージを本当の理由。

わかりやすくするための開発者、おした統合され、マイクロソフトのコンポーネントと NuGet パッケージを 2 つに。

-   Google Play サービス (Froyo) (バインド`google-play-services-froyo.jar`)
-   Google Play サービス (バインド`google-play-services.jar`)

### <a name="which-one-should-be-used"></a>どれを使用する必要がありますか。

ほぼすべてのケースでは、Google Play サービスを使用してください。 (Froyo) パッケージを使用する唯一の理由は、Froyo は積極的に対象とするかどうかです。 Google からこの個別の .jar ファイルが存在する唯一の理由が Froyo がデバイスのほんの一部であるので、自体にこの .jar ファイルが Google Play サービスの固定された、サポートされていないスナップショットであるため、サポートを停止しました。

### <a name="note-about-gingerbread"></a>ジンジャーブレッドに関する注意事項

ジンジャーブレッドには、フラグメントが既定では、サポートはありません。 とこのため、バインディング内のクラスの一部はされませんジンジャーブレッド デバイス上で実行時、アプリで使用できるようにします。 クラスに`MapFragment`ジンジャーブレッドでが動作しなくなり、そのサポート バリアントを代わりに使用する必要があります`SupportMapFragment`です。 開発者は次のトピックを使用することです。 この非互換性は、Google からの Google Play サービス ドキュメントに記録されます。

### <a name="what-happens-to-the-old-componentsnugets"></a>古いコンポーネント/NuGet への対処方法

必要なくなりましたのでおある Delisted/無効になっている次のコンポーネント/NuGets:

-   Google Play サービス (ジンジャーブレッド)
-   Google Play サービス (JellyBean)
-   Google Play サービス (KitKat)

既存の_Google プレイ サービス (ICS)_ コンポーネント/Nuget の名前は_Google Play サービス_最新今後で保持されるとします。 この 1 つを使用するには、無効化/Delisted パッケージの 1 つを参照するすべてのプロジェクトを更新する必要があります。

無効になっているコンポーネントがまだ存在し、それらを破損しないようにするでは、参照いるプロジェクトの復元可能な必要があります。 同様に delisted NuGet パッケージは引き続き存在し、復元することができます。 これらは更新されません前方に移動します。
