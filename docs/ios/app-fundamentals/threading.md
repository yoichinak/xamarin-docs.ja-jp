---
title: Xamarin でのスレッド処理
description: このドキュメントでは、Xamarin iOS アプリケーションでの system.object Api の使用方法について説明します。 タスク並列ライブラリ、応答性の高いアプリケーションの構築、およびガベージコレクションについて説明します。
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: d8267d4def0f7c24c660dfb4d301c111a92bb0b9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767154"
---
# <a name="threading-in-xamarinios"></a>Xamarin でのスレッド処理

Xamarin の iOS ランタイムを使用すると、開発者は、スレッド (`System.Threading.Thread, System.Threading.ThreadPool`) を使用する場合は明示的に、非同期デリゲートパターンまたは BeginXXX メソッドを使用する場合は暗黙的に .net スレッド api にアクセスできます。また、タスク並列ライブラリ。

Xamarin では、アプリケーションを構築するために[タスク並列ライブラリ](https://msdn.microsoft.com/library/dd460717.aspx)(TPL) を使用することを強くお勧めします。いくつかの理由があります。
- 既定の TPL スケジューラは、タスクの実行をスレッドプールに委任します。これにより、スレッドの数が多すぎるために CPU 時間の競合が発生することを回避しながら、プロセスの実行に必要なスレッド数が動的に増加します。 
- TPL タスクに関しては、操作について考える方が簡単です。 豊富な Api セットを使用して、それらを簡単に操作したり、スケジュールを設定したり、実行をシリアル化したり、多数のタスクを同時に起動したりすることができます。 
- これは、新しいC#非同期言語拡張機能を使用したプログラミングの基礎となります。 

スレッドプールは、システムで使用可能な CPU コアの数、システムの負荷、およびアプリケーションの要求に基づいて、必要に応じてスレッドの数を徐々に拡大します。 このスレッドプールを使用するには、で`System.Threading.ThreadPool`メソッドを呼び出すか、既定`System.Threading.Tasks.TaskScheduler`の (*並列フレームワーク*の一部) を使用します。

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

実行時には、目的 C ランタイムによってオブジェクトが作成および解放されます。 オブジェクトに "自動リリース" のフラグが設定されている場合、目的の C ランタイムは、それらの`NSAutoReleasePool`オブジェクトをスレッドの現在のに解放します。 Xamarin は、メインスレッド`NSAutoRelease`に対してとの`System.Threading.ThreadPool`すべてのスレッドに対して1つのプールを作成します。 これは、TaskScheduler の既定のを使用して作成されたすべてのスレッドをカバーします。

を使用して`System.Threading`独自のスレッドを作成する場合は、データのリークを防ぐために、独自`NSAutoRelease`のプールを用意する必要があります。 これを行うには、次のコードでスレッドをラップするだけです。

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

メモ:Xamarin iOS 5.2 では、自動的に提供されるため、 `NSAutoReleasePool`独自のものを提供する必要はありません。

## <a name="related-links"></a>関連リンク

- [UI スレッドの操作](~/ios/user-interface/ios-ui/ui-thread.md)
