---
title: 目的 C のバインドの概要
description: このドキュメントでは、コマンドラインバインド、バインドC#プロジェクト、目標マジックペンなど、目的の C コードのバインディングを作成するさまざまな方法の概要について説明します。 また、バインディングのしくみについても説明します。
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: be2f7f555b76d472f7a66d95e661bb2f5884c58f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725342"
---
# <a name="overview-of-objective-c-bindings"></a>目的 C のバインドの概要

_バインドプロセスのしくみの詳細_

Xamarin で使用する目的の C ライブラリをバインドするには、次の3つの手順を実行します。

1. C# 「Api 定義」を記述して、.net でのネイティブ API の公開方法と、基になる目標 (C) へのマッピングについて説明します。 これは、`interface` やC#さまざまなバインド**属性**などの標準の構成体を使用して行われます (この[簡単な例](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)を参照)。

2. 「」でC#"API 定義" を記述したら、それをコンパイルして "binding" アセンブリを生成します。 これを行うには、[**コマンドライン**](#commandline)を使用するか、Visual Studio for Mac または Visual Studio で[**バインドプロジェクト**](#bindingproject)を使用します。

3. その後、"binding" アセンブリが Xamarin アプリケーションプロジェクトに追加されます。これにより、定義した API を使用してネイティブ機能にアクセスできます。
   バインドプロジェクトは、アプリケーションプロジェクトと完全に分離されています。

   > [!NOTE]
   > 手順1は、[**目標マジックペン**](#objectivesharpie)を支援することで自動化できます。 このメソッドは、目的 C API を調べ、提案C#された "API 定義" を生成します。 マジックペンによって作成されたファイルをカスタマイズし、バインドプロジェクト (またはコマンドライン) で使用して、バインドアセンブリを作成できます。 目標マジックペンは、それ自体によってバインドを作成するのではなく、大きなプロセスのオプションの一部にすぎません。

また、[そのしくみ](#howitworks)の技術的な詳細を確認することもできます。これは、バインディングを記述するのに役立ちます。

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>コマンドラインバインド

(Xamarin. Mac を使用している場合は `bmac-native`) の `btouch-native` を使用して、バインドを直接構築できます。 これは、手動でC#作成した API 定義 (または目的のマジックペンを使用) をコマンドラインツール (iOS の場合は`btouch-native`、Mac の場合は `bmac-native`) に渡すことによって機能します。

これらのツールを呼び出すための一般的な構文は次のとおりです。

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

上のコマンドを実行すると、現在のディレクトリに `cocos2d.dll` ファイルが生成されます。このファイルには、プロジェクトで使用できる完全にバインドされたライブラリが含まれます。 これは、バインドプロジェクト ([以下](#bindingproject)で説明) を使用してバインドを作成するために Visual Studio for Mac 使用するツールです。

<a name="bindingproject" />

## <a name="binding-project"></a>プロジェクトのバインド

バインドプロジェクトは Visual Studio for Mac または Visual Studio で作成できます (Visual Studio は iOS バインドのみをサポートしています)。また、バインド用の API 定義を簡単に編集およびビルドできます (コマンドラインを使用する場合は)。

バインドプロジェクトを作成して使用してバインディングを作成する方法については、この[入門ガイド](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started)に従ってください。

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objective Sharpie

目標マジックペンはもう1つの独立したコマンドラインツールであり、バインドの作成の初期段階に役立ちます。 バインド自体は作成されません。代わりに、ターゲットのネイティブライブラリの API 定義を生成する最初の手順を自動化します。

ネイティブライブラリ、ネイティブフレームワーク、およびバインドに組み込むことができる API 定義を解析する方法については、[目標マジックペン docs](~/cross-platform/macios/binding/objective-sharpie/index.md)を参照してください。

<a name="howitworks" />

## <a name="how-binding-works"></a>バインドのしくみ

[[Register]](xref:Foundation.RegisterAttribute)属性、 [[Export]](xref:Foundation.ExportAttribute)属性、および[手動の目標 c セレクターの呼び出し](~/ios/internals/objective-c-selectors.md)を一緒に使用して、新しい (以前にバインドされていない) 目的 c の型を手動でバインドすることができます。

まず、バインドする型を見つけます。 ディスカッションの目的 (および簡潔さ) については、 [NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator)型をバインドします (これは既に[NSEnumerator](xref:Foundation.NSEnumerator)にバインドされています。以下の実装は単なる例です)。

次に、 C#型を作成する必要があります。 これを名前空間に配置することをお勧めします。目標 C では名前空間がサポートされていないため、`[Register]` 属性を使用して、Xamarin が目的の C ランタイムに登録する型名を変更する必要があります。 このC#型は、 [NSObject](xref:Foundation.NSObject)からも継承する必要があります。

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

第3に、使用するセレクターごとに、目的 C のドキュメントを確認し、 [Objcruntime](xref:ObjCRuntime.Selector)インスタンスを作成します。 クラス本体内に配置します。

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

4番目の型では、コンストラクターを指定する必要があります。 コンストラクターの呼び出しを基底クラスのコンストラクターにチェーンする*必要があり*ます。 `[Export]` 属性を使用すると、指定したセレクター名を持つコンストラクターを、目的の C コードで呼び出すことができます。

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

5番目の方法として、手順 3. で宣言した各セレクターに対してメソッドを指定します。 これらは、`objc_msgSend()` を使用して、ネイティブオブジェクトのセレクターを呼び出します。 [GetNSObject ()](xref:ObjCRuntime.Runtime.GetNSObject*)を使用して、適切に型指定された `NSObject` (サブ) 型に `IntPtr` を変換することに注意してください。 メソッドを目的の C コードから呼び出すことができるようにするには、メンバーが**仮想**である*必要があり*ます。

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

まとめ:

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
