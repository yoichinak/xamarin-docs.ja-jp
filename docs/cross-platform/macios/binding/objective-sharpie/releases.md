---
title: 目標マジックペンリリース履歴
description: このドキュメントでは、目的の C コードへのバインディングのC#作成を自動化するために使用されるツール、目標マジックペンのリリース履歴について説明します。
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: f60be3f7dc14749f5cd58d5228c17fa85282cd78
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725351"
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
* [テレメトリの送信オプション](https://twitter.com/Symbiatch/status/760373403878559744)を `sharpie pod bind` から `sharpie bind`に保持します。

## <a name="32-june-14-2016"></a>3.2 (2016 年6月14日)

[ダウンロード3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Xcode 8 Beta 1、iOS 10、macOS 10.12、tvOS 10 および watchOS 3 のサポート。

## <a name="31-may-31-2016"></a>3.1 (2016 年5月31日)

[バージョン3.1.1 のダウンロード](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Cocoアポストロフィ Ds 1.0 のサポート
* 最初にネイティブ `.framework` を構築し、それをバインドすることで、Cocoアポストロフィ Ds バインディングの信頼性を向上させました。
* Cocoアポストロフィ Ds `.framework` とバインド定義を `Binding` ディレクトリにコピーして、Xamarin. iOS および Xamarin. Mac バインドプロジェクトとの統合を容易にする

## <a name="30-october-5-2015"></a>3.0 (2015 年10月5日)

[ダウンロード3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Xcode 7 で導入された軽量のジェネリックと null 値の許容を含む新しい目標 C 言語機能のサポート
* 最新の iOS および Mac Sdk がサポートされます。
* Xcode プロジェクトのビルドと解析がサポートされるようになりました。 これで、完全な Xcode プロジェクトを目標マジックペンに渡すことができるようになりました。これにより、適切な処理を行うことができます (例: `sharpie bind Project.xcodeproj -sdk ios`)。
* [共同](https://cocoapods.org)でのサポート 目標マジックペン (例: `sharpie pod init ios AFNetworking && sharpie pod bind`) から直接、Cocoアポストロフィ Ds を構成、構築、バインドできるようになりました。
* フレームワークの構造は `module.modulemap`によって定義されるため、バインドフレームワークでは、フレームワークでサポートされている場合、Clang モジュールが有効になり、フレームワークの正しい解析が行われます。
* フレームワーク製品を構築する Xcode プロジェクトでは、Xcode プロジェクトの非フレームワークターゲットとして、中間の製品ターゲットではなくその製品を解析すると、自動的に解決できないあいまいさが生じる可能性があります。

## <a name="216-march-17-2015"></a>2.1.6 (2015 年3月17日)

* 二項演算子式のバインドが固定されています。式の左辺が正しく正しく交換されていません (例: `1 << 0` が `0 << 1`として正しくバインドされていません)。 Adam Kemp に感謝しています。
* `NSInteger` と `NSUInteger` が `nint` ではなく `int` および `uint` としてバインドされている問題を修正しました。`-DNS_BUILD_32_LIKE_64` が Clang に渡されるようになりました。これにより、解析 `objc/NSObjCRuntime.h` が i386 で想定どおりに機能します。
* Mac OS X Sdk (`-sdk macosx10.10`など) の既定のアーキテクチャは、i386 ではなく x86_64 されるようになりました。そのため、既定値をオーバーライドする場合を除き、`-arch` は省略できます。

## <a name="210-march-15-2015"></a>2.1.0 (2015 年3月15日)

[ダウンロード2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc # 27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): `ArgumentSemantic` の使用時に `using ObjCRuntime;` が生成されることを確認します。
* [bxc # 27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): `DllImport` の使用時に `using System.Runtime.InteropServices;` が生成されることを確認します。
* [bxc # 27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): 既定 `DllImport` `__Internal`からシンボルを読み込みます。
* [bxc # 27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): 前方宣言された目標 C コンテナー宣言をスキップします。
* [bxc # 27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): プロトコルの種類を具象インターフェイスとして1つの修飾でバインドします (`Foundation.NSObject<Foo>`ではなく `Foo` として`id<Foo>` ます)。
* [bxc # 28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): 値が `u` に安全に適合する場合に、`uL` サフィックスと `Int32`サフィックスを削除するために、`UInt32`、`UInt64`、および `Int64` リテラルを `Int32` としてバインドします。
* [bxc # 28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): 元のネイティブ名が `k` プレフィックスで始まる場合に、列挙型名のマッピングを修正します。
* 引数の型がC#プリミティブ型にマップされない `sizeof` C 式は clang で評価され、無効なC#生成を回避するために整数リテラルとしてバインドされます。
* 型がブロックであるプロパティについて、目的の C 構文を修正しました (目標 C コードは、バインドされた宣言上のコメントに表示されます)。
* Decayed の型を元の型としてバインドします (Clang でセマンティック分析を行うときに `int*` に decays を`int[]` しますが、元の書き込みができる `int[]` としてバインドします)。

今回のリリースで修正された多くのバグを報告するために、Dunkin を Dave に感謝いたしました。

## <a name="200-march-9-2015"></a>2.0.0: 2015 年3月9日

[ダウンロード2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

目標マジックペン2.0 は、Clang ベースのドライバーとパーサーを強化し、新しい NRefactory ベースのバインドエンジンを機能させるメジャーリリースです。 これらの強化されたコンポーネントは、特に型バインドの場合に、_より優れたバインディングを提供_します。 他にも多くの機能強化が行われています。目標マジックペンは、今後のリリースでユーザーに表示できる機能が多数用意されています。

目標マジックペン2.0 は Clang 3.6.1 に基づいています。

### <a name="type-binding-improvements"></a>型のバインディングの機能強化

* 目標-C ブロックがサポートされるようになりました。 これには、`typedef`によって指定される匿名/インラインブロックおよびブロックが含まれます。 匿名ブロックは `System.Action` または `System.Func` デリゲートとしてバインドされますが、名前付きブロックは、厳密な名前の `delegate` 型としてバインドされます。

* 匿名列挙型の名前付けのヒューリスティックが改善されました。これは、`typedef` が `long` や `int`などの組み込みの整数型に解決される直前のものです。

* C ポインターは、`System.IntPtr`でC#はなく `unsafe` ポインターとしてバインドされるようになりました。 これにより、ポインターパラメーターを `out` または `ref` パラメーターに変換する必要がある場合に、バインディングがより明確になります。 パラメーターを `out` または `ref`にする必要があるかどうかを常に推論することはできません。そのため、簡単に監査できるように、ポインターはバインドに保持されます。

* 上記のポインターバインディングの例外は、目的の C オブジェクトへの2ランクのポインターがパラメーターとして検出された場合です。 このような場合、慣例は広く、パラメーターは `out` (`NSError **error` → `out NSError error`など) としてバインドされます。

### <a name="verify-attribute"></a>属性の検証

多くの場合、目標マジックペンによって生成されたバインディングには、`[Verify]` 属性で注釈が付けられることがわかります。 これらの属性は、バインディングを元の C/マジックペン宣言と比較することによって、目的が正しいことを_確認_する必要があることを示しています (これは、バインドされた宣言の上にあるコメントで指定されます)。

バインドされているすべての宣言に対して検証を使用する_ことをお勧め_しますが、`[Verify]` 属性で注釈が付けられている宣言では、このことが_必要_ これは、多くの場合、バインドを最適に生成する方法を推測するために、元のネイティブソースコードに十分なメタデータがないためです。 最適なバインドを決定するには、ヘッダーファイル内のドキュメントまたはコードコメントを参照することが必要になる場合があります。

詳細については、[属性の検証](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)に関するドキュメントを参照してください。

### <a name="other-notable-improvements"></a>その他の注目すべき機能強化

* バインドされた型に基づいて、`using` ステートメントが生成されるようになりました。 たとえば、`NSURL` がバインドされている場合は、`using Foundation;` ステートメントも生成されます。

* `struct` と `union` の宣言は、共用体に `[FieldOffset]` トリックを使用してバインドされるようになりました。

* 定数式の初期化子を持つ列挙値が適切にバインドされるようになりました。完全な式はにC#変換されます。

* 可変個引数のメソッドとブロックがバインドされるようになりました。

* `-framework` オプションを使用してフレームワークがサポートされるようになりました。 詳細については、[ネイティブフレームワークのバインド](~/cross-platform/macios/binding/objective-sharpie/index.md)に関するドキュメントを参照してください。

* 目標-C ソースコードが自動的に検出されるようになりました。これにより、`-ObjC` または `-xobjective-c` を Clang に手動で渡す必要がなくなります。

* Clang module usage (`@import`) が自動検出されるようになりました。これにより、Clang で新しいモジュールのサポートを使用するライブラリに対して、`-fmodules` を Clang に手動で渡す必要がなくなります。

* これで、Xamarin Unified API が既定のバインディングターゲットになりました。`-classic` オプションを使用して、32ビットのみ Classic API を対象にします。

### <a name="notable-bug-fixes"></a>注目すべきバグ修正

* 目標 C カテゴリで使用されている場合に `instancetype` バインドを修正する
* 完全名の目標-C カテゴリ
* `I` (`NSCopying`ではなく `INSCopying` など) を使用して、目的の C プロトコルにプレフィックスを付ける

## <a name="1135-december-21-2014"></a>1.1.35: 2014 年12月21日

[ダウンロード1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

マイナーなバグの修正。

## <a name="111-december-15-2014"></a>1.1.1: 2014 年12月15日

[V2.0 のダウンロード](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 は2013年4月にマジックペンの目標の初期プレビューに従って、Xamarin での1.5 年後の最初のメジャーリリースでした。 このリリースは、一般に、新しい Clang バックエンドを利用して、さまざまなネイティブライブラリで安定し、使用可能であると考えられる最初のリリースです。
