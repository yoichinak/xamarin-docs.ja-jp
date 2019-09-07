---
title: Xamarin. Android ListView とアクティビティのライフサイクル
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 1c093d3d67ace3b0f9186fca8226d4ef631d9af0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762308"
---
# <a name="xamarinandroid-listview-and-the-activity-lifecycle"></a>Xamarin. Android ListView とアクティビティのライフサイクル

アクティビティは、起動、実行中、一時停止、停止中など、アプリケーションの実行中に特定の状態を処理します。 詳細と、状態遷移を処理するための具体的なガイドラインについては、[アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)に関するチュートリアルを参照してください。
アクティビティのライフサイクルを理解し、正しい場所に`ListView`コードを配置することが重要です。

このドキュメントのすべての例では、アクティビティの`OnCreate`メソッドで "セットアップタスク" を実行し、(必要に応じて) で`OnDestroy`"破棄" を実行します。 一般的に、この例では変更されない小さなデータセットを使用しているため、データを頻繁に再読み込みする必要はありません。

ただし、データが頻繁に変更される場合や、大量のメモリを使用する場合は、異なるライフサイクルメソッドを使用し`ListView`てを設定し、更新することが適切な場合があります。 たとえば、基になるデータが絶えず変化している場合 (または他のアクティビティの更新によって影響`OnStart`を`OnResume`受ける可能性がある場合)、またはでアダプターを作成すると、アクティビティが表示されるたびに最新のデータが表示されるようになります。

アダプターがメモリやマネージカーソルなどのリソースを使用している場合は、それらのリソースがインスタンス化された場所 (例として、 で作成さ`OnStart`れたオブジェクトは、 `OnStop`で破棄できます)。

## <a name="configuration-changes"></a>構成の変更

構成の変更によって、特&ndash;に画面の回転とキーボード&ndash;の表示を変更すると、現在のアクティビティが破棄されて再作成される`ConfigurationChanges`可能性があることに注意する必要があります (特に指定しない限り、属性)。 これは、通常の状況下`ListView`では、デバイスのローテーションによってと`Adapter`が再作成されることを意味します ( `OnResume`とで`OnPause`コードを記述していない場合)。スクロール位置と行の選択状態は失われます。

次の属性は、構成の変更の結果として、アクティビティが破棄されて再作成されるのを防ぎます。

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

その後、アクティビティは`OnConfigurationChanged` 、これらの変更に適切に応答するようにをオーバーライドする必要があります。 構成の変更を処理する方法の詳細については、のドキュメントを参照してください。
