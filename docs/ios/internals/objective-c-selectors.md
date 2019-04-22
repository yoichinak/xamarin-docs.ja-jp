---
title: Xamarin.iOS での OBJECTIVE-C セレクター
description: このドキュメントでは、c# から OBJECTIVE-C セレクターと対話する方法について説明します。 これには、セレクターとその際に考慮する必要がありますが、技術的な考慮事項を呼び出す方法について説明します。
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/12/2017
ms.openlocfilehash: 15db59945f482728f760006095e294bc5628c8bd
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870177"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin.iOS での OBJECTIVE-C セレクター

Objective C の言語がに基づいて*セレクター*します。 セレクターがオブジェクトに送信できるメッセージまたは*クラス*します。 [Xamarin.iOS](~/ios/internals/api-design/index.md)マップのインスタンス、インスタンス メソッドへのセレクターおよびクラスの静的メソッドへのセレクター。

通常の C 関数とは異なり、C++ メンバー関数と同様に) 呼び出すことができません直接セレクターを使用して、 [P/invoke](https://www.mono-project.com/docs/advanced/pinvoke/)セレクターが代わりに、OBJECTIVE-C のクラスに送信されるか、インスタンスを使用して、 [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
関数。

Objective C でのメッセージの詳細については、Apple の参照してください[オブジェクトの操作](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2)ガイド。

## <a name="example"></a>例

起動すると、 [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
セレクターに[ `NSString`](https://developer.apple.com/documentation/foundation/nsstring)します。
(Apple のドキュメント) から宣言です。

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

この API には、次の特徴があります。

- 戻り値の型は`CGSize`Unified api。
- `font`パラメーターが、 [UIFont](xref:UIKit.UIFont) (型 () から間接的に派生と[NSObject](xref:Foundation.NSObject)にマップし、 [System.IntPtr](xref:System.IntPtr)。
- `width`パラメーター、`CGFloat`にマップされて`nfloat`します。
- `lineBreakMode`パラメーター、 [ `UILineBreakMode` ](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc)、として Xamarin.iOS で既にバインドされているが、 [`UILineBreakMode`](xref:UIKit.UILineBreakMode)
列挙体です。

正規表現のまとめ、`objc_msgSend`宣言に一致する必要があります。

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

このメソッドを呼び出すには、次のようにコードを使用します。

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

返される値は、サイズが 8 バイト未満であった構造体をされていた (などの古い`SizeF`Unified Api に切り替える前に使用)、デバイスでクラッシュしたが、シミュレーターで実行は、上記のコード。 サイズの 8 ビット未満の値を返すセレクターを呼び出すには、宣言、`objc_msgSend_stret`関数。

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

このメソッドを呼び出すには、次のようにコードを使用します。

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

## <a name="invoking-a-selector"></a>セレクターの起動

セレクターを呼び出すと、3 つの手順があります。

1. 選択対象を取得します。
2. セレクターの名前を取得します。
3. 呼び出す`objc_msgSend`適切な引数を使用します。

### <a name="selector-targets"></a>セレクターのターゲット

セレクターをターゲットとは、オブジェクトのインスタンスまたは OBJECTIVE-C クラスのいずれかです。 ターゲットがインスタンスであり、バインドされた Xamarin.iOS 型の取得元である場合は、使用、 [ `ObjCRuntime.INativeObject.Handle` ](xref:ObjCRuntime.INativeObject.Handle)プロパティ。

ターゲットが、クラスの場合を使用して、 [ `ObjCRuntime.Class` ](xref:ObjCRuntime.Class)クラスのインスタンスへの参照を取得するを使用し、 [ `Class.Handle` ](xref:ObjCRuntime.Class.Handle)プロパティ。

### <a name="selector-names"></a>セレクター名

セレクター名は、Apple のドキュメントに表示されます。 たとえば、 [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring?language=objc)が含まれています[ `sizeWithFont:` ](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc)と[ `sizeWithFont:forWidth:lineBreakMode:` ](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc)セレクター。 埋め込みおよび末尾のコロンは、セレクターの名前の一部であるし、省略することはできません。

セレクターの名前を使用すると、作成できます、 [ `ObjCRuntime.Selector` ](xref:ObjCRuntime.Selector)のインスタンス。

### <a name="calling-objcmsgsend"></a>Objc_msgSend を呼び出す

`objc_msgSend` オブジェクトには、(セレクター) メッセージを送信します。 このファミリの関数は、少なくとも 2 つの必要な引数を受け取ります。 セレクターのターゲット (インスタンスまたはクラスの処理)、自体、セレクターとセレクターの必要な引数。 インスタンスとセレクター引数には、 `System.IntPtr`、し、残りのすべての引数は、セレクターが必要ですが、たとえば型と一致する必要があります、`nint`の`int`、または`System.IntPtr`すべて`NSObject`-派生型。 使用して、 [`NSObject.Handle`](xref:Foundation.NSObject.Handle)
取得するプロパティ、 `IntPtr` Objective C 型のインスタンスにします。

1 つ以上を使用する必要がある`objc_msgSend`関数。

- 使用[ `objc_msgSend_stret` ](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)構造体を返すセレクター。 ARM では、これが含まれますすべて戻り値の型は列挙型ではない、または C の組み込み型のいずれか (`char`、 `short`、 `int`、 `long`、 `float`、 `double`)。 On x86 (シミュレーター) では、このメソッドはサイズが 8 バイトより大きいすべての構造に使用する必要があります (`CGSize` 8 バイトで、使用しない`objc_msgSend_stret`シミュレーターで)。 
- 使用[ `objc_msgSend_fpret` ](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc)のセレクターを返す、浮動小数点値を x86 のみにします。 この関数は、ARM; で使用する必要はありません。代わりに、`objc_msgSend`します。 
- メイン[objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)関数は、他のすべてのセレクターを使用します。

決まったら、`objc_msgSend`関数を呼び出す必要があります (シミュレーターおよびデバイス必要がありますそれぞれ別の方法)、通常を使用する[ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute)メソッドの後に呼び出す関数を宣言します。

一連の事前に作成された`objc_msgSend`で見つかる宣言`ObjCRuntime.Messaging`します。

## <a name="different-invocations-on-simulator-and-device"></a>デバイスとシミュレーターの別の呼び出し

上記で説明した、Objective C には 3 種類の`objc_msgSend`メソッド: 通常の呼び出し、浮動小数点値 (x86 のみ) を返す呼び出しおよび構造体の値を返す呼び出しのいずれか。 後者の場合、サフィックスを含む`_stret`で`ObjCRuntime.Messaging`します。

特定の構造体 (以下で説明するルール) を返すメソッドを呼び出す場合、最初のパラメーターとして戻り値を持つメソッドを呼び出す必要があります、`out`値。

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

ルールを使用する場合、 `_stret_` x86 と ARM ではメソッドが異なります。
バインディングが、シミュレーターとデバイスの両方で動作する場合は、次のようにコードを追加します。

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

### <a name="using-the-objcmsgsendstret-method"></a>Objc_msgSend_stret メソッドを使用してください。

ARM で作成するときに使用します [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
列挙型または列挙型の基本型のいずれかではない任意の値型の (`int`、 `byte`、 `short`、 `long`、 `double`、 `float`)。

X86 用のビルド時に使用します。 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
列挙型または列挙型の基本型のいずれかではない任意の値型の (`int`、 `byte`、 `short`、 `long`、 `double`、 `float`) とネイティブのサイズが 8 バイトを超えています。

### <a name="creating-your-own-signatures"></a>独自の署名を作成します。

次[gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c)独自の署名を作成するために必要な場合に使用できます。

## <a name="related-links"></a>関連リンク

- [OBJECTIVE-C セレクター](https://developer.xamarin.com/samples/mac-ios/Objective-C/)サンプル
