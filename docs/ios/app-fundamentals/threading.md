---
title: Xamarin でのスレッド処理
description: このドキュメントでは、Xamarin iOS アプリケーションでの system.object Api の使用方法について説明します。 タスク並列ライブラリ、応答性の高いアプリケーションの構築、およびガベージコレクションについて説明します。
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 1c9282c790aa5436667b37e1861a96afffcaa668
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009437"
---
# <a name="threading-in-xamarinios"></a>Xamarin でのスレッド処理

Xamarin.iOS ランタイムを使用すると、開発者は、スレッド (`System.Threading.Thread, System.Threading.ThreadPool`) を使用する場合は明示的に .NET スレッド API にアクセスできます。また、非同期デリゲートパターンまたは BeginXXX メソッドを使用する場合は、タスクをサポートするすべての API の範囲を明示的に指定することもできます。並列ライブラリ。

Xamarin では、アプリケーションを構築するために[タスク並列ライブラリ](https://msdn.microsoft.com/library/dd460717.aspx)(TPL) を使用することを強くお勧めします。いくつかの理由があります。

- 既定の TPL スケジューラは、タスクの実行をスレッドプールに委任します。これにより、スレッドの数が多すぎるために CPU 時間の競合が発生することを回避しながら、プロセスの実行に必要なスレッド数が動的に増加します。 
- TPL タスクに関しては、操作について考える方が簡単です。 豊富な API セットを使用して、それらを簡単に操作したり、スケジュールを設定したり、実行をシリアル化したり、多数のタスクを同時に起動したりすることができます。 
- これは、新しいC#非同期言語拡張機能を使用したプログラミングの基礎となります。 

スレッドプールは、システムで使用可能な CPU コアの数、システムの負荷、およびアプリケーションの要求に基づいて、必要に応じてスレッドの数を徐々に拡大します。 このスレッドプールを使用するには、`System.Threading.ThreadPool` のメソッドを呼び出すか、既定の `System.Threading.Tasks.TaskScheduler` (*並列フレームワーク*の一部) を使用します。

通常、開発者は、応答性の高いアプリケーションを作成する必要があり、メイン UI の実行ループをブロックしたくない場合に、スレッドを使用します。

 <a name="Developing_Responsive_Applications" />

## <a name="developing-responsive-applications"></a>応答性の高いアプリケーションの開発

UI 要素へのアクセスは、アプリケーションのメインループを実行しているのと同じスレッドに制限する必要があります。 スレッドからメイン UI を変更する場合は、次のように NSObject を使用してコードをキューに挿入する必要があります[。](xref:Foundation.NSObject)

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

上の例では、アプリケーションをクラッシュさせる可能性のある競合状態を発生させることなく、メインスレッドのコンテキストでデリゲート内のコードを呼び出します。

 <a name="Threading_and_Garbage_Collection" />

## <a name="threading-and-garbage-collection"></a>スレッド処理とガベージコレクション

実行時には、Objective-C ランタイムによってオブジェクトが作成および解放されます。 オブジェクトに "自動リリース" のフラグが設定されている場合、Objective-C ランタイムは、それらのオブジェクトをスレッドの現在の `NSAutoReleasePool`に解放します。 Xamarin は、`System.Threading.ThreadPool` とメインスレッドのすべてのスレッドに対して、1つの `NSAutoRelease` プールを作成します。 これは、TaskScheduler の既定のを使用して作成されたすべてのスレッドをカバーします。

`System.Threading` を使用して独自のスレッドを作成する場合は、データのリークを防ぐために `NSAutoRelease` プールを用意する必要があります。 これを行うには、次のコードでスレッドをラップするだけです。

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

注: Xamarin.iOS 5.2 以降では、独自の `NSAutoReleasePool` を提供する必要がなくなりました。自動的に提供される予定です。

## <a name="related-links"></a>関連リンク

- [UI スレッドの操作](~/ios/user-interface/ios-ui/ui-thread.md)
