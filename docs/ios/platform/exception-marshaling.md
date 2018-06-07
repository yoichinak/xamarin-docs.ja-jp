---
title: Xamarin.iOS でマーシャ リング例外
description: このドキュメントでは、Xamarin.iOS アプリでネイティブおよびマネージの例外を使用する方法について説明します。 これは、これらの問題のソリューションと発生する可能性がある問題について説明します。
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/05/2017
ms.openlocfilehash: dcf1074aacb6d139d107dac01fa86f459831d5f9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786744"
---
# <a name="exception-marshaling-in-xamarinios"></a>Xamarin.iOS でマーシャ リング例外

_Xamarin.iOS には、特にネイティブ コードでの例外に応答を支援する新しいイベントが含まれています。_

マネージ コードと OBJECTIVE-C の両方があるランタイムの例外 (catch または finally 句) をサポートします。

ただし、その実装は、さまざまなこと、ランタイム ライブラリ (モノのランタイムと Objective C ランタイム ライブラリ) に問題がある例外を処理し、その他の言語で記述されたコードを実行していることを意味します。

このドキュメントでは、発生する可能性がある問題と考えられる解決策について説明します。

サンプルのプロジェクトも含まれています。[例外がマーシャ リング](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)、さまざまなシナリオとその解決方法をテストに使用することができます。

## <a name="problem"></a>問題

問題は、例外がスローされ、スタック フレームをアンワインド中に発生がスローされた例外の種類と一致しないときに発生します。

Xamarin.iOS または Xamarin.Mac この典型的な例は、ネイティブ API、Objective C 例外がスローし、その Objective C 例外何らかの方法で処理する必要がマネージ フレームのスタック アンワインド プロセスに達する場合にです。

既定のアクションは、何ができます。 このサンプルでは、Objective C のランタイム マネージ アンワインド フレームをできるようにすることを意味します。 これは、Objective C のランタイムがマネージ フレームをアンワインドする方法を把握していないため、問題があります。たとえばいずれかが実行されません`catch`または`finally`そのフレーム内の句。

### <a name="broken-code"></a>壊れたコード

次のコード例を考えてみます。

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Objective C NSInvalidArgumentException ネイティブ コードでスローされます。

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

され、スタック トレースは次のようになります。

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

0 ~ 3 のフレームはネイティブ フレーム、および Objective C ランタイムでスタック アンワインダー_できます_フレームをアンワインドします。 具体的には、任意の OBJECTIVE-C は実行`@catch`または`@finally`句。

ただし、OBJECTIVE-C スタック アンワインダーは_いない_フレームは、アンワインドできませんが、マネージ例外のロジックは実行されませんを管理対象のフレーム数 (4 ~ 6 をフレーム) を正しくアンワインドの対応します。

つまり、一般的にはないことを次のようにこれらの例外をキャッチします。

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

これは、マネージ OBJECTIVE-C スタック アンワインダーが認識していないため`catch`句、およびどちらが、`finally`句が実行されます。

ときに上記のコード サンプル_は_効果的では Objective C 例外 Objective C の通知のメソッドがありますので[ `NSSetUncaughtExceptionHandler` ] [ 2]、Xamarin.iOS と Xamarin.Mac を使用して、その時点でしようとマネージ コードの例外をすべて Objective C の例外に変換します。

## <a name="scenarios"></a>シナリオ

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>シナリオ 1: 管理対象の catch ハンドラーと Objective C の例外のキャッチ

Objective C の例外をキャッチすることは次のシナリオでは、管理を使用して`catch`ハンドラー。

1. Objective C 例外がスローされます。
2. Objective C のランタイムはスタックを走査 (ただし、アンワインドはありません)、ネイティブを探して`@catch`例外を処理できるハンドラー。
3. Objective C のランタイムには、いずれかの検出されない`@catch`ハンドラーを呼び出して`NSGetUncaughtExceptionHandler`、Xamarin.iOS/Xamarin.Mac によってインストールされているハンドラーが呼び出されます。
4. Xamarin.iOS/Xamarin.Mac's ハンドラーは、Objective C の例外をマネージ例外に変換し、スローされます。 Objective C 以降 (それを処理) のみ、ランタイムは履歴をアンワインドしていない、現在のフレームは、同じ Objective C 例外がスローされました。

モノラル ランタイムが正しく Objective C のフレームをアンワインドする方法を把握していないために、別の問題はここでは、発生します。

Xamarin.iOS' キャッチされない Objective C 例外コールバックが呼び出されると、スタックは、次のようには。

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

ここでは、マネージ フレームだけは 8 ~ 10 のフレームが 0 のフレームにマネージ例外がスローされます。 これは、Mono ランタイム アンワインドする必要がネイティブ フレーム 0 ~ 7、上記で説明した問題と同じ問題が発生することを意味します任意 Objective C を実行しませんが、モノの実行時には、ネイティブ フレームをアンワインドは、`@catch`または`@finally`句.

コードの例:

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

および`@finally`についてこのフレームをアンワインド モノのランタイムはわからないために、句は実行されません。

これのバリエーションは、マネージ コードおよび取得するネイティブ フレームをアンワインドし、マネージ例外をスローする、最初に管理されている`catch`句。

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

マネージ`UIApplication:Main`メソッドの呼び出しは、ネイティブ`UIApplicationMain`メソッド、および、iOS は、最終的に、マネージを呼び出す前にネイティブ コードの実行の多くを実行、`AppDelegate:FinishedLaunching`ネイティブとマネージの例外は、スタック フレームの多くがあっても、メソッドスローされます。

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

0 ~ 1 のフレームと 27-30 は管理、すべての人との間ではネイティブです。 モノラルがない Objective C のこれらのフレームをアンワインドかどうか`@catch`または`@finally`句が実行されます。

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>シナリオ 2: Objective C の例外をキャッチすることができません。

次のシナリオでは_いない_Objective C の例外をキャッチすることを使用して管理されている`catch`ハンドラー Objective C 例外は、別の方法で処理されたため。

1. Objective C 例外がスローされます。
2. Objective C のランタイムはスタックを走査 (ただし、アンワインドはありません)、ネイティブを探して`@catch`例外を処理できるハンドラー。
3. Objective C のランタイムを検索、`@catch`ハンドラーがスタックをアンワインドし、実行を開始、`@catch`ハンドラー。

このシナリオは、メイン スレッドでないためこのようなコード Xamarin.iOS アプリによく見られます。

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

つまり、メイン スレッドではありません本当に未処理 Objective C の例外を Objective C の例外をマネージ コードの例外に変換する、コールバックが呼び出されないためです。

これは非常に一般的にもデバッガーでのほとんどの UI オブジェクトを調べることが試行で、実行されているプラットフォーム (存在しないセレクターに対応するプロパティを取得するので Xamarin.Mac をサポートしているよりも以前の macOS バージョンで Xamarin.Mac アプリのデバッグXamarin.Mac 含まれているためより高い macOS バージョンのサポート)。 このようなセレクターを呼び出すことがスローされます、 `NSInvalidArgumentException` (「認識されないセレクターに送信しています...」) が最終的に、プロセスをクラッシュが発生します。

要約するを持つ Objective C ランタイムかモノラル ランタイム アンワインド フレームがないようにプログラミングされているのか、ハンドルは、クラッシュ、メモリ リークおよびその他の種類の (誤) の予期しない動作など、未定義の動作につながることができます。

## <a name="solution"></a>ソリューション

Xamarin.iOS 10 および Xamarin.Mac 2.10 は、任意のマネージ コードとネイティブの境界にマネージ コードと Objective C の例外をキャッチし、その例外を他の型に変換するためのサポートが追加されました。

擬似コードで次のようになります。

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Objc_msgSend に P/invoke をインターセプトすると、し、これは、代わりに呼び出されます。

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

(Objective C の例外へのマネージ例外をマーシャ リング) 逆の場合は、以下のように実行されます。

マネージ コードとネイティブの境界で例外をキャッチするコストを必要としない、そのため、常にではありません既定で有効にします。

- Xamarin.iOS/tvOS: Objective C の例外のインターセプションは、シミュレーターでは有効です。
- Xamarin.watchOS: インターセプトはすべてのケースで適用されます、フレームが、ガベージ コレクターを混乱させるため、OBJECTIVE-C ランタイム アンワインドが管理できるようにすることと、いずれかのハングまたはクラッシュなります。
- Xamarin.Mac: Objective C の例外のインターセプションは、デバッグ ビルドの場合に有効です。

[ビルド時フラグ](#build_time_flags)で、既定で有効でない場合は、途中受信を有効にする方法について説明します。

## <a name="events"></a>イベント

例外をインターセプト後に発生する 2 つの新しいイベントがある:`Runtime.MarshalManagedException`と`Runtime.MarshalObjectiveCException`です。

両方のイベントが渡される、`EventArgs`スローされた元の例外を格納しているオブジェクト (、`Exception`プロパティ)、および`ExceptionMode`例外をマーシャ リングする方法を定義するプロパティです。

`ExceptionMode`プロパティを変更できるイベントのハンドラーで実行するすべてのカスタム処理に従って動作を変更するハンドラー。 1 つの例は、特定の例外が発生した場合、このプロセスを中止することです。

変更、`ExceptionMode`プロパティ 1 つのイベントに適用されます、すべての例外をインターセプト、将来には影響しません。

次のモードを使用できます。

- `Default`: 既定値は、プラットフォームによって異なります。 `ThrowObjectiveCException` GC が協調モード (watchOS) の場合と`UnwindNativeCode`それ以外の場合 (iOS/watchOS/macOS)。 既定値は後で変更できます。
- `UnwindNativeCode`: これは、(未定義) 以前の動作です。 GC を協調動作モードを使用する場合は使用できません (watchOS で唯一のオプションは; そのため、これは watchOS で有効なオプション) が、その他のすべてのプラットフォームの既定のオプションです。
- `ThrowObjectiveCException`: Objective C 例外にマネージ例外を変換し、Objective C の例外をスローします。 これは、watchOS での既定値です。
- `Abort`: このプロセスを中止します。
- `Disable`: なさないイベント ハンドラーでこの値を設定するが、イベントの発生後に遅すぎますで無効にするための例外の途中受信、無効にします。 いずれの場合はかどうか、設定すると、その動作として`UnwindNativeCode`です。

Objective C の例外をマネージ コードにマーシャ リングでは、次のモードを使用できます。

- `Default`: 既定値は、プラットフォームによって異なります。 `ThrowManagedException` GC が協調モード (watchOS) の場合と`UnwindManagedCode`それ以外の場合 (iOS/tvOS/macOS)。 既定値は後で変更できます。
- `UnwindManagedCode`: これは、(未定義) 以前の動作です。 GC を協調動作モードを使用する場合は使用できません (watchOS で唯一の有効な GC モードは; したがってこれは watchOS で有効なオプション) が、その他のすべてのプラットフォームの既定値です。
- `ThrowManagedException`: マネージ例外を Objective C の例外に変換し、マネージ例外をスローします。 これは、watchOS での既定値です。
- `Abort`: このプロセスを中止します。
- `Disable`: 例外の途中受信を無効になります、なさないを設定するため、ハンドラーが、1 回、イベントが発生した場合にこの値で無効にするには遅すぎます。 いずれの場合もかどうか設定すると、そのプロセスを中断します。

したがって、確認には、例外がマーシャ リングされるたびに、これを行うことができます。

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

次のオプションを渡すことは**mtouch** (Xamarin.iOS アプリ) のおよび**mmp** (Xamarin.Mac アプリの場合) が、決定されます例外インターセプションが有効になっている場合、既定のアクションを設定発生する必要があります。

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

除く`disable`、これらの値と同じ、`ExceptionMode`に渡される値、`MarshalManagedException`と`MarshalObjectiveCException`イベント。

`disable`オプションが されます_ほとんど_実行オーバーヘッドを追加することはないときに例外をインターセプトをまだお点を除いて、途中受信を無効にします。 マーシャ リングのイベントは、実行されているプラットフォームの既定のモードをされている既定のモードで、これらの例外のも発生します。

## <a name="limitations"></a>制限事項

のみにするには、P/invoke をインターセプトお、 `objc_msgSend` Objective C の例外をキャッチしようとしているときに、関数のファミリです。 これはすべて Objective C の例外をスローし、別の C 関数に P/invoke は引き続き (これは向上しますが、将来)、古いマスター_キーと未定義の動作を実行することを意味します。

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>関連リンク

- [例外のマーシャ リング (サンプル)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
