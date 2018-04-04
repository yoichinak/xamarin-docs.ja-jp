---
title: 概要
description: バインディング プロセスのしくみの詳細
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: d1a90934cf7a9a832172f32ed95cf3e254e04385
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="overview"></a>概要

_バインディング プロセスのしくみの詳細_

Xamarin を使用するため、Objective C ライブラリのバインドは、3 つの手順の取得します。

1. 書き込む c#「API 定義」を記述する方法、ネイティブの API で公開されている .NET、および基になる目標 C. にマップする方法 これを行うように構築を使用して標準的な c#`interface`とさまざまなバインディング**属性**(これを参照してください[簡単な例](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API))。

2. 1 回コンパイル「バインド」アセンブリを生成するために、C# の場合は、「API 定義」を記述しました。 これで、 [**コマンドライン**](#commandline)またはを使用して、 [**バインド プロジェクト**](#bindingproject) Mac または Visual Studio の Visual Studio でします。

3. 定義した API を使用して、ネイティブの機能にアクセスできるように、その「バインド」アセンブリは、Xamarin アプリケーション プロジェクトに追加されます。
  バインド プロジェクトが、アプリケーション プロジェクトから完全に分離します。

**注:**の支援を受けて、手順 1 を自動化できる[**目標ペンを使わず**](#objectivesharpie)です。 Objective C API を確認し、提案された c#「API 定義」が生成されます。 目標ペンを使わずによって作成されたファイルをカスタマイズして (またはコマンド ラインで) バインド プロジェクトでは、それらを使用してバインド アセンブリを作成します。 目標ペンを使わずには、単独でバインドは作成されません、大規模なプロセスのオプションの一部だけです。

技術的な詳細を参照することもできます。[しくみ](#howitworks)、バインドを作成するするのに役立ちます。

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>コマンド ライン バインド

使用することができます、 `btouch-native` Xamarin.iOS の (または`bmac-native`Xamarin.Mac を使用している場合) を直接バインドを作成します。 手動で作成した c# API の定義を渡すことによって動作 (または目標ペンを使わずに使用) コマンド ライン ツールに (`btouch-native` iOS 用または`bmac-native`Mac 用)。


これらのツールを起動するための一般的な構文です。

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

上記のコマンドは、ファイルを生成`cocos2d.dll`現在のディレクトリにし、プロジェクトで使用できる、完全にバインドされるライブラリが含まれます。 これは、Visual Studio for Mac を使用してバインド プロジェクトを使用する場合に、バインドを作成するツール (説明[下](#bindingproject))。


<a name="bindingproject" />

## <a name="binding-project"></a>プロジェクトのバインド

バインド プロジェクトでは、Mac または (Visual Studio のみをサポートしている iOS バインド) Visual Studio の Visual Studio で作成することができ、API に対するビルド定義ではなく、コマンドラインを使用して) バインドを編集しやすきます。

この後に[ファースト ステップ ガイド](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started)作成およびバインドを生成するためにバインディング プロジェクトを使用する方法を確認します。

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>目標ペンを使わず

目標ペンを使わずは、バインディングの作成の初期の段階を支援する、別の別のコマンド ライン ツールです。 単独で、バインドは作成されませんではなく、対象のネイティブ ライブラリの API の定義を生成する最初の手順を自動化します。

読み取り、[目標ペンを使わず docs](~/cross-platform/macios/binding/objective-sharpie/index.md)バインドを組み込まれた API 定義にネイティブ ライブラリ、ネイティブ フレームワーク、および CocoaPods を解析する方法を学習します。

<a name="howitworks" />

## <a name="how-binding-works"></a>バインディングのしくみ

使用することは、 [[登録]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/)属性、 [[エクスポート]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)属性、および[手動 OBJECTIVE-C セレクター呼び出し](~/ios/internals/objective-c-selectors.md)一緒に手動でバインドする新しい (以前バインドされていない) Objective C の型。

最初に、バインドする型を検索します。 説明の目的 (およびわかりやすくするため)、バインドを[NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html)型 (に既にバインドされている[Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); 実装の例は例だけを目的)。

次に、c# の型を作成する必要があります。 名前空間にこれを配置することがお使用する必要があります Objective C が名前空間をサポートしていないため、 `[Register]` Xamarin.iOS は Objective C のランタイムに登録した型名を変更する属性。 C# の型を継承する必要がありますも[Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

3 番目、Objective C のドキュメントを確認し、作成[ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/)各セレクターを使用する場合のインスタンス。 クラスの本体内でこれらを配置します。

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

4 番目の種類は、コンス トラクターを提供する必要があります。 *必要があります*基底クラス コンス トラクターに、コンス トラクターの呼び出しを連結します。 `[Export]`属性が指定されたセレクターの名前を持つコンス トラクターを呼び出す Objective C コードを許可します。

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

5 番目に、手順 3. で宣言されている、セレクターの各メソッドを提供します。 これらを使用して、`objc_msgSend()`ネイティブ オブジェクトで、セレクターを起動します。 使用に注意してください[Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr))に変換する、`IntPtr`に適切に型指定された`NSObject`(sub) 型です。 Objective C コードで、メンバーから呼び出せるようにするメソッドの場合*必要があります*する**仮想**です。

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

