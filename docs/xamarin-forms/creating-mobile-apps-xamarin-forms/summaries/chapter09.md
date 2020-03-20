---
title: '第 9 章の概要: プラットフォーム固有の API 呼び出し'
description: 'Xamarin.Forms で Mobile Apps を作成する: 第 9 章の概要: プラットフォーム固有の API 呼び出し'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 3aec84ec6598a45bb989d4bbc1705fd797382755
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334560"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>第 9 章の概要: プラットフォーム固有の API 呼び出し

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)

> [!NOTE] 
> このページのメモでは、Xamarin.Forms が書籍に記載されている資料と異なる部分が示されています。

プラットフォームによって異なるコードを実行することが必要になる場合があります。 この章では、その手法について説明します。

## <a name="preprocessing-in-the-shared-asset-project"></a>共有アセット プロジェクトでの前処理

Xamarin.Forms の共有アセット プロジェクトでは、C# のプリプロセッサ ディレクティブ `#if`、`#elif` と `endif` を使用して、プラットフォームごとに異なるコードを実行できます。 これについては、[**PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1) で示されています

[![可変書式設定された段落のトリプル スクリーンショット](images/ch09fg01-small.png "デバイス モデルとオペレーティング システム")](images/ch09fg01-large.png#lightbox "デバイス モデルとオペレーティング システム")

ただし、結果として得られるコードは、乱雑で読みにくくなる可能性があります。

## <a name="parallel-classes-in-the-shared-asset-project"></a>共有アセット プロジェクトでの並列クラス

SAP でプラットフォーム固有のコードを実行するためのより構造化されたアプローチについては、[**PlatInfoSap2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) サンプルで示されています。 各プラットフォーム プロジェクトには、名前は同じですが、実装はそのプラットフォームに固有である、クラスとメソッドが含まれています。 SAP では、単純にクラスがインスタンス化されて、メソッドが呼び出されます。

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService とポータブル クラス ライブラリ

> [!NOTE] 
> ポータブル クラス ライブラリは、.NET Standard ライブラリに置き換えられています。 本書のすべてのサンプル コードは、.NET Standard ライブラリを使用するように変換されています。

通常、ライブラリでアプリケーション プロジェクト内のクラスにアクセスすることはできません。 この制限により、**PlatInfoSap2** で示されている手法はライブラリでは使用できないように見えます。 しかし、Xamarin.Forms に含まれる [`DependencyService`](xref:Xamarin.Forms.DependencyService) という名前のクラスは、.NET リフレクションを使用して、ライブラリからアプリケーション プロジェクトのパブリック クラスにアクセスします。

ライブラリでは、各プラットフォームで使用する必要があるメンバーを含む `interface` を定義する必要があります。 その場合、各プラットフォームには、そのインターフェイスの実装が含まれます。 インターフェイスを実装するクラスは、アセンブリ レベルで [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) により示される必要があります。

その後、ライブラリでは、`DependencyService` のジェネリック [`Get`](xref:Xamarin.Forms.DependencyService.Get*) メソッドを使用して、インターフェイスを実装するプラットフォーム クラスのインスタンスを取得します。

これは、[**DisplayPlatformInfo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) サンプルで示されています。

## <a name="platform-specific-sound-generation"></a>プラットフォーム固有のサウンド生成

[**MonkeyTapWithSound**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) サンプルでは、各プラットフォームのサウンド生成機能にアクセスすることによって、**MonkeyTap** プログラムにビープ音を追加しています。

## <a name="related-links"></a>関連リンク

- [第 9 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [第 9 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [依存関係サービス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
