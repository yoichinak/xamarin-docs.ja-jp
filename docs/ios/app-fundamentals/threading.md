---
title: Xamarin.iOS でのスレッド
description: このドキュメントでは、Xamarin.iOS アプリケーションで System.Threading Api を使用する方法について説明します。 これには、応答性の高いアプリケーションは、およびガベージ コレクションの作成タスク並列ライブラリで、について説明します。
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: 7dbb0044f09d5bc00f2393eb647efba05a061c3f
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669545"
---
# <a name="threading-in-xamarinios"></a>Xamarin.iOS でのスレッド

Xamarin.iOS ランタイムにより、開発者にアクセス、.NET Api では、スレッドを使用するときに明示的に両方のスレッド (`System.Threading.Thread, System.Threading.ThreadPool`) デリゲートの非同期パターンまたは BeginXXX メソッドだけでなく、完全な範囲をサポートする Api を使用するときに暗黙的に、タスク並列ライブラリ。



Xamarin の使用を強く推奨、[タスク並列ライブラリ](https://msdn.microsoft.com/library/dd460717.aspx)(TPL) のいくつかの理由からアプリケーションを構築します。
-  既定の TPL スケジューラでは、さらに、プロセスは、スレッドが多すぎるが CPU 時間の競合する最終的なシナリオを回避しながら行わときに、必要なスレッドの数を増加は動的にスレッド プールにタスクの実行を委任します。 
-  TPL のタスクに関する操作について検討しやすくなります。 簡単に操作、スケジュール設定でそれらの実行をシリアル化したり、多くの Api の豊富なセットと並列で起動できます。 
-  新しい c# async 言語拡張を使用したプログラミングの基礎となります。 


スレッド プールが緩やかに変化に応じてスレッドの数に基づいて拡張、システム、システムの負荷およびアプリケーションの要求で使用可能な CPU コアの数。 いずれかのメソッドを呼び出すことによってこのスレッド プールを使用できる`System.Threading.ThreadPool`または既定値を使用して`System.Threading.Tasks.TaskScheduler`(の一部、*並列処理フレームワーク*)。

通常の開発者は、応答性の高いアプリケーションを作成して、ループを実行する主な UI をブロックしないときに、スレッドを使用します。

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>応答性の高いアプリケーションの開発

UI 要素へのアクセスは、アプリケーションのメイン ループを実行している同じスレッドに制限する必要があります。 使用して、コードをキューがスレッドからの主要な UI を変更する場合は、 [NSObject.InvokeOnMainThread](xref:Foundation.NSObject)、次のようにします。

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

上記は、アプリケーションがクラッシュする可能性がある可能性がありますのある競合状態を発生させることがなく、メイン スレッドのコンテキストでデリゲート内のコードを呼び出します。

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>スレッド処理とガベージ コレクション

実行の過程では、OBJECTIVE-C ランタイムを作成し、オブジェクトを解放します。 オブジェクトは、「自動リリース」OBJECTIVE-C ランタイムでそれらのオブジェクトはリリースのフラグが設定されている場合、スレッドの現在`NSAutoReleasePool`します。 Xamarin.iOS を作成します`NSAutoRelease`からすべてのスレッドのプール、`System.Threading.ThreadPool`とメインのスレッド。 この拡張機能では、System.Threading.Tasks で既定の TaskScheduler を使用して作成されたすべてのスレッドについて説明します。

使用して、独自のスレッドを作成する場合`System.Threading`自分が所有するを指定する必要が`NSAutoRelease`データがリークしていることを防ぐためにプールします。 これを行うには、単に次のコードでスレッドをラップします。

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

メモ:Xamarin.iOS 5.2 以降は、独自に提供する必要はありません`NSAutoReleasePool`を自動的に 1 つ提供はできなくなります。


## <a name="related-links"></a>関連リンク

- [UI スレッドの操作](~/ios/user-interface/ios-ui/ui-thread.md)
