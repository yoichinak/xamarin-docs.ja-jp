---
title: Xamarin. iOS のダークモード
description: ダークモードは、明るいテーマとダークテーマの新しいシステム全体のオプションです。 iOS ユーザーがテーマを選択したり、iOS が外観を動的に変更したりできるようになりました。
ms.prod: xamarin
ms.assetid: 4F44446E-36B6-4743-9B4D-32278D1D3D66
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/28/2019
ms.openlocfilehash: 77f852914e41b636901348a0490d2d5927b044f0
ms.sourcegitcommit: e27e29c14b783263e063baaa65d4eecb8dd31f57
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98628788"
---
# <a name="dark-mode-in-xamarinios"></a>Xamarin. iOS のダークモード

ダークモードは、明るいテーマとダークテーマのシステム全体のオプションです。 iOS ユーザーがテーマを選択できるようになりました。または、iOS が環境や時刻に基づいて外観を動的に変更できるようになります。

このドキュメントでは、iOS 13 アプリケーションでのダークモードとダークモードのサポートについて説明します。

## <a name="requirements"></a>必要条件

ダークモードでは、iOS 13 と Xcode 11、Xamarin 12.99、Visual Studio 2019 または Visual Studio 2019 for Mac (Xcode 11 サポート付き) が必要です。

## <a name="turning-on-dark-mode"></a>ダークモードを有効にする

Apple では、iOS 13 の開発者メニューを使用して、ダークモードとライトモードを切り替えることができます。 IOS 13 シミュレーターで [ **設定** ] を開き、[ **開発者** ] セクションを選択してから、 **ダーク外観** スイッチまでスクロールします。 変更は、シミュレーター環境全体に反映されます。

![ダークモードを有効にする](dark-mode-images/LightAndDark_DeveloperSetting.png)

## <a name="assets-for-light-and-dark-modes"></a>ライトモードとダークモード用のアセット

Visual Studio のアセットカタログでは、各表示モード (Universal、Dark、および Light) のオプションのイメージと色がサポートされるようになりました。 このようにイメージと色を定義すると、iOS によって適切なイメージと色が自動的に選択されます。

IOS プロジェクトで **アセット** を開き、新しいイメージセットを追加します。 ターゲットのどの解像度でも、ユニバーサル、濃色、および明るいイメージを指定できます。 次のスクリーンショットには、濃いのイメージと、"Microsoft ロゴ" という名前の明るいものがあります。

![ライトモードとダークモード用のアセット](dark-mode-images/LightAndDark_AssetCatalog2.png)

Asset **. xcassets** には、色の定義である **BackgroundColor** と **TitleColor** のエントリも含まれています。 これらの色は、アプリケーション全体で使用される名前で使用できるようになりました。 このスクリーンショットに示すように、 **BackgroundColor** はビューの背景に割り当てられ、ラベルに **TitleColor** ます。

![スクリーンショットでは、モバイルデバイスが明るいモードで、暗いモードで背景とタイトルの色が異なることが示されています。](dark-mode-images/LightAndDark_01.png)

## <a name="dynamic-system-colors"></a>動的システムカラー

Apple では、新しいダークモードの設定に基づいて外観を動的に調整する新しいセマンティック色が導入されました。

## <a name="summary"></a>まとめ

この記事では、iOS の場合はダークモードが導入され、アセットカタログを使用して各モードのイメージと色が指定されています。

## <a name="related-links"></a>関連リンク

- [ダークモードのデザインガイドライン](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/dark-mode/)
- [セマンティックの色](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#dynamic-system-colors)
