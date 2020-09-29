---
title: Xamarin. iOS のイメージとアイコン
description: このセクションには、Xamarin iOS アプリでのイメージの操作に関するさまざまな記事が含まれています。これには、アイコンや起動画面を使用したり、コントロールに追加したり、カスタムドキュメントの種類のアイコンを指定したりできます。
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 6048690bc68c3998b67dc89fdc191ea9158ae952
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437270"
---
# <a name="images-and-icons-in-xamarinios"></a>Xamarin. iOS のイメージとアイコン

_このセクションには、Xamarin iOS アプリでのイメージの操作に関するさまざまな記事が含まれています。これには、アイコンや起動画面を使用したり、コントロールに追加したり、カスタムドキュメントの種類のアイコンを指定したりできます。_

IOS アプリ内でイメージアセットを使用するには、いくつかの方法があります。 単純に、アプリの UI の一部としてイメージを表示し、またはなどの UI コントロールに割り当てて、 `UIButton` `UIImageView` アイコンと起動画面を提供するから、次の方法で ios アプリに優れたアートワークを簡単に追加できるようになります。 

- **解像度に依存しない画像** – iOS の組み込みサポートを使用して、さまざまなデバイスの解像度と種類 (IPhone、iPad など) でのイメージの操作をサポートします。
- **アセットカタログのイメージセット** - **アセットカタログのイメージセット** を使用して、アプリで必要な特定のイメージ資産のすべてのバージョンを管理およびグループ化します。
- **Ios designer のイメージ** -ios designer を使用して、コントロールのイメージを設定します。
- **コード内のイメージ** –クラスのメソッドを使用して、 `UIImage` イメージアセットを読み込んで操作し、C# コードで UI コントロールに割り当てます。
- **アプリケーションアイコン** -すべての iOS アプリで必要なアプリアイコンを定義します。 これは、アプリを起動するためにユーザーが iOS ホーム画面からタップするアイコンです。 また、このアイコンは Game Center によって使用されます (該当する場合)。
- **スポットライトアイコン** -アプリのスポットライトアイコンを定義します。 ユーザーがスポットライト検索でアプリの名前を入力するたびに、このアイコンが表示されます。
- **設定アイコン** -アプリの **設定** アイコンを定義します。 ユーザーが iOS デバイスで **設定** アプリを入力すると、アプリの設定リストの最後にこのアイコンが表示されます。 
- **起動画面** -アプリの起動画面を定義します。 ユーザーがアプリアイコンをタップし、最初のビューが表示される前に、空の画面が表示されます。 幸い、iOS には、ストーリーボードを使用して、空の画面の代わりにイメージを表示するためのサポートが含まれています。 
- **ITunes アイコン** -iTune アイコンを提供します。 アプリを配信するアドホック方式 (企業ユーザー向けまたは実際のデバイスでのベータテスト用) を使用する場合、開発者は、iTunes でアプリを表すために使用される512x512 と1024x1024 イメージも含める必要があります。
- **ドキュメントアイコン** -Xamarin iOS アプリがサポートまたは作成する特定のドキュメントの種類のアイコンとしてイメージを使用します。

IOS アプリのイメージ資産を作成する際には、いくつかの考慮事項を考慮する必要があります。また、これらの資産が使用される場所もいくつかあります。 これらはいずれも、必要なイメージアセットの数だけでなく、アセットの作成方法にも影響を与えます。 次のトピックでは、必要なイメージ資産の種類、それらの資産をアプリケーションのバンドルに含める方法、およびイメージ資産を使用して必要な機能を提供する方法について説明します。

## <a name="displaying-an-image"></a>[イメージの表示](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

この記事では、Xamarin の iOS アプリにイメージ資産を含め、C# コードを使用するか、iOS デザイナーのコントロールに割り当ててイメージを表示する方法について説明します。

## <a name="application-icons"></a>[アプリケーション アイコン](~/ios/app-fundamentals/images-icons/app-icons.md)

この記事では、アプリアイコンとして使用する Xamarin iOS アプリでのイメージアセットの追加と管理について説明します。

## <a name="alternate-app-icons"></a>[代替アプリ アイコン](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple では、アプリによるアイコンの管理を可能にする iOS 10.3 の機能強化がいくつか追加されています。

- `ApplicationIconBadgeNumber` -スプリングボードのアプリアイコンのバッジを取得または設定します。
- `SupportsAlternateIcons` - `true` アプリに別のアイコンのセットがある場合。
- `AlternateIconName` -現在選択されている代替アイコンの名前を返し `null` ます。プライマリアイコンを使用する場合はを返します。
- `SetAlternameIconName` -アプリのアイコンを指定した代替アイコンに切り替えるには、このメソッドを使用します。

## <a name="launch-screens"></a>[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)

この記事では、さまざまな種類のストーリーボードを使用して、iOS デバイスのサイズと解像度ごとにユニバーサル起動画面を提供する方法について説明します。

## <a name="custom-document-types"></a>[カスタム ドキュメント タイプ](~/ios/app-fundamentals/images-icons/custom-document-types.md)

この記事では、カスタムドキュメントの種類のアイコンとして使用する Xamarin. iOS アプリでのイメージアセットの追加と管理について説明します。

## <a name="related-links"></a>関連リンク

- [イメージの操作 (サンプル)](/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタムアイコンとイメージ作成のガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)