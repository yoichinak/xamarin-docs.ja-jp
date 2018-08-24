---
title: WatchOS Xamarin でナビゲーションの使用
description: このドキュメントでは、ナビゲーション watchOS アプリケーションで使用する方法について説明します。 これは、モーダル インターフェイス、階層間を移動、およびページ ベースのインターフェイスについて説明します。
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c9bcfc388164060549ca7010d11671abfa8230ac
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790641"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>WatchOS Xamarin でナビゲーションの使用

ウォッチで利用可能な最も簡単なナビゲーション オプションは、単純な[モーダル ポップアップ](#modal)現在シーンの上に表示されます。

複数のシーン watch アプリがあるとは、使用可能な 2 つのナビゲーション パラダイムです。

- [階層ナビゲーション](#Hierarchical_Navigation)
- [ページ ベースのインターフェイス](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>モーダル インターフェイス

使用して、`PresentController`をモーダルでインターフェイスのコント ローラーを開くメソッドです。 インターフェイス コント ローラーで既に定義する必要があります、 **Interface.storyboard**です。

```csharp
PresentController ("pageController","some context info");
```

モーダルとして表示されるコント ローラーは、画面全体 (前のシーンをカバーする) を使用します。 既定では、タイトル設定**キャンセル**し、タップすることと、コント ローラーが閉じます。

モーダルとして表示されるコント ローラーをプログラムで終了するには、する呼び出します`DismissController`です。

```csharp
DismissController();
```

モーダル画面は、1 つのシーンまたはページ ベースのレイアウトを使用できます。

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>階層ナビゲーション

使用可能なを後方に移動する方法と同様、スタックのようにシーンを提示`UINavigationController`iOS で動作します。 シーンをナビゲーション スタックにプッシュおよびポップ (プログラムまたはユーザーの選択によって) できます。

![](navigation-images/hierarchy-1.png "シーンをナビゲーション スタックにプッシュできる") ![](navigation-images/hierarchy-2.png "シーンをナビゲーション スタックからポップすることができます")

同様に、iOS は、左のエッジ スワイプは、階層間を移動スタック内の親のコント ローラーに移動します。

両方の[WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog)と[WatchTables](https://developer.xamarin.com/samples/WatchTables)サンプルには、階層的なナビゲーションが含まれています。

### <a name="pushing-and-popping-in-code"></a>プッシュおよびポップ コードで

キットは、包括的な"ナビゲーション controller"を監視は、iOS のように作成するだけでプッシュを使用して、コント ローラー、`PushController`メソッドおよびナビゲーションのスタックは自動的に作成されます。

```csharp
PushController("secondPageController","some context info");
```

ウォッチの画面が含まれます、**戻る**が左、上のボタンはプログラムによってもナビゲーション履歴を使用して、シーンを削除`PopController`です。

```csharp
PopController();
```

Ios はもナビゲーション履歴を使用して、ルートに返すことのできる`PopToRootController`です。

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Segues を使用します。

Segues 階層を移動するストーリー ボードのシーンを作成できます。 ターゲットのシーン、オペレーティング システムの呼び出しのコンテキストを取得する`GetContextForSegue`を新しいインターフェイス コント ローラーを初期化します。

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

ページ ベースのインターフェイスは、左から右と同様の方法をスワイプ`UIPageViewController`iOS で動作します。 現在表示されているどのページを表示する画面の下部では、インジケーターのドットが表示されます。

![](navigation-images/paged-1.png "サンプルの最初のページ") ![](navigation-images/paged-2.png "サンプル 2 ページ目") ![](navigation-images/paged-5.png "5 ページ目のサンプル")


ページ ベースのインターフェイス、watch アプリの主要な UI を使用します`ReloadRootControllers`インターフェイス コント ローラー コンテキストとコンテキストの配列を使用します。

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

ページ ベースのコント ローラーで、ルートではないを示すこともできますを使用して`PresentController`からアプリの他のシーンのいずれか。

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>関連リンク

- [WatchKitCatalog (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WatchTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchTables/)
