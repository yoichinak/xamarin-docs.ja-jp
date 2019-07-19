---
title: Xamarin ファイアウォールの構成手順
description: このドキュメントでは、企業環境で Xamarin が動作できるように、ファイアウォールで許可する必要があるホストの一覧を示します。
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: asb3993
ms.author: amburns
ms.date: 07/17/2019
ms.openlocfilehash: b2509bb99a5caf5b508d34aafc0f0c4862a7e563
ms.sourcegitcommit: 9a2a21974d35353c3765eb683ef2fd7161c1d94a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/19/2019
ms.locfileid: "68329957"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin ファイアウォールの構成手順

_会社で Xamarin のプラットフォームが動作できるように、ファイアウォールで許可する必要があるホストの一覧。_

Xamarin 製品をインストールし、製品が正常に動作するには、ソフトウェアに必要なツールと更新プログラムをダウンロードできるように、特定のエンドポイントをアクセス可能にする必要があります。 管理者または会社がファイアウォール設定を厳格にしている場合は、インストール、ライセンス、コンポーネントなどで問題が発生する可能性があります。 このドキュメントでは、Xamarin が動作するためにファイアウォールで許可する必要がある既知のエンドポイントのいくつかについて説明します。 このホワイトリストには、ダウンロードに含まれるすべてのサードパーティ ツールに必要なエンドポイントを含めません。 このリストを使用しても問題が解決しない場合は、Apple または Android のインストールに関するトラブルシューティング ガイドを参照してください。

## <a name="endpoints-to-allow"></a>許可するエンドポイント

### <a name="xamarin-installer"></a>Xamarin インストーラー

最新リリースの Xamarin インストーラーを使用するときにソフトウェアを適切にインストールするには、次の既知のアドレスを追加する必要があります。

- xamarin.com (インストーラーのマニフェスト)
- dl.xamarin.com (パッケージのダウンロード場所)
- dl.google.com (Android SDK をダウンロードする場合)
- download.oracle.com (JDK)
- visualstudio.com (セットアップ パッケージのダウンロード場所)
- go.microsoft.com (セットアップ URL の解決)
- aka.ms (セットアップ URL の解決)

Mac を使用していて Xamarin.Android のインストールの問題が発生している場合は、macOS が Java をダウンロードできることを確認してください。

### <a name="nuget-including-xamarinforms"></a>NuGet (Xamarin.Forms を含む)

NuGet にアクセスするには、次のアドレスを追加する必要があります (Xamarin.Forms は NuGet としてパッケージ化されています)。

- www.nuget.org (NuGet にアクセスする場合)
- globalcdn.nuget.org (NuGet のダウンロード)
- dl-ssl.google.com (Android および Xamarin.Forms 用 Google コンポーネント)

### <a name="software-updates"></a>ソフトウェア更新プログラム

ソフトウェア更新プログラムを適切にダウンロードするには、次のアドレスを追加する必要があります。

- software.xamarin.com (更新機能サービス)
- download.visualstudio.microsoft.com
- dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac エージェント

Xamarin Mac エージェントを使用して Visual Studio を Mac ビルド ホストに接続するには、SSH ポートを開く必要があります。 この既定値は**ポート 22** です。
