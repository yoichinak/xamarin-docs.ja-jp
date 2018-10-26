---
title: チュートリアル - Apple の Instruments ツールの使用
description: この記事では、Apple の Instruments ツールを使用して、Xamarin でビルドされた iOS アプリケーションのメモリ問題を診断する方法について説明します。 Instruments を起動し、ヒープ スナップショットを取得してメモリの増加を分析する方法などを示します。
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: d0c79a5eb417762531245256ff062c5c34ca394c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112326"
---
# <a name="walkthrough---using-apples-instruments-tool"></a>チュートリアル - Apple の Instruments ツールの使用

_この記事では、Apple の Instruments ツールを使用して、Xamarin でビルドされた iOS アプリケーションのメモリ問題を診断する方法について説明します。Instruments を起動し、ヒープ スナップショットを取得してメモリの増加を分析する方法を示します。また、Instruments を使用して、メモリの問題を引き起こしている正確なコード行を表示および特定する方法も示します。_

このページでは、**Xcode の Instruments ツール**を使用して iOS アプリケーションのメモリ問題を診断する方法を示します。
最初に、[MemoryDemo サンプル](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)をダウンロードし、Visual Studio for Mac で **before** ソリューションを開きます。

## <a name="diagnosing-the-memory-issues"></a>メモリ問題を診断する

1. Visual Studio for Mac で、**[ツール] > [Instruments の起動]** メニュー項目から **Instruments** を起動します。
2. **[実行] > [デバイスにアップロード]** メニュー項目を選択し、デバイスにアプリケーションをアップロードします。
3. **[Allocations]\(割り当て\)** テンプレート (白のボックスがあるオレンジ色のアイコン) を選択します。

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "[Allocations]\(割り当て\) テンプレートを選択する")

4. ウィンドウの上部にある **[Choose a profiling template for:]\(プロファイリング テンプレートの選択\)** リストで **[Memory Demo]** アプリケーションを選択します。 最初に iOS デバイスをクリックして、インストールされたアプリケーションが表示されたメニューを展開します。

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Memory Demo アプリケーションを選択する")

5. (ウィンドウの右下にある) **[選択]** ボタンを押し、**Instruments** を起動します。 このテンプレートは、上部ウィンドウに [Allocations]\(割り当て\) と [VM Tracker]\(VM トラッカー\) の 2 つの項目を表示します。

6. Instruments で**レコード** ボタン (左上の赤い円) を押すと、アプリケーションが起動します。

7. 上部ウィンドウ (アプリが実行中で、[Dirty]\(ダーティ\) と [Resident Size]\(常駐サイズ\) の 2 つのセクションが含まれます) で **[VM Tracker]\(VM トラッカー\)** 行を選択します。 **[Inspector]\(検査\)** ウィンドウで、**[Show Display Settings]\(表示設定の表示\)** オプション (歯車アイコン) を選択し、スクリーンショットの右下に表示されている **[Automatic Snapshotting]\(スナップショットの自動作成\)** チェックボックスをオンにします。

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "[Show Display Settings]\(表示設定の表示\) オプション (歯車アイコン) を選択し、[Automatic Snapshotting]\(スナップショットの自動作成\) チェックボックスをオンにする")

8. 上部ウィンドウ (アプリが実行中で、*[All Heap and Anonymous VM]\(すべてのヒープと匿名 VM\)* と表示されます) で **[Allocations]\(割り当て\)** 行を選択します。
9. **[Inspector]\(検査\)** ウィンドウで、**[Show Display Settings]\(表示設定の表示\)** オプション (歯車アイコン) を選択し、**[Mark Generation]\(マークの生成\)** ボタンを押してベースラインを確立します。 ウィンドウの上部のタイムラインに小さな赤いフラグが表示されます。
10. アプリケーションをスクロールし、**[Mark Generation]\(マークの生成\)** をもう一度選択します (数回繰り返します)。
11. **[停止]** ボタンをクリックします。
12. **[Growth]\(増加\)** が最も大きい **Generation** ノードを展開し、**[Growth]\(増加\)** で並べ替えます (降順)。
13. **[Inspector]\(検査\)** ウィンドウを**スタック トレース** を示す **[Show Extended Detail]\(拡張詳細の表示\)** ("E") に変更します。

14. **&lt;non-object>** ノードが過剰なメモリの増加を示していることに注目してください。 詳細を表示するにはこのノードの横にある矢印をクリックします。スタック トレース内を右クリックして、ウィンドウに **[Source Location]\(ソースの場所\)** を追加します。

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "ソースの場所をウィンドウに追加する")

15. **[Size]\(サイズ\)** で並べ替えて、**[Extended Detail]\(拡張詳細\)** ビューを表示します。

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "[Size]\(サイズ\) で並べ替えて、[Extended Detail]\(拡張詳細\) ビューを表示します。")

16. 呼び出し履歴で目的のエントリをクリックして、関連するコードを表示します。

    ![](walkthrough-apples-instrument-images/05-related-code.png "関連するコードを表示する")

この場合、新しいイメージが作成されて各セルのコレクションに保存され、既存のコレクション ビューは再利用されません。

## <a name="resolving-the-memory-issues"></a>メモリ問題の解決

Instruments を使用してこれらの問題を解決し、アプリケーションを再実行することができます。

クラス レベルで 1 つのインスタンスを宣言することで、イメージとセル オブジェクトを毎回作成する代わりに、次のように既存のプールから再利用することができます。

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Dequeue a cell from the reuse pool
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);

    // Reuse the image declared at the class level
    imageCell.ImageView.Image = image;

    return imageCell;
}
```

これで、アプリケーションを実行したときのメモリ使用量が大幅に減少します。生成間の **[Growth]\(増加\)** は、コードを修正する前の MiB (メガバイト単位) ではなく、Kib (キロバイト単位) で測定されるようになりました。

![](walkthrough-apples-instrument-images/06-reduced-memory.png "アプリのメモリ使用量を表示する")

改善されたコードは、Visual Studio for Mac の **after** ソリューションの [MemoryDemo サンプル](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)で入手できます。

[Xamarin.iOS ガベージ コレクション](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)に関するこのコミュニティ ブログは、Xamarin.iOS のメモリ問題に対処するためのリファレンスとして役立ちます。

## <a name="summary"></a>まとめ

この記事では、Instruments を使用してメモリの問題を診断する方法を紹介しました。
Visual Studio for Mac 内から Instruments を起動して、メモリ割り当てのテンプレートを読み込み、スナップショットを使用してメモリの問題を特定する方法を説明しました。
最後に、問題が解決されたことを確認するため、アプリケーションを再検証しました。

## <a name="related-links"></a>関連リンク

- [MemoryDemo サンプル](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Xamarin.iOS ガベージ コレクション (ブログ記事)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
