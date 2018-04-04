---
title: スレッド
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6d178231cd45d3b251a26c47abd47bf22b6c2716
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="threading"></a>スレッド

Xamarin.iOS ランタイム開発者へアクセスを提供、.NET Api では、スレッドを使用するときに明示的に両方のスレッド (`System.Threading.Thread, System.Threading.ThreadPool`) と暗黙的に非同期的なデリゲート パターンまたは BeginXXX メソッドだけでなく、完全範囲の Api をサポートするを使用して、タスク並列ライブラリです。



Xamarin の使用を強く推奨、[タスク並列ライブラリ](http://msdn.microsoft.com/en-us/library/dd460717.aspx)(TPL) のいくつかの理由からアプリケーションを構築するため。
-  既定の TPL スケジューラでは、さらに、動的に拡張処理が行わ、スレッドが多すぎるがどこで CPU 時間の競合を終了するシナリオを回避しながら、必要なスレッドの数のスレッド プールにタスクの実行を委任します。 
-  TPL のタスクに関する操作について考えると簡単です。 簡単にして、操作にスケジュール、それらの実行をシリアル化したり、多く機能豊富な一連の Api と並列でを起動できます。 
-  これは、新しい c# 非同期言語拡張機能を使用したプログラミングの基盤です。 


スレッド プールが緩やかに変化に応じてスレッドの数に基づいて拡張システム、システムの負荷とアプリケーションのニーズに使用できる CPU コアの数。 いずれかのメソッドを呼び出すことによってこのスレッド プールを使用できます`System.Threading.ThreadPool`または既定値を使用して`System.Threading.Tasks.TaskScheduler`(の一部、*並列フレームワーク*)。

通常の開発者は、応答性の高いアプリケーションを作成して、ループを実行する主要な UI をブロックしないようにするときに、スレッドを使用します。

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>応答性の高いアプリケーションの開発

UI 要素へのアクセスは、アプリケーションのメイン ループを実行しているのと同じスレッドに制限する必要があります。 スレッドのスレッドから主要な UI を変更する場合は、する必要がありますキューに配置し、コードを使用して[NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/)、次のようにします。

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

上記のアプリケーションがクラッシュする可能性がある可能性がありますのある競合状態を引き起こすことがなく、メイン スレッドのコンテキストでは、デリゲート内のコードを呼び出します。

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>スレッドとガベージ コレクション

実行の過程で Objective C のランタイムは作成し、オブジェクトを解放します。 「自動リリース」Objective C ランタイムでは、それらのオブジェクトを解放します。 オブジェクトは、フラグが設定された場合、スレッドの現在`NSAutoReleasePool`です。 Xamarin.iOS 作成`NSAutoRelease`からすべてのスレッドのプール、`System.Threading.ThreadPool`とメイン スレッドに対してです。 この拡張機能では、System.Threading.Tasks で既定の TaskScheduler を使用して作成されたすべてのスレッドをについて説明します。

使用して、独自のスレッドを作成する場合`System.Threading`自分が所有するを指定する必要は`NSAutoRelease`をデータがリークしていることを防ぐためにプールします。 これを行うには、単に次のコードでスレッドをラップします。

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

注: Xamarin.iOS 5.2 以降されませんが、独自に提供する`NSAutoReleasePool`を自動的にいずれかの指定ができなくなります。


## <a name="related-links"></a>関連リンク

- [UI スレッドの操作](~/ios/user-interface/ios-ui/ui-thread.md)
