---
title: Xamarin. Android ListView とアクティビティのライフサイクル
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: a4ebda89ad3bdcb663dc9d7cd513e45760163c34
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028945"
---
# <a name="xamarinandroid-listview-and-the-activity-lifecycle"></a>Xamarin. Android ListView とアクティビティのライフサイクル

アクティビティは、起動、実行中、一時停止、停止中など、アプリケーションの実行中に特定の状態を処理します。 詳細と、状態遷移を処理するための具体的なガイドラインについては、[アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)に関するチュートリアルを参照してください。
アクティビティのライフサイクルを理解し、`ListView` コードを正しい場所に配置することが重要です。

このドキュメントのすべての例では、アクティビティの `OnCreate` メソッドで "セットアップタスク" を実行し、必要に応じて `OnDestroy`で "破棄" を実行します。 一般的に、この例では変更されない小さなデータセットを使用しているため、データを頻繁に再読み込みする必要はありません。

ただし、データが頻繁に変更される場合や、大量のメモリを使用する場合は、異なるライフサイクルメソッドを使用して `ListView`を設定し、更新することが適切な場合があります。 たとえば、基になるデータが絶えず変化している (または他の活動の更新によって影響を受ける可能性がある) 場合、`OnStart` または `OnResume` でアダプタを作成すると、アクティビティが表示されるたびに最新のデータが表示されるようになります。

アダプターがメモリやマネージカーソルなどのリソースを使用している場合は、それらのリソースがインスタンス化された場所 (例として、 `OnStart` で作成されたオブジェクトは `OnStop`で破棄できます)。

## <a name="configuration-changes"></a>構成の変更

構成の変更は、特に画面の回転とキーボードの &ndash; 表示の &ndash; によって、現在のアクティビティが破棄され、再作成される可能性があることに注意する必要があります (`ConfigurationChanges` 属性を使用して指定しない限り)。 これは、通常の状況下では、デバイスのローテーションによって `ListView` と `Adapter` が再作成されることを意味します (`OnPause` および `OnResume`でコードを記述していない場合)。スクロール位置と行選択の状態は失われます。

次の属性は、構成の変更の結果として、アクティビティが破棄されて再作成されるのを防ぎます。

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

その後、アクティビティは `OnConfigurationChanged` をオーバーライドして、これらの変更に適切に応答する必要があります。 構成の変更を処理する方法の詳細については、のドキュメントを参照してください。
