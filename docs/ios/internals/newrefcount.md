---
title: 新しい参照の Xamarin.iOS でシステムのカウント
description: このドキュメントでは、Xamarin の拡張の参照カウントの既定では、すべての Xamarin.iOS アプリケーションで有効になっているシステムについて説明します。
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: 8c7b1a88284156cb5d4261f18d5659ed66dfaf64
ms.sourcegitcommit: ee626f215de02707b7a94ba1d0fa1d75b22ab84f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54879330"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>新しい参照の Xamarin.iOS でシステムのカウント

Xamarin.iOS 9.2.1 には、既定ですべてのアプリケーションにシステムをカウントする強化された参照が導入されています。 これを使用を追跡し、以前のバージョンの Xamarin.iOS 修正困難であった多くのメモリ問題を解消できます。

## <a name="enabling-the-new-reference-counting-system"></a>新しい参照カウント システムを有効にします。

有効になってシステムのカウントの新しい ref 時点で、Xamarin 9.2.1**すべて**既定のアプリケーション。

既存のアプリケーションを開発している場合は、ことを確認する .csproj ファイルを確認することができますのすべての出現箇所`MtouchUseRefCounting`に設定されている`true`と同様の下。

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

設定されている場合`false`アプリケーションを使用しないが、新しいツールです。

### <a name="using-older-versions-of-xamarin"></a>以前のバージョンの Xamarin を使用します。

Xamarin.iOS 7.2.1 システムのカウントの新しい参照の拡張のプレビューの機能の上とします。

**クラシック API:**

この新しい参照カウント システムを有効にするには確認、**参照カウント拡張を使用して、** チェック ボックスをオンにある、 **詳細設定**  タブのプロジェクトの**iOS ビルド オプション**、次に示すよう。 

[![](newrefcount-images/image1.png "新しい参照カウント システムを有効にします。")](newrefcount-images/image1.png#lightbox)

これらのオプションが新しいバージョンの Visual Studio for mac。 削除されたことに注意してください。

 **[Unified API:](~/cross-platform/macios/unified/index.md)**

 新しい参照カウントの拡張機能は、Unified API に必要な既定で有効にする必要があります。 お使いの IDE の以前のバージョンをこの値が自動的にオンになっていない可能性があり、自分でによってチェックを配置する必要があります。

    
> [!IMPORTANT]
> この機能の以前のバージョンが誕生 MonoTouch 5.2 がに対してのみ可能でした**sgen**実験的プレビューとして。 この新しい高度なバージョンも使用できるようになりました、 **Boehm**ガベージ コレクターです。


これまでされている Xamarin.iOS によって管理されるオブジェクトの 2 つの種類: 余分なメモリ内状態を保持することで、通常はネイティブ オブジェクト (ピア オブジェクト) と拡張したり、新しい機能 (派生オブジェクト)、単なるラッパーしていた。 以前できました状態で、ピア オブジェクトを拡張しましたが (の追加などにより、C#イベント ハンドラー) が、そのオブジェクトが参照されていないと、収集されたを移動できるようにします。 これは、後でクラッシュを引き起こす可能性 (例: OBJECTIVE-C ランタイム、マネージ オブジェクトにコールバックかどうか)。

新しいシステムは、追加情報の格納時にランタイムによって管理されているオブジェクトに、ピア オブジェクトを自動的にアップグレードします。

これは、ような状況で発生したさまざまなクラッシュを解決できます。

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

参照カウントの拡張子を除いた、このコードをため、クラッシュは`cell`、収集可能になりますので、`TouchDown`が未解決のポインターに変換するデリゲートします。

参照カウントの拡張機能は、ネイティブ コードでネイティブ オブジェクトが保持される指定により、マネージ オブジェクトは、状態を維持できず、そのコレクション。

新しいシステムでは、必要性も削除されます*ほとんど*プライベート バッキング フィールドは既定のインスタンスを維持する方法 - バインドで使用します。 管理対象のリンカーは、これらすべてを削除するのに十分なスマート*不要な*新しい参照を使用してアプリケーションからのフィールドは、拡張機能をカウントします。

これは、各管理対象のオブジェクトのインスタンスがよりも前にメモリを消費することを意味します。 いくつかのバッキング フィールドが必要なくなったいくつかのメモリを解放する困難になること、OBJECTIVE-C ランタイムでの参照を保持は関連する問題も解決します。
