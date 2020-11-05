---
title: アクティビティインジケーター Xamarin.Forms
description: ActivityIndicator コントロールは、進行状況を示すことなく、アプリケーションが時間のかかるアクティビティに関与していることをユーザーに示します。 この記事では、XAML とコードで ActivityIndicator を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 739fc90eda513e4627f5804283bdcab9c0cbacb4
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373121"
---
# <a name="no-locxamarinforms-activityindicator"></a>Xamarin.Forms ActivityIndicator
[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

コントロールには、 Xamarin.Forms [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) アプリケーションが長時間のアクティビティに関与していることを示すアニメーションが表示されます。 とは異なり、で [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) は `ActivityIndicator` 進行状況が示されません。 は、 `ActivityIndicator` から継承さ [`View`](xref:Xamarin.Forms.View) れます。

次のスクリーンショットは、 `ActivityIndicator` iOS と Android のコントロールを示しています。

![IOS と Android での ActivityIndicator のスクリーンショット](activityindicator-images/activityindicators-default.png "IOS と Android での ActivityIndicator のスクリーンショット")

コントロールは、 `ActivityIndicator` 次のプロパティを定義します。

* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)`Color`の表示色を定義する値です `ActivityIndicator` 。
* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) を `bool` `ActivityIndicator` 表示、アニメーション、または非表示にするかどうかを示す値です。 値がの場合 `false` 、は表示され `ActivityIndicator` ません。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、を `ActivityIndicator` スタイル設定し、データバインディングのターゲットにすることができます。

## <a name="create-an-activityindicator"></a>ActivityIndicator を作成する

クラスは、 `ActivityIndicator` XAML でインスタンス化できます。 この `IsRunning` プロパティは、コントロールが表示され、アニメーション化されるかどうかを決定します。 `IsRunning` プロパティでは、既定値が `false` に設定されます。 次の例は、 `ActivityIndicator` 省略可能なプロパティセットを使用して、XAML でをインスタンス化する方法を示してい `IsRunning` ます。

```xaml
<ActivityIndicator IsRunning="true" />
```

`ActivityIndicator`コードでを作成することもできます。

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>ActivityIndicator の外観のプロパティ

プロパティは、 `Color` 色を定義し `ActivityIndicator` ます。 次の例は、 `ActivityIndicator` プロパティセットを使用して XAML でをインスタンス化する方法を示してい `Color` ます。

```xaml
<ActivityIndicator Color="Orange" />
```

`Color`プロパティは、コードでを作成するときに設定することもでき `ActivityIndicator` ます。

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

次のスクリーンショットは、 `ActivityIndicator` `Color` IOS および Android でプロパティがに設定されているを示してい `Color.Orange` ます。

![IOS と Android のスタイル付き ActivityIndicator のスクリーンショット](activityindicator-images/activityindicators-styled.png "IOS と Android のスタイル付き ActivityIndicator のスクリーンショット")

## <a name="related-links"></a>関連リンク

* [ActivityIndicator デモ](/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)