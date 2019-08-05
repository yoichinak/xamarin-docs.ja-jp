---
title: Xamarin.iOS における例外のマーシャ リング
description: このドキュメントでは、Xamarin.iOS アプリでネイティブおよびマネージ例外を使用する方法について説明します。 これは、発生する可能性がある問題とこれらの問題の解決方法について説明します。
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/05/2017
ms.openlocfilehash: 167d6ac421bdd2652e7f8474e1ea21bd9040723f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075096"
---
# <a name="exception-marshaling-in-xamarinios"></a>Xamarin.iOS における例外のマーシャ リング

_Xamarin.iOS には、特にネイティブ コードで、例外に応答するのに役立つ新しいイベントが含まれています。_

マネージ コードと Objective-C の両方でランタイム例外 (try、catch、finally 句) をサポートします。

ただし、その実装は、さまざまな例外を処理し、その他の言語で記述されたコードを実行する必要があるときに、ランタイム ライブラリ (Mono ランタイムと Objective C ランタイム ライブラリ) が問題があること。

このドキュメントでは、発生する可能性がある問題と考えられる解決策について説明します。

サンプル プロジェクトも含まれています。[例外のマーシャ リング](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)、これは、さまざまなシナリオとその解決策のテストを使用できます。

## <a name="problem"></a>問題

問題は、例外がスローされ、スタック フレームのアンワインド中に発生したがスローされた例外の種類と一致しないときに発生します。

Xamarin.iOS、Xamarin.Mac これの典型的な例は、ネイティブ API、Objective C 例外がスローし、その Objective C 例外何らかの方法で処理する必要がマネージ フレームのスタック アンワインド プロセスに達する場合にです。

既定のアクションでは、何もしません。 このサンプルでは、OBJECTIVE-C ランタイム アンワインドのマネージ フレームできるようにすることを意味します。 Objective C のランタイムがマネージ フレームをアンワインドする方法を知らないので、これは問題のある、たとえばが実行されません`catch`または`finally`そのフレーム内の句。

### <a name="broken-code"></a>壊れたコード

次のコード例を検討してください。

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

これにより、ネイティブ コードで、Objective C NSInvalidArgumentException がスローされます。

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

スタック トレースには、次のようになります。

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

0 ~ 3 のフレームはネイティブ フレーム、および、OBJECTIVE-C ランタイムでスタック アンワインダー_できます_それらのフレームをアンワインドします。 具体的には、任意の OBJECTIVE-C では実行`@catch`または`@finally`句。

ただし、Objective C のスタック アンワインダーは_いない_フレームは、アンワインドできませんが、マネージ例外のロジックは実行されません (フレーム 4 ~ 6) の管理対象のフレームを正しくアンワインドの対応。

つまりことは通常、次の方法でこれらの例外をキャッチします。

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

Objective C のスタック アンワインダーが、管理対象について知らないので、これは`catch`句、およびどちらが、`finally`句が実行されます。

ときに上記のコード サンプル_は_効果的では Objective C 例外 Objective C の通知のメソッドがありますので[`NSSetUncaughtExceptionHandler`][2]、Xamarin.iOS と Xamarin.Mac を使用して、その時点でしようとマネージ コードの例外をすべて Objective C の例外に変換します。

## <a name="scenarios"></a>シナリオ

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>シナリオ 1 - マネージ catch ハンドラーと OBJECTIVE-C 例外のキャッチ

Objective C の例外をキャッチすることは次のシナリオでは、管理を使用して`catch`ハンドラー。

1. Objective C 例外がスローされます。
2. Objective C のランタイムは、スタックを走査 (ただし、アンワインドはありません)、ネイティブ探して`@catch`例外を処理できるハンドラー。
3. Objective C のランタイムはいずれかで検出されない`@catch`ハンドラー、呼び出し`NSGetUncaughtExceptionHandler`、Xamarin.iOS/Xamarin.Mac によってインストールされているハンドラーを呼び出します。
4. Xamarin.iOS/Xamarin.Mac's ハンドラーは、Objective C 例外にマネージ例外を変換し、それをスローします。 Objective C 以降 (これを説明しました) のみ、ランタイムは、履歴をアンワインドしていない、現在のフレームは、同じ Objective C 例外がスローされました。

別の問題は、Mono ランタイムが正しく Objective C のフレームをアンワインドする方法を知らないので、ここでは、発生します。

Xamarin.iOS のキャッチされない Objective C 例外コールバックが呼び出されると、スタックは、次のようには。

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

ここでは、唯一のマネージ フレームは 8 ~ 10 のフレームが 0 のフレームでマネージ例外がスローされます。 これは、Mono ランタイム アンワインドする必要がある 0 ~ 7 のネイティブ フレーム上で説明した問題と同じ問題が発生することを意味します Objective C を実行しませんが、Mono ランタイムは、ネイティブ フレームにアンワインドが、`@catch`または`@finally`句.

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

および`@finally`このフレームをアンワインドする Mono ランタイムがそれについて知らないので、句は実行されません。

このバリエーションは、マネージ コードおよび取得するネイティブ フレームをアンワインドし、マネージ例外をスローする、最初に管理されている`catch`句。

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

マネージ`UIApplication:Main`メソッドの呼び出しは、ネイティブ`UIApplicationMain`メソッドをおよび、iOS に最終的に、管理対象を呼び出す前に、ネイティブ コード実行の多くを行う`AppDelegate:FinishedLaunching`メソッドは、多くのマネージ例外がスタック上のネイティブ フレームがスローされます。

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

0 ~ 1 のフレームと 27 ~ 30 管理されます、それらの間ではネイティブ。 Mono は Objective C、これらのフレームをアンワインドかどうか`@catch`または`@finally`句が実行されます。

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>シナリオ 2 - Objective C の例外をキャッチすることはできません。

次のシナリオでは_いない_Objective C の例外をキャッチすることを使用してマネージ`catch`ハンドラー Objective C 例外が別の方法で処理されるので。

1. Objective C 例外がスローされます。
2. Objective C のランタイムは、スタックを走査 (ただし、アンワインドはありません)、ネイティブ探して`@catch`例外を処理できるハンドラー。
3. Objective C のランタイムを検索、`@catch`ハンドラーは、スタックのアンワインドし、実行を開始、`@catch`ハンドラー。

このシナリオは、メイン スレッドでは通常、このようなコードがあるため、Xamarin.iOS アプリによく見られます。

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

つまり、メイン スレッドで例外が発生しない本当にハンドルされない Objective C、および、Objective C の例外をマネージ例外に変換する、コールバックが呼び出されないためです。

これは非常に一般的にも、以前の macOS バージョンで実行中のプラットフォーム (存在しないセレクターに対応するプロパティをフェッチしようと試みるはデバッガーでのほとんどの UI オブジェクトを検査するため、Xamarin.Mac がサポートする以上の Xamarin.Mac アプリのデバッグXamarin.Mac が含まれるため高い macOS バージョンのサポート)。 このようなセレクターを呼び出すことがスローされます、 `NSInvalidArgumentException` (「認識されないセレクターに送信...」)、最終的にクラッシュするプロセスを停止します。

Objective C のランタイムまたは Mono ランタイム アンワインド フレームがないようにプログラミングされてをすることをまとめると、ハンドルは、クラッシュ、メモリ リーク、およびその他の種類の (mis) 予期しない動作など、未定義の動作につながることができます。

## <a name="solution"></a>ソリューション

Xamarin.iOS 10 と Xamarin.Mac 2.10 では、任意のマネージ コードとネイティブ境界でマネージ コードと Objective C の例外をキャッチし、その例外を他の型に変換するためのサポートが追加されました。

擬似コードは、次のように見えます。

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Objc_msgSend に P/invoke をインターセプトすると、し、これは代わりに呼び出されます。

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

逆のケース (Objective C の例外をマネージ例外をマーシャ リング) のような画面が実行されます。

マネージ コードとネイティブの境界での例外のキャッチはコストのないため、常にではありません既定で有効です。

- Xamarin.iOS/tvOS: OBJECTIVE-C 例外のインターセプションが有効なは、シミュレーターでします。
- Xamarin.watchOS: インターセプションが常に適用するには、フレームが、ガベージ コレクターを混同ため、OBJECTIVE-C ランタイム アンワインドを管理できるようにすることおよびハングまたはクラッシュするいずれか。
- Xamarin.Mac: デバッグ ビルドの場合、Objective C 例外のインターセプションが有効です。

[ビルド時フラグ](#build_time_flags)セクションが既定では無効な場合は、傍受を有効にする方法について説明します。

## <a name="events"></a>イベント

例外をインターセプトすると発生する 2 つの新しいイベントがある:`Runtime.MarshalManagedException`と`Runtime.MarshalObjectiveCException`します。

両方のイベントが渡される、`EventArgs`スローされた元の例外を格納しているオブジェクト (、`Exception`プロパティ)、および`ExceptionMode`例外をマーシャ リングする方法を定義するプロパティ。

`ExceptionMode`プロパティを変更できる、イベント ハンドラーでカスタム処理に従って動作を変更するハンドラー。 1 つの例は、特定の例外が発生した場合、プロセスを中止することです。

変更、`ExceptionMode`プロパティ 1 つのイベントに適用される、すべての例外をインターセプト、将来には影響しません。

次のモードを使用できます。

- `Default`:既定値は、プラットフォームによって異なります。 `ThrowObjectiveCException` 、GC が協調モード (watchOS) の場合と`UnwindNativeCode`それ以外の場合 (iOS/watchOS/macOS)。 既定値は、今後変更可能性があります。
- `UnwindNativeCode`:これは、(未定義) 以前の動作です。 GC を協調モードを使用する場合は使用できません (watchOS で唯一のオプションは、そのため、これは watchOS で有効なオプション) が、その他のすべてのプラットフォームの既定のオプション。
- `ThrowObjectiveCException`:Objective C 例外にマネージ例外を変換し、Objective C の例外をスローします。 これは、watchOS の既定値です。
- `Abort`:プロセスを中止します。
- `Disable`:イベント ハンドラーでこの値を設定する意味をなさないが無効にする遅すぎますが、イベントが発生すると、例外のインターセプトを無効にします。 いずれの場合もかどうか、設定の動作として`UnwindNativeCode`します。

マネージ コードへの Objective C の例外をマーシャ リングには、次のモードを使用できます。

- `Default`:既定値は、プラットフォームによって異なります。 `ThrowManagedException` 、GC が協調モード (watchOS) である場合と`UnwindManagedCode`それ以外の場合 (iOS と tvOS/macOS)。 既定値は、今後変更可能性があります。
- `UnwindManagedCode`:これは、(未定義) 以前の動作です。 GC を協調モードを使用する場合は使用できません (watchOS で唯一の有効な GC モードは、watchOS で有効なオプションでないこのため)、その他のすべてのプラットフォームの既定値は。
- `ThrowManagedException`:Objective C の例外をマネージ例外に変換し、マネージ例外をスローします。 これは、watchOS の既定値です。
- `Abort`:プロセスを中止します。
- `Disable`: ハンドラーが 1 回、イベントが発生したイベントには、この値を設定する意味をなさないためは、無効にするには遅すぎるを例外の傍受を無効にします。 いずれの場合もかどうか、そのプロセスを中止します。

したがって、については、例外をマーシャ リングするたびに、これを行うことができます。

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

## <a name="build-time-flags"></a>ビルド時フラグ

次のオプションを渡すことは**mtouch** (Xamarin.iOS アプリ) 用と**mmp** (の Xamarin.Mac アプリの場合) には、例外インターセプションが有効になっているかを判断し、既定のアクションを設定するは発生する必要があります。

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

除く`disable`、これらの値と同じですが、`ExceptionMode`に渡される値、`MarshalManagedException`と`MarshalObjectiveCException`イベント。

`disable`オプションは_ほとんど_点を除いて実行のオーバーヘッドも追加しない場合も例外をインターセプトしましたが、傍受を無効にします。 マーシャ リング イベントは、実行されているプラットフォームの既定のモードをされている既定のモードでのこれらの例外も発生します。

## <a name="limitations"></a>制限事項

のみにするには、P/invoke をインターセプトしました、 `objc_msgSend` Objective C の例外をキャッチしようとしたときに、関数のファミリです。 これは、OBJECTIVE-C 例外をスローし、別の C 関数に P/invoke を (これは向上しますが、今後) 古いと未定義の動作を実行してもすることを意味します。

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>関連リンク

- [例外のマーシャ リング (サンプル)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
