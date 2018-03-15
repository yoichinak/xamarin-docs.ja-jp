---
title: "影響の概要"
description: "エフェクトをカスタマイズするには、各プラットフォームでネイティブ コントロールは、小規模のスタイル設定の変更に通常使用されます。 この技術概要効果を示します、効果とカスタム レンダラーは、境界示し PlatformEffect クラスについて説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 037c82aa31c167e44a88619cba91a5be8035d0fa
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-effects"></a>影響の概要

_エフェクトをカスタマイズするには、各プラットフォームでネイティブ コントロールは、小規模のスタイル設定の変更に通常使用されます。この技術概要効果を示します、効果とカスタム レンダラーは、境界示し PlatformEffect クラスについて説明します。_

Xamarin.Forms[ページ、レイアウトとコントロール](~/xamarin-forms/user-interface/controls/index.md)クロスプラット フォーム モバイル ユーザー インターフェイスを記述する一般的な API を表示します。 各ページ、レイアウト、およびコントロールを使用して、各プラットフォームの異なる方法で表示、 `Renderer` (Xamarin.Forms 表記に対応する)、ネイティブなコントロールを作成するクラスが画面で、配置し、共有で指定された動作を追加コードです。

開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 ただし、単純なコントロールのカスタマイズを実行するカスタム レンダラー クラスの実装は、ヘビーウェイト応答では多くの場合です。 効果より簡単にカスタマイズするには、各プラットフォームでネイティブ コントロールを許可する、このプロセスを簡略化します。

サブクラス化で効果はプラットフォーム固有のプロジェクトに作成、`PlatformEffect`コントロールとし、効果は使用されている場合、Xamarin.Forms ポータブル クラス ライブラリ (PCL) または共有ライブラリのプロジェクトに適切なコントロールに添付しています。

## <a name="why-use-an-effect-over-a-custom-renderer"></a>カスタム レンダラーを特殊効果を使用する理由

効果は、コントロールのカスタマイズを簡略化は、再利用しを再利用をさらに高めてパラメーター化することができます。

特殊効果を実現できるものは、カスタム レンダラーを実現できます。 ただし、カスタム レンダラーは、柔軟性と効果よりもカスタマイズを提供します。 次のガイドラインには、カスタム レンダラー経由で効果を選択するための状況が一覧表示します。

- プラットフォーム固有のコントロールのプロパティの変更が、目的の結果を達成するには、特殊効果をお勧めします。
- カスタム レンダラーは、プラットフォーム固有のコントロールのメソッドをオーバーライドする必要がある場合に必要です。
- カスタム レンダラーは、Xamarin.Forms コントロールを実装するプラットフォーム固有のコントロールを置換する必要がある場合に必要です。

## <a name="subclassing-the-platformeffect-class"></a>PlatformEffect クラスをサブクラス化

次の表に、名前空間を`PlatformEffect`各プラットフォームでは、およびそのプロパティの型のクラス。

|プラットフォーム|名前空間|コンテナー|コントロール|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|表示|
|Windows Phone 8.1|Xamarin.Forms.Platform.WinRT|FrameworkElement|FrameworkElement|
|ユニバーサル Windows プラットフォーム (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

各プラットフォームに応じた`PlatformEffect`クラスは、次のプロパティを公開します。

- `Container` – レイアウトを実装するために使用されているプラットフォームに固有のコントロールを参照します。
- `Control` – Xamarin.Forms コントロールを実装するために使用されているプラットフォームに固有のコントロールを参照します。
- `Element` – レンダリングされている Xamarin.Forms コントロールを参照します。

効果には、コンテナー、コントロール、または任意の要素にアタッチされているために、関連付けられている要素に関する型情報はありません。 したがって、効果がサポートされていない要素に接続されているときに気付かれないようにまたは、例外をスローする必要があります。 ただし、 `Container`、 `Control`、および`Element`プロパティは、実装する型にキャストすることができます。 詳細については、これらの種類を参照してください[レンダラー基底クラスとネイティブ コントロール](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。

各プラットフォームに応じた`PlatformEffect`クラスは次のメソッドは、特殊効果を実装するをオーバーライドする必要がありますを公開します。

- [`OnAttached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnAttached()/) – Xamarin.Forms コントロールに影響が接続されているときに呼び出されます。 各プラットフォームに固有の効果クラスで、このメソッドのオーバーライド バージョンとは、例外処理の効果を指定した Xamarin.Forms コントロールに適用できませんに備えてと共に、コントロールのカスタマイズを実行する場所です。
- [`OnDetached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnDetached()/) – 効果は、Xamarin.Forms コントロールからデタッチされるときに呼び出されます。 各プラットフォームに固有の効果クラスで、このメソッドのオーバーライド バージョンとは、イベント ハンドラーを登録解除などの効果のクリーンアップを実行する場所です。

さらに、`PlatformEffect`公開、 [ `OnElementPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E.OnElementPropertyChanged/p/System.ComponentModel.PropertyChangedEventArgs/)メソッドを上書きすることもできます。 このメソッドは、要素のプロパティが変更されたときに呼び出されます。 各プラットフォームに固有の効果クラスで、このメソッドのオーバーライド バージョンとは、Xamarin.Forms コントロールにバインド可能なプロパティの変更に応答する場所です。 このオーバーライドは、何度も呼び出すことができるよう、変更されているプロパティのチェックを実行常にする必要があります。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
