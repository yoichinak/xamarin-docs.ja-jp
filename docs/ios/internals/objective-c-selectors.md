---
title: Xamarin. iOS の Objective-C セレクター
description: このドキュメントでは、のC# から Objective-C の セレクターを操作する方法について説明します。 セレクターを呼び出す方法と、その際に考慮する必要がある技術的な考慮事項について説明します。
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/12/2017
ms.openlocfilehash: 79f226c137c3ab6b1dd2de9f92cb868056aa9d59
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022282"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin. iOS の Objective-C セレクター

Objective-C 言語は*セレクター*に基づいています。 セレクターは、オブジェクトまたは*クラス*に送信できるメッセージです。 [Xamarin iOS](~/ios/internals/api-design/index.md) は、インスタンスセレクターをインスタンスメソッドに、クラスセレクターを静的メソッドにマップします。

通常の C 関数 (および同様の C++ メンバー関数) とは異なり、 [P/invoke](https://www.mono-project.com/docs/advanced/pinvoke/) を使用してセレクターを直接呼び出すことはできません。セレクターは、[`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) 関数を使用して Objective-C クラスまたはインスタンスに送信されます。
プロシージャ.

Objective-C のメッセージの詳細については、「Apple の[オブジェクトの操作](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2)ガイド」を参照してください。

## <a name="example"></a>例

[`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)を呼び出すとします。
[`NSString`](https://developer.apple.com/documentation/foundation/nsstring)のセレクター。
(Apple のドキュメントからの) 宣言は次のとおりです。

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

この API には次の特性があります。

- 戻り値の型は、Unified API に対して `CGSize` ます。
- `font` パラメーターは、 [Uifont](xref:UIKit.UIFont) (および[NSObject](xref:Foundation.NSObject)から派生した型 (間接的)) で、 [IntPtr](xref:System.IntPtr)にマップされます。
- `CGFloat``width` パラメーターは `nfloat`にマップされます。
- `lineBreakMode` パラメーターの[`UILineBreakMode`](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc)は、 [`UILineBreakMode`](xref:UIKit.UILineBreakMode)として既に Xamarin. iOS にバインドされています。
列挙.

すべてをまとめて、`objc_msgSend` 宣言を一致させる必要があります。

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

返された値が、サイズが8バイト未満の構造体 (統合 Api に切り替える前に使用されていた古い `SizeF` と同様) であった場合、上記のコードはシミュレーターで実行されているがデバイスでクラッシュした可能性があります。 サイズが8ビット未満の値を返すセレクターを呼び出すには、`objc_msgSend_stret` 関数を宣言します。

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
3. 適切な引数を指定して `objc_msgSend` を呼び出します。

### <a name="selector-targets"></a>セレクターターゲット

セレクターターゲットは、オブジェクトインスタンスまたは Objective-C クラスのいずれかです。 ターゲットがインスタンスであり、バインドされた Xamarin. iOS の種類からのものである場合は、 [`ObjCRuntime.INativeObject.Handle`](xref:ObjCRuntime.INativeObject.Handle)プロパティを使用します。

ターゲットがクラスの場合は、 [`ObjCRuntime.Class`](xref:ObjCRuntime.Class)を使用してクラスインスタンスへの参照を取得し、 [`Class.Handle`](xref:ObjCRuntime.Class.Handle)プロパティを使用します。

### <a name="selector-names"></a>セレクター名

セレクター名は、Apple のドキュメントに記載されています。 たとえば、 [`NSString`](https://developer.apple.com/documentation/foundation/nsstring?language=objc)には、 [`sizeWithFont:`](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc)セレクターと[`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc)セレクターが含まれています。 埋め込みと末尾のコロンはセレクター名の一部であるため、省略することはできません。

セレクター名を指定したら、そのインスタンスの[`ObjCRuntime.Selector`](xref:ObjCRuntime.Selector)インスタンスを作成できます。

### <a name="calling-objc_msgsend"></a>Objc_msgSend の呼び出し

`objc_msgSend` は、メッセージ (セレクター) をオブジェクトに送信します。 この関数ファミリは、少なくとも2つの必須引数を受け取ります。セレクターターゲット (インスタンスまたはクラスハンドル)、セレクター自体、およびセレクターに必要な引数です。 インスタンスとセレクターの引数は `System.IntPtr`である必要があり、残りのすべての引数はセレクターが想定する型 (たとえば、`int`の `nint`、またはすべての `NSObject`派生型の `System.IntPtr` と一致する必要があります。 [`NSObject.Handle`](xref:Foundation.NSObject.Handle)を使用する
このプロパティを使用して、 Objective-C 型のインスタンスの `IntPtr` を取得します。

複数の `objc_msgSend` 関数があります。

- 構造体を返すセレクターには[`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)を使用します。 ARM では、これには、列挙型でも、C の組み込み型 (`char`、`short`、`int`、`long`、`float`、`double`) でもないすべての戻り値の型が含まれます。 X86 (シミュレーター) では、サイズが8バイトを超えるすべての構造体に対してこのメソッドを使用する必要があります (`CGSize` は8バイトで、シミュレーターで `objc_msgSend_stret` を使用しません)。 
- X86 でのみ浮動小数点値を返すセレクターには[`objc_msgSend_fpret`](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc)を使用します。 この関数を ARM で使用する必要はありません。代わりに、`objc_msgSend`を使用します。 
- Main [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)関数は、他のすべてのセレクターに対して使用されます。

呼び出す必要がある `objc_msgSend` 関数を決定したら (シミュレーターとデバイスにそれぞれ異なるメソッドが必要になる場合があります)、通常の[`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute)メソッドを使用して、後で呼び出す関数を宣言できます。

事前に定義された一連の `objc_msgSend` 宣言は、`ObjCRuntime.Messaging`にあります。

## <a name="different-invocations-on-simulator-and-device"></a>シミュレーターとデバイスでの異なる呼び出し

前に説明したように、Objective-C には 3 種類の `objc_msgSend` メソッドがあります。1 つは通常の呼び出し用、もう 1 つは浮動小数点値を返す呼び出し用 (x86 のみ)、もう 1 つは構造体の値を返す呼び出し用です。 後者には、`ObjCRuntime.Messaging`に `_stret` サフィックスが含まれています。

特定の構造体 (以下で説明するルール) を返すメソッドを呼び出す場合は、`out` 値として最初のパラメーターとして戻り値を指定してメソッドを呼び出す必要があります。

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

`_stret_` 方法を使用する場合のルールは、x86 と ARM で異なります。
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

ARM 用にビルドする場合は、 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)を使用します。
列挙型ではない値型、または列挙型のいずれかの基本型 (`int`、`byte`、`short`、`long`、`double`、`float`)。

X86 用にビルドする場合は、 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)を使用します。
列挙型ではない値型、または列挙体の基本型 (`int`、`byte`、`short`、`long`、`double`、`float`) ではなく、ネイティブサイズが8バイトを超える値型。

### <a name="creating-your-own-signatures"></a>独自の署名の作成

必要に応じて、次の[gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c)を使用して独自の署名を作成できます。

## <a name="related-links"></a>関連リンク

- [目標-C セレクターの](https://developer.xamarin.com/samples/mac-ios/Objective-C/)サンプル
