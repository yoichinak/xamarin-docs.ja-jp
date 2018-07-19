---
title: Xamarin.iOS での OBJECTIVE-C セレクター
description: このドキュメントでは、c# から OBJECTIVE-C セレクターと対話する方法について説明します。 これには、セレクターとその際に考慮する必要がありますが、技術的な考慮事項を呼び出す方法について説明します。
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: d8708e518e967083bcfb95c88f8c20fddbb44b53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998795"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin.iOS での OBJECTIVE-C セレクター

Objective C の言語がに基づいて*セレクター*します。 セレクターがオブジェクトに送信できるメッセージまたは*クラス*します。 [Xamarin.iOS](~/ios/internals/api-design/index.md)マップのインスタンス、インスタンス メソッドへのセレクターおよびクラスの静的メソッドへのセレクター。

通常の C 関数とは異なり、C++ メンバー関数と同様に) 呼び出すことができません直接セレクターを使用して、 [P/invoke](http://www.mono-project.com/docs/advanced/pinvoke/)します。
(*確保*: 理論上は、非仮想の C++ メンバー関数の P/invoke を使用できますより無視痛みの世界はコンパイラごと名前マングル、について心配する必要があります)。代わりに、セレクターは、OBJECTIVE-C のクラスに送信されるか、インスタンスを使用して、 [ `objc_msgSend`関数](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend)します。

わかります[Objective C のメッセージングについて、この便利なガイド](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html)に便利です。

<a name="Example" />

## <a name="example"></a>例

起動すると、 [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:)セレクター。
(Apple のドキュメント) から宣言です。

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  戻り値の型は*CGSize* Unified api。
-  *フォント*パラメーターが、 [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (型が (直接) から派生しない[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) にマップされますので、 [System.IntPtr](xref:System.IntPtr)します。
-  *幅*パラメーター、 *CGFloat*にマップされて*nfloat*します。
-  *LineBreakMode*パラメーター、 *UILineBreakMode* 、として Xamarin.iOS で既にバインドされて、 [UILineBreakMode 列挙](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/)します。


すべてをまとめた、配置し、一致する objc_msgSend 宣言します。

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

変数を宣言する必要があります。

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

宣言されると、適切なパラメーターを取得したら呼び出すことができます。

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

返される値は、サイズが 8 バイト未満であった構造体をされていた (などの古い`SizeF`Unified Api に切り替える前に使用)、デバイスでクラッシュしたが、シミュレーターで実行は、上記のコード。 サイズは、その結果 8 ビット未満の値を返すセレクターを呼び出すために*も*を宣言する必要があります、`objc_msgSend_stret()`関数。

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

呼び出しになります。

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>セレクターの起動

セレクターを呼び出すと、3 つの手順があります。

1.  選択対象を取得します。
1.  セレクターの名前を取得します。
1.  適切な引数を持つ objc_msgSend() を呼び出します。


<a name="Selector_Targets" />

### <a name="selector-targets"></a>セレクターのターゲット

セレクターをターゲットとは、オブジェクトのインスタンスまたは OBJECTIVE-C クラスのいずれかです。 ターゲットがインスタンスであり、バインドされた Xamarin.iOS 型の取得元である場合は、使用、 [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/)プロパティ。

ターゲットが、クラスの場合を使用して、 [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/)クラスのインスタンスへの参照を取得するを使用し、 [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/)プロパティ。


<a name="Selector_Names" />

### <a name="selector-names"></a>セレクター名

セレクター名は、Apple のドキュメント内で一覧表示されます。 たとえば、 [UIKit NSString 拡張メソッド](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html)含める[sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:)と[sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:)します。 埋め込みおよび末尾のコロンは重要ですとセレクターの名前の一部であります。

セレクターの名前を使用すると、作成できます、 [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/)のインスタンス。


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Objc_msgSend() を呼び出す

 `objc_msgSend()` オブジェクトへのメッセージ (セレクター) の送信に使用されます。 このファミリの関数は、少なくとも 2 つの必要な引数を受け取ります。 セレクターのターゲット (インスタンスまたはクラスの処理)、自体、セレクターとし、特定のセレクターに必要な引数。 インスタンスとセレクター引数には、 `System.IntPtr`、し、残りのすべての引数は、セレクターが必要ですが、例: 型と一致する必要があります、`nint`の`int`、または`System.IntPtr`すべて`NSObject`-派生型。 使用して、 [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/)プロパティを取得する、`IntPtr`の Objective C 型のインスタンス。

残念ながらは 1 つ以上`objc_msgSend()`関数。

使用[ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret)セレクター構造体を返します。
"興味のあること"ARM でこれにはすべて戻り値の型が含まれています。*いない*列挙体、または C の組み込み型 (char、short、int、long、float、double) のいずれか。 On x86 (シミュレーター) では、すべての構造体のサイズが 8 バイトより大きいために使用するこの必要があります。 (CGSize----上記の例では使用が 8 バイトを正確があり、したがって使用しない`objc_msgSend_stret()`シミュレーター)。

使用[ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret)浮動を返すセレクターは、x86 のみに値をポイントします。 この関数は、ARM; で使用する必要はありません。代わりに、`objc_msgSend()`します。

メイン[objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend)関数は、他のすべてのセレクターを使用します。

決まったら、`objc_msgSend()`関数を呼び出す必要があります (および、1 つ以上、シミュレーターやデバイス用などがあります)、通常を使用することができます[ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute)メソッドの後に呼び出す関数を宣言します。

一連の事前に作成された`objc_msgSend()`で見つかる宣言[ `ObjCRuntime.Messaging`](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/)します。


<a name="ugly" />

## <a name="the-ugly"></a>悪い

Objective C が 3 種類の`objc_msgSend`メソッド: 通常の呼び出し、浮動小数点値 (x86 のみ) を返す呼び出しおよび構造体の値を返す呼び出しのいずれか。 後者の場合、サフィックスを含む`_stret`で`ObjCRuntime.Messaging`します。

特定の構造 (ルールは、以下の説明) を返すメソッドを呼び出す場合は、次のように、out 値として最初のパラメーターとして戻り値を持つメソッドを呼び出す必要があります。

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

処理はここでわかりにくくなるため、使用する必要がある場合のルール_stret_は異なる X86 と ARM では、する場合は、バインディングが、シミュレーターと、デバイスの両方で動作する必要がありますを次のようなコードを追加します。

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>Objc を使用して\_msgSend\_stret メソッド

ルールを使用する場合、 [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret)のこのような**ARM**:

-  値の列挙型または列挙型の基本型のいずれかではない型 (int, byte、short、long、double、float)。


ルールを使用する場合、 [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret)のこのような**X86**:

-  値の列挙体、または列挙型の基本型のいずれかではない型 (int, byte、short、long、double、float) ネイティブ サイズが 8 バイトを超えるとします。


### <a name="creating-your-own-signatures"></a>独自の署名を作成します。

次[gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c)独自の署名を作成するために必要な場合に使用できます。



## <a name="related-links"></a>関連リンク

- [セレクターのサンプル](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
