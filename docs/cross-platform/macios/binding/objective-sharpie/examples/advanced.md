---
title: (手動) の実際の例の詳細
description: このドキュメントでは、目標油性は、内部で洞察を提供する目的油性への入力としての xcodebuild 出力を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: e820a0c208907a95dda4a50427bb4dac27b88964
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977903"
---
# <a name="advanced-manual-real-world-example"></a>(手動) の実際の例の詳細

**この例では、 [Facebook から POP ライブラリ](https://github.com/facebook/pop)します。**

ここでは、バインディング、Apple を使用するより高度なアプローチ`xcodebuild`最初 POP プロジェクトをビルドするツールし、目標油性の入力を手動で推測します。 これは基本的に、内部的には、前のセクションの目的油性の実行内容について説明します。

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Xcode プロジェクトが POP ライブラリ (`pop.xcodeproj`) を使って`xcodebuild`POP を構築します。 このプロセスは、目標油性が解析する必要があるヘッダー ファイルを生成さらに可能性があります。 これは、ため、ビルド前に、バインドが重要です。 使用して構築するときに`xcodebuild`を渡すことと同じ SDK 識別子とアーキテクチャを目標油性に渡す (および目標油性 3.0 は通常はこれを行うことを忘れないでください!) にします。

```
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

ある多くのビルド、コンソールに出力される情報の一部として`xcodebuild`します。 "CpHeader"ターゲットが実行されたこと確認できますように、上に表示されるヘッダー ファイルがビルド出力ディレクトリにコピーされた場合。 これは、場合、およびバインディングが容易: ネイティブ ライブラリの構築の一環として、ヘッダー ファイルは多くの場合、"パブリックに"使用できる場所にバインドを容易に解析を行いコピーします。 POP のヘッダー ファイルがあることをわかっていますここで、`build/Headers`ディレクトリ。

私たちは、POP をバインドする準備ができました。 Sdk を構築することがわかって`iphoneos8.1`で、`arm64`アーキテクチャ、および重要なヘッダー ファイルがある`build/Headers`POP の git checkout します。 見る場合、`build/Headers`ディレクトリ、ヘッダー ファイルの数がわかります。

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

見る場合`POP.h`は、ライブラリのメインの最上位レベルのヘッダー ファイルを確認できます`#import`s 他のファイル。 このため、のみを渡す必要がある`POP.h`に目的の油性 clang はバック グラウンドで残りの部分に操作を行います。

```
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

渡していることを確認は、`-scope build/Headers`目標油性への引数。 C# と OBJECTIVE-C ライブラリにする必要がありますので`#import`または`#include`他のヘッダー ファイルのライブラリと、バインドする API ない実装の詳細を`-scope`引数で定義されていない任意の API を無視する目標油性に指示をファイル内のどこか、`-scope`ディレクトリ。

検索は、`-scope`引数が明確に実装されているライブラリの省略可能な多くの場合、ただし、それを明示的に提供することも問題はありません。

さらに、指定した`-c -Ibuild/headers`します。 まず、`-c`引数に指示をコマンドライン引数を解釈を停止し、後続の引数を渡す目的油性_clang コンパイラに直接_します。 そのため、 `-Ibuild/Headers` clang を検索するよう指示する clang コンパイラ引数に含まれる`build/Headers`、POP ヘッダーが住んでいる場所であります。 この引数を指定せず clang はわからないファイルを配置する場所を`POP.h`は`#import`ing します。 _目的の油性の使用に関するほとんどすべて「問題」が clang に渡す方法を見極めるに要約_します。

### <a name="completing-the-binding"></a>バインディングの完了

目標油性が生成されるようになりました`Binding/ApiDefinitions.cs`と`Binding/StructsAndEnums.cs`ファイル。

これらは、バインド時では、目標油性の基本的な最初のパスと、いくつかのケースがありますすべて必要があります。 開発者は通常に目標油性が完了したら、生成されたファイルを手動で変更する必要がありますただし前述のよう[問題を修正](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)を自動的に処理できなかったツールによって。

更新が完了したら、これら 2 つのファイルが Mac を Visual Studio でバインド プロジェクトに今すぐ追加するまたはに直接渡される、`btouch`または`bmac`最終的なバインドを生成するツール。

バインディング プロセスの詳細な説明を参照してください、[完全なチュートリアルの手順](~/ios/platform/binding-objective-c/walkthrough.md)します。
