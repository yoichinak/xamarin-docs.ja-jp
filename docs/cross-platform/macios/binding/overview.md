---
title: OBJECTIVE-C バインディングの概要
description: このドキュメントを作成するさまざまな方法の概要を説明するC#Objective C コード、コマンド ライン バインド、バインド プロジェクトの場合は、目的の油性などのバインド。 また、バインドの動作方法も説明します。
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: asb3993
ms.author: amburns
ms.date: 11/25/2015
ms.openlocfilehash: 3f15eaf9171ac44b870239fb5ffa14edd6210360
ms.sourcegitcommit: ee626f215de02707b7a94ba1d0fa1d75b22ab84f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54879304"
---
# <a name="overview-of-objective-c-bindings"></a>OBJECTIVE-C バインディングの概要

_バインディング プロセスのしくみの詳細_

Xamarin で使用するため、OBJECTIVE-C ライブラリのバインドは、3 つの手順は受け取ります。

1. 書き込みをC#"API の定義"方法について説明するのには、ネイティブ API は .NET、および基になる OBJECTIVE-C にマップする方法で公開 これは、標準を使用してC#などのコンストラクト`interface`とさまざまなバインド**属性**(これを参照してください[簡単な例](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API))。

2. 「API の定義」を作成するとC#、「バインド」アセンブリを生成するためにコンパイルします。 これで実行することができます、 [**コマンドライン**](#commandline)またはを使用して、 [**バインド プロジェクト**](#bindingproject) Visual Studio for Mac または Visual Studio でします。

3. その「バインド」アセンブリは、定義した API を使用して、ネイティブ機能にアクセスできるように、Xamarin アプリケーション プロジェクトに追加されます。
  バインド プロジェクトは、アプリケーション プロジェクトから完全に分離します。

**注:** 手順 1 のサポートで自動化できる[**目標油性**](#objectivesharpie)します。 Objective C API を確認し、提案された生成C#「API の定義」。 目標油性によって作成されたファイルをカスタマイズして、バインド プロジェクト (または、コマンドラインで) 使用することができます、バインド アセンブリを作成します。 目標油性が単独でバインドを作成していないより大きなプロセスのオプションの一部だけです。

技術的な詳細を参照することもできます。[しくみ](#howitworks)、を理解できると、バインドを作成します。

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>コマンド ライン バインド

使用することができます、 `btouch-native` Xamarin.iOS 用 (または`bmac-native`Xamarin.Mac を使用している場合) に直接バインドを作成します。 動作を渡すことによって、 C# API 定義を手動で作成した (または目標油性を使用して) コマンド ライン ツールに (`btouch-native` iOS 用または`bmac-native`for Mac)。


これらのツールを起動するための一般的な構文です。

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

上記のコマンドでは、ファイルを生成します。`cocos2d.dll`現在のディレクトリにし、プロジェクトで使用できる、完全にバインドされるライブラリが含まれます。 これは、Visual Studio for Mac を使用してバインド プロジェクトを使用する場合に、バインドを作成するツール (「[下](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>プロジェクトのバインド

バインド プロジェクトでは、for Mac または Visual Studio が (Visual Studio は、iOS のバインドをのみサポート)、Visual Studio で作成でき、ビルド (コマンドラインを使用) とバインドの API 定義を編集しやすきます。

この後に[ファースト ステップ ガイド](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started)を作成およびバインド プロジェクトを使用してバインディングを作成する方法を参照してください。

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objective Sharpie

油性の目標は、バインディングの作成の初期の段階を支援する、独立した別のコマンド ライン ツールです。 単独で、バインドは作成されませんではなく、対象のネイティブ ライブラリの API 定義の生成の最初のステップを自動化します。

読み取り、[目標油性 docs](~/cross-platform/macios/binding/objective-sharpie/index.md)に API 定義のバインディングを組み込まれたにネイティブ ライブラリやネイティブ フレームワークは、CocoaPods を解析する方法について説明します。

<a name="howitworks" />

## <a name="how-binding-works"></a>バインドのしくみ

使用することは、 [[登録]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/)属性、 [[エクスポート]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)属性、および[手動 OBJECTIVE-C セレクター呼び出し](~/ios/internals/objective-c-selectors.md)まとめて手動でバインドする新しい (以前Objective C 型のバインド解除) します。

最初に、バインドする型を検索します。 ディスカッションの目的 (とわかりやすくするため)、バインドします、 [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html)型 (でバインド既に[Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); 以下の実装が目的では例だけ)。

次に、作成する必要があります、C#型。 名前空間に配置するこのたいします可能性があります。使用する必要があります Objective C が名前空間をサポートしていないため、 `[Register]` Xamarin.iOS は、OBJECTIVE-C ランタイムを登録する型名を変更する属性。 C#型を継承する必要がありますも[Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

3 番目に、OBJECTIVE-C のドキュメントを確認し、作成[ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/)を使用する各セレクターのインスタンス。 これらのクラスの本文内に配置します。

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

4 番目に、型は、コンス トラクターを提供する必要があります。 *する必要があります*基底クラスのコンス トラクターに、コンス トラクターの呼び出しを連結します。 `[Export]`属性が指定されたセレクターの名前を持つコンス トラクターを呼び出す Objective C コードを許可します。

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

5 番目に、手順 3 で宣言されている、セレクターの各メソッドを提供します。 これらを使用して、`objc_msgSend()`をネイティブ オブジェクトのセレクターを呼び出します。 使用に注意してください[Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr))に変換する、`IntPtr`に適切に型指定された`NSObject`(サブ) 型。 Objective C コードで、メンバーから呼び出せるメソッド場合*する必要があります*する**仮想**します。

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

まとめ。

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```

## <a name="related-links"></a>関連リンク

- [Xamarin University のコース:OBJECTIVE-C バインディング ライブラリをビルド](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース:目標油性で、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)