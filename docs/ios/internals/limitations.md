---
title: Xamarin. iOS の制限事項
description: このドキュメントでは、Xamarin の iOS の制限事項、ジェネリックの説明、NSObjects の汎用サブクラス、ジェネリックオブジェクトでの P/Invoke などについて説明します。
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/09/2018
ms.openlocfilehash: 83c71ebf844102a7d3a16969868f187237fb0d04
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753333"
---
# <a name="limitations-of-xamarinios"></a>Xamarin. iOS の制限事項

Xamarin を使用するアプリケーションは静的コードにコンパイルされるため、実行時にコード生成を必要とする機能を使用することはできません。

次に示したのは、デスクトップの Mono と比較した、iOS の制限事項です。

 <a name="Limited_Generics_Support" />

## <a name="limited-generics-support"></a>制限付きジェネリックのサポート

従来の Mono/.NET とは異なり、iPhone 上のコードは JIT コンパイラによって要求時にコンパイルされるのではなく、事前に静的にコンパイルされます。

Mono の[フル AOT](https://www.mono-project.com/docs/advanced/aot/#full-aot) テクノロジでは、ジェネリックに関していくつかの制限があります。これは、コンパイル時にすべてのジェネリック インスタンスを事前に決定できるわけではないためです。このコードは常に Just-In-Time コンパイラを使用して実行時にコンパイルされるので、通常の .NET または Mono ランタイムでは問題になりません。しかし、これにより、Xamarin.iOS のような静的コンパイラの課題が生じます。

開発者が実行する一般的な問題には、次のようなものがあります。

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />

### <a name="generic-subclasses-of-nsobjects-are-limited"></a>NSObjects の汎用サブクラスは制限されています

現在、Xamarin.iOS では、ジェネリック メソッドのサポートがないなど、NSObject クラスの汎用サブクラスを作成するためのサポートが制限されています。7\.2.1 の場合、次のように NSObjects の汎用サブクラスを使用できます。

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> NSObjects の汎用サブクラスは可能ですが、いくつかの制限があります。 詳細については、 [NSObject ドキュメントの汎用サブクラス](~/ios/internals/api-design/nsobject-generics.md)を参照してください。

 <a name="No_Dynamic_Code_Generation" />

## <a name="no-dynamic-code-generation"></a>動的なコード生成なし

iOS カーネルでは、アプリケーションがコードを動的に生成できないようにするため、Xamarin は、どのような形式の動的コード生成もサポートしていません。不足している機能には次が含まれます。

- システムのリフレクションは使用できません。
- System.string はサポートされていません。
- 型を動的に作成することはサポートされていません (タイプ gettype ("MyType ' 1"))。ただし、既存の型 (たとえば、GetType ("System.string") は正常に動作します)。
- 逆コールバックは、コンパイル時にランタイムに登録する必要があります。

 <a name="System.Reflection.Emit" />

### <a name="systemreflectionemit"></a>System.Reflection.Emit

システムリフレクションの欠如。 **生成**とは、ランタイムコードの生成に依存するコードが動作しないことを意味します。 これには次のようなものがあります。

- 動的言語ランタイム。
- 動的言語ランタイムの上に構築されたすべての言語。
- リモート処理の TransparentProxy、またはランタイムがコードを動的に生成するその他のもの。

  > [!IMPORTANT]
  > リフレクションを混同**し**ないでください。**リフレクション**を使用して出力します。 リフレクションは、コードを動的に生成し、そのコードを Jit してネイティブコードにコンパイルします。 IOS の制限 (JIT コンパイルなし) のため、これはサポートされていません。

しかし、型 GetType ("someClass")、メソッドの一覧表示、プロパティの一覧表示、属性と値の取得などを含むリフレクション API 全体は正常に機能します。

### <a name="using-delegates-to-call-native-functions"></a>デリゲートを使用したネイティブ関数の呼び出し

デリゲートをC#使用してネイティブ関数を呼び出すには、デリゲートの宣言を次の属性のいずれかで修飾する必要があります。

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute)(推奨されるのは、クロスプラットフォームで、.NET Standard 1.1 以降と互換性があるためです)
- [MonoNativeFunctionWrapperAttribute](xref:ObjCRuntime.MonoNativeFunctionWrapperAttribute)

これらの属性のいずれかを指定しないと、次のような実行時エラーが発生します。

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```

 <a name="Reverse_Callbacks" />

### <a name="reverse-callbacks"></a>逆コールバック

標準 Mono では、関数ポインターの代わりに C# のデリゲート インスタンスをアンマネージ コードに渡すことができます。通常、ランタイムは、これらの関数ポインターを小さなサンクに変換し、アンマネージ コードがマネージ コードにコールバックできるようにします。

Mono では、これらのブリッジは Just-in-Time コンパイラによって実装されます。iPhone が必要とする Ahead Of Time コンパイラを使用する場合は、この時点で 2 つの重要な制限があります。

- [MonoPInvokeCallbackAttribute](xref:ObjCRuntime.MonoPInvokeCallbackAttribute)を使用して、すべてのコールバックメソッドにフラグを付ける必要があります。
- メソッドは静的メソッドである必要がありますが、インスタンスメソッドはサポートされていません。

<a name="No_Remoting" />

## <a name="no-remoting"></a>リモート処理なし

リモート処理スタックは、Xamarin.iOS では使用できません。

 <a name="Runtime_Disabled_Features" />

## <a name="runtime-disabled-features"></a>実行時に無効な機能

Mono の iOS ランタイムでは、次の機能が無効になっています。

- プロファイラー
- リフレクション。生成
- リフレクション. 生成. 保存機能
- COM バインディング
- JIT エンジン
- メタデータ検証機能 (JIT がないため)

 <a name=".NET_API_Limitations" />

## <a name="net-api-limitations"></a>.NET API の制限事項

公開されている .NET API は完全なフレームワークのサブセットであり、すべての iOS で利用できるわけではありません。 [現在サポートされているアセンブリの一覧](~/cross-platform/internals/available-assemblies.md)については、FAQ を参照してください。

具体的には、Xamarin.iOS で使用される API プロファイルには、System.Configuration が含まれていないため、外部 XML ファイルを使用してランタイムの動作を構成することはできません。
