---
title: リリース履歴
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 9a29b131af706b9dedc808a156cdfaffa4173882
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="release-history"></a>リリース履歴

## <a name="34-october-11-2017"></a>3.4 (2017 年 10 月 11 日)

[V3.4.0 をダウンロードします。](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Xcode 9 のサポート: iOS 11、macOS 10.13、tvOS 11、および watchOS 4
* SIMD と tgmath に関する問題を修正してくださいようになりました
* 製品利用統計情報が完全に削除されました

## <a name="33-august-3-2016"></a>3.3 (2016 年 8 月 3 日)

[V3.3.0 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* 10 および watchOS 3 Xcode 8 Beta 4、iOS 10、macOS 10.12、tvOS をサポートします。
* Clang マスター ビルド (2016-08-02) 最新バージョンに更新
* [製品利用統計情報の送信オプションを永続化](https://twitter.com/Symbiatch/status/760373403878559744)から`sharpie pod bind`に`sharpie bind`です。

## <a name="32-june-14-2016"></a>3.2 (2016 年 6 月 14 日)

[V3.2.3 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* 10 および watchOS 3 Xcode 8 Beta 1、iOS 10、macOS 10.12、tvOS をサポートします。

## <a name="31-may-31-2016"></a>3.1 (31、2016 年 5 月)

[V3.1.1 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* CocoaPods 1.0 のサポート
* 最初にネイティブを構築して CocoaPods バインディングの信頼性を向上`.framework`とし、バインドします。
* コピー CocoaPods`.framework`バインディングに定義し、 `Binding` Xamarin.iOS および Xamarin.Mac バインド プロジェクトとの統合を容易にディレクトリ

## <a name="30-october-5-2015"></a>3.0 (2015 年 10 月 5 日)

[V3.0.8 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Xcode 7 で導入するように、軽量のジェネリックおよび null 値許容属性、Objective C 言語の新機能のサポート
* 最新の iOS および Mac の Sdk のサポート。
* Xcode プロジェクトのビルドとサポートの解析です。 目標ペンを使わずに、完全な Xcode プロジェクトを今すぐ渡すことができを行うには、正しいことを確認する最善の状態が実行されるか (例: `sharpie bind Project.xcodeproj -sdk ios`)。
* [CocoaPods](https://cocoapods.org)をサポートします。 今すぐ構成、ビルド、しバインド CocoaPods 目標ペンを使わずから直接 (例: `sharpie pod init ios AFNetworking && sharpie pod bind`)。
* バインドするときのフレームワーク、Clang モジュールが有効になりますフレームワークをサポートする場合、それらにより、framework の構造が定義されているために、フレームワークの最も適切に解析結果としてその`module.modulemap`です。
* Xcode プロジェクトのフレームワークの製品をビルドする場合に、Xcode プロジェクト内の非 framework のターゲットには、あいまいさが自動的に解決できない可能性があります。 とおり中級者向けの製品のターゲットではなく、その製品を解析します。

## <a name="216-march-17-2015"></a>2.1.6 (2015 年 3 月 17 日)

[V2.1.6 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* 固定の二項演算子式のバインド: 式の左側にある正しくとスワップされていない、右側 (例:`1 << 0`として正しくバインドされた`0 << 1`)。 これを確認しながらの Adam Kemp をいただきありがとうございます。
* 問題を修正しました`NSInteger`と`NSUInteger`としてバインドされている`int`と`uint`の代わりに`nint`と`nuint`i386 です。`-DNS_BUILD_32_LIKE_64`解析させる Clang に渡されるようになりました`objc/NSObjCRuntime.h`i386 で期待どおりに動作します。
* Mac OS X 用 Sdk を既定のアーキテクチャ (例: `-sdk macosx10.10`) i386 ではなく x86_64 がそのため、`-arch`が必要な既定値をオーバーライドする場合を除き、省略できます。

## <a name="210-march-15-2015"></a>2.1.0 (2015 年 3 月 15 日)

[ダウンロードのうち、v2.1.0 より](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc #27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): ことを確認`using ObjCRuntime;`と生成`ArgumentSemantic`を使用します。
* [bxc #27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): ことを確認`using System.Runtime.InteropServices;`と生成`DllImport`を使用します。
* [bxc #27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): 既定の`DllImport`からシンボルの読み込みに`__Internal`です。
* [bxc #27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): OBJECTIVE-C コンテナー宣言の事前宣言をスキップします。
* [bxc #27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): 具体的なインターフェイスとして修飾で 1 つのプロトコルの種類のバインド (`id<Foo>`として`Foo`の代わりに`Foundation.NSObject<Foo>`)。
* [bxc #28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): バインド`UInt32`、 `UInt64`、および`Int64`としてリテラル`Int32`を削除する、`u`や`uL`値が安全に収まらない場合にサフィックスの付いた`Int32`です。
* [bxc #28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): ネイティブの元の名前が始まるときに列挙型名のマッピングを修正、`k`プレフィックス。
* `sizeof` C 式を引数の型が、c# のプリミティブ型にマップされていないは Clang で評価され、無効な c# 生成を回避するのには整数リテラルとしてバインドします。
* プロパティの型が、ブロックの Objective C 構文の修正 (Objective C コードは、バインドされた宣言の上のコメントに表示されます)。
* 崩壊すると型を元の型としてバインド (`int[]`を減衰`int*`Clang でセマンティック分析中に、バインドによって書かれた元`int[]`代わりに)。

このポイント リリースで修正されたバグの多くのレポート Dave Dunkin に非常によくいただきありがとうございます。

## <a name="200-march-9-2015"></a>2.0.0: 2015 年 3 月 9 日

[V2.0.0 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

目標のペンを使わず 2.0 は、機能が強化された Clang ベースのドライバーとパーサーと新しい NRefactory ベースのバインディング エンジンのメジャー リリースです。 これらの強化されたコンポーネントでは、_多く_バインド、特に型のバインディング関連の向上します。 今後のリリースで多くのユーザーに表示される機能が得られます目標ペンを使わずに内在するその他の多くの機能強化が加えられました。

目標のペンを使わず 2.0 は、Clang 3.6.1 に基づいています。

### <a name="type-binding-improvements"></a>型のバインディングの機能強化

* Objective C のブロックはサポートされています。 これには、匿名/インライン ブロックが含まれます、使用して名前付きブロック`typedef`です。 匿名のブロックとしてバインド`System.Action`または`System.Func`名前付きブロックは、厳密な名前をバインドするときに、デリゲート`delegate`型です。

* 直前に指定されている列挙型の匿名の強化された名前付けヒューリスティックがある、`typedef`などの組み込みの整数型に解決する`long`または`int`です。

* C のポインターが c# としてバインドするようになりました`unsafe`の代わりにポインター`System.IntPtr`です。 これは、結果より明確にポインター パラメーターを有効にすることとのバインドで`out`または`ref`パラメーター。 常にパラメーターができるかどうかを推測することはできません`out`または`ref`で容易に監査を許可するバインディングで、ポインターが保持されるため、します。

* パラメーターとして OBJECTIVE-C オブジェクトへの 2 ランク ポインターが発生した場合に上記のポインターのバインディングに例外です。 このような場合は、規則は、大部分を占めていて、パラメーターとしてバインドする`out`(例: `NSError **error` → `out NSError error`)。

### <a name="verify-attribute"></a>属性を確認してください。

ある目標ペンを使わずによって生成されたバインドは今すぐで注釈を付けるを多くの場合、検索、`[Verify]`属性。 これらの属性を示す必要がある_確認_目標ペンを使わずが元の C/Objective C 宣言 (にバインドされている宣言の上にコメントが表示されます) を使用してバインディングを比較することによって、正しいことをでした。

検証が_推奨_バインドのすべての宣言が最も高い_必要_で注釈を宣言の`[Verify]`属性。 これは、多くの状況がないため十分なメタデータ最適バインディングを作成する方法を推測する元のネイティブのソース コードでです。 ドキュメントまたは最適なバインディング決定を行うヘッダー ファイル内のコードのコメントを参照する必要があります。

参照してください、[確認属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)詳細についてはドキュメントです。

### <a name="other-notable-improvements"></a>その他の注目に値する機能強化

* `using` ステートメントでは、バインドされている種類に基づいて生成するようになりました。 インスタンスの場合、`NSURL`バインドされました、`using Foundation;`ステートメントも生成されます。

* `struct` および`union`宣言連結されますを使用して、`[FieldOffset]`共用体のトリックします。

* Enum 値定数式の初期化子を正しく連結されます。完全な式は、c# に変換されます。

* 可変個引数メソッドとブロックはバインドされました。

* フレームワークが経由でサポートされています、`-framework`オプション。 ドキュメントを参照して[ネイティブ フレームワークのバインド](http://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks)詳細についてはします。

* Objective C ソース コードになります自動的に認識されるようになりましたを渡す必要性を排除する必要があります`-ObjC`または`-xobjective-c`Clang を手動でにします。

* Clang モジュールの使用方法 (`@import`) 自動検出は、今すぐに渡す必要性を排除する必要があります`-fmodules`Clang で新しいモジュールのサポートを使用するライブラリを手動で Clang にします。

* Xamarin Unified API は、既定のバインディング ターゲットです。使用して、`-classic`を 32 ビットのみ Classic API を対象とするオプションです。

### <a name="notable-bug-fixes"></a>主なバグ修正

* 修正`instancetype`OBJECTIVE-C カテゴリで使用されるときにバインディング
* Objective C のカテゴリの名前を完全に
* Objective C のプロトコルのプレフィックス`I`(例:`INSCopying`の代わりに`NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 2014 年 12 月 21 日

[V1.1.35 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

軽微なバグの修正。

## <a name="111-december-15-2014"></a>1.1.1: 2014 年 12 月 15 日

[V1.1.1 をダウンロードします。](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 が内部で使用して Xamarin が 2013 年 4 月で次の目標ペンを使わずの初期プレビューでの開発の 1.5 年後に最初のメジャー リリースしました。 このリリースは、安定してさまざまな新しい Clang バックエンドを機能させると、ネイティブ ライブラリの使用可能と見なされる通常最初です。

