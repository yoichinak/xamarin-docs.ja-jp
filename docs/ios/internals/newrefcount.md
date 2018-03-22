---
title: "新しい参照カウントのシステム"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 723a9c4a052f7f432ba0f32ec501af3221b2696f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="new-reference-counting-system"></a>新しい参照カウントのシステム

Xamarin.iOS 9.2.1 には、既定ですべてのアプリケーションにシステムをカウント強化された参照が導入されました。 追跡し、Xamarin.iOS の以前のバージョンで修正が困難でした多くのメモリの問題を回避するのには使用できます。

## <a name="enabling-the-new-reference-counting-system"></a>新しい参照システムのカウントを有効にします。

Xamarin 9.2.1 時点でシステムをカウントする新しい ref が有効になって**すべて**既定のアプリケーションです。

既存のアプリケーションを開発している場合は、ことを確認する .csproj ファイルを確認することができますのすべての出現`MtouchUseRefCounting`に設定されている`true`と同様の下。

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

設定されている場合`false`アプリケーションが、新しいツールを使用していないされます。

### <a name="using-older-versions-of-xamarin"></a>Xamarin の以前のバージョンを使用します。

Xamarin.iOS 7.2.1 および上機能拡張のプレビュー、新しい参照システムをカウントします。

**クラシック API:**

この新しい参照カウント システムを有効にするには確認、**参照カウントの拡張機能を使用して**については、チェック ボックスをオン、**詳細** タブ、プロジェクトの**iOS のビルド オプション**、次のようにします。 

[![](newrefcount-images/image1.png "新しい参照カウント システムを有効にします。")](newrefcount-images/image1.png#lightbox)

For mac、これらのオプションを Visual Studio の新しいバージョンで削除されていることに注意してください。

 **[統一された API:](~/cross-platform/macios/unified/index.md)**

 新しい参照の拡張機能のカウントは、統合 API の必須であり、既定で有効にする必要があります。 IDE の古いバージョンをこの値が自動的にオンになっていない可能性があり、自分でこれによって、チェックを配置する必要があります。

    
> [!IMPORTANT]
> この機能の以前のバージョンが経ちましたが MonoTouch 5.2 がないためだけに使用できる**sgen**実験用のプレビューとして。 この新しい強化されたバージョンも使用できるようになりました、 **Boehm**ガベージ コレクターです。


従来されている Xamarin.iOS によって管理されるオブジェクトの 2 つの種類: 通常余分なメモリ内状態を保持することで、ネイティブ オブジェクト (ピア オブジェクト) と拡張や新機能 (派生オブジェクト) に組み込まれているものをラップするラッパーだけを実行していた。 以前 (たとえば、c# のイベント ハンドラーを追加すること) で状態のピア オブジェクトを補強おできますが、参照されていないと、収集の移動オブジェクトできるようにすることでした。 これが原因で、クラッシュ後で (例: Objective C のランタイムがマネージ オブジェクトにコールバックかどうか)。

新しいシステムは、追加情報の格納時にランタイムによって管理されるオブジェクトに、ピア オブジェクトを自動的にアップグレードします。

これは、このような場面で発生したクラッシュのさまざまなを解決できます。

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

参照カウントの拡張子を除いた、このコードをためにクラッシュは`cell`、回収可能になりますしているため、`TouchDown`デリゲート、付随的なポインターに変換します。

参照カウントの拡張機能により、マネージ オブジェクトは存続により、そのコレクションには、ネイティブ コードで、ネイティブなオブジェクトが保持される提供します。

新しいシステムも不要*ほとんど*プライベート [バインド]-これは既定のインスタンスを維持する方法で使用されるフィールドをバックアップします。 マネージ リンカーは、すべてのものを削除するのに十分なスマート*不要な*新しい参照を使用してアプリケーションからのフィールドは、拡張機能をカウントします。

これは、各マネージ オブジェクトのインスタンスにする前によりも少ないメモリが消費されることを意味します。 また、いくつかのバッキング フィールドが必要なくなったので、一部のメモリを再利用が困難 Objective C ランタイムでの参照を保持する、関連する問題を解決します。
