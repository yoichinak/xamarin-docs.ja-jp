---
title: 9 章の概要です。 プラットフォーム固有の API 呼び出し
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a988c207185fa2305631be67bdd628de089d247e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>9 章の概要です。 プラットフォーム固有の API 呼び出し

プラットフォームによって異なるいくつかのコードを実行する必要があります。 この章では、テクニックについて説明します。

## <a name="preprocessing-in-the-shared-asset-project"></a>共有アセット プロジェクト内の前処理

Xamarin.Forms 共有アセット プロジェクトは c# プリプロセッサ ディレクティブを使用する各プラットフォーム用の別のコードを実行できる`#if`、 `#elif`、および`endif`です。 これはではデモンストレーション[ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![変数の 3 倍のスクリーン ショットの書式設定されたパラグラフ](images/ch09fg01-small.png "デバイス モデルとオペレーティング システム")](images/ch09fg01-large.png#lightbox "デバイス モデルとオペレーティング システム")

ただし、結果のコードには、厄介して読みにくくなるができます。

## <a name="parallel-classes-in-the-shared-asset-project"></a>共有アセット プロジェクト内の並列クラス

SAP のプラットフォーム固有のコードを実行する構造化されたアプローチを示す、 [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2)サンプルです。 プラットフォーム プロジェクトそれぞれは、同じ名前のクラスとメソッドが含まれていますが、その特定のプラットフォームの実装します。 SAP 単にクラスをインスタンス化し、メソッドを呼び出します。

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService およびポータブル クラス ライブラリ

ライブラリは、アプリケーション プロジェクトでクラスを通常どおりアクセスできません。 この制限に示す手法を防止するように見えます**PlatInfoSap2** PCL に使用されているからです。 ただし、Xamarin.Forms がという名前のクラスを含む[ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) .NET リフレクションを使用して PCL から、アプリケーション プロジェクトでクラスをパブリックにアクセスします。

PCL を定義する必要があります、`interface`メンバーの各プラットフォームで使用する必要があるとします。 その後、そのインターフェイスの実装には各プラットフォームが含まれます。 インターフェイスを実装するクラスを識別する必要があります、 [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/)アセンブリ レベル上。

PCL を使用して、ジェネリック[ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/)メソッドの`DependencyService`インターフェイスを実装するプラットフォーム クラスのインスタンスを取得します。

これに示されている、 [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo)サンプルです。

## <a name="platform-specific-sound-generation"></a>プラットフォーム固有のサウンドの生成

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound)サンプルを追加するビープ音、 **MonkeyTap**各プラットフォームでサウンド生成機能にアクセスするプログラムです。



## <a name="related-links"></a>関連リンク

- [9 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [9 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [依存関係サービス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
