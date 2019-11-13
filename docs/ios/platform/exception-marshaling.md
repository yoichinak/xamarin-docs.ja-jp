---
title: Xamarin での例外のマーシャリング
description: このドキュメントでは、Xamarin iOS アプリでネイティブ例外とマネージ例外を使用する方法について説明します。 発生する可能性がある問題と、これらの問題の解決策について説明します。
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/05/2017
ms.openlocfilehash: ca3d4dfcd773a4f236ffbfd715cb53b514f6e2a3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032520"
---
# <a name="exception-marshaling-in-xamarinios"></a>Xamarin での例外のマーシャリング

_Xamarin. iOS には、例外 (特にネイティブコード) に応答するための新しいイベントが含まれています。_

マネージコードと Objective-C の両方で、ランタイム例外 (try/catch/finally 句) がサポートされています。

ただし、これらの実装は異なります。つまり、ランタイムライブラリ (Mono ランタイムライブラリと目的 C ランタイムライブラリ) では、例外を処理し、他の言語で記述されたコードを実行する必要がある場合に問題が発生します。

このドキュメントでは、発生する可能性がある問題と、考えられる解決策について説明します。

また、サンプルプロジェクトと例外の[マーシャリング](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)も含まれており、さまざまなシナリオとそのソリューションをテストするために使用できます。

## <a name="problem"></a>問題

この問題は、例外がスローされ、スタックアンワインド中に、スローされた例外の種類と一致しないフレームが検出されたときに発生します。

このサンプルの典型的な例として、Xamarin. iOS または Xamarin. Mac があります。これは、ネイティブ API が目的 C の例外をスローした後、スタックアンワインドプロセスがマネージフレームに達したときに、その目的の C の例外をなんらかの方法で処理する必要がある場合です。

既定のアクションでは、何も実行されません。 上記のサンプルでは、これは、目標 C ランタイムアンワインドマネージフレームを可能にすることを意味します。 これは、目的の C ランタイムがマネージフレームをアンワインドする方法を認識しないため、問題になります。たとえば、そのフレーム内の `catch` 句や `finally` 句は実行されません。

### <a name="broken-code"></a>破損したコード

次のコード例について考えてみます。

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

これにより、ネイティブコードで目的の C NSInvalidArgumentException がスローされます。

```
NSInvalidArgumentException *** setObjectForKey: key cannot be nil
```

スタックトレースは次のようになります。

```
0   CoreFoundation          __exceptionPreprocess + 194
1   libobjc.A.dylib         objc_exception_throw + 52
2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
3   libobjc.A.dylib         objc_msgSend + 102
4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()
```

フレーム0-3 はネイティブフレームであり、アンワインダーランタイムの stack のスタックは、これらのフレームをアンワインド_することができ_ます。 具体的には、Objective-C の `@catch` または `@finally` 句を実行します。

ただし、Objective-C スタックアンワインダーは、フレームがアンワインドされるのに、マネージフレーム (フレーム 4-6) を適切にアンワインドすることはでき_ません_が、マネージ例外ロジックは実行されません。

つまり、通常、次のような例外をキャッチすることはできません。

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

これは、アンワインダー stack がマネージ `catch` 句を認識しておらず、どちらの `finally` 句も実行されないためです。

上記のコードサンプル_が有効な_場合、これは、処理されていない目的 c の例外 ( [`NSSetUncaughtExceptionHandler`][2]、Xamarin. IOS と xamarin. Mac で使用) が通知され、その時点ですべての目標を変換しようとしていることが原因です。マネージ例外の例外。

## <a name="scenarios"></a>監視プロセス

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>シナリオ 1-マネージド catch ハンドラーを使用して目的 C の例外をキャッチする

次のシナリオでは、マネージ `catch` ハンドラーを使用して、C の例外をキャッチすることができます。

1. Objective-C の例外がスローされます。
2. Objective-C ランタイムはスタックにステップインし (ただし、アンワインドしません)、例外を処理できるネイティブ `@catch` ハンドラーを探します。
3. この目的では、C ランタイムは `@catch` ハンドラーを検出せず `NSGetUncaughtExceptionHandler`を呼び出し、Xamarin. iOS/Xamarin. Mac によってインストールされたハンドラーを呼び出します。
4. Xamarin. iOS/Xamarin. Mac ハンドラーは、目的の C 例外をマネージ例外に変換し、スローします。 目的の C ランタイムではスタックがアンワインドされていないので (ウォークのみ)、現在のフレームは、目的の C 例外がスローされたものと同じです。

ここで別の問題が発生します。これは、Mono ランタイムが、目的の C フレームを適切にアンワインドする方法を認識しないためです。

Xamarin. iOS の "キャッチされていない目標-C" 例外のコールバックが呼び出されると、スタックは次のようになります。

```
 0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
 1 CoreFoundation           __handleUncaughtException + 809
 2 libobjc.A.dylib          _objc_terminate() + 100
 3 libc++abi.dylib          std::__terminate(void (*)()) + 14
 4 libc++abi.dylib          __cxa_throw + 122
 5 libobjc.A.dylib          objc_exception_throw + 337
 6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
 7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
 8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
 9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]
```

ここでは、唯一のマネージフレームはフレーム8-10 ですが、マネージ例外はフレーム0でスローされます。 これは、Mono ランタイムがネイティブフレーム0-7 をアンワインドする必要があることを意味します。これにより、前に説明した問題と同等の問題が発生します。ただし、Mono ランタイムはネイティブフレームをアンワインドしますが、Objective-C の `@catch` や `@finally` 句は実行されません。

コード例:

```objc
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

また、このフレームをアンワインドする Mono ランタイムで認識されていないため、`@finally` 句は実行されません。

このようなバリエーションとして、マネージコードでマネージ例外をスローした後、ネイティブフレームをアンワインドして最初のマネージ `catch` 句を取得します。

```csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

マネージ `UIApplication:Main` メソッドは、ネイティブの `UIApplicationMain` メソッドを呼び出します。その後、管理対象の例外がスローされたときにスタック上に多数のネイティブフレームを使用して、最終的にマネージ `AppDelegate:FinishedLaunching` メソッドを呼び出す前に、iOS はネイティブコードを多数実行します。:

```
 0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
 1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
 2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
 5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
 6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
 7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
 8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
 9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
17: FrontBoardServices      -[FBSSerialQueue _performNext]
18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
21: CoreFoundation          __CFRunLoopDoSources0
22: CoreFoundation          __CFRunLoopRun
23: CoreFoundation          CFRunLoopRunSpecific
24: CoreFoundation          CFRunLoopRunInMode
25: UIKit                   -[UIApplication _run]
26: UIKit                   UIApplicationMain
27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])
```

フレーム0-1 と27-30 は管理されていますが、間にあるものはすべてネイティブです。 これらのフレームによって Mono がアンワインドされた場合、Objective-C の `@catch` または `@finally` の句は実行されません。

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>シナリオ 2-目的 C の例外をキャッチできない

次のシナリオでは、Objective-C の例外が別の方法で処理されたため、マネージ `catch` ハンドラーを使用して、Objective-C の例外をキャッチすることはでき_ません_。

1. Objective-C の例外がスローされます。
2. Objective-C ランタイムはスタックにステップインし (ただし、アンワインドしません)、例外を処理できるネイティブ `@catch` ハンドラーを探します。
3. Objective-C ランタイムは、`@catch` ハンドラーを検索し、スタックをアンワインドして、`@catch` ハンドラーの実行を開始します。

このシナリオは通常、Xamarin iOS アプリで見られます。メインスレッドでは、通常、次のようなコードがあります。

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

つまり、メインスレッドでは、実際にはハンドルされない目的 C の例外が発生することはないため、このコールバックでは、C の例外をマネージ例外に変換することはできません。

これは、Xamarin より前の macOS バージョンで Xamarin.Mac アプリをデバッグする場合にも非常に一般的です。 Mac でサポートされているのは、デバッガー内のほとんどの UI オブジェクトを検査すると、実行中のプラットフォームに存在しないセレクターに対応するプロパティを取得しようとするためです (Xamarin.Mac では、より高い macOS バージョンがサポートされています)。 このようなセレクターを呼び出すと、`NSInvalidArgumentException` ("認識されていないセレクターがに送信されました") がスローされ、最終的にプロセスがクラッシュします。

まとめると、Objective-C ランタイムまたは処理用にプログラミングされていない Mono ランタイムアンワインドフレームを持つと、クラッシュ、メモリリーク、その他の種類の予測できない (mis) 動作などの未定義の動作が発生する可能性があります。

## <a name="solution"></a>解決策:

Xamarin. iOS 10 と Xamarin.Mac 2.10 では、マネージネイティブ境界でマネージ例外と目的 C 例外の両方をキャッチし、その例外を他の型に変換するためのサポートが追加されました。

擬似コードでは、次のようになります。

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Objc_msgSend への P/Invoke がインターセプトされ、代わりにが呼び出されます。

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

また、逆の場合にも同様の処理が行われます (マネージ例外を目的の C 例外にマーシャリングします)。

マネージネイティブの境界で例外をキャッチすることはコストがかかりません。そのため、既定では有効になっていません。

- TvOS: シミュレーターでは、Objective-C の例外のインターセプトが有効になっています。
- WatchOS: インターセプトはすべての場合に適用されます。これは、C ランタイムのアンワインドマネージフレームによってガベージコレクターが混乱し、ハングまたはクラッシュするようになるためです。
- Xamarin. Mac: デバッグビルドでは、Objective-C の例外のインターセプトが有効になっています。

[ビルド時のフラグ](#build_time_flags)セクションでは、既定で有効になっていない場合に、傍受を有効にする方法について説明します。

## <a name="events"></a>イベント

例外がインターセプトされると、`Runtime.MarshalManagedException` と `Runtime.MarshalObjectiveCException`の2つの新しいイベントが発生します。

両方のイベントには、スローされた元の例外 (`Exception` プロパティ) を含む `EventArgs` オブジェクトと、例外のマーシャリング方法を定義するための `ExceptionMode` プロパティが渡されます。

イベントハンドラーで `ExceptionMode` プロパティを変更して、ハンドラーで実行されるカスタム処理に従って動作を変更できます。 1つの例として、特定の例外が発生した場合にプロセスを中止することがあります。

`ExceptionMode` プロパティを変更しても、1つのイベントに適用されます。これは、今後受け取る例外には影響しません。

次のモードを使用できます。

- `Default`: 既定値はプラットフォームによって異なります。 GC が協調モード (watchOS) の場合は `ThrowObjectiveCException`、それ以外の場合は `UnwindNativeCode` (iOS/watchOS/macOS)。 既定値は将来変更される可能性があります。
- `UnwindNativeCode`: これは前 (未定義) の動作です。 これは、GC を協調モードで使用する場合は使用できません (これは watchOS の唯一のオプションであるため、watchOS では有効なオプションではありません) が、他のすべてのプラットフォームの既定のオプションです。
- `ThrowObjectiveCException`: マネージ例外を Objective-C 例外に変換し、Objective-C 例外をスローします。 これは、watchOS の既定値です。
- `Abort`: プロセスを中止します。
- `Disable`: 例外のインターセプトを無効にするため、イベントハンドラーにこの値を設定するのは理にかなっていませんが、イベントが発生した後で無効にするのは遅すぎます。 どのような場合でも、設定した場合は `UnwindNativeCode`として動作します。

Objective-C の例外をマネージコードにマーシャリングする場合、次のモードを使用できます。

- `Default`: 既定値はプラットフォームによって異なります。 GC が協調モード (watchOS) の場合は `ThrowManagedException`、それ以外の場合は `UnwindManagedCode` (iOS/tvOS/macOS)。 既定値は将来変更される可能性があります。
- `UnwindManagedCode`: これは前 (未定義) の動作です。 このオプションは、GC を協調モードで使用している場合は使用できません (これは watchOS の有効な GC モードのみであるため、watchOS の有効な GC モードではありません) が、他のすべてのプラットフォームの既定値です。
- `ThrowManagedException`: 目的の C 例外をマネージ例外に変換し、マネージ例外をスローします。 これは、watchOS の既定値です。
- `Abort`: プロセスを中止します。
- `Disable`:D によって例外のインターセプトが行われます。そのため、イベントハンドラーでこの値を設定するのは理にかなっていませんが、イベントが発生した後で無効にするのは遅すぎます。 どのような場合でも、設定するとプロセスが中止されます。

したがって、例外がマーシャリングされるたびに、次の操作を行うことができます。

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags" />

## <a name="build-time-flags"></a>ビルド時のフラグ

次のオプションを**mtouch**に渡すことができます (Xamarin アプリの場合)。また、 **mmp**アプリの場合は、例外のインターセプトが有効になっているかどうかを判断し、既定のアクションを設定します。

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

`disable`を除き、これらの値は `MarshalManagedException` および `MarshalObjectiveCException` イベントに渡される `ExceptionMode` の値と同じです。

`disable` オプションでは、_ほとんど_の場合、インターセプトが無効になります。ただし、実行のオーバーヘッドが追加されない場合でも例外を受け取ることができます。 これらの例外に対してマーシャリングイベントが発生しても、既定のモードは実行中のプラットフォームの既定のモードになります。

## <a name="limitations"></a>制限事項

目標 C 例外をキャッチしようとすると、`objc_msgSend` ファミリの関数に対してのみ P/Invoke がインターセプトされます。 これは、別の C 関数への P/Invoke が、その後、Objective-C の例外をスローすることを意味しますが、以前の動作と未定義の動作が実行されます (これは将来改善される可能性があります)。

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc

## <a name="related-links"></a>関連リンク

- [例外のマーシャリング (サンプル)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
