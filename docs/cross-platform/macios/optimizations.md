---
title: ビルドの最適化
description: このドキュメントでは、Xamarin および Xamarin アプリのビルド時に適用されるさまざまな最適化について説明します。
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: davidortinau
ms.author: daortin
ms.date: 04/16/2018
ms.openlocfilehash: d3076e35d6e9b7e44ec2f9537f250295eaff7e2e
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453559"
---
# <a name="build-optimizations"></a>ビルドの最適化

このドキュメントでは、Xamarin および Xamarin アプリのビルド時に適用されるさまざまな最適化について説明します。

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>EnsureUIThread/NSApplication を削除します。

[EnsureUIThread][1] (xamarin の場合) または `NSApplication.EnsureUIThread` (xamarin. Mac の場合) の呼び出しを削除します。

この最適化により、次の種類のコードが変更されます。

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

次のようになります。

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

この最適化では、リンカーを有効にする必要があり、属性を持つメソッドにのみ適用され `[BindingImpl (BindingImplOptions.Optimizable)]` ます。

既定では、リリースビルドで有効になっています。

既定の動作は、 `--optimize=[+|-]remove-uithread-checks` mtouch/mmp に渡すことでオーバーライドできます。

[1]: /dotnet/api/UIKit.UIApplication.EnsureUIThread

## <a name="inline-intptrsize"></a>Inline IntPtr. サイズ

ターゲットプラットフォームに従っての定数値をインラインで指定し `IntPtr.Size` ます。

この最適化により、次の種類のコードが変更されます。

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

(64 ビットプラットフォーム用にビルドする場合) は次のようになります。

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

この最適化では、リンカーを有効にする必要があり、属性を持つメソッドにのみ適用され `[BindingImpl (BindingImplOptions.Optimizable)]` ます。

既定では、単一のアーキテクチャを対象とする場合、またはプラットフォームアセンブリ (**Xamarin.iOS.dll**、 **Xamarin.TVOS.dll**、 **Xamarin.WatchOS.dll** または **Xamarin.Mac.dll**) の場合に有効になります。

複数のアーキテクチャを対象とする場合、この最適化により、32ビットバージョンと64ビットバージョンのアプリ用に異なるアセンブリが作成されます。また、両方のバージョンをアプリに含める必要があります。これにより、最終的なアプリサイズを減らすのではなく、効果的に拡大できます。

既定の動作は、 `--optimize=[+|-]inline-intptr-size` mtouch/mmp に渡すことでオーバーライドできます。

## <a name="inline-nsobjectisdirectbinding"></a>Inline NSObject

`NSObject.IsDirectBinding` は、特定のインスタンスがラッパー型であるかどうかを判断するインスタンスプロパティです (ラッパー型はネイティブ型にマップされるマネージ型です。たとえば、マネージ型はネイティブ型にマップされます。 `UIKit.UIView` `UIView` 逆の場合はユーザー型、この場合はユーザー型になり `class MyUIView : UIKit.UIView` ます)。

`IsDirectBinding`使用するのバージョンは値によって決定されるため、目標値 C を呼び出すときのの値を把握しておく必要が `objc_msgSend` あります。

次のコードのみが指定されています。

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

`UIView.SomeProperty`の値 `IsDirectBinding` が定数ではなく、インライン化できないことを確認できます。

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

ただし、アプリ内のすべての型を確認し、から継承する型がないことを確認することができ `NSUrl` ます。したがって、値を定数にインライン化するのは安全です `IsDirectBinding` `true` 。

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

特に、この最適化によって次の種類のコードが変更されます (のバインドコード `NSUrl.AbsoluteUrl` )。

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

次のようになります (アプリにのサブクラスがないと判断できる場合 `NSUrl` )。

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

この最適化では、リンカーを有効にする必要があり、属性を持つメソッドにのみ適用され `[BindingImpl (BindingImplOptions.Optimizable)]` ます。

既定では、xamarin. iOS では既定で有効になっており、既定では、xamarin. Mac では常に無効になっています。つまり、特定のクラスがサブクラス化されないことを判断することはできません。

既定の動作は、 `--optimize=[+|-]inline-isdirectbinding` mtouch/mmp に渡すことでオーバーライドできます。

## <a name="inline-runtimearch"></a>インラインランタイム. Arch

この最適化により、次の種類のコードが変更されます。

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

次のようにします (デバイス用にビルドする場合)。

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

この最適化では、リンカーを有効にする必要があり、属性を持つメソッドにのみ適用され `[BindingImpl (BindingImplOptions.Optimizable)]` ます。

既定では、Xamarin. iOS (Xamarin. Mac では使用できません) で常に有効になっています。

既定の動作は、mtouch に渡すことでオーバーライドでき `--optimize=[+|-]inline-runtime-arch` ます。

## <a name="dead-code-elimination"></a>デッドコードの排除

この最適化により、次の種類のコードが変更されます。

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

ドラッグ

```csharp
Console.WriteLine ("Doing this");
```

また、次のような定数比較も評価します。

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

式が常に true であることを確認 `8 == 8` し、次のように減らします。

```csharp
Console.WriteLine ("Doing this");
```

これは、次の種類のコードを変換できるため、インライン展開の最適化と共に使用する場合に強力な最適化です (これはのバインドコードです `NFCIso15693ReadMultipleBlocksConfiguration.Range` )。

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

これには、(64 ビットデバイス用にビルドする場合と、アプリにサブクラスがないことを確認する場合にも使用でき `NFCIso15693ReadMultipleBlocksConfiguration` ます)。

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

AOT コンパイラは、既にこのようなデッドコードを排除することができますが、この最適化はリンカー内で実行されます。つまり、使用されなくなった (他の場所で使用されていない) 複数のメソッドがあることをリンカーが確認できます。

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

この最適化では、リンカーを有効にする必要があり、属性を持つメソッドにのみ適用され `[BindingImpl (BindingImplOptions.Optimizable)]` ます。

常に既定で有効になっています (リンカーが有効になっている場合)。

既定の動作は、 `--optimize=[+|-]dead-code-elimination` mtouch/mmp に渡すことでオーバーライドできます。

## <a name="optimize-calls-to-blockliteralsetupblock"></a>BlockLiteral の呼び出しを最適化します。 SetupBlock

マネージデリゲートに対して目的の C ブロックを作成する場合は、Xamarin iOS/Mac ランタイムにブロック署名があることが必要です。 これは、かなり負荷のかかる操作である可能性があります。 この最適化により、ビルド時にブロック署名が計算され、 `SetupBlock` 代わりにシグネチャを引数として受け取るメソッドを呼び出すように IL が変更されます。 これにより、実行時に署名を計算する必要がなくなります。

ベンチマークでは、ブロックの呼び出し速度が 10 ~ 15 倍になることが示されています。

次の [コード](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211)が変換されます。

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

ドラッグ

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

この最適化では、リンカーを有効にする必要があり、属性を持つメソッドにのみ適用され `[BindingImpl (BindingImplOptions.Optimizable)]` ます。

静的レジストラーを使用すると、既定で有効になります (Xamarin. iOS では、静的レジストラーはデバイスのビルドに対して既定で有効になっていますが、Xamarin. Mac では、静的レジストラーはリリースビルドでは既定で有効になっています)。

既定の動作は、 `--optimize=[+|-]blockliteral-setupblock` mtouch/mmp に渡すことでオーバーライドできます。

## <a name="optimize-support-for-protocols"></a>プロトコルのサポートを最適化する

Xamarin. iOS/Mac ランタイムには、マネージ型が目標 C プロトコルを実装する方法に関する情報が必要です。 この情報は、インターフェイス (およびこれらのインターフェイスの属性) に格納されます。これらは、非常に効率的な形式ではなく、リンカーにも適していません。

1つの例として、これらのインターフェイスには、属性内のすべてのプロトコルメンバーに関する情報が格納され `[ProtocolMember]` ます。これには、これらのメンバーのパラメーターの型への参照が含まれます。 これは、このようなインターフェイスを実装するだけで、そのインターフェイスで使用されるすべての型がリンカーによって保持されることを意味します。これは、アプリが呼び出さないか、実装しないオプションのメンバーでも同様です。

この最適化により、静的レジスタは、実行時に簡単にすばやく検索できるメモリをほとんど使用しない効率的な形式で、必要な情報を格納します。

また、これらのインターフェイスを保持する必要があるとは限りません。また、関連する属性もありません。

この最適化を行うには、リンカーと静的レジスタの両方を有効にする必要があります。

Xamarin. iOS では、リンカーと静的レジストラーの両方が有効になっている場合、この最適化は既定で有効になっています。

Xamarin. Mac では、この最適化は既定では有効になりません。これは、Xamarin. Mac ではアセンブリの動的な読み込みがサポートされており、これらのアセンブリがビルド時に既知ではない (したがって、最適化されていない) ためです。

既定の動作は、 `--optimize=-register-protocols` mtouch/mmp に渡すことでオーバーライドできます。

## <a name="remove-the-dynamic-registrar"></a>動的レジストラーを削除する

Xamarin と Xamarin の両方のランタイムには、 [マネージ型](~/ios/internals/registrar.md) を目的の C ランタイムに登録するためのサポートが含まれています。 ビルド時または実行時 (またはビルド時には部分的に実行) で実行できますが、ビルド時に完全に完了した場合は、実行時に実行するためのサポートコードを削除することを意味します。 この結果、アプリのサイズが大幅に減少します。特に、拡張アプリや watchOS アプリなどの小規模なアプリの場合です。

この最適化を行うには、静的レジスタとリンカーの両方が有効になっている必要があります。

リンカーは、動的レジストラーを削除するのが安全かどうかを判断しようとします。そうでない場合は、それを削除しようとします。

Xamarin. Mac では、実行時にアセンブリを動的に読み込むことができます (ビルド時には認識されません)。そのため、ビルド時に安全な最適化であるかどうかを判断することはできません。 つまり、この最適化は、Xamarin. Mac アプリでは既定では有効になりません。

既定の動作は、 `--optimize=[+|-]remove-dynamic-registrar` mtouch/mmp に渡すことでオーバーライドできます。

既定値がオーバーライドされて動的レジストラーが削除された場合、リンカーは安全でないことを検出した場合に警告を出力します (ただし、動的レジストラーは削除されます)。

## <a name="inline-runtimedynamicregistrationsupported"></a>インラインランタイム。 DynamicRegistrationSupported

ビルド時に決定されたの値をインラインで指定 `Runtime.DynamicRegistrationSupported` します。

動的レジストラーが削除された場合 (「 [動的レジストラーの](#remove-the-dynamic-registrar) 最適化を削除する」を参照)、これは定数 `false` 値です。それ以外の場合は定数値 `true` です。

この最適化により、次の種類のコードが変更されます。

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

動的レジストラーが削除されると、次のようになります。

```csharp
throw new Exception ("dynamic registration is not supported");
```

動的レジストラーが削除されない場合は、次のようになります。

```csharp
Console.WriteLine ("do something");
```

この最適化では、リンカーを有効にする必要があり、属性を持つメソッドにのみ適用され `[BindingImpl (BindingImplOptions.Optimizable)]` ます。

常に既定で有効になっています (リンカーが有効になっている場合)。

既定の動作は、 `--optimize=[+|-]inline-dynamic-registration-supported` mtouch/mmp に渡すことでオーバーライドできます。

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>事前計算ブロックのマネージデリゲートを作成するためのメソッド

前の例では、ブロックをパラメーターとして受け取り、マネージコードによってそのメソッドがオーバーライドされた場合、そのブロックのデリゲートを作成する必要があります。

バインディングジェネレーターによって生成されるバインディングコードには、属性が含まれます `[BlockProxy]` 。この属性は、 `Create` この操作を実行できるメソッドを使用して型を指定します。

次の目的 C コードを指定します。

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

次のバインドコードを記述します。

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

ジェネレーターは次のものを生成します。

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

前の C を呼び出すと `[ObjCBlockTester callClassCallback]` 、パラメーターの属性が表示されるようになります `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` 。 その後 `Create` 、その型のメソッドを検索し、そのメソッドを呼び出してデリゲートを作成します。

この最適化により、 `Create` ビルド時にメソッドが検出され、静的レジストラーは、属性とリフレクションを使用して、メタデータトークンを使用して実行時にメソッドを検索するコードを生成します (これははるかに高速です。また、リンカーは、対応するランタイムコードを削除してアプリを小さくすることもできます)。

Mmp/mtouch がメソッドを見つけることができない場合は、 `Create` MT4174/MM4174 警告が表示され、代わりに実行時に参照が実行されます。
最も可能性の高い原因は、手動で記述されたバインドコードです。必要な `[BlockProxy]` 属性はありません。

この最適化を行うには、静的レジスタを有効にする必要があります。

既定では、静的レジストラーが有効になっている限り、常に有効になります。

既定の動作は、 `--optimize=[+|-]static-delegate-to-block-lookup` mtouch/mmp に渡すことでオーバーライドできます。