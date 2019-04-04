---
title: ListView とアクティビティのライフ サイクル
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b2328759b3158920bc8683ec14c2aebefd7a04ae
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117721"
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView とアクティビティのライフ サイクル

アクティビティでは、起動など、アプリケーションの実行として特定の状態を移動実行は一時停止中、停止しています。 詳細については、および状態移行の処理に関する具体的なガイドラインは、、[アクティビティ ライフ サイクルのチュートリアル](~/android/app-fundamentals/activity-lifecycle/index.md)を参照してください。
アクティビティのライフ サイクルと場所を理解することが重要、`ListView`適切な場所でコード。

'セットアップ タスク'、アクティビティの実行の例では、このドキュメントは、すべて`OnCreate`メソッド (必須) の場合とで '分解' を実行`OnDestroy`します。 データをより頻繁に再読み込みする必要はありません、例は一般に、変更されない小さなデータ セットを使用します。

ただし、データは頻繁に変更または多くのメモリを使用する場合がありますの作成し、更新を別のライフ サイクル メソッドを使用する適切な`ListView`します。 たとえば、基になるデータが絶えず変化する (またはその他のアクティビティの更新プログラムの影響を受ける可能性があります) のアダプターを作成し、`OnStart`または`OnResume`により、アクティビティが表示されるたびに、最新のデータが表示されます。

アダプターは、メモリ、または管理対象のカーソルなどのリソースを使用している場合 (例:、インスタンスが位置する補完的な方法でこれらのリソースを解放する注意してください。 作成されたオブジェクト`OnStart`のでは破棄できます`OnStop`)。


## <a name="configuration-changes"></a>構成の変更

その構成の変更を保存することが重要&ndash;特に画面の回転とキーボードの可視性&ndash;は破棄され再作成するのには、現在のアクティビティが発生することができます (それ以外の場合を使用して指定しない限り、 `ConfigurationChanges`属性の場合)。 つまり、通常の状況では、デバイスを回転させるとは、`ListView`と`Adapter`を再作成して (コードを記述する場合、`OnPause`と`OnResume`) スクロールの位置と行の選択の状態は失われます。

次の属性は、破棄および構成の変更の結果として再作成されてからアクティビティができなくなります。

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

アクティビティをオーバーライドする必要がありますし、`OnConfigurationChanged`適切にそれらの変更に応答します。 構成の変更を処理する方法の詳細については、ドキュメントを参照してください。

