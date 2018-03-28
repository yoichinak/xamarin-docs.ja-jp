---
title: 制限事項
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c099797f0687f198ed220c1bd366bd93ab6c6e99
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="limitations"></a>制限事項

Xamarin.iOS を使用して iPhone 上のアプリケーションが静的コードにコンパイルされるので、実行時にコード生成を必要とするすべての機能を使用することはできません。

デスクトップ モノラルと比較して、Xamarin.iOS 制限事項を次に示します。

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>限られたジェネリックのサポート

従来の Mono/.NET とは異なり、iPhone 上のコードは、前もって JIT コンパイラによって要求時にコンパイルされるのではなく静的にコンパイルされます。

モノラルの[完全 AOT](http://www.mono-project.com/docs/advanced/aot/#full-aot)テクノロジ ジェネリックに関していくつかの制限には、これらはすべてジェネリック インスタンス化はコンパイル時に事前に決定できるため、発生します。 コードのコンパイル時のコンパイラで、のみを使用して実行時に常に、通常の .NET またはモノラル ランタイムの問題はありません。 このするという難題 Xamarin.iOS などの静的コンパイラ。

開発者が遭遇する一般的な問題のものがあります。

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>ジェネリック NSObjects サブクラスが制限されます。

Xamarin.iOS 現在に対するサポートが制限などのジェネリック メソッドはサポートされていません、NSObject クラスの汎用のサブクラスを作成します。 7.2.1、時点では、NSObjects の汎用のサブクラスを使用可能であれば、次のようです。

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> NSObjects の汎用的なサブクラスが可能ですが、いくつかの制限があります。 読み取り、 [NSObject の汎用サブクラス](~/ios/internals/api-design/nsobject-generics.md)詳細についてはドキュメント



### <a name="pinvokes-in-generic-types"></a>ジェネリック型で P/呼び出し

ジェネリック クラスの P/invoke はサポートされていません。

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo null 許容型ではサポートされていません

リフレクションの Property.SetInfo を使用して、null 許容の値に設定する&lt;T&gt;は現在サポートされていません。

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>ディクショナリのキーと値の型

値の型を使用してディクショナリとして&lt;TKey, TValue&gt;キーが問題のある、既定値として、ディクショナリのコンス トラクターが EqualityComparer の使用を試みます&lt;TKey&gt;です。既定値です。 EqualityComparer&lt;TKey&gt;です。既定では、さらにしようとすると、IEqualityComparer を実装する新しい型をインスタンス化にリフレクションを使用して&lt;TKey&gt;インターフェイスです。

これは参照型の機能 (、リフレクションを新規作成 + 型手順はスキップされます)、クラッシュの種類し、デバイス上で使用しようとしました。 後ではなくすばやく焼き付けるが値。

 **回避策**: 手動で実装する、 [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/)新しい型のインターフェイスし、その種類のインスタンスを提供、[ディクショナリ&lt;TKey、TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/)コンス トラクターです。


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>動的なコードを生成しません。

IPhone のカーネルにより、アプリケーション コードを動的に生成するため、iPhone の Mono は、何らかの動的なコード生成をサポートしていません。 次の設定があります。

-  System.Reflection.Emit は使用できません。
-  System.Runtime.Remoting はサポートされていません。
-  型の動的作成はサポートされていません (なしで、Type.GetType (以下"MyType"1")) が、既存の型 (で、Type.GetType ("System.String") などのはうまく) を検索します。 
-  逆方向のコールバックは、コンパイル時に、ランタイムに登録する必要があります。


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

System.Reflection の欠如。 **出力**ランタイム コードの生成に依存するコードが動作しないことを意味します。 これなどが含まれます。

-  動的言語ランタイム。
-  言語では、動的言語ランタイムの上に構築します。
-  リモート処理の TransparentProxy、またはその他の要素が発生するコードを動的に生成するランタイム。 


 **重要:**と混同しないでください**Reflection.Emit**で**リフレクション**です。 Reflection.Emit は、コードを動的に生成するについて説明して、jit 処理のコードとネイティブ コードにコンパイルします。 IPhone (JIT コンパイルされません) の制限によりこれがサポートされていません。

で、Type.GetType ("someClass")、メソッドを一覧表示する属性と値をフェッチするプロパティを一覧表示を含む、全体のリフレクション API は正常に機能します。

 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>コールバックを取り消す

標準モノラルを c# デリゲート インスタンスを代わりに、関数ポインターのアンマネージ コードに渡すことはできます。 ランタイムは、により、マネージ コードにコールバックするアンマネージ コードを小さなサンクにこれらの関数ポインターを変換すると通常します。

モノラルでこれらのブリッジは、ジャスト イン タイムによって実装されるコンパイラです。 ときに、時間の先行コンパイラを使用して必要な iPhone でこの時点では 2 つの重要な制限があります。

-  使用してコールバック メソッドのすべてのフラグを設定する必要があります、 [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  メソッドが静的メソッドである必要はありませんサポートのインスタンス メソッドです。 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>ないリモート処理

リモート処理スタックでは、Xamarin.iOS で使用できません。


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>実行時に、機能が無効になっています

モノラルの iOS ランタイムでは、次の機能を無効になっています。

-  プロファイラー
-  Reflection.Emit
-  Reflection.Emit.Save 機能
-  COM のバインド
-  JIT エンジン
-  メタデータの検証ツール (JIT がないため)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API の制限事項

公開される .NET API は、iOS で使用できるわけではないが、完全なフレームワークのサブセットです。 FAQ を参照してください、[現在サポートされているアセンブリのリスト](~/cross-platform/internals/available-assemblies.md)です。



具体的には、外部 XML ファイルを使用して、ランタイムの動作を構成することはできませんので、Xamarin.iOS で使用される API のプロファイルには、System.Configuration は含みません。
