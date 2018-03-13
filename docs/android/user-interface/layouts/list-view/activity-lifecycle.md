---
title: "ListView コントロールとアクティビティのライフ サイクル"
ms.topic: article
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b8ee113a321dbc84cf12a7ef4bb5084c5307115b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView コントロールとアクティビティのライフ サイクル

アクティビティを通過、特定の状態の起動など、アプリケーションの実行と実行が一時停止中、および停止します。 詳細については、状態遷移を処理する方法の特定のガイドラインを参照してください、[アクティビティ ライフ サイクルのチュートリアル](~/android/app-fundamentals/activity-lifecycle/index.md)です。
アクティビティのライフ サイクルと場所を理解することが重要、`ListView`正しい場所にコード。

このドキュメントの例を実行するセットアップ タスク アクティビティの`OnCreate`メソッド (必須) の場合とで 'teardown' を実行`OnDestroy`です。 データをより頻繁に再読み込みする必要はありません、例は通常、変更されない小さなデータ セットを使用します。

ただし場合、データは頻繁に変更または多くのメモリ使用がありますメソッドを使用して別のライフ サイクルと、挿入、更新する適切な`ListView`です。 たとえば、基になるデータが絶えず変更 (またはその他の活動の更新の影響を受ける可能性があります) のアダプターを作成し、`OnStart`または`OnResume`すれば、アクティビティが表示されるたびに最新のデータが表示されます。

アダプターは、メモリ、または管理対象のカーソルなどのリソースを使用している場合、インスタンス化 (に補完的なメソッドでこれらのリソースを解放する注意してください。 作成されたオブジェクト`OnStart`できますで破棄する`OnStop`)。


## <a name="configuration-changes"></a>構成の変更

その構成の変更を保存することが重要&ndash;画面の回転とキーボードの可視性の特に&ndash;破棄して再作成するのには、現在のアクティビティが発生することができます (それ以外の場合を使用して指定しない限り、 `ConfigurationChanges`属性の場合) です。 つまり、通常の状況では、デバイスを回転させる原因は、`ListView`と`Adapter`を再作成して (でコードを記述する場合を除き、`OnPause`と`OnResume`) スクロール位置と行の選択状態が失われます。

次の属性は破棄され、構成の変更の結果として再作成されてからアクティビティを妨げます。

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

アクティビティをオーバーライドする必要があります、`OnConfigurationChanged`適切にそれらの変更に応答します。 構成の変更を処理する方法の詳細については、マニュアルを参照してください。

