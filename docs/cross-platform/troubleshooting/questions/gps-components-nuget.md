---
title: Google Play 開発者サービス コンポーネントと NuGet の統合
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 8a7fd77a3f6460f0edbd76f8a4ccf45b32b3ed87
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284989"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Google Play 開発者サービス コンポーネントと NuGet の統合

## <a name="history"></a>履歴

いくつかの Google Play 開発者サービスコンポーネントと NuGet パッケージが使用されています。

- Google Play 開発者サービス (Froyo)
- Google Play 開発者サービス (ジンジャー)
- Google Play 開発者サービス (ICS)
- Google Play 開発者サービス (JellyBean)
- Google Play 開発者サービス (KitKat)

Google は、実際には、Google Play 開発者サービスに対して2つの .jar ファイルのみを出荷します。

- `google-play-services-froyo.jar`
- `google-play-services.jar`

ツールでは、特定のアプリに対し`aapt.exe`て使用されていたリソース API の最大レベルを正しく認識できなかったため、このような違いはありませんでした。 これは、ジンジャーのような低い API レベルで Google Play 開発者サービス (KitKat) バインドを使用しようとした場合に、コンパイルエラーを受け取りました。

## <a name="unifying-google-play-services"></a>Google Play 開発者サービスの統合

新しいバージョンの Xamarin. Android では、使用するリソース`aapt.exe`の最大バージョンを指定するようになりました。そのため、この問題は解決します。

つまり、ジンジャー/ICS/JellyBean/KitKat 用に個別のパッケージを作成するのは実際の理由ではありません (ただし、異なる .jar ファイルであるため、Froyo の別のバインドが必要です)。

開発者の作業を容易にするために、ここでは、コンポーネントと NuGet パッケージを2つに統合しました。

- Google Play 開発者サービス (Froyo) (バインド`google-play-services-froyo.jar`)
- Google Play 開発者サービス (バインド`google-play-services.jar`)

### <a name="which-one-should-be-used"></a>どれを使用すればよいでしょうか。

ほとんどの場合、Google Play 開発者サービスを使用する必要があります。 (Froyo) パッケージを使用する唯一の理由は、Froyo を積極的に対象としている場合です。 この jar ファイルが Google に存在する唯一の理由は、ユーザー自身がデバイスのごく一部を介しているためです。この .jar ファイルは、Google Play 開発者サービスの固定された、サポートされていないスナップショットです。

### <a name="note-about-gingerbread"></a>ジンジャーに関する注意事項

ジンジャーでは既定でフラグメントがサポートされていません。そのため、このバインディングの一部のクラスは、ジンジャーデバイスで実行時にアプリで使用できなくなります。 のよう`MapFragment`なクラスは、ジンジャーでは機能しません。代わりに`SupportMapFragment`、サポートバリアントを使用する必要があります。 どちらを使用するかは、開発者が知る必要があります。 この非互換性については、Google によって Google Play 開発者サービスのドキュメントに記載されています。

### <a name="what-happens-to-the-old-componentsnugets"></a>古いコンポーネント/NuGet はどうなりますか。

これらは不要になったため、次のコンポーネント/Nuget を無効/Delisted しています。

- Google Play 開発者サービス (ジンジャー)
- Google Play 開発者サービス (JellyBean)
- Google Play 開発者サービス (KitKat)

既存の_Google Play 開発者サービス (ICS)_ コンポーネント/Nuget の名前が_Google Play 開発者サービス_に変更され、今後最新の状態に保たれるようになりました。 無効/Delisted パッケージを参照しているすべてのプロジェクトを、このパッケージを使用するように更新する必要があります。

無効になっているコンポーネントはまだ存在しており、それらがまだ参照されているプロジェクトについては、中断を避けるために復元可能である必要があります。 同様に、delisted NuGet パッケージも存在し、復元できます。 今後更新されることはありません。
