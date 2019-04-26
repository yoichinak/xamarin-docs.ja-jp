---
title: 目標油性リリース履歴
description: このドキュメントでは、目標ペンを使わず、C# Objective C コードへのバインドの作成を自動化するためのツールのリリース履歴について説明します。
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 03e4a5ac8906d2593cbdf3c15f6b2d1f4a2c6d19
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199650"
---
# <a name="objective-sharpie-release-history"></a>目標油性リリース履歴

## <a name="34-october-11-2017"></a>3.4 (2017 年 10 月 11 日)

[V3.4.0 をダウンロードします。](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Xcode 9 のサポート: iOS 11、macOS 10.13、11、tvOS、watchOS 4
* SIMD と tgmath に関する問題に固定するようになりました
* 製品利用統計情報が完全に削除されました

## <a name="33-august-3-2016"></a>3.3 (2016 年 8 月 3 日)

[V3.3.0 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* 10 と watchOS 3 は、Xcode 8 ベータ 4、iOS 10、macOS 10.12、tvOS をサポートします。
* Clang マスター ビルド (2016-08-02) の最新バージョンに更新
* [テレメトリの送信オプションを保持](https://twitter.com/Symbiatch/status/760373403878559744)から`sharpie pod bind`に`sharpie bind`します。

## <a name="32-june-14-2016"></a>3.2 (2016 年 6 月 14 日)

[V3.2.3 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* 10 と watchOS 3 は、Xcode 8 Beta 1、10 を iOS、macOS 10.12、tvOS をサポートします。

## <a name="31-may-31-2016"></a>3.1 (2016 年 5 月 31 日)

[V3.1.1 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* CocoaPods 1.0 のサポート
* 最初に、ネイティブを構築することにより CocoaPods バインディングの信頼性を向上`.framework`しバインドします。
* CocoaPods をコピー`.framework`バインディングに定義し、 `Binding` Xamarin.iOS および Xamarin.Mac バインド プロジェクトとの統合を容易にディレクトリ

## <a name="30-october-5-2015"></a>3.0 (2015 年 10 月 5 日)

[V3.0.8 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Xcode 7 で導入された、軽量なジェネリックと、null 値の許容を含む新しい Objective C 言語の機能のサポート
* 最新の iOS および Mac の Sdk をサポートします。
* Xcode プロジェクトの構築と解析のサポート。 目標油性に完全な Xcode プロジェクトを渡すことができますようになりましたし、実行する適切な処理を把握することをお勧めが実行されるか (例: `sharpie bind Project.xcodeproj -sdk ios`)。
* [CocoaPods](https://cocoapods.org)サポートします。 ことができますようになりました構成、ビルド、および目標油性から直接 CocoaPods をバインド (例: `sharpie pod init ios AFNetworking && sharpie pod bind`)。
* フレームワークをバインドするときに Clang モジュールが有効になります、framework がサポートする場合、それらによって、framework の構造が定義されているために、フレームワークの最も適切に解析結果としてその`module.modulemap`します。
* Framework の製品をビルドする Xcode プロジェクトでは、Xcode プロジェクト内の非 framework のターゲットには、あいまいさが自動的に解決できない可能性があります、中間製品のターゲットではなく、その製品を解析します。

## <a name="216-march-17-2015"></a>2.1.6 (2015 年 3 月 17 日)

[V2.1.6 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* 固定の二項演算子式バインディング: で、右側の式の左側にあるが正しくとスワップ (例:`1 << 0`として正しくバインドされました`0 << 1`)。 Adam Kemp にこれを確認しながらのありがとうございます。
* 問題を修正しました`NSInteger`と`NSUInteger`としてバインドされている`int`と`uint`の代わりに`nint`と`nuint`i386;`-DNS_BUILD_32_LIKE_64`解析させる Clang に渡されるようになりました`objc/NSObjCRuntime.h`i386 で期待どおりに動作します。
* Mac OS X 用 Sdk の既定のアーキテクチャ (例: `-sdk macosx10.10`)、i386 ではなく x86_64 がので、`-arch`が必要な既定値を上書きしない限りは省略できます。

## <a name="210-march-15-2015"></a>2.1.0 (2015 年 3 月 15 日)

[V2.1.0 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxC#27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849):確認`using ObjCRuntime;`と生成されます`ArgumentSemantic`使用されます。
* [bxC#27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850):確認`using System.Runtime.InteropServices;`と生成されます`DllImport`使用されます。
* [bxC#27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852):既定の`DllImport`からシンボルの読み込みに`__Internal`します。
* [bxC#27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848):OBJECTIVE-C でコンテナーの宣言の事前宣言をスキップします。
* [bxC#27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846):具体的なインターフェイスとして修飾で 1 つのプロトコルの種類のバインド (`id<Foo>`として`Foo`の代わりに`Foundation.NSObject<Foo>`)。
* [bxC#28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037):バインド`UInt32`、 `UInt64`、および`Int64`としてリテラル`Int32`を削除する、`u`や`uL`サフィックスの値が安全に収まらないときに`Int32`します。
* [bxC#28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038):元のネイティブ名が始まるときに列挙型名のマッピングを修正、`k`プレフィックス。
* `sizeof` C 式を引数の型が、C# のプリミティブ型にマップされていないは Clang で評価され、無効な C# 生成を回避するのには整数リテラルとしてバインドします。
* プロパティの型が、ブロックの Objective C の構文を修正 (Objective C コードは、バインド宣言を超えたコメントに表示されます)。
* 放射性型を元の型としてバインド (`int[]`を減衰`int*`Clang でセマンティック分析中に、バインドに書き込まれた元`int[]`代わりに)。

このポイント リリースで修正されたバグの多くのレポートの Dave Dunkin にとっては非常にありがとうございます。

## <a name="200-march-9-2015"></a>2.0.0:2015 年 3 月 9 日

[V2.0.0 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

目標の油性 2.0 は、機能が強化された Clang ベースのドライバーとパーサーと新しい NRefactory ベースのバインディング エンジンがメジャー リリースです。 これらの強化されたコンポーネントでは、_多く_特に関連型のバインディングのバインドが向上します。 今後のリリースで多くのユーザーに表示される機能が得られます目標油性内部的なその他の多くの機能強化が加えられました。

目標油性 2.0 は、Clang 3.6.1 に基づいています。

### <a name="type-binding-improvements"></a>型のバインディングの機能強化

* Objective C の要素はサポートされています。 匿名/インライン ブロックが含まれます、使用して名前付きブロック`typedef`します。 匿名のブロックとしてバインドされる`System.Action`または`System.Func`名前付きブロックは、厳密な名前をバインドするときに、デリゲート`delegate`型。

* すぐに付いている匿名の列挙型の名前付けヒューリスティックを改善がある、`typedef`などの組み込みの整数型を解決する`long`または`int`します。

* C のポインターが C# としてバインドするようになりました`unsafe`の代わりにポインター`System.IntPtr`です。 これは、結果より明確にポインター パラメーターを有効にすることとのバインドで`out`または`ref`パラメーター。 常にパラメーターができるかどうかを推測することはできません`out`または`ref`で容易に監査を許可するバインディングで、ポインターが保持されるため、します。

* パラメーターとして OBJECTIVE-C オブジェクトへの 2 ランク ポインターが発生した場合に、上記のポインター バインドする例外です。 このような場合は、規則が広く使用されて、パラメーターとしてバインドする`out`(例: `NSError **error` → `out NSError error`)。

### <a name="verify-attribute"></a>属性を確認します。

目的の油性によって生成されたバインドで注釈ようになりましたを多くの場合、検索、`[Verify]`属性。 これらの属性を示す必要がある_確認_目標油性が (これが提供する、バインド宣言の上にコメントで) 元の C/Objective C 宣言とのバインドを比較することによって、正しいことをでした。

検証が_推奨_、バインドされたすべての宣言が最も高い_必要_の宣言の注釈が付けられた、`[Verify]`属性。 これは、多くの状況でがないため十分なメタデータで最適バインディングを作成する方法を推測する元のネイティブなソース コード。 ドキュメントまたは最高のバインドを決定するヘッダー ファイル内のコードのコメントを参照する必要があります。

参照してください、[確認属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)詳細についてはドキュメントです。

### <a name="other-notable-improvements"></a>その他の重要な改善点

* `using` ステートメントはこれで、バインドの種類に基づいて生成されます。 たとえば場合、`NSURL`バインドされた、`using Foundation;`ステートメントも生成されます。

* `struct` `union`宣言連結されますを使用して、`[FieldOffset]`共用体のトリックです。

* Enum 値定数式の初期化子を正しく連結されます。完全な式は、C# に変換されます。

* 可変個引数のメソッドとブロックはバインドされました。

* 使用してフレームワークがサポートされています、`-framework`オプション。 上のドキュメントを参照して[ネイティブ フレームワークのバインド](https://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks)の詳細。

* Objective C ソース コードは自動検出するようになりましたを渡す必要性を排除する必要があります`-ObjC`または`-xobjective-c`Clang の手動でにします。

* Clang のモジュールの使用方法 (`@import`) が自動検出を渡す必要性を排除する必要があります`-fmodules`Clang で新しいモジュールのサポートを使用するライブラリを手動で Clang にします。

* Xamarin の Unified API は、既定のバインディング ターゲットです。使用して、 `-classic` 32 ビットのみのクラシック API を対象とするオプション。

### <a name="notable-bug-fixes"></a>重要なバグ修正

* 修正`instancetype`Objective C カテゴリで使用する場合のバインド
* Objective C のカテゴリの完全名
* Objective C を使用するプロトコルのプレフィックス`I`(例:`INSCopying`の代わりに`NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35:2014 年 12 月 21 日

[V1.1.35 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

軽微なバグの修正。

## <a name="111-december-15-2014"></a>1.1.1:2014 年 12 月 15 日

[V1.1.1 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 は、内部使用および Xamarin が 2013 年 4 月で次の目的油性の初期のプレビューでの開発の 1.5 年後の最初のメジャー リリースされました。 このリリースでは安定してさまざまな新しい Clang バックエンドを備えた、ネイティブ ライブラリの使用可能と見なされる通常します。

