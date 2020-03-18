---
title: Objective-C バインドの概要
description: このドキュメントでは、コマンド ライン バインド、バインド プロジェクト、Objective Sharpie など、Objective-C コードの C# バインドを作成するさまざまな方法の概要について説明します。 また、バインドのしくみについても説明します。
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: be2f7f555b76d472f7a66d95e661bb2f5884c58f
ms.sourcegitcommit: 60d2243809d8e980fca90b9f771e72f8c0e64d71
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "76725342"
---
# <a name="overview-of-objective-c-bindings"></a>Objective-C バインドの概要

"_バインド プロセスのしくみの詳細_"

Xamarin で使用する Objective-C ライブラリをバインドするには、次の 3 つの手順を実行します。

1. .NET でどのようにネイティブ API が公開されるか、また、それがどのように基底の Objective-C にマップされるかを記述するために、C# "API 定義" を記述します。 これは、`interface` のような標準 C# 構成体と、さまざまなバインド**属性**を使用して行われます (この[簡単な例](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)を参照してください)。

2. C# で "API 定義" を記述したら、それをコンパイルして "バインド" アセンブリを生成します。 これは、[**コマンド ライン**](#commandline)で、または Visual Studio for Mac または Visual Studio の[**バインド プロジェクト**](#bindingproject)を使用して実行できます。

3. その後、その "バインド" アセンブリが Xamarin アプリケーション プロジェクトに追加されます。これにより、定義した API を使用してネイティブ機能にアクセスできます。
   バインド プロジェクトは、アプリケーション プロジェクトから完全に分離されています。

   > [!NOTE]
   > 手順 1 は、[**Objective Sharpie**](#objectivesharpie) を使用して自動化できます。 これは、Objective-C API を調べ、提案の C# "API 定義" を生成します。 Objective Sharpie によって作成されたファイルをカスタマイズし、それをバインド プロジェクト(またはコマンド ライン)で使用して、バインド アセンブリを作成できます。 Objective Sharpie は、それ自体でバインドを作成するのではなく、より大きなプロセスのオプションの一部にすぎません。

また、バインドを記述するのに役立つ[そのしくみ](#howitworks)の技術的な詳細を参照することもできます。

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>コマンド ライン バインド

Xamarin.iOS の `btouch-native` (または Xamarin.Mac を使用している場合は `bmac-native`) を使って、直接バインドを構築することができます。 これは、手動で (または Objective Sharpie を使用して) 作成した C# API 定義 をコマンド ライン ツール (iOS の場合は`btouch-native`、Mac の場合は `bmac-native`) に渡すことによって機能します。

これらのツールを呼び出すための一般的な構文は次のとおりです。

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

上記のコマンドによって、現在のディレクトリに `cocos2d.dll` ファイルが生成されます。これには、お使いのプロジェクトで使用できる完全にバインドされたライブラリが含まれます。 これは、バインド プロジェクト([下記](#bindingproject)参照) を使用する場合に Visual Studio for Mac がバインドの作成に使用するツールです。

<a name="bindingproject" />

## <a name="binding-project"></a>バインド プロジェクト

バインド プロジェクトは、Visual Studio for Mac または Visual Studio (Visual Studio は iOS バインドのみをサポートします) で作成することができ、(コマンド ラインを使用する場合と比べて) バインド の API 定義の編集と構築が容易になります。

この[使用開始ガイド](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started)に従って、バインド プロジェクトを作成および使用してバインドを生成する方法を確認してください。

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objective Sharpie

Objective Sharpie は、もう 1 つの別のコマンド ライン ツールであり、バインドの作成の初期段階に役立ちます。 これは、それ自体でバインドを作成するのではなく、ターゲットのネイティブ ライブラリの API 定義を生成する最初の手順を自動化します。

ネイティブ ライブラリ、ネイティブ フレームワーク、CocoaPods を解析して、バインドに組み込むことができる API 定義にする方法については、[Objective Sharpie に関するドキュメント](~/cross-platform/macios/binding/objective-sharpie/index.md)を参照してください。

<a name="howitworks" />

## <a name="how-binding-works"></a>バインドのしくみ

[[Register]](xref:Foundation.RegisterAttribute) 属性、[[Export]](xref:Foundation.ExportAttribute) 属性、[手動による Objective-C セレクターの呼び出し](~/ios/internals/objective-c-selectors.md)を一緒に使用して、新しい (以前にバインドされていない) Objective-C 型を手動でバインドすることができます。

最初に、バインドする型を見つけます。 説明 (と簡潔さ) のために、[NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator) 型をバインドします (これは既に [Foundation.NSEnumerator](xref:Foundation.NSEnumerator) にバインドされています。以下の実装は単なる例です)。

次に、C# 型を作成する必要があります。 これを名前空間に配置したい場合があります。Objective-C では名前空間がサポートされていないため、`[Register]` 属性を使用して、Xamarin.iOS が Objective-C ランタイムに登録する型名を変更する必要があります。 C# 型は [Foundation.NSObject](xref:Foundation.NSObject) から継承する必要もあります。

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

3 番目に、Objective-C のドキュメントを確認して、使用するセレクターごとに [ObjCRuntime.Selector](xref:ObjCRuntime.Selector) インスタンスを作成します。 これらをクラス本体内に配置します。

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

4 番目に、使用する型ではコンストラクターを提供する必要があります。 コンストラクターの呼び出しを基底クラスのコンストラクターにチェーンする "*必要があります*"。 `[Export]` 属性により、Objective-C コードは指定されたセレクター名のコンストラクターを呼び出すことができます。

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

5 番目に、手順 3 で宣言した各セレクターのメソッドを指定します。 これらは、`objc_msgSend()` を使用して、ネイティブ オブジェクトのセレクターを呼び出します。 `IntPtr` を適切に型指定された `NSObject` (サブ) 型に変換するために [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*) が使用されていることに注意してください。 このメソッドを Objective-C コードから呼び出せるようにするには、メンバーを**仮想**にする "*必要があります*"。

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

すべてを組み合わせると次のようになります。

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
