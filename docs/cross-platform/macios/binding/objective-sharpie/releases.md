---
title: 目標マジックペンリリース履歴
description: このドキュメントでは、目標ペンを使わず、C# Objective C コードへのバインドの作成を自動化するためのツールのリリース履歴について説明します。
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: conceptdev
ms.author: crdun
ms.date: 10/11/2017
ms.openlocfilehash: fa50ae16b69436936f0a7a8a5cf0aeaa54dfedfb
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765667"
---
# <a name="objective-sharpie-release-history"></a>目標マジックペンリリース履歴

## <a name="34-october-11-2017"></a>3.4 (2017 年10月11日)

[ダウンロード3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Xcode 9 のサポート: iOS 11、macOS 10.13、tvOS 11、および watchOS 4
* SIMD と tgmath.h> に関する問題が修正されました
* テレメトリが完全に削除されました

## <a name="33-august-3-2016"></a>3.3 (8 月3日、2016日)

[ダウンロード3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Xcode 8 Beta 4、iOS 10、macOS 10.12、tvOS 10 および watchOS 3 のサポート。
* 最新の Clang マスタービルド (2016-08-02) に更新されました
* [テレメトリの送信オプション](https://twitter.com/Symbiatch/status/760373403878559744)を`sharpie pod bind`から`sharpie bind`に保持します。

## <a name="32-june-14-2016"></a>3.2 (2016 年6月14日)

[ダウンロード3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Xcode 8 Beta 1、iOS 10、macOS 10.12、tvOS 10 および watchOS 3 のサポート。

## <a name="31-may-31-2016"></a>3.1 (2016 年5月31日)

[バージョン3.1.1 のダウンロード](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Cocoアポストロフィ Ds 1.0 のサポート
* 最初にネイティブ`.framework`を構築し、次にバインドすることによって、cocoアポストロフィ ds バインディングの信頼性を向上させました。
* Cocoアポストロフィ ds `.framework`とバインド定義を`Binding`ディレクトリにコピーして、xamarin. iOS および xamarin. Mac バインドプロジェクトとの統合を容易にする

## <a name="30-october-5-2015"></a>3.0 (2015 年10月5日)

[ダウンロード3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Xcode 7 で導入された軽量のジェネリックと null 値の許容を含む新しい目標 C 言語機能のサポート
* 最新の iOS および Mac Sdk がサポートされます。
* Xcode プロジェクトのビルドと解析がサポートされるようになりました。 これで、完全な Xcode プロジェクトを目標マジックペンに渡すことができるようになりました。これにより、適切な処理`sharpie bind Project.xcodeproj -sdk ios`を行うことができます (例:)。
* [共同](https://cocoapods.org)でのサポート 目標マジックペン (例`sharpie pod init ios AFNetworking && sharpie pod bind`:) から直接、cocoアポストロフィを構成、構築、バインドできるようになりました。
* フレームワークの構造はによっ`module.modulemap`て定義されるため、バインドフレームワークでは、フレームワークがサポートされている場合、clang モジュールが有効になり、フレームワークの正しい解析が行われます。
* フレームワーク製品を構築する Xcode プロジェクトでは、Xcode プロジェクトの非フレームワークターゲットとして、中間の製品ターゲットではなくその製品を解析すると、自動的に解決できないあいまいさが生じる可能性があります。

## <a name="216-march-17-2015"></a>2.1.6 (2015 年3月17日)

[ダウンロード2.1.6](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* 二項演算子式のバインドを修正しました。式の左辺が正しく正しく交換されていません ( `1 << 0`例: が正しく`0 << 1`バインドされていません)。 Adam Kemp に感謝しています。
* と`NSInteger`を i386 `int` `nint` `NSUInteger` で`nuint`は`uint`なく and としてバインドする問題を修正しました。は、i386 で解析`objc/NSObjCRuntime.h`が想定どおりに動作するように、clang に渡されるようになりました。 `-DNS_BUILD_32_LIKE_64`
* Mac OS X sdk (など`-sdk macosx10.10`) の既定のアーキテクチャは、i386 ではなく x86_64 になりました。したがっ`-arch`て、既定値をオーバーライドする必要がない場合は省略できます。

## <a name="210-march-15-2015"></a>2.1.0 (2015 年3月15日)

[ダウンロード2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxC#27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849):が`using ObjCRuntime;`使用され`ArgumentSemantic`たときにが生成されることを確認します。
* [bxC#27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850):が`using System.Runtime.InteropServices;`使用され`DllImport`たときにが生成されることを確認します。
* [bxC#27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852):シンボル`DllImport`の読み込み元の`__Internal`既定値。
* [bxC#27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848):前に宣言した目標 C コンテナー宣言をスキップします。
* [bxC#27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846):プロトコルの種類を、具象インターフェイスとして (`id<Foo>` `Foo`では`Foundation.NSObject<Foo>`なく) 1 つの修飾でバインドします。
* [bxC#28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037):、 `UInt32` `Int32` `Int32` 、および`uL`リテラルをとしてバインド`u`し、値が安全にに適合する場合に、サフィックスとサフィックスを削除します。 `Int64` `UInt64`
* [bxC#28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038):元のネイティブ名が`k`プレフィックスで始まる場合に、列挙型名のマッピングを修正します。
* `sizeof` C 式を引数の型が、C# のプリミティブ型にマップされていないは Clang で評価され、無効な C# 生成を回避するのには整数リテラルとしてバインドします。
* 型がブロックであるプロパティについて、目的の C 構文を修正しました (目標 C コードは、バインドされた宣言上のコメントに表示されます)。
* Decayed 型を元の型とし`int[]`てバインド`int*`します (clang でセマンティック分析中に decays にバインドしますが`int[]` 、代わりに元の書き込みとしてバインドします)。

今回のリリースで修正された多くのバグを報告するために、Dunkin を Dave に感謝いたしました。

## <a name="200-march-9-2015"></a>2.0.02015年3月9日

[ダウンロード2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

目標マジックペン2.0 は、Clang ベースのドライバーとパーサーを強化し、新しい NRefactory ベースのバインドエンジンを機能させるメジャーリリースです。 これらの強化されたコンポーネントは、特に型バインドの場合に、_より優れたバインディングを提供_します。 他にも多くの機能強化が行われています。目標マジックペンは、今後のリリースでユーザーに表示できる機能が多数用意されています。

目標マジックペン2.0 は Clang 3.6.1 に基づいています。

### <a name="type-binding-improvements"></a>型のバインディングの機能強化

* 目標-C ブロックがサポートされるようになりました。 これには、via `typedef`という名前の匿名/インラインブロックとブロックが含まれます。 匿名ブロックはまたは`System.Action` `System.Func`デリゲートとしてバインドされますが、名前付きブロック`delegate`は厳密な名前付きの型としてバインドされます。

* 匿名列挙型の名前付けのヒューリスティックが改善されました。 `typedef`これは、 `long`または`int`のような組み込みの整数型にすぐに解決されます。

* C のポインターが C# としてバインドするようになりました`unsafe`の代わりにポインター`System.IntPtr`です。 これは、結果より明確にポインター パラメーターを有効にすることとのバインドで`out`または`ref`パラメーター。 常にパラメーターができるかどうかを推測することはできません`out`または`ref`で容易に監査を許可するバインディングで、ポインターが保持されるため、します。

* 上記のポインターバインディングの例外は、目的の C オブジェクトへの2ランクのポインターがパラメーターとして検出された場合です。 このような場合、慣例が広く、パラメーターがとして`out`バインドされ`NSError **error`ます`out NSError error`(例: →)。

### <a name="verify-attribute"></a>属性の検証

多くの場合、目標マジックペンによって生成されたバインディングには`[Verify]` 、属性で注釈が付けられることがわかります。 これらの属性は、バインディングを元の C/マジックペン宣言と比較することによって、目的が正しいことを_確認_する必要があることを示しています (これは、バインドされた宣言の上にあるコメントで指定されます)。

バインドされているすべての宣言に対して検証を使用する_ことをお勧め_しますが、 `[Verify]`属性で注釈が付けられている宣言では、最も多くの場合 これは、多くの場合、バインドを最適に生成する方法を推測するために、元のネイティブソースコードに十分なメタデータがないためです。 最適なバインドを決定するには、ヘッダーファイル内のドキュメントまたはコードコメントを参照することが必要になる場合があります。

詳細については、[属性の検証](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)に関するドキュメントを参照してください。

### <a name="other-notable-improvements"></a>その他の注目すべき機能強化

* `using`バインドされた型に基づいてステートメントが生成されるようになりました。 たとえば、 `NSURL`がバインド`using Foundation;`されている場合は、ステートメントも同様に生成されます。

* `struct`と`union`の宣言は、共用体に対し`[FieldOffset]`てトリックを使用してバインドされるようになりました。

* Enum 値定数式の初期化子を正しく連結されます。完全な式は、C# に変換されます。

* 可変個引数のメソッドとブロックがバインドされるようになりました。

* フレームワークは、オプションを`-framework`使用してサポートされるようになりました。 詳細については、[ネイティブフレームワークのバインド](~/cross-platform/macios/binding/objective-sharpie/index.md)に関するドキュメントを参照してください。

* 目標-C ソースコードが自動的に検出されるようになりました。これに`-ObjC`より`-xobjective-c` 、clang を手動で渡す必要がなくなります。

* Clang module usage (`@import`) が自動検出されるようになりました。これに`-fmodules`より、clang の新しいモジュールサポートを使用するライブラリに対して clang に手動で渡す必要がなくなります。

* これで、Xamarin Unified API が既定のバインディングターゲットになりました。`-classic`オプションを使用して、32ビットのみの Classic API を対象にします。

### <a name="notable-bug-fixes"></a>注目すべきバグ修正

* 目標`instancetype` C カテゴリで使用される場合のバインドの修正
* 完全名の目標-C カテゴリ
* (などのでは`I` `NSCopying`なく) を使用`INSCopying`して、目的の C プロトコルをプレフィックスとして使用する

## <a name="1135-december-21-2014"></a>1.1.35:2014年12月21日

[ダウンロード1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

軽微なバグ修正。

## <a name="111-december-15-2014"></a>1.22014年12月15日

[V2.0 のダウンロード](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 は2013年4月にマジックペンの目標の初期プレビューに従って、Xamarin での1.5 年後の最初のメジャーリリースでした。 このリリースは、一般に、新しい Clang バックエンドを利用して、さまざまなネイティブライブラリで安定し、使用可能であると考えられる最初のリリースです。
