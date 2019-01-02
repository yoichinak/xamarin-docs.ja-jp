---
title: Xamarin.Forms の DependencyService
description: 開発者は Xamarin.Forms を使って、プラットフォーム固有のプロジェクトで動作を定義できます。 その後、DependencyService によって適切なプラットフォームの実装が検索され、共有コードでネイティブ機能にアクセスできるようになります。
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: f07b26ed06e85e1b20a29459d2f8f0d2626a2855
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054869"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms の DependencyService

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/UsingDependencyService)

_開発者は Xamarin.Forms を使って、プラットフォーム固有のプロジェクトで動作を定義できます。その後、DependencyService によって適切なプラットフォームの実装が検索され、共有コードでネイティブ機能にアクセスできるようになります。_

このガイドは次の記事で構成されています。

- **[概要](introduction.md)** &ndash; `DependencyService` という概念の全体的なアーキテクチャについて紹介します。
- **[音声合成の実装](text-to-speech.md)** &ndash; 各プラットフォームのネイティブ音声合成システムの使用例について説明します。
- **[デバイスの向きのチェック](device-orientation.md)** &ndash; デバイスの向きを判断するための、プラットフォームのネイティブ API の使用例について説明します。
- **[バッテリ情報の取得](battery-info.md)** &ndash; バッテリの状態に関する情報を取得するためのネイティブ API の使用例について説明します。
- **[ライブラリからの写真の選択](photo-picker.md)** &ndash; 電話の画像ライブラリから写真を選択するための、ネイティブ API の使用例について説明します。


## <a name="related-links"></a>関連リンク

- [DependencyService の使用 (サンプル)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
