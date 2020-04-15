---
title: Xamarin.Mac アプリのネイティブ クラッシュをデバッグする
description: このドキュメントでは、Objective-C ランタイムで発生した例外をデバッグする方法について説明します。 アサーション エラー、コールバックの問題、例外バブリングなどについて取り上げています。
ms.prod: xamarin
ms.assetid: B0C0CE31-2737-4969-8EA5-D39D3333E9C2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: 40d849ad403f2f47c00be9d3da7b59fc27ce8002
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725494"
---
# <a name="debugging-a-native-crash-in-a-xamarinmac-app"></a>Xamarin.Mac アプリのネイティブ クラッシュをデバッグする

## <a name="overview"></a>概要

プログラミング エラーが発生すると、ネイティブ Objective-C ランタイムでクラッシュが発生することがあります。 この場合、C# の例外とは異なり、修正対象となるコード内の特定の行が示されません。 場合によっては、見つけて修正することが容易ではなく、追跡が非常に難しいこともあります。

実際のネイティブ クラッシュの例をいくつか見てみましょう。

## <a name="example-1-assertion-failure"></a>例 1: アサーション エラー

単純なテスト アプリケーションで発生するクラッシュ例の最初の数行です (この情報は、 **[アプリケーション出力]** に表示されます)。

```csharp
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.381 NSOutlineViewHottness[79111:1304993] (
  0   CoreFoundation                      0x91888343 __raiseError + 195
  1   libobjc.A.dylib                     0x9a5e6a2a objc_exception_throw + 276
  2   CoreFoundation                      0x918881ca +[NSException raise:format:arguments:] + 138
  3   Foundation                          0x950742b1 -[NSAssertionHandler handleFailureInMethod:object:file:lineNumber:description:] + 118
  4   AppKit                              0x975db476 -[NSTableView _uncachedRectHeightOfRow:] + 373
  5   AppKit                              0x975db2f8 -[_NSTableRowHeightStorage _uncachedRectHeightOfRow:] + 143
  6   AppKit                              0x975db206 -[_NSTableRowHeightStorage _cacheRowHeights] + 167
  7   AppKit                              0x975db130 -[_NSTableRowHeightStorage _createRowHeightsArray] + 226
  8   AppKit                              0x975b5851 -[_NSTableRowHeightStorage _ensureRowHeights] + 73
  9   AppKit                              0x975b5790 -[_NSTableRowHeightStorage computeTableHeightForNumberOfRows:] + 89
  10  AppKit                              0x975b4c38 -[NSTableView _totalHeightOfTableView] + 220
```

先頭に数値が付いている行がネイティブ スタック トレースです。 ここから、行の高さを処理する `NSTableView` 内のどこかでクラッシュが発生したことがわかります。 `NSAssertionHandler` から `NSException (objc_exception_throw)` が生成され、アサーション エラーが表示されます。

```csharp
rowHeight error: The value must be > 0 for row 0, but the delegate
<NSOutlineView_ViewDelegate: 0xaa01860> gave -1.000
```

このように、一部の `NSOutlineViewDelegate` メソッドが負の数を返すことがわかります。 問題となったのは以下の部分です。

```csharp
public override nfloat GetRowHeight (NSTableView tableView, nint row)
{
    return -1;
}
```

## <a name="example-2-callback-jumped-into-middle-of-nowhere"></a>例 2: コールバックが不明な場所にジャンプした

```csharp
Stacktrace:

 at <unknown> <0xffffffff>
 at (wrapper managed-to-native) MonoMac.AppKit.NSApplication.NSApplicationMain (int,string[]) <IL 0x000a4, 0xffffffff>
 at MonoMac.AppKit.NSApplication.Main (string[]) [0x00041] in /Users/donblas/Programming/xamcore-master/src/AppKit/NSApplication.cs:107
 at NSOutlineViewHottness.MainClass.Main (string[]) [0x00007] in /Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/Main.cs:14
 at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr) <IL 0x00050, 0xffffffff>

Native stacktrace:

Debug info from gdb:

(lldb) command source -s 0 '/tmp/mono-gdb-commands.qrHllW'
Executing commands in '/private/tmp/mono-gdb-commands.qrHllW'.
(lldb) process attach --pid 79229
Process 79229 stopped
Executable module set to "/Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/bin/Debug/NSOutlineViewHottness.app/Contents/MacOS/NSOutlineViewHottness".
Architecture set to: i386-apple-macosx.
(lldb) thread list
Process 79229 stopped
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 thread #2: tid = 0x142790, 0x9af768d2 libsystem_kernel.dylib`kevent64 + 10, queue = 'com.apple.libdispatch-manager'
 thread #3: tid = 0x142792, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #4: tid = 0x142794, 0x9af6fa6a libsystem_kernel.dylib`semaphore_wait_trap + 10
 thread #5: tid = 0x142795, 0x9af75772 libsystem_kernel.dylib`__recvfrom + 10
 thread #6: tid = 0x142799, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #7: tid = 0x14279a, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #8: tid = 0x14279b, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #9: tid = 0x1427f8, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #10: tid = 0x1427fe, 0x9af6fa2e libsystem_kernel.dylib`mach_msg_trap + 10
(lldb) thread backtrace all
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 * frame #0: 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10
   frame #1: 0x986bfb25 libsystem_c.dylib`waitpid$UNIX2003 + 48
   frame #2: 0x028ba36d libmono-2.0.dylib`mono_handle_native_sigsegv(signal=11, ctx=0x03115fe0) + 541 at mini-exceptions.c:2323
   frame #3: 0x0290a8bb libmono-2.0.dylib`mono_arch_handle_altstack_exception(sigctx=<unavailable>, fault_addr=<unavailable>, stack_ovf=0) + 155 at exceptions-x86.c:1159
   frame #4: 0x0280b4fd libmono-2.0.dylib`mono_sigsegv_signal_handler(_dummy=<unavailable>, info=<unavailable>, context=<unavailable>) + 445 at mini.c:6861
   frame #5: 0x91ef403b libsystem_platform.dylib`_sigtramp + 43
   frame #6: 0x9a5dd0bd libobjc.A.dylib`objc_msgSend + 45
   frame #7: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #8: 0x9773ba91 AppKit`-[NSApplication sendAction:to:from:] + 548
   frame #9: 0x9773b82d AppKit`-[NSControl sendAction:to:] + 102
   frame #10: 0x97934d36 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 176
   frame #11: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #12: 0x97787975 AppKit`-[NSCell _sendActionFrom:] + 161
   frame #13: 0x979188ea AppKit`-[NSButtonCell _sendActionFrom:] + 55
   frame #14: 0x979366e6 AppKit`__48-[NSCell trackMouse:inRect:ofView:untilMouseUp:]_block_invoke965 + 43
   frame #15: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #16: 0x977a3d00 AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2815
   frame #17: 0x977a2df4 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 524
   frame #18: 0x977a233b AppKit`-[NSControl mouseDown:] + 762
   frame #19: 0x97cbc112 AppKit`-[_NSThemeWidget mouseDown:] + 378
   frame #20: 0x97d36d74 AppKit`-[NSWindow _reallySendEvent:] + 12353
   frame #21: 0x977201f9 AppKit`-[NSWindow sendEvent:] + 409
   frame #22: 0x976cdc67 AppKit`-[NSApplication sendEvent:] + 4679
   frame #23: 0x9754807c AppKit`-[NSApplication run] + 1003
```

これは追跡がはるかに困難だった問題です。 マネージド スタック トレースの一番上に `at <unknown> <0xffffffff>` または `MonoMac.ObjCRuntime.Runtime.GetNSObject (IntPtr ptr)` が表示される場合、ガベージ コレクションされたオブジェクトでマネージド コードの実行が試行されていると考えられます。 ネイティブ スタック トレースで、`trackMouse:inRect:ofView:untilMouseUp` から `NSCell _sendActionFrom` に推移していることがわかるので、どこかでクリック イベントを処理し、その結果、C# にコールバックしようとして問題が起きています。

一般的に、このようなエラーの追跡は困難です。 ここでは、`GC.Collect(2)` をボタン ハンドラーに追加して (ガベージ コレクションを強制して) この問題を追跡することで、問題を再現可能にしました。 この例を分けて、問題が解消されるまでコードのセクションを削除すると、[このバグ](https://bugzilla.xamarin.com/show_bug.cgi?id=23378)が判明しました。

```csharp
mainWindowController.Window.StandardWindowButton (NSWindowButton.CloseButton).Activated += HandleActivated;
```

イベントが登録されていても、`StandardWindowButton()` から返された `NSButton` が収集されていました (これはバグです)。 そのイベントをクリックして呼び出そうとすると、ボタンがガベージ コレクションされている場合、クラッシュが発生します。

この問題の根本原因ではありませんが、このようなスタック トレースは、関数 `[Export]` で不適切なメソッド シグネチャが Objective-C にエクスポートされることで発生する可能性があります。 たとえば、メソッドが `out string` というパラメーターを想定している場合、`string` と入力すると、同じようにクラッシュする可能性があります。

## <a name="example-3-callbacks-and-managed-objects"></a>例 3:コールバックとマネージド オブジェクト

多くの Cocoa API では、何らかのイベントが発生して応答する機会が必要なときや、タスクの実行に何らかのデータが必要なときに、ライブラリによって "コールバック" が発生します。 主に **Delegate** と **DataSource** のパターンが考えられるかもしれませんが、このような動作の API は多数あります。 たとえば、`NSView` のメソッドをオーバーライドしてビジュアル ツリーに挿入すると、特定のイベントが発生したときに AppKit からコールバックされると考えられます。

ほとんどの場合、Xamarin.Mac は、これらのコールバックのマネージド オブジェクトがコールバックされている間にガベージ コレクションされないようにします。 ただし、まれではありますが、バインディングのバグでこの処理が妨害されることがあります。 この問題が発生すると、次のように不快なクラッシュが発生します。

```csharp
Thread 0 Crashed:: Dispatch queue: com.apple.main-thread

0   libsystem_kernel.dylib              0x98c2f69a __pthread_kill + 10
1   libsystem_pthread.dylib             0x90341f19 pthread_kill + 101
2   libsystem_c.dylib                   0x9453feee abort + 156
3   libmonosgen-2.0.dylib               0x020bfba5 mono_handle_native_sigsegv + 757
4   libmonosgen-2.0.dylib               0x0210b812 mono_arch_handle_altstack_exception + 162
5   libmonosgen-2.0.dylib               0x0200c55e mono_sigsegv_signal_handler + 446
6   libsystem_platform.dylib            0x9513003b _sigtramp + 43
7   ???                                 0xffffffff 0 + 4294967295
8   libmonosgen-2.0.dylib               0x0200c3a0 mono_sigill_signal_handler + 48
9   com.apple.AppKit                    0x99f76041 -[NSView setFrame:] + 448
10  com.apple.AppKit                    0x9a1fd4ea -[NSToolbarView adjustToWindow:attachedToEdge:] + 198
11  com.apple.AppKit                    0x9a1fd414 -[NSToolbar _adjustViewToWindow] + 68
12  com.apple.AppKit                    0x9a01eb0d -[NSToolbar _windowWillShowToolbar] + 79

```

このガイドでは、このようなバグが発生した場合に追跡する方法、バグを解決するために適切に報告する方法、解決するまでの間、コードでバグを回避する方法について説明します。

### <a name="locating"></a>特定

このようなバグが発生するほとんどの場合、主に発生する現象はネイティブ クラッシュです。通常、スタックの上部のフレームに `mono_sigsegv_signal_handler` や `_sigtrap` のようなものが表示されます。 Cocoa は C# コードに対してコールバックを試行し、ガベージ コレクションされたオブジェクトに当たり、クラッシュします。 ただし、このようなシンボルがあるすべてのクラッシュは、このようなバインディングの問題が原因で発生するわけではないので、それが問題であることを確認するために、追加の調査が必要です。

このようなバグの追跡が困難なのはガベージ コレクションによって問題のオブジェクトが破棄された**後**にのみ、このようなバグが発生するからです。 これらのバグのいずれかが発生したと思われる場合は、スタート アップ シーケンスのどこかに次のコードを追加します。

```csharp
new System.Threading.Thread (() =>
{
    while (true) {
         System.Threading.Thread.Sleep (1000);
         GC.Collect ();
    }
}).Start ();
```

これで、ガベージ コレクターが強制的に 1 秒間隔で実行されます。 アプリケーションを再実行し、バグを再現してみましょう。 すぐにクラッシュする場合、またはランダムにではなく常にクラッシュする場合は、手順どおりに進んでいます。

### <a name="reporting"></a>レポート

次の手順は、問題を Xamarin に報告して、今後のリリースでバインディングを修正できるようにします。 法人またはエンタープライズ ライセンスをお持ちの場合は、次の場所でチケットを開いてください。

[visualstudio.microsoft.com/vs/support/](https://visualstudio.microsoft.com/vs/support/)

それ以外の場合は、次の場所で既存の問題を検索してください。

- [Xamarin.Mac フォーラム](https://forums.xamarin.com/categories/xamarin-mac)を確認する
- [問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues)を検索する
- GitHub の問題に切り替わる前に、Xamarin の問題は [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) で追跡されていました。 そこで一致する問題を検索してください。
- 一致する問題が見つからない場合は、[GitHub の問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues/new)に新しい問題を提出してください。

GitHub の問題はすべて公開されています。 コメントまたは添付ファイルを非表示にすることはできません。

次の情報について、できるだけ多くを含めてください。

- 問題を再現する簡単な例。 これは**重要**です (可能な場合)。
- クラッシュの完全なスタック トレース。
- クラッシュの周囲の C# コード。   

### <a name="working-around"></a>回避策

問題を追跡した後、バインディングが修正されるまで回避策を適用して問題を修正する作業は簡単です。 目的は、開いている参照を維持することで、残っているメモリからオブジェクト (**View**、**Delegate**、**DataSource**) が破棄されないようにすることです。

オブジェクトのインスタンスが 1 つのみの単純な例では、コードを次のように変更します。

```csharp
void AddObject ()
{
    item.View = new MyView ();
    ...
}
```

次のように静的変数を使用します。

```csharp
static NSObject view;
...

void AddObject ()
{
    view = new MyView ();
    item.View = view;
    ...
}
```

複数のインスタンスが作成される場合は、静的 `HashSet` を使用できます。

```csharp
static HashSet<NSObject> collection = new HashSet<NSObject> ();
...

void AddObject ()
{
    item.View = new MyView ();
    collection.Add (item.View );
    ...
}
```

バインディングが修正され、修正を含む Xamarin.Mac のバージョンにアップグレードしたら、回避策のコードを削除できます。

## <a name="exception-bubbling-and-objective-c"></a>例外バブリングと Objective-C

呼び出し元の Objective-C メソッドにマネージド コードを "エスケープ" することを C# 例外に許可しないでください。 許可すると、結果は未定義ですが、一般的にはクラッシュします。 ユーザーが問題を迅速に解決できるように、一般的にネイティブ クラウドとマネージド クラッシュの両方に役立つ情報を表示するよう取り組んでいます。

すべてのマネージド/ネイティブ境界でマネージド例外をキャッチするためのインフラストラクチャを設定する場合、技術的な理由で行き詰まることがなく、それほど費用がかからず、多くの一般的な処理で発生する遷移が_多数_あります。 多くの処理、特に UI スレッドを含む処理は短時間で完了する必要があります。そうしないと、アプリが中断し、許容できないパフォーマンスの問題が発生します。 これらのコールバックの多くは、スローされる可能性がほとんどない非常に単純なものなので、このオーバーヘッドはコストが高く、このような場合には不要です。

そのため、try/catches を自動的に設定しません。 (ブール値や単純な計算を返すなどの処理よりも) 重要な処理をコーディングする場合は、手動でキャッチすることができます。
