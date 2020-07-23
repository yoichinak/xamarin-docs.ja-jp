---
title: Xamarin. iOS の新しい参照カウントシステム
description: このドキュメントでは、Xamarin の強化された参照カウントシステムについて説明します。既定では、すべての Xamarin iOS アプリケーションで有効になっています。
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: a9d8b8fa1826c1a7dafb3d6c3e3ab45d05c1aaa8
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938711"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>Xamarin. iOS の新しい参照カウントシステム

9.2.1 では、既定ですべてのアプリケーションに対して拡張参照カウントシステムが導入されました。 これを使用すると、以前のバージョンの Xamarin では追跡や修正が困難だった多くのメモリの問題を回避できます。

## <a name="enabling-the-new-reference-counting-system"></a>新しい参照カウントシステムを有効にする

Xamarin 9.2.1 の As では、新しい参照カウントシステムが既定で**すべて**のアプリケーションに対して有効になっています。

既存のアプリケーションを開発している場合は、.csproj ファイルをチェックして、次のように、のすべての出現が `MtouchUseRefCounting` に設定されていることを確認できます。 `true`

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

これがに設定されている場合、 `false` アプリケーションは新しいツールを使用しません。

### <a name="using-older-versions-of-xamarin"></a>以前のバージョンの Xamarin の使用

7.2.1 以降の機能では、新しい参照カウントシステムのプレビューが強化されています。

**Classic API：**

この新しい参照カウントシステムを有効にするには、次のように、プロジェクトの**IOS ビルドオプション**の **[詳細設定**] タブにある [**参照カウントの拡張機能を使用**する] チェックボックスをオンにします。 

[![新しい参照カウントシステムを有効にする](newrefcount-images/image1.png)](newrefcount-images/image1.png#lightbox)

これらのオプションは、新しいバージョンの Visual Studio for Mac では削除されています。

 **[Unified API：](~/cross-platform/macios/unified/index.md)**

 新しい参照カウント拡張は、Unified API に必要であり、既定で有効になっている必要があります。 以前のバージョンの IDE では、この値が自動的にチェックされない場合があるため、自分で確認を行う必要がある場合があります。

> [!IMPORTANT]
> この機能の以前のバージョンは、Monotouch.dialog 5.2 以降のものですが、試用版**プレビューとし**てのみ使用できました。 この新しい拡張バージョンは、 **Boehm**ガベージコレクターでも使用できるようになりました。

従来、Xamarin によって管理されるオブジェクトには2種類ありました。これは、ネイティブオブジェクト (ピアオブジェクト) のラッパーであるだけでなく、新しい機能 (派生オブジェクト) を拡張または導入したものです。通常は、メモリ内の状態を追加します。 以前は、(C# のイベントハンドラーを追加するなどして) 状態を持つピアオブジェクトを拡張することもできましたが、オブジェクトは参照されず、収集されるようになりました。 これにより、後でクラッシュが発生する可能性があります (たとえば、目的の C ランタイムがマネージオブジェクトにコールバックされた場合など)。

新しいシステムは、追加情報を格納するときに、ランタイムによって管理されるオブジェクトに自動的にピアオブジェクトをアップグレードします。

これにより、次のような状況で発生したさまざまなクラッシュが解決されます。

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

参照カウントの拡張がない場合、このコードは、 `cell` が収集可能になり、そのデリゲートが未解決のポインターに変換されるため、クラッシュし `TouchDown` ます。

ネイティブオブジェクトがネイティブコードによって保持されていれば、参照カウント拡張によってマネージオブジェクトが引き続き有効であり、コレクションを防ぐことができます。

また、新しいシステムにより、バインドで使用される*ほとんど*のプライベートバッキングフィールドが不要になります。これは、インスタンスを維持するための既定のアプローチです。 マネージリンカーは、新しい参照カウントの拡張機能を使用して、アプリケーションから*不要*なフィールドをすべて削除するのに十分なスマートです。

つまり、各マネージオブジェクトインスタンスは、以前よりもメモリ使用量が少なくなります。 また、関連する問題を解決します。これは、一部のバッキングフィールドに、目的の C ランタイムによって不要になった参照が保持されるため、一部のメモリを再利用するのは困難です。
