---
title: Xamarin. iOS の目標 C セレクター
description: このドキュメントでは、のC#目的 C セレクターを操作する方法について説明します。 セレクターを呼び出す方法と、その際に考慮する必要がある技術的な考慮事項について説明します。
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/12/2017
ms.openlocfilehash: 17b845345175d80237bcfdb171461f2c763c364e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291846"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin. iOS の目標 C セレクター

Objective-C 言語は*セレクター*に基づいています。 セレクターは、オブジェクトまたは*クラス*に送信できるメッセージです。 [Xamarin.iOS](~/ios/internals/api-design/index.md)はインスタンス セレクターをインスタンスメソッドにマッピングし、クラス セレクターを静的メソッドにマッピングします。

通常の C 関数とは異なり(C++ メンバー関数と同様に)、[P/invoke](https://www.mono-project.com/docs/advanced/pinvoke/) を使用して直接セレクターを呼び出すことができません。代わりにセレクターは、[`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) 関数を使用して、Objective-C クラスまたはインスタンスに送信されます。
プロシージャ.

Objective-C でのメッセージの詳細については、Apple の[Working with Objects](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2)ガイドを参照してください。

## <a name="example"></a>例

たとえば、[`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
セレクターを[`NSString`](https://developer.apple.com/documentation/foundation/nsstring)選択します。
(Apple のドキュメントからの) 宣言は次のとおりです。

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

この API には次の特性があります。

- 戻り値の型`CGSize`は Unified API です。
- パラメーターは、 [uifont](xref:UIKit.UIFont) (および[NSObject](xref:Foundation.NSObject)から派生した型 (間接的)) で、[System.IntPtr](xref:System.IntPtr) にマップされます。`font`
- パラメーターのは、に`nfloat`マップされます。 `CGFloat` `width`
- `lineBreakMode` [パラメーター`UILineBreakMode`](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc)のは、既に Xamarin. iOS にバインドされています。[`UILineBreakMode`](xref:UIKit.UILineBreakMode)
列挙.

すべてをまとめると、宣言`objc_msgSend`は次のようになります。

```csharp
CGSize objc_msgSend(
    IntPtr target, 
    IntPtr selector, 
    IntPtr font, 
    nfloat width, 
    UILineBreakMode mode
);
```

次のように宣言します。

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

このメソッドを呼び出すには、次のようなコードを使用します。

```csharp
NSString target = ...
Selector selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont font = ...
nfloat width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, 
    selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode
);
```

返された値が、サイズが8バイト未満の構造体 (統合 api に切り替える`SizeF`前に使用されていたものと同じ) であった場合、上記のコードはシミュレーターで実行されたがデバイスでクラッシュしたことになります。 サイズが8ビット未満の値を返すセレクターを呼び出すには、 `objc_msgSend_stret`関数を宣言します。

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

このメソッドを呼び出すには、次のようなコードを使用します。

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, 
        selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode
    );
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode
    );
```

## <a name="invoking-a-selector"></a>セレクターの呼び出し

セレクターを呼び出すには、次の3つの手順を実行します。

1. セレクターターゲットを取得します。
2. セレクター名を取得します。
3. 適切`objc_msgSend`な引数を指定してを呼び出します。

### <a name="selector-targets"></a>セレクターターゲット

セレクターターゲットは、オブジェクトインスタンスまたは目的 C クラスのいずれかです。 ターゲットがインスタンスであり、バインドされた Xamarin. iOS の種類からのもの[`ObjCRuntime.INativeObject.Handle`](xref:ObjCRuntime.INativeObject.Handle)である場合は、プロパティを使用します。

ターゲットがクラスの場合は、を[`ObjCRuntime.Class`](xref:ObjCRuntime.Class)使用してクラスインスタンスへの参照を取得し、 [`Class.Handle`](xref:ObjCRuntime.Class.Handle)プロパティを使用します。

### <a name="selector-names"></a>セレクター名

セレクター名は、Apple のドキュメントに記載されています。 たとえば、に[`NSString`](https://developer.apple.com/documentation/foundation/nsstring?language=objc)は[`sizeWithFont:`](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc)セレクター [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc)とセレクターがあります。 埋め込みと末尾のコロンはセレクター名の一部であるため、省略することはできません。

セレクター名を作成したら、その[`ObjCRuntime.Selector`](xref:ObjCRuntime.Selector)インスタンスを作成できます。

### <a name="calling-objc_msgsend"></a>Objc_msgSend の呼び出し

`objc_msgSend`オブジェクトにメッセージ (セレクター) を送信します。 この関数ファミリは、少なくとも2つの必須引数を受け取ります。セレクターターゲット (インスタンスまたはクラスハンドル)、セレクター自体、およびセレクターに必要な引数です。 インスタンス引数とセレクター `System.IntPtr`引数はである必要があり、残りのすべての引数はセレクターが想定`nint`する型 (たとえば`int`、 `System.IntPtr`の場合) また`NSObject`はすべての派生型のに一致する必要があります。 使用する[`NSObject.Handle`](xref:Foundation.NSObject.Handle)
目標 C 型`IntPtr`のインスタンスのを取得するためのプロパティ。

複数の`objc_msgSend`関数があります。

- 構造[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)体を返すセレクターに使用します。 ARM では、これには、列挙型でも、C の組み込み型 (`char` `int`、 `float` `short` `long`、、、、 `double`) でもないすべての戻り値の型が含まれます。 X86 (シミュレーター) では、サイズが8バイトを超えるすべての構造体に対してこのメソッドを`CGSize`使用する必要があります`objc_msgSend_stret` (は8バイトで、シミュレーターでは使用されません)。 
- X86 [`objc_msgSend_fpret`](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc)でのみ浮動小数点値を返すセレクターに対して使用します。 この関数を ARM で使用する必要はありません。代わりに、を`objc_msgSend`使用します。 
- Main [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)関数は、他のすべてのセレクターに対して使用されます。

呼び出す必要がある`objc_msgSend`関数 (シミュレーターとデバイスにそれぞれ異なる方法が必要) を決定したら、通常[`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute)のメソッドを使用して関数を宣言し、後で呼び出すことができます。

事前に定義`objc_msgSend`された一連の宣言は、 `ObjCRuntime.Messaging`「」にあります。

## <a name="different-invocations-on-simulator-and-device"></a>シミュレーターとデバイスでの異なる呼び出し

前述のように、目標 C には3種類`objc_msgSend`のメソッドがあります。1つは通常の呼び出し用、もう1つは浮動小数点値を返す呼び出し用 (x86 のみ)、もう1つは構造体の値を返す呼び出し用です。 後者には、に`_stret` `ObjCRuntime.Messaging`サフィックスが含まれています。

特定の構造体 (以下で説明する規則) を返すメソッドを呼び出す場合は、 `out`値として最初のパラメーターとして戻り値を指定してメソッドを呼び出す必要があります。

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

`_stret_`メソッドを使用する場合のルールは、x86 と ARM で異なります。
バインドがシミュレーターとデバイスの両方で動作するようにするには、次のようなコードを追加します。

```csharp
if (Runtime.Arch == Arch.DEVICE)
{
    PointF ret;
    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);
    return ret;
} 
else
{
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
}
```

### <a name="using-the-objc_msgsend_stret-method"></a>Objc_msgSend_stret メソッドの使用

ARM 用にビルドする場合は、[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
列挙型ではない値型、または列挙体の基本型 (`int` `short`、 `double` `byte` `long`、、、、 `float`)。

X86 用にビルドする場合は、を使用します。[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
列挙型ではない値型、または列挙体の基本型 (`int` `short`、 `double` `byte` `long`、、、、 `float`) のいずれかの値型で、ネイティブサイズが8バイトを超えています。

### <a name="creating-your-own-signatures"></a>独自の署名の作成

必要に応じて、次の[gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c)を使用して独自の署名を作成できます。

## <a name="related-links"></a>関連リンク

- [目標-C セレクターの](https://developer.xamarin.com/samples/mac-ios/Objective-C/)サンプル
