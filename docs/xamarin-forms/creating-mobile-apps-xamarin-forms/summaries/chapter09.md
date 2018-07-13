---
title: 第 9 章の概要です。 プラットフォーム固有の API 呼び出し
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 9 章の概要。 プラットフォーム固有の API 呼び出し'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 8a035da3dec468df291a19849ca89964c6707589
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994758"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>第 9 章の概要です。 プラットフォーム固有の API 呼び出し

プラットフォームによって異なるいくつかのコードを実行する必要があります。 この章では、手法について説明します。

## <a name="preprocessing-in-the-shared-asset-project"></a>共有アセット プロジェクトの前処理

Xamarin.Forms 共有アセット プロジェクトは c# プリプロセッサ ディレクティブを使用してプラットフォームごとに別のコードを実行できる`#if`、 `#elif`、および`endif`します。 これは、方法については[ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![変数の 3 倍になるスクリーン ショットは、段落を書式設定された](images/ch09fg01-small.png "デバイス モデルとオペレーティング システム")](images/ch09fg01-large.png#lightbox "デバイス モデルとオペレーティング システム")

ただし、結果のコードには、厄介して読みにくくなるができます。

## <a name="parallel-classes-in-the-shared-asset-project"></a>共有アセット プロジェクト内の並列クラス

SAP でプラットフォーム固有のコードを実行する方法のより構造化された方法については、 [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2)サンプル。 各プラットフォーム プロジェクトには、同じ名前のクラスおよびメソッドが含まれていますが、その特定のプラットフォームの実装します。 SAP 単にクラスをインスタンス化し、メソッドを呼び出します。

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService とポータブル クラス ライブラリ

ライブラリは、アプリケーション プロジェクトでクラスを通常どおりアクセスできません。 この制限のように示した手法を防ぐために**PlatInfoSap2** PCL に使用されているからです。 ただし、Xamarin.Forms には、という名前のクラスが含まれます。 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) .NET リフレクションを使用して、PCL から、アプリケーション プロジェクトでクラスをパブリックにアクセスします。

PCL を定義する必要があります、`interface`メンバーの各プラットフォームで使用する必要があるとします。 次に、そのインターフェイスの実装には各プラットフォームが含まれます。 インターフェイスを実装するクラスを識別する必要があります、 [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute)アセンブリ レベル上。

PCL を使用して、ジェネリック[ `Get` ](xref:Xamarin.Forms.DependencyService.Get*)メソッドの`DependencyService`インターフェイスを実装するプラットフォームのクラスのインスタンスを取得します。

これは、方法については、 [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo)サンプル。

## <a name="platform-specific-sound-generation"></a>プラットフォーム固有のサウンドの生成

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound)サンプルにビープ音を追加する、 **MonkeyTap**各プラットフォームでのサウンド生成機能にアクセスしてプログラム。



## <a name="related-links"></a>関連リンク

- [第 9 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [第 9 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [依存関係サービス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
