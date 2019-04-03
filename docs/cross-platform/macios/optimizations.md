---
title: ビルドの最適化
description: このドキュメントでは、Xamarin.iOS および Xamarin.Mac アプリのビルド時に適用されるさまざまな最適化について説明します。
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: conceptdev
ms.author: crdun
ms.date: 04/16/2018
ms.openlocfilehash: f1aa805b9b7a16ad1e8af573cf4170f885eb0197
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870353"
---
# <a name="build-optimizations"></a>ビルドの最適化

このドキュメントでは、Xamarin.iOS および Xamarin.Mac アプリのビルド時に適用されるさまざまな最適化について説明します。

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>UIApplication.EnsureUIThread の削除/NSApplication.EnsureUIThread

呼び出しを削除します。 [UIApplication.EnsureUIThread] [ 1] (Xamarin.iOS) 用または`NSApplication.EnsureUIThread`(用、Xamarin.Mac)。

この最適化は、次のようなコードに変更されます。

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

次のように

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

この最適化が有効にするリンカーを必要とはメソッドにのみ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

既定ではリリースを有効になっている次のように構築します。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]remove-uithread-checks`mtouch/mmp にします。

[1]: https://docs.microsoft.com/dotnet/api/UIKit.UIApplication.EnsureUIThread

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

に従って (64 ビット プラットフォーム用の構築) の場合。

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

この最適化が有効にするリンカーを必要とはメソッドにのみ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

既定で有効になって、1 つのアーキテクチャを対象とする場合、またはプラットフォーム アセンブリ (**Xamarin.iOS.dll**、 **Xamarin.TVOS.dll**、 **Xamarin.WatchOS.dll**または**Xamarin.Mac.dll**)。

この最適化が 32 ビット バージョンと、アプリの 64 ビット バージョンの異なるアセンブリを作成する場合、複数のアーキテクチャを対象にして、両方のバージョンが効果的に、最終的なアプリのサイズを増やすと、アプリに含まれる必要があります。です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]inline-intptr-size`mtouch/mmp にします。

## <a name="inline-nsobjectisdirectbinding"></a>インライン NSObject.IsDirectBinding

`NSObject.IsDirectBinding` インスタンス プロパティかどうかを特定のインスタンスのラッパー型かどうかは、(ラッパーの型でのマネージ インスタンスは、ネイティブ型にマップするマネージ型を`UIKit.UIView`ネイティブ マップ」と入力`UIView`型の逆がユーザーの種類、ここで`class MyUIView : UIKit.UIView`ユーザーの種類になります)。

値を調べる必要がある`IsDirectBinding`値のバージョンを指定するために、OBJECTIVE-C を呼び出すときに`objc_msgSend`を使用します。

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

ようにして確認できます`UIView.SomeProperty`の値`IsDirectBinding`定数でないし、インライン化することはできません。

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

ただし、アプリで、すべての型を確認し、継承した型がないことを確認することは`NSUrl`とをインライン化するには、安全ではそのため、`IsDirectBinding`定数に値`true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

具体的には、この最適化はコードの次の種類を変更 (これは、バインドのコードを`NSUrl.AbsoluteUrl`)。

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

次に (場合を判断できるのサブクラスがない`NSUrl`アプリ)。

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

この最適化が有効にするリンカーを必要とはメソッドにのみ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

常に Xamarin.iOS では、既定で有効になって、Xamarin.Mac の既定で常に無効になります (ため Xamarin.Mac でアセンブリを動的に読み込むこともできますが、そのことはできません、特定のクラスがサブクラス化しないことを確認する)。

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

に従って (デバイスの構築) の場合。

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

この最適化が有効にするリンカーを必要とはメソッドにのみ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

Xamarin.ios (Xamarin.Mac の利用はありません) は既定で常に有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]inline-runtime-arch`mtouch にします。

## <a name="dead-code-elimination"></a>配信不能のコードの削除

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

このような定数の比較も評価されます。

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

あると判断し、式`8 == 8`は、常に true し、サイズを小さくします。

```csharp
Console.WriteLine ("Doing this");
```

これは、次のようなコードを変換できるためのインライン展開の最適化と併用する場合に、強力な最適化 (これは、バインドのコードを`NFCIso15693ReadMultipleBlocksConfiguration.Range`)。

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

これに (64 ビットのデバイスを構築して場合があることを確認することもない`NFCIso15693ReadMultipleBlocksConfiguration`アプリ内のサブクラス)。

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

AOT コンパイラでは、次のように実行されないコードを排除することがあるが、リンカーは、リンカーをもう使用されず、(他の場所で使用される) 場合を除き、したがって削除可能性がある複数のメソッドがあることを確認できます。 つまり内でこの最適化が行われます:

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

この最適化が有効にするリンカーを必要とはメソッドにのみ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

(リンカーを有効になっている) 場合は既定で常に有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]dead-code-elimination`mtouch/mmp にします。

## <a name="optimize-calls-to-blockliteralsetupblock"></a>BlockLiteral.SetupBlock への呼び出しを最適化します。

Xamarin.iOS/Mac ランタイムは委任ブロック シグネチャ管理された、OBJECTIVE-C でブロックを作成するときに通知する必要があります。 負荷のかかる操作があります。 この最適化は、ビルド時にブロックの署名を計算し、呼び出す IL を変更、`SetupBlock`を引数として、署名を受け取るメソッド。 これを行うには、実行時に署名を計算するための必要性が回避できます。

ベンチマークでは、これは 10 ~ 15 の倍数でブロックを呼び出す速くことを示します。

次を変換[コード](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

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

この最適化が有効にするリンカーを必要とはメソッドにのみ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

(静的レジストラー デバイスのビルドの既定で有効にし、静的レジストラーがリリースの既定で有効になっている Xamarin.Mac でビルドを Xamarin.iOS) で静的なレジストラーを使用する場合は既定で有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]blockliteral-setupblock`mtouch/mmp にします。

## <a name="optimize-support-for-protocols"></a>プロトコルのサポートを最適化します。

Xamarin.iOS/Mac ランタイムでは、マネージ型の実装 OBJECTIVE-C プロトコルに関する情報が必要です。 インターフェイス (および、これらのインターフェイスの属性) でこの情報が格納されている、非常に効率的な形式ではない、リンカーに適したでもありません。

1 つの例は、これらのインターフェイスがプロトコルのすべてのメンバーに関する情報を格納する、`[ProtocolMember]`属性は、特にそれらのメンバーのパラメーターの型への参照を含めることができます。 つまり、そのインターフェイスで使用されるすべての型を保持するリンカーと、このようなインターフェイスを実装するだけ、省略可能なメンバーの場合でも、アプリまたはしないでください呼び出しを実装します。

この最適化は実行時に検索を簡単には少量のメモリを使用する効率的な形式で必要な情報を格納する静的レジストラーになります。

教えてくれるでしょう、リンカーには必ずしも必要がないことをこれらのインターフェイスも、関連属性のいずれかを保持します。

この最適化では、リンカーと、静的なレジストラーの両方を有効にする必要があります。

Xamarin.iOS でリンカーと、静的なレジストラーの両方が有効にすると、この最適化は既定で有効です。

Xamarin.Mac でそれらのアセンブリとアセンブリを動的に読み込む Xamarin.Mac サポート可能性がありますいない既知のビルド時に (されために最適化されていない) ために既定では、この最適化が有効には、ことはありません。

渡すことによって、既定の動作をオーバーライドできます`--optimize=-register-protocols`mtouch/mmp にします。

## <a name="remove-the-dynamic-registrar"></a>動的登録を削除します。

Xamarin.iOS および Xamarin.Mac ランタイムの両方のサポートを組み込む[マネージ型を登録する](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/)OBJECTIVE-C ランタイムでします。 これは実行するか、ビルド時または実行時に (またはビルド時および実行時に、残りの部分に部分的に) が、ビルド時に完全に完了している場合、実行時に操作を行うためのサポート コードを削除できるようになります。 これは、結果、アプリのサイズが大幅に低下具体的には小さなアプリ拡張機能など、watchOS アプリ。

この最適化では、静的なレジストラーとリンカーの両方を有効にする必要があります。

動的登録、および場合は削除を試みますが、削除する安全なは、リンカーが試行されます。

Xamarin.Mac に動的に (これは、ビルド時に認識されませんでした) 実行時のアセンブリの読み込みをサポートするため、ビルド時にこれは安全な最適化かどうかを判断することはできません。 これは、この最適化が決して Xamarin.Mac アプリの既定で有効になっていることを意味します。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]remove-dynamic-registrar`mtouch/mmp にします。

検出した場合に、リンカーが警告を発する場合は、動的なレジストラーを削除する既定値がオーバーライドされると、安全ではありません (ただし、動的なレジストラーが削除されます)。

## <a name="inline-runtimedynamicregistrationsupported"></a>インライン Runtime.DynamicRegistrationSupported

値のインラインの`Runtime.DynamicRegistrationSupported`ビルド時に決定されます。

動的レジストラーが削除された場合 (を参照してください、[動的登録を削除](#remove-the-dynamic-registrar)最適化)、これは、定数`false`値には、それ以外の場合これは定数`true`値。

この最適化は、次のようなコードに変更されます。

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

次の動的なレジストラーが削除されたとき。

```csharp
throw new Exception ("dynamic registration is not supported");
```

次の動的なレジストラーが削除されていない場合。

```csharp
Console.WriteLine ("do something");
```

この最適化が有効にするリンカーを必要とはメソッドにのみ適用し、`[BindingImpl (BindingImplOptions.Optimizable)]`属性。

(リンカーを有効になっている) 場合は既定で常に有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]inline-dynamic-registration-supported`mtouch/mmp にします。

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Objective C のブロックのマネージ デリゲートを作成する方法を事前計算します。

Objective C を呼び出すと、セレクターをパラメーターとしてのブロックを受け取りし、マネージ コードがオーバーライドされたメソッドは、Xamarin.iOS、Xamarin.Mac のランタイムがそのブロックのデリゲートを作成する必要があるとします。

バインディング ジェネレーターによって生成されたバインド コードが含まれます、`[BlockProxy]`属性を持つ型を指定します、`Create`これを実行できるメソッド。

次のような Objective C コードを指定します。

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

コード ジェネレーターが生成されます。

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

Objective C を呼び出すと`[ObjCBlockTester callClassCallback]`、Xamarin.iOS Xamarin.Mac ランタイムを見て、/、`[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]`パラメーターの属性。 検索し、 `Create` 、その型のメソッド、デリゲートを作成するには、そのメソッドを呼び出します。

この最適化は検索、`Create`ビルド時、および静的レジストラーにメソッドでは、代わりに、属性とリフレクション (これははるかに高速でき、また、リンカーを使用してメタデータ トークンを使用して実行時にメソッドを検索するコードを生成します。アプリを小さくすること、対応するランタイム コードを削除) します。

Mmp/mtouch は見つけることがない場合、`Create`メソッドは、MT4174/MM4174 警告が表示されます、およびルックアップを代わりに実行時に実行されます。
最も考えられる原因はせず、必要なバインドのコードを記述して手動で`[BlockProxy]`属性。

この最適化では、静的なレジストラーを有効にする必要があります。

(ただし、静的なレジストラーを有効になっている) は既定で常に有効です。

渡すことによって、既定の動作をオーバーライドできます`--optimize=[+|-]static-delegate-to-block-lookup`mtouch/mmp にします。
