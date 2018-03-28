---
title: Objective C セレクター
ms.topic: article
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7b7f64288695ecc0f9f57ec670c4e9ff2e44804c
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="objective-c-selectors"></a>Objective C セレクター

Objective C の言語がに基づいて*セレクター*です。 セレクターがオブジェクトに送信できるメッセージまたは*クラス*です。 [Xamarin.iOS](~/ios/internals/api-design/index.md)マップがインスタンス化するインスタンス メソッド、セレクターとクラスの静的メソッド セレクター。

標準 C 関数とは異なり、C++ メンバー関数と同様に) 直接呼び出すことができませんセレクターを使用して、 [P/invoke](http://www.mono-project.com/docs/advanced/pinvoke/)です。
(*確保*: 理論的には、C++ メンバー関数の非仮想、P/invoke を使用する可能性がありますより無視ペインの世界である名前のマングル コンパイラごと、について心配する必要があります)。セレクターが代わりに、OBJECTIVE-C クラスに送信されるか、インスタンスを使用して、 [ `objc_msgSend`関数](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend)です。

検索することがあります[Objective C のメッセージングに役立つこのガイド](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html)に便利です。

<a name="Example" />

## <a name="example"></a>例

呼び出し先とすると、 [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:)セレクター。
(Apple のドキュメント) から、宣言は次のとおりです。

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  戻り値の型は*CGSize* Unified api です。
-  *フォント*パラメーターは、 [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (型 () から間接的に派生と[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) にマップされてしたがって[System.IntPtr](https://developer.xamarin.com/api/type/System.IntPtr/)です。
-  *幅*、パラメーター、 *CGFloat*にマップされて*nfloat*です。
-  *LineBreakMode* 、パラメーター、 *UILineBreakMode* 、として Xamarin.iOS に既にバインドされて、 [UILineBreakMode 列挙](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/)です。


すべてをまとめるとに一致する objc_msgSend 宣言します。

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

宣言する必要があります。

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

宣言されると、適切なパラメーターを作成したら呼び出すことができます。

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

返された値がサイズが 8 バイト未満となった構造体をされていた (旧バージョンと同様に`SizeF`Unified Api に切り替える前に使用)、上記のコードはデバイスでクラッシュしたが、シミュレーターで実行がします。 サイズは、その結果 8 ビット未満の値を返すセレクターを呼び出すために*も*を宣言する必要があります、`objc_msgSend_stret()`関数。

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

セレクターを呼び出すには、3 つの手順があります。

1.  選択対象を取得します。
1.  セレクターの名前を取得します。
1.  適切な引数を持つ objc_msgSend() を呼び出します。


<a name="Selector_Targets" />

### <a name="selector-targets"></a>セレクターのターゲット

セレクター ターゲットとは、オブジェクトのインスタンスまたは OBJECTIVE-C クラスのいずれかです。 ときに、ターゲット インスタンスは、バインド Xamarin.iOS 型から使用して、 [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/)プロパティです。

ターゲットが、クラスの場合を使用して[ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) 、クラスのインスタンスへの参照を取得するを使用し、 [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/)プロパティです。


<a name="Selector_Names" />

### <a name="selector-names"></a>セレクターの名前

セレクターの名前は、Apple のドキュメントに記載されています。 たとえば、 [UIKit NSString 拡張メソッド](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html)含める[sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:)と[sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:)です。 埋め込みおよび後続のコロンが重要であり、セレクターの名前の一部であります。

セレクターの名前を作成したら、作成、 [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/)のインスタンス。


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>通話 objc_msgSend()

 `objc_msgSend()` メッセージを送信する (セレクター) オブジェクトを使用します。 このファミリの関数は、少なくとも 2 つの必要な引数を受け取り: セレクター ターゲット (インスタンスまたは処理クラス)、自体、セレクターとし、特定のセレクターの必要な引数。 インスタンスとセレクターの引数にする必要があります`System.IntPtr`、残りのすべての引数は、セレクターが必要ですが、例: 型を一致させる必要があります、`nint`の`int`、または`System.IntPtr`すべて`NSObject`-派生型です。 使用して、 [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/)を取得するプロパティ、 `IntPtr` Objective C 型のインスタンスにします。

残念ながら、複数の 1 つある`objc_msgSend()`関数。

使用して[ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret)構造体を返すことがセレクター用です。
「興味深い」、ARM 上このやすくするためにすべての戻り型が含まれますが*いない*列挙体、または C の組み込み型 (char、short、int、long、float、double) のいずれか。 X86 (シミュレーター) では、すべての構造体のサイズが 8 バイトより大きいために使用するこの必要があります。 (CGSize----上記の例で使用される 8 バイト、厳密には、使用していないため`objc_msgSend_stret()`シミュレーター)。

使用して[ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret)浮動を返すセレクターは、x86 のみに値をポイントします。 この関数は、ARM; で使用する必要はありません。代わりに、`objc_msgSend()`です。

メイン[objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend)関数は他のすべてのセレクターを使用します。

これが決まったら、`objc_msgSend()`関数を呼び出す必要があります (および、1 つ以上、シミュレーターおよびデバイスの例があります)、通常を使用することができます[ `[DllImport]` ](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.DllImportAttribute/)以降の呼び出しに関数を宣言するメソッド。

一連の事前に作成された`objc_msgSend()`宣言は含まれて[ `ObjCRuntime.Messaging`](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/)です。


<a name="ugly" />

## <a name="the-ugly"></a>悪い

Objective C が次の 3 種類の`objc_msgSend`メソッド: を浮動小数点値 (x86 のみ) を返すための呼び出しとの呼び出しで構造体の値を返す正規の呼び出しに 1 つです。 後者にサフィックスを含む`_stret`で`ObjCRuntime.Messaging`です。

特定の構造 (ルールは、次に示す) を返すメソッドを呼び出す場合は、次のように、out 値として最初のパラメーターとして、戻り値を持つメソッドを呼び出す必要があります。

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

点はここでわかりにくくなるため、使用する必要がある場合のルール_stret_が異なる X86 と ARM では、する場合は、バインド、シミュレーターと、デバイスの両方で動作する必要がありますを次のようなコードを追加します。

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>使用して、objc\_msgSend\_stret メソッド

ルールを使用する場合、 [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret)の次のように、 **ARM**:

-  値の列挙型または列挙型の基本型のいずれかではない型 (int, byte、short、long、double、float)。


ルールを使用する場合、 [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret)の次のように、 **X86**:

-  値の列挙型、または列挙型の基本型のいずれかではない型 (int, byte、short、long、double、float) ネイティブ サイズが 8 バイトを超えるとします。


### <a name="creating-your-own-signatures"></a>独自の署名を作成します。

次[gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c)独自の署名を作成するために必要な場合に使用できます。



## <a name="related-links"></a>関連リンク

- [セレクターのサンプル](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
