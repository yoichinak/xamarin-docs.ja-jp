---
title: ビルドの最適化
description: このドキュメントでは、Xamarin.iOS および Xamarin.Mac アプリのビルド時に適用されるさまざまな最適化について説明します。
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
dateupdated: 04/16/2018
ms.openlocfilehash: 42d9903e75267a9578fb320b0bc532ad4f0fd71a
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="build-optimizations"></a>ビルドの最適化

このドキュメントでは、Xamarin.iOS および Xamarin.Mac アプリのビルド時に適用されるさまざまな最適化について説明します。

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>UIApplication.EnsureUIThread の削除/NSApplication.EnsureUIThread

呼び出しを削除[UIApplication.EnsureUIThread] [ 1] (用 Xamarin.iOS) または`NSApplication.EnsureUIThread`(Xamarin.Mac) 用です。

この最適化は、次のようなコードに変更されます。

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

次に

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

この最適化が有効にするリンカーが必要ですを持つメソッドにだけ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

既定ではリリースを有効になっている次のように構築します。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]remove-uithread-checks`mtouch/mmp にします。

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>インライン IntPtr.Size

定数値のインラインの`IntPtr.Size`ターゲット プラットフォームに応じて。

この最適化は、次のようなコードに変更されます。

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

(ときに、64 ビット プラットフォーム用にビルド)。

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

この最適化が有効にするリンカーが必要ですを持つメソッドにだけ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

既定では、1 つのアーキテクチャを対象とする場合に有効になっているまたはプラットフォーム アセンブリ (**Xamarin.iOS.dll**、 **Xamarin.TVOS.dll**、 **Xamarin.WatchOS.dll**または**Xamarin.Mac.dll**)。

この最適化が 32 ビット バージョンと、アプリの 64 ビット バージョンの異なるアセンブリを作成する複数のアーキテクチャを対象にする場合と、どちらのバージョンが効果的に減少最終的なアプリのサイズを増やすことをアプリに含めるにはです。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]inline-intptr-size`mtouch/mmp にします。

## <a name="inline-nsobjectisdirectbinding"></a>インライン NSObject.IsDirectBinding

`NSObject.IsDirectBinding` かどうかを特定のインスタンスのラッパー型かインスタンス プロパティには (ネイティブ型にマップされるマネージ型は、以外のマネージ インスタンスのラッパー型`UIKit.UIView`ネイティブにマップ`UIView`- 逆は、ユーザーの種類、ここでは`class MyUIView : UIKit.UIView`ユーザーの種類になります)。

値を調べる必要がある`IsDirectBinding`値のバージョンと判断されるため、OBJECTIVE-C を呼び出すときに`objc_msgSend`を使用します。

次のコードのみを指定します。

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

あると判定できるお`UIView.SomeProperty`の値`IsDirectBinding`定数ではありませんし、インライン展開することはできません。

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

ただし、アプリで、すべての種類を確認し、継承する型がないことを確認することは`NSUrl`であり、したがってインラインに安全な`IsDirectBinding`を定数値`true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

具体的には、この最適化は、次の型を変更コード (これは、バインディングのコードを`NSUrl.AbsoluteUrl`)。

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

次に (ときに決定できるのサブクラスがない`NSUrl`アプリで)。

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

この最適化が有効にするリンカーが必要ですを持つメソッドにだけ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

常に Xamarin.iOS、既定で有効になって、Xamarin.Mac について、常に既定で無効 (Xamarin.Mac でアセンブリを動的に読み込むことなのでことはできません、特定のクラスがサブクラス化しないことを決定する)。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]inline-isdirectbinding`mtouch/mmp にします。

## <a name="inline-runtimearch"></a>インライン Runtime.Arch

この最適化は、次のようなコードに変更されます。

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

(デバイスの作成) するときに、次に

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

この最適化が有効にするリンカーが必要ですを持つメソッドにだけ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

Xamarin.iOS (Xamarin.Mac の利用はありません) の既定では常に有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]inline-runtime-arch`mtouch にします。

## <a name="dead-code-elimination"></a>停止しているコードの削除

この最適化は、次のようなコードに変更されます。

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

:

```csharp
Console.WriteLine ("Doing this");
```

次のように、定数の比較も評価されます。

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

わかったと式`8 == 8`は、常に true とにサイズを小さきます。

```csharp
Console.WriteLine ("Doing this");
```

次のようなコードを変換できるため、インライン展開の最適化と共に使用すると、強力な最適化です (これは、バインディングのコードを`NFCIso15693ReadMultipleBlocksConfiguration.Range`)。

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

これに (とき 64 ビットのデバイス用にビルド、およびがあることを確認することもありません`NFCIso15693ReadMultipleBlocksConfiguration`アプリ内のサブクラス)。

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

AOT コンパイラは既には、次のように実行されないコードを防止することが、この最適化は、リンカーは、リンカーは使用されませんとは、(別の場所を使用) 限り、削除したがってされる複数のメソッドがあることを確認できないことを意味の内部実行:

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

この最適化が有効にするリンカーが必要ですを持つメソッドにだけ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

(リンカーが有効になっている) 場合は、既定ではこれは常に有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]dead-code-elimination`mtouch/mmp にします。

## <a name="optimize-calls-to-blockliteralsetupblock"></a>BlockLiteral.SetupBlock への呼び出しを最適化します。

Xamarin.iOS/Mac ランタイムは、委任ブロック署名マネージため、OBJECTIVE-C ブロックを作成するときにする必要があります。 非常に高価な操作があります。 この最適化はビルド時にブロックの署名を計算し、呼び出しに IL を変更、`SetupBlock`メソッドを代わりに、引数として、署名を使用します。 これを行うため、実行時に署名を計算する必要があります。

ベンチマークでは、これは 10 ~ 15 のブロックを呼び出す速くことを示しています。

次を変換する[コード](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

この最適化が有効にするリンカーが必要ですを持つメソッドにだけ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

(静的レジストラーがデバイスのビルドの既定で有効にし、Xamarin.Mac 静的レジストラーはリリースの既定で有効では、構築 Xamarin.iOS) で、静的なレジストラーを使用する場合、既定で有効にされます。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]blockliteral-setupblock`mtouch/mmp にします。

## <a name="optimize-support-for-protocols"></a>プロトコルのサポートを最適化します。

Xamarin.iOS/Mac ランタイムでは、マネージ型を実装して Objective C のプロトコルに関する情報が必要です。 インターフェイス (およびこれらのインターフェイスの属性)、この情報は、非常に効率的な形式ではないもありませんリンカーに対応します。

1 つの例は、これらのインターフェイスがプロトコルのすべてのメンバーに関する情報を格納する、`[ProtocolMember]`属性は、特にこれらのメンバーのパラメーターの型への参照が含まれています。 つまり、単にこのようなインターフェイスを実装すると、そのインターフェイスで使用されるすべての型を保持する、リンカー、省略可能なメンバーに対しても、アプリしないを呼び出すかを実装します。

この最適化は簡単かつすばやく実行時に検索できるほとんどのメモリを使用する効率的な形式で必要な情報を格納する静的レジストラーが作成されます。

説明リンカーこと必ずしも必要はありません、これらのインターフェイスも、関連属性のいずれかを保持するためにします。

この最適化は、リンカーや静的レジストラーの両方を有効にする必要があります。

Xamarin.iOS でリンカーと静的レジストラーの両方が有効になっているときにこの最適化は既定で有効にします。

Xamarin.Mac でアセンブリを動的に読み込む Xamarin.Mac サポートし、それらのアセンブリ可能性がありますいないビルド時に既知の (されために最適化されていない) ために既定では、この最適化が有効には、ことはありません。

渡すことによって、既定の動作をオーバーライドできます`--optimize=-register-protocols`mtouch/mmp にします。

## <a name="remove-the-dynamic-registrar"></a>動的登録を削除します。

Xamarin.iOS と Xamarin.Mac ランタイムの両方がサポートされるよう[マネージ型を登録する](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/)Objective C のランタイムとします。 するか、または実行できます。 ビルド時または実行時に (ビルド時および実行時に残りの部分に部分的に) が実行時に行うためのサポート コードを削除するには完全に行われた場合ビルド時に、ことを意味します。 これは、結果、アプリのサイズが大幅に低下具体的には拡張機能などの小さいアプリまたは watchOS アプリ。

この最適化は、静的レジストラーとリンカーの両方を有効にする必要があります。

リンカーは、動的登録は、および削除を試みますような場合は削除しても安全かどうかの決定試みます。

Xamarin.Mac (いないビルド時に呼ばれていました) を実行時にアセンブリの読み込みを動的にサポートするため、ビルド時にこれは、安全な最適化するかどうかを判断することはできません。 これは Xamarin.Mac アプリ用の既定では決してこの最適化が有効になっていることを意味します。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]remove-dynamic-registrar`mtouch/mmp にします。

検出した場合に、リンカーで警告が生成オーバーライドする場合、既定値は、動的なレジストラーを削除する、安全ではありません (ただし、動的レジストラーは削除されます)。

## <a name="inline-runtimedynamicregistrationsupported"></a>インライン Runtime.DynamicRegistrationSupported

値のインラインの`Runtime.DynamicRegistrationSupported`ビルド時に決定されます。

動的レジストラーが削除された場合 (を参照してください、[動的登録を削除する](#remove-the-dynamic-registrar)最適化)、これは、定数`false`値、それ以外の場合これは定数で`true`値。

この最適化は、次のようなコードに変更されます。

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

次の動的レジストラーが削除されたとき。

```csharp
throw new Exception ("dynamic registration is not supported");
```

次の動的レジストラーが削除されていない場合。

```csharp
Console.WriteLine ("do something");
```

この最適化が有効にするリンカーが必要ですを持つメソッドにだけ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

(リンカーが有効になっている) 場合は、既定ではこれは常に有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]inline-dynamic-registration-supported`mtouch/mmp にします。

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Objective C ブロックにマネージ デリゲートを作成するメソッドを事前計算します。

パラメーターとしてブロック Objective C を呼び出すと、セレクターを受け取るし、その結果、マネージ コードがオーバーライドされたメソッドは、Xamarin.iOS、/、Xamarin.Mac ランタイムがそのブロックのデリゲートを作成する必要があります。

バインディング ジェネレーターによって生成されたバインド コードが含まれます、`[BlockProxy]`を持つ型を指定する属性、`Create`これを実行できるメソッドです。

次の Objective C コードを指定します。

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

次のバインド コード:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

ジェネレーターが生成されます。

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }
    
    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...
        
    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;
        
        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }
    
    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Objective C を呼び出すと`[ObjCBlockTester callClassCallback]`、Xamarin.iOS Xamarin.Mac ランタイムを見て、/、`[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]`パラメーターの属性です。 検索し、`Create`その型のメソッド、デリゲートを作成するには、そのメソッドを呼び出すとします。

この最適化は検索、`Create`属性とリフレクション (これははるかに高速であり、リンカーができますを使用して、代わりに、メタデータ トークンを使用して実行時にメソッドを検索するコードをビルド時、および静的レジストラーにメソッドが生成されます削除する場合、アプリを小さくすること、対応するランタイム コード)。

Mmp/mtouch が見つからなかったことがない場合、`Create`メソッド、MT4174/MM4174 警告が表示されます、および検索を代わりに実行時に実行されます。
最も考えられる原因はせず、必要なバインド コードを記述して手動で`[BlockProxy]`属性。

この最適化では、静的なレジストラーを有効にする必要があります。

ある限り、静的なレジストラーを有効になっている) 常に既定で有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]static-delegate-to-block-lookup`mtouch/mmp にします。
