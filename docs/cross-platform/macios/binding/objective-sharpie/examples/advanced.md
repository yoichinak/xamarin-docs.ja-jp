---
title: 高度な (手動) 実際の例
description: このドキュメントでは、マジックペンに対する入力として xcodebuild の出力を使用する方法について説明します。これにより、マジックペンがどのような目的で行われているかについての洞察が得られます。
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 5e36a66949c55a85d84cbbb17fa4d276e3af1eee
ms.sourcegitcommit: acbaedbcb78bb5629d4a32e3b00f11540c93c216
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980428"
---
# <a name="advanced-manual-real-world-example"></a>高度な (手動) 実際の例

**この例では、 [Facebook の POP ライブラリ](https://github.com/facebook/pop)を使用します。**

このセクションでは、より高度なバインド方法について説明します。ここでは、Apple の `xcodebuild` ツールを使用して最初に POP プロジェクトをビルドしてから、目標マジックペンの入力を手動で推測します。 これは、基本的に、前のセクションで説明したマジックペンの目的で行われていることを表します。

```bash
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

POP ライブラリには Xcode プロジェクト (`pop.xcodeproj`) があるため、`xcodebuild` を使用して POP を構築するだけで済みます。 このプロセスによって、目的のマジックペンが解析する必要のあるヘッダーファイルが生成される場合があります。 このため、バインド前に構築することが重要です。 `xcodebuild` 経由でビルドする場合、目標マジックペンに渡すのと同じ SDK 識別子とアーキテクチャを使用してください (ただし、目標マジックペン3.0 は通常これを行うことができます)。

```bash
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

コンソールには、`xcodebuild`の一部として多数のビルド情報が出力されます。 上に示したように、ヘッダーファイルがビルド出力ディレクトリにコピーされると、"CpHeader" ターゲットが実行されていることがわかります。 これは多くの場合、バインドを容易にするために、ネイティブライブラリのビルドの一部として、ヘッダーファイルが "パブリック" の使用可能な場所にコピーされることが多く、これにより、解析が容易になります。 この例では、POP のヘッダーファイルが `build/Headers` ディレクトリにあることがわかっています。

これで、POP をバインドする準備ができました。 ここでは、`arm64` アーキテクチャを使用して SDK `iphoneos8.1` 用にビルドする必要があります。また、注目するヘッダーファイルは、POP git チェックアウトの下に `build/Headers` ます。 `build/Headers` ディレクトリを見ると、いくつかのヘッダーファイルが表示されます。

```bash
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

`POP.h`を見ると、他のファイルを `#import`するライブラリの主要な最上位レベルのヘッダーファイルであることがわかります。 このため、`POP.h` を目標マジックペンに渡すだけで、clang がバックグラウンドで残りの部分を実行します。

```bash
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

`-scope build/Headers` 引数が目標マジックペンに渡されたことがわかります。 C との c 言語のライブラリは、バインドする API ではなく、ライブラリの実装の詳細である他のヘッダーファイルを `#import` または `#include` する必要があるため、`-scope` 引数は、`-scope` ディレクトリ内のファイルで定義されていない API を無視するようにマジックペンに指示します。

`-scope` 引数は、クリーンに実装されたライブラリではオプションであることがよくありますが、明示的に指定しても害はありません。

また、`-c -Ibuild/headers`を指定しました。 まず、`-c` 引数は、コマンドライン引数の解釈を停止し、後続の引数を_clang コンパイラに直接_渡すことを目標マジックペンに指示します。 したがって、`-Ibuild/Headers` は、`build/Headers`下でインクルードを検索するように clang に指示する clang コンパイラ引数です。これは、POP ヘッダーがライブである場所です。 この引数が指定されていない場合、clang は `POP.h` が `#import`れるファイルの場所を認識しません。 _目標マジックペンを使用したほとんどすべての "問題" は、clang に渡すものを解明するために要約_されています。

### <a name="completing-the-binding"></a>バインディングの完了

これで、目標マジックペンが生成され、`Binding/StructsAndEnums.cs` ファイル `Binding/ApiDefinitions.cs` 生成されました。

これらは、バインディングでのマジックペンの基本的な最初のパスであり、いくつかのケースで必要になる場合があります。 ただし、前述のように、開発者は、目的のマジックペンが完了した後に、生成されたファイルを手動で変更して、ツールによって自動的に処理されなかった[問題を修正](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)する必要があります。

更新が完了すると、これら2つのファイルを Visual Studio for Mac のバインドプロジェクトに追加できるようになりました。または、`btouch` または `bmac` ツールに直接渡して最終的なバインドを生成することもできます。

バインドプロセスの詳しい説明については、[完全なチュートリアルの手順](~/ios/platform/binding-objective-c/walkthrough.md)を参照してください。
