---
title: Objective-C 開発者向けの C# Primer
description: このドキュメントでは、Objective-C 開発者向けに C# について説明します。 プロトコルとインターフェイス、カテゴリと拡張メソッド、フレームワークとアセンブリ、セレクターと名前付きパラメーターなどを考察しながら、2 つの言語を比較します。
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: df477dc0e4708a1d309810b5b8d4f755f3c49afb
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669818"
---
# <a name="c-primer-for-objective-c-developers"></a>Objective-C 開発者向けの C# Primer

_Xamarin.iOS では、C# で記述されたプラットフォームに依存しないコードをプラットフォーム間で共有できます。ただし、既存の iOS アプリケーションは既に作成されている Objective-C コードを活用できます。この記事は、Xamarin と C# 言語への移行を検討している Objective-C 開発者向けの簡単な入門となります。_

Objective-C で開発された iOS および OS X アプリケーションは、プラットフォーム固有のコードが必要ない場所で C# を活用してこのようなコードを Apple 以外のデバイスで使用できるようにすることで、Xamarin のメリットを得られます。 Web サービス、JSON と XML の解析、カスタム アルゴリズムなどをクロスプラットフォーム方式で使用できます。

Objective-C の既存の資産を維持しながら Xamarin を利用するには、バインディングと呼ばれる Xamarin のテクノロジでこれを C# に公開できます。バインディングは、Objective-C コードをマネージドな C# の世界に公開します。 また、必要な場合はコードを行ごとに C# に移植することもできます。 ただし、バインディングと移植のどちらのアプローチであっても、既存の Objective-C コードを Xamarin.iOS で効果的に活用するには Objective-C と C# の知識がある程度必要です。

## <a name="objective-c-interop"></a>Objective-C の相互運用

現在、Xamarin.iOS を使用して Objective-C から呼び出すことができる C# のライブラリを作成するメカニズムはサポートされていません。 その主な理由は、バインディングに加えて Mono ランタイムも必要であるためです。 ただし、ユーザー インターフェイスを含め、Objective-C のロジックの大部分を作成できます。 これを行うには、ライブラリに Objective-C コードをラップし、それに対するバインディングを作成します。 Xamarin.iOS はアプリケーションのブートストラップに必要です (つまり、`Main` エントリ ポイントを作成する必要があります)。 その後、他のロジックを Objective-C に作成して、バインディングによって (または P/Invoke を使用して) C# に公開されます。 これにより、Objective-C にプラットフォーム固有のロジックを保持し、C# でプラットフォームに依存しない部分を開発できます。

この記事は、両方の言語の主要な類似点の一部に注目すると共に、いくつかの違いを比較しており、既存の Objective-C コードへのバインディングか C# への移植かにかかわらず、Xamarin.iOS を使用する C# に移行する際の入門として利用できます。

バインディングの作成について詳しくは、[Objective-C のバインディング](~/ios/platform/binding-objective-c/index.md)に関する記事に記載されている他のドキュメントをご覧ください。

## <a name="language-comparison"></a>言語の比較

Objective-C と C# は、構文とランタイムの観点からはまったく異なる言語です。 Objective-C は動的言語であり、メッセージ パッシング スキームを使用します。一方、C# は静的に型指定されます。 構文の観点では、Objective-C は Smalltalk に似ており、C# はその基本的な構文の多くが Java から派生していますが、ここ数年で成熟し、Java を超える多くの機能が含まれています。

とは言うものの、Objective-C と C# のいくつかの言語機能は機能的に似ています。 C# から Objective-C コードへのバインディングを作成する場合、または Objective-C を C# に移植する場合は、こうした類似性を理解することが役立ちます。

### <a name="protocols-vs-interfaces"></a>プロトコルとインターフェイス

Objective-C と C# はどちらも単一継承言語です。 ただし、どちらの言語も、特定のクラスでの複数のインターフェイスの実装をサポートしています。 Objective-C では、これらの論理インターフェイスは "*プロトコル*" と呼ばれ、C# では "*インターフェイス*" と呼ばれます。 実装の観点からは、C# インターフェイスと Objective-C プロトコルの主な違いは、Objective-C ではオプションのメソッドを使用できることです。 詳しくは、「[Events, Delegates and Protocols](~/ios/app-fundamentals/delegates-protocols-and-events.md)」(イベント、デリゲート、プロトコル) の記事をご覧ください。

### <a name="categories-vs-extension-methods"></a>カテゴリと拡張メソッド

Objective-C では、"*カテゴリ*" を使用して、実装コードがないクラスにメソッドを追加できます。 C# では、"*拡張メソッド*" と呼ばれるものを通じて同様の概念を利用できます。

拡張メソッドでは、クラスに静的メソッドを追加できます。C# の静的メソッドは Objective-C のクラス メソッドに似ています。 たとえば、次のコードは `ScrollToBottom` という名前のメソッドを `UITextView` クラスに追加します。このクラスは UIKit から Objective-C の `UITextView` クラスにバインドされているマネージド クラスです。

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

その後、`UITextView` のインスタンスがコードに作成されると、メソッドは次に示すようにオートコンプリートの一覧で使用可能になります。

 ![](primer-images/01-extensionmethodintellisense.png "オートコンプリートで使用できるメソッド")

拡張メソッドが呼び出されるときに、インスタンスはこの例の `textView` のような引数に渡されます。

### <a name="frameworks-vs-assemblies"></a>フレームワークとアセンブリ

Objective-C は、関連するクラスをフレームワークと呼ばれる特殊なディレクトリにパッケージ化します。 一方、C# と .NET では、アセンブリはプリコンパイルされたコードの再利用可能な部分を提供するために使用されます。 iOS の外部での環境では、アセンブリには、実行時に Just-In-Time (JIT) でコンパイルされる中間言語コード (IL) が含まれています。 ただし、Apple は iOS アプリケーションで JIT を許可していません。 そのため、Xamarin を使用する iOS をターゲットとする C# コードは Ahead Of Time (AOT) でコンパイルされ、アプリケーション バンドルに含まれるメタデータ ファイルと共に単一の UNIX 実行可能ファイルを生成します。

### <a name="selectors-vs-named-parameters"></a>セレクターと名前付きパラメーター

Objective-C メソッドでは、その性質上、セレクターにパラメーターが含まれます。 たとえば、`AddCrayon:WithColor:` などのセレクターは、各パラメーターがコードで使用される場合に何を意味するかを明確にします。 C# はオプションで名前付き引数もサポートします。

たとえば、名前付き引数を使用した C# の同様のコードは次のようになります。

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

C# 言語ではこのサポートがバージョン 4.0 で追加されましたが、実際にはめったに使用されません。 ただし、コードで明示する必要がある場合は、これでサポートされます。

### <a name="headers-and-namespaces"></a>ヘッダーと名前空間

C のスーパー セットである Objective-C は、実装ファイルから独立したパブリック宣言用のヘッダーを使用します。 C# では、ヘッダー ファイルは使用されません。 Objective-C とは異なり、C# コードは名前空間に含まれます。 一部の名前空間で使用可能なコードを含める場合は、実装ファイルの先頭にディレクティブを使用して追加するか、完全な名前空間で型を修飾します。

たとえば、次のコードには `UIKit` 名前空間が含まれているため、その名前空間内のすべてのクラスを実装に使用できます。

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

また、上記のコードの namespace キーワードは、実装ファイル自体に対して使用する名前空間を設定します。 複数の実装ファイルが同じ名前空間を共有する場合、ディレクティブの使用に名前空間を含める必要はありません。暗黙的に指定されます。

### <a name="properties"></a>プロパティ

Objective-C と C# の両方に、アクセサー メソッドに関連する高度な抽象化を提供するプロパティの概念があります。 Objective-C では、@property コンパイラ ディレクティブはアクセサー メソッドを効果的に生成するために使用されます。 これに対し、C# には、言語自体にプロパティのサポートが含まれます。 次の例に示すように、C# プロパティは、バッキング フィールドにアクセスする長いスタイルを使用するか、より短い自動プロパティ構文を使用して実装できます。

```csharp
// automatic property syntax
public string Name { get; set; }

// property implemented with a backing field
string address;
public string Address {
    get {
        // could add additional code here
        return address;
    }
    set {
        address = value;
    }
}
```

### <a name="static-keyword"></a>Static キーワード

*static* キーワードの意味は、Objective-C と C# とで大きく異なります。 Objective-C の静的関数は、関数のスコープを現在のファイルに制限するために使用されます。 一方、C# では、スコープは *public*、*private*、*internal* キーワードを通じて管理されます。

Objective-C の変数に static キーワードが適用される場合、変数は関数呼び出し間でその値を保持します。

C# にも static キーワードがあります。 メソッドに適用する場合は、事実上、Objective-C での `+` 修飾子と同じ効果があります。 つまり、クラス メソッドを作成します。 同様に、フィールド、プロパティ、イベントなどの他のコンストラクトに適用される場合は、その型の任意のインスタンスではなく、そのコンストラクトを宣言した型の一部になります。 静的クラスにすることもできます。その場合、クラスで定義されているすべてのメソッドが静的である必要があります。

### <a name="nsarray-vs-list-initialization"></a>NSArray とリストの初期化

Objective-C には、`NSArray` で使用されるリテラル構文が含まれるようになり、初期化が簡単になっています。 一方、C# には `List` と呼ばれる "*汎用的*" で高度な型があります。このため、リストに保持する型は、リストを作成するコード (C++ のテンプレートのように) で提供できます。 さらに、リストでは、次に示すように自動初期化構文がサポートされます。

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>ブロックとラムダ式

Objective-C は、"*ブロック*" を使用してクロージャを作成します。クロージャでは、そこで囲まれている状態を使用できる関数をインラインで作成できます。 C# には、ラムダ式を使用する同様の概念があります。 C# では、ラムダ式は次のように `=>` 演算子で作成されます。

```csharp
(args) => {
    //  implementation code
};
```

ラムダ式について詳しくは、Microsoft の [C# プログラミング ガイド](https://msdn.microsoft.com/library/vstudio/bb397687.aspx)のページをご覧ください。

## <a name="summary"></a>まとめ

この記事では、Objective-C と C# 間でさまざまな言語機能を比較しました。 場合によっては、ラムダ式とブロックや、拡張メソッドとカテゴリなど、両方の言語に存在する類似機能について説明しました。 さらに、C# の名前空間と static キーワードの意味など、言語間で異なる部分を比較しました。
