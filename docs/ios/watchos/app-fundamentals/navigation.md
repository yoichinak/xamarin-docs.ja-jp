---
title: WatchOS Xamarin でのナビゲーションの操作
description: このドキュメントでは、watchOS アプリケーションでナビゲーションを操作する方法について説明します。 これは、モーダル インターフェイス、階層型ナビゲーション、およびページ ベースのインターフェイスについて説明します。
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 7502998e8e8da5e011ef1ec0ab7dcc6e3d892d3d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286051"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>WatchOS Xamarin でのナビゲーションの操作

Watch で使用可能な最も簡単なナビゲーション オプションは、単純な[モーダル ポップアップ](#modal)現在のシーンの上に表示されます。

複数のシーン ウォッチ アプリケーションは使用可能な 2 つのナビゲーション パラダイムです。

- [階層ナビゲーション](#Hierarchical_Navigation)
- [ページ ベースのインターフェイス](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>モーダル インターフェイス

使用して、`PresentController`インターフェイス コント ローラーをモーダルとして開くメソッドです。 インターフェイス コント ローラーで既に定義する必要があります、 **Interface.storyboard**します。

```csharp
PresentController ("pageController","some context info");
```

コント ローラーのモーダルとして表示では、画面全体を (前のシーンをカバーする) を使用します。 既定では、タイトル設定されます**キャンセル**し、タップすることで、コント ローラーが閉じます。

モーダルとして表示されるコント ローラーをプログラムで閉じる呼び出す`DismissController`します。

```csharp
DismissController();
```

モーダルの画面は、1 つのシーンまたはページ ベースのレイアウトを使用できます。

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>階層ナビゲーション

移動を遡ってと同じように、スタックのようにシーンを表示します。 `UINavigationController` iOS で動作します。 シーンをナビゲーション スタックにプッシュし、(プログラムまたはユーザーの選択によって) からポップします。

![](navigation-images/hierarchy-1.png "シーンをナビゲーション スタックにプッシュできる") ![](navigation-images/hierarchy-2.png "シーンをナビゲーション スタックからポップすることができます")

同様に、iOS 左エッジ スワイプが階層型ナビゲーション スタックの親のコント ローラーに移動します。

両方の[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)と[WatchTables](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)サンプルには、階層型ナビゲーションが含まれています。

### <a name="pushing-and-popping-in-code"></a>プッシュとポップをコードで

キットは、包括的な「ナビゲーション コント ローラー」をご覧ください。 - iOS と同様に作成する単純にプッシュを使用して、コント ローラー、`PushController`メソッドとナビゲーション スタックが自動的に作成されます。

```csharp
PushController("secondPageController","some context info");
```

ウォッチの画面が含まれます、**戻る**左、上のボタンを使ってナビゲーション スタックからシーンを削除できますもプログラムで`PopController`します。

```csharp
PopController();
```

ですので、iOS でもを使用して、ナビゲーション スタックのルートに返すこと`PopToRootController`します。

```csharp
PopToRootController();
```

### <a name="using-segues"></a>セグエを使用して

セグエを階層型ナビゲーションを定義するストーリー ボード内のシーン間に作成できます。 ターゲットのシーン、オペレーティング システムの呼び出しのコンテキストを取得する`GetContextForSegue`新しいインターフェイス コント ローラーを初期化します。

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```

<a name="Page-Based_Interfaces"/>

## <a name="page-based-interfaces"></a>ページ ベースのインターフェイス

ページ ベースのインターフェイスは、左から右と同様の方法をスワイプ`UIPageViewController`iOS で動作します。 現在表示されているどのページを表示する画面の下部には、インジケーターのドットが表示されます。

![](navigation-images/paged-1.png "サンプルの最初のページ") ![](navigation-images/paged-2.png "サンプル 2 ページ目") ![](navigation-images/paged-5.png "5 ページ目のサンプル")


ページ ベースのインターフェイスを watch アプリのメイン UI にするを使用して`ReloadRootControllers`インターフェイス コント ローラーとコンテキストの配列。

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

ページ ベースのコント ローラーではないルートを表示することもできます。 を使用して`PresentController`からアプリ内の他のシーンのいずれか。

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WatchTables (サンプル)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchTables/)
