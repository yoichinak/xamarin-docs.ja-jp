---
title: (手動) の実際の例の詳細
description: このドキュメントでは、目標ペンを使わずは、内部での目標ペンを使わずに洞察を提供するへの入力として xcodebuild の出力を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 7af9700a9b661280c2ee32a1f65cdc01234cbe37
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781257"
---
# <a name="advanced-manual-real-world-example"></a>(手動) の実際の例の詳細

**この例では、[の Facebook からの POP ライブラリ](https://github.com/facebook/pop)です。**

ここでは、バインディング、Apple を使用するより高度なアプローチ`xcodebuild`ツールを最初にプロジェクトをビルド、POP、および目標ペンを使わずに入力を手動で推測します。 これは、本質的には、目標ペンを使わずが何を前のセクションで、内部でについて説明します。

```csharp
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Xcode プロジェクトの POP ライブラリがあるため (`pop.xcodeproj`)、使用することだけ`xcodebuild`POP をビルドします。 このプロセスは、目標ペンを使わずが解析する必要があるヘッダー ファイルを生成さらに可能性があります。 これは、ビルド前に、バインディングは、重要な理由です。 使用して構築するときに`xcodebuild`渡す同じ SDK 識別子とアーキテクチャをことを確認する目標ペンを使わずに渡す (目標ペンを使わず 3.0 通常はこれをすることを忘れないでください!)。

```csharp
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

ある多数のコンソールでのビルド情報が出力の一部として`xcodebuild`です。 上に表示されるようが分かります"CpHeader"ターゲットが実行されたヘッダー ファイルがビルド出力ディレクトリにコピーされた場合。 場合、このバインディングをもしやすくなり: ネイティブ ライブラリのビルドの一部として、ヘッダー ファイルは、多くの場合、位置にコピー「パブリック」消耗解析バインドを容易にすることができます。 この場合がわかっている POP のヘッダー ファイルで、`build/Headers`ディレクトリ。

今すぐ POP をバインドする準備が整いました。 SDK をビルドすることが分かって`iphoneos8.1`で、`arm64`アーキテクチャ、および配慮してヘッダー ファイルにある`build/Headers`POP git チェック アウトの下。 見た場合、`build/Headers`ディレクトリ、ヘッダー ファイルの数を見てみましょう。

```csharp
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

見た場合`POP.h`は、ライブラリのメイン最上位レベルのヘッダー ファイルを確認できます`#import`s 他のファイルです。 このため、のみを渡す必要がある`POP.h`目標ペンを使わずに clang はバック グラウンドで残りの部分に操作を実行します。

```csharp
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

渡されたことがわかります、`-scope build/Headers`目標ペンを使わずに渡す引数。 C および Objective C のライブラリである必要がありますので`#import`または`#include`他のヘッダー ファイル、ライブラリと、バインドする API いないの実装の詳細を`-scope`目標で定義されていないすべての API を無視するペンを使わずに指定する、ファイル内のどこかに、`-scope`ディレクトリ。

検索は、`-scope`引数がクリーンに実装されているライブラリの省略可能な多くの場合、ただし害はありませんでこれを明示的に指定します。

さらに、指定した`-c -Ibuild/headers`です。 まず、`-c`引数をコマンドライン引数を解釈を停止し、後続の引数を渡す目標ペンを使わずに対し指定_clang コンパイラに直接_です。 したがって、`-Ibuild/Headers`は下にある clang を検索するよう指示する clang コンパイラ引数が含まれます`build/Headers`、POP ヘッダーが住んでいる場所があります。 この引数なし clang はわからないファイルを配置する場所を`POP.h`は`#import`しています。 _ほぼすべて「の問題」目標ペンを使わずを使用して別に clang に渡す把握_です。

### <a name="completing-the-binding"></a>バインディングの完了

目標ペンを使わずが生成されるようになりました`Binding/ApiDefinitions.cs`と`Binding/StructsAndEnums.cs`ファイル。

これらは、バインド時では、目標ペンを使わずの基本的な最初のパスであり、まれにありますだけで済みます。 開発者は通常を完了した後目標ペンを使わずに、生成されたファイルを手動で変更する必要がありますただし前述のよう[問題を修正して](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)を自動的に処理できなかったツールによってです。

またはこれら 2 つのファイル for Mac を Visual Studio でのバインディング プロジェクトに追加できるようになりましたに直接渡される、更新プログラムが完了したら、`btouch`または`bmac`最後のバインドを作成するためのツールです。

バインディング プロセスの詳細な説明を参照してください、[完全なチュートリアルの手順](~/ios/platform/binding-objective-c/walkthrough.md)です。

