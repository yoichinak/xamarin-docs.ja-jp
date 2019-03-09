---
title: Xamarin.iOS の制限事項
description: このドキュメントでは、ジェネリック、NSObjects、汎用のオブジェクトでは、P/invoke などの汎用サブクラスについて説明する Xamarin.iOS の制限事項について説明します。
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/09/2018
ms.openlocfilehash: a6a4ef9fb36fde067fa58fec9a6206b1dbc1fbf0
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668349"
---
# <a name="limitations-of-xamarinios"></a>Xamarin.iOS の制限事項

Xamarin.iOS を使用してアプリケーションが静的コードにコンパイルされるため、実行時コード生成を必要とする機能を使用することはできません。

これらは、Mono デスクトップに比べて Xamarin.iOS の制限事項です。

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>制限付きのジェネリックのサポート

Mono と .NET の従来とは異なり、iPhone 上のコードは前もって必要に応じて、JIT コンパイラでコンパイルされているのではなく静的にコンパイルされます。

Mono の[AOT の完全な](https://www.mono-project.com/docs/advanced/aot/#full-aot)テクノロジ ジェネリックに関していくつかの制限には、これらはすべてジェネリック インスタンス化を事前コンパイル時に決定できるためが発生します。 コードのコンパイル時にコンパイラで、だけを使用して常に、通常の .NET または Mono ランタイムの問題はありません。 これにより、Xamarin.iOS のような静的コンパイラにとっての課題です。

開発者が直面する一般的な問題のいくつか挙げます。

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>ジェネリック NSObjects サブクラスは制限されています

Xamarin.iOS には、現在の状態で、ジェネリック メソッドのサポートなしなど、NSObject クラスの汎用サブクラスを作成するためのサポートが限られています。 7.2.1、時点では、NSObjects の汎用サブクラスを使用可能であれば、次のようなです。

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> NSObjects の汎用サブクラスが可能ですが、いくつかの制限があります。 読み取り、 [NSObject の](~/ios/internals/api-design/nsobject-generics.md)詳細については、ドキュメント


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>動的なコードを生成しません。

IOS カーネルにより、コードを動的に生成するアプリケーションであるために、Xamarin.iOS は、何らかの動的なコード生成をサポートしていません。 不足している機能には次が含まれます。

-  System.Reflection.Emit は使用できません。
-  System.Runtime.Remoting はサポートされていません。
-  型の動的作成はサポートされていません (ありません Type.GetType (以下"MyType"1")) が、既存の型 (Type.GetType ("System.String") うまく機能など) を検索します。 
-  逆方向のコールバックは、コンパイル時に、ランタイムを登録する必要があります。


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

System.Reflection の欠如。 **出力**ランタイム コードの生成に依存するコードが動作しないことを意味します。 これなどが含まれます。

-  動的言語ランタイム。
-  任意の言語は、動的言語ランタイムの上に構築します。
-  リモート処理の TransparentProxy、またはその他のコードを動的に生成する実行時になるもの。 


 **重要:** 混同しないでください**Reflection.Emit**で**リフレクション**します。 Reflection.Emit はコードを動的に生成する詳細については、その jit 処理コードとネイティブにコンパイルされたコードがあります。 IOS (JIT コンパイルなし) で制限があるためこれはサポートされていません。

全体のリフレクション API など Type.GetType ("someClass") を一覧表示するメソッド、プロパティを一覧表示する属性と値をフェッチしていますが問題なく動作します。

### <a name="using-delegates-to-call-native-functions"></a>ネイティブ関数を呼び出すデリゲートの使用

C# のデリゲートを使用してネイティブ関数を呼び出すには、次の属性のいずれかのデリゲートの宣言を装飾する必要があります。

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (推奨、クロス プラットフォームと .NET Standard 1.1 以降と互換性があるため)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

これらの属性のいずれかの指定に失敗するなどのランタイム エラーが発生します。

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>逆方向のコールバック

標準の Mono でを c# のデリゲートのインスタンスを代わりに関数ポインターをアンマネージ コードに渡すことができます。 ランタイムでは、マネージ コードにコールバックするアンマネージ コードを許可する小さなサンクにこれらの関数ポインターを変換は通常します。

ジャストイン タイムで Mono でこれらのブリッジが実装されているコンパイラ。 ときに、時間の先行コンパイラを使用して必要な iPhone でこの時点では 2 つの重要な制限があります。

-  すべてのコールバック メソッドでフラグを設定する必要があります、 [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  メソッドが静的メソッドにする必要はありませんサポートのインスタンス メソッド。 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>なしのリモート処理

リモート処理スタックは、Xamarin.iOS でご利用いただけません。


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>ランタイムの機能を無効になっています

Mono の iOS ランタイムでは、次の機能を無効になっています。

-  プロファイラー
-  Reflection.Emit
-  Reflection.Emit.Save 機能
-  COM のバインド
-  JIT エンジン
-  メタデータの検証ツール (JIT がないため)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API の制限事項

公開される .NET API は、iOS で使用できるわけではないが、完全なフレームワークのサブセットです。 FAQ を参照してください、[現在サポートされているアセンブリの一覧](~/cross-platform/internals/available-assemblies.md)します。



具体的には、外部 XML ファイルを使用して、ランタイムの動作を構成することはできませんので、Xamarin.iOS で使用される API プロファイルは、System.Configuration を含まれません。
