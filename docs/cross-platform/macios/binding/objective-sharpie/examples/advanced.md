---
title: 高度な (手動) 実際の例
description: このドキュメントでは、マジックペンに対する入力として xcodebuild の出力を使用する方法について説明します。これにより、マジックペンがどのような目的で行われているかについての洞察が得られます。
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 6dbaf904c31d1a778a25e591ee94c4d354f5698a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765736"
---
# <a name="advanced-manual-real-world-example"></a>高度な (手動) 実際の例

**この例では、 [Facebook の POP ライブラリ](https://github.com/facebook/pop)を使用します。**

このセクションでは、より高度なバインド方法について説明します`xcodebuild` 。ここでは、Apple のツールを使用して最初に POP プロジェクトをビルドしてから、目標マジックペンの入力を手動で推測します。 これは、基本的に、前のセクションで説明したマジックペンの目的で行われていることを表します。

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Pop ライブラリには Xcode プロジェクト (`pop.xcodeproj`) があるため、を使用`xcodebuild`して pop を構築するだけで済みます。 このプロセスによって、目的のマジックペンが解析する必要のあるヘッダーファイルが生成される場合があります。 このため、バインド前に構築することが重要です。 を使用`xcodebuild`してビルドするときに、目標マジックペンに渡すのと同じ SDK 識別子とアーキテクチャを使用することを確認してください (ただし、目標マジックペン3.0 は通常これを行うことができます)。

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

コンソールには、の`xcodebuild`一部として多数のビルド情報が出力されます。 上に示したように、ヘッダーファイルがビルド出力ディレクトリにコピーされると、"CpHeader" ターゲットが実行されていることがわかります。 これは多くの場合、バインドを容易にするために、ネイティブライブラリのビルドの一部として、ヘッダーファイルが "パブリック" の使用可能な場所にコピーされることが多く、これにより、解析が容易になります。 この場合、POP のヘッダーファイルが`build/Headers`ディレクトリにあることがわかります。

これで、POP をバインドする準備ができました。 アーキテクチャを`build/Headers` `iphoneos8.1` `arm64`使用して SDK をビルドしたいと考えています。また、注意する必要があるヘッダーファイルは、POP git チェックアウトの下にあります。 ディレクトリを調べると、 `build/Headers`いくつかのヘッダーファイルが表示されます。

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

ご覧のよう`#import`に、その他のファイルであるライブラリの主要なトップレベルヘッダーファイルであることがわかります。`POP.h` このため、目標マジックペンに渡す`POP.h`必要があるだけで、clang がバックグラウンドで残りの部分を実行します。

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

目標マジックペンに`-scope build/Headers`引数が渡されたことがわかります。 C との c 言語ライブラリは、 `#import`バインド`#include`する api ではなく、ライブラリの実装の詳細である必要があるため、この`-scope`引数は、マジックペンで定義されていない api を無視するように、目標を指定します。ディレクトリ内の任意`-scope`の場所にファイルを置いてください。

完全に実装されたライブラリでは、多くの場合、引数は省略可能ですが、明示的に指定しても害はありません。`-scope`

さらに、を`-c -Ibuild/headers`指定しました。 最初に、 `-c`引数は、コマンドライン引数の解釈を停止し、後続の引数を_clang コンパイラに直接_渡すことを目標マジックペンに指示します。 したがって`-Ibuild/Headers` 、は、の下`build/Headers`にインクルードを検索するように clang に指示する clang コンパイラ引数です。ここで、POP ヘッダーはライブです。 この引数を指定しない場合、clang は、処理`POP.h`中`#import`のファイルの場所を認識しません。 _目標マジックペンを使用したほとんどすべての "問題" は、clang に渡すものを解明するために要約_されています。

### <a name="completing-the-binding"></a>バインディングの完了

目標マジックペンが生成さ`Binding/ApiDefinitions.cs`れ`Binding/StructsAndEnums.cs` 、ファイルが作成されました。

これらは、バインディングでのマジックペンの基本的な最初のパスであり、いくつかのケースで必要になる場合があります。 ただし、前述のように、開発者は、目的のマジックペンが完了した後に、生成されたファイルを手動で変更して、ツールによって自動的に処理されなかった[問題を修正](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)する必要があります。

更新が完了すると、これら2つのファイルを Visual Studio for Mac のバインドプロジェクトに追加できるようになりました。 `btouch`また`bmac`は、またはツールに直接渡して最終的なバインドを作成することもできます。

バインドプロセスの詳しい説明については、[完全なチュートリアルの手順](~/ios/platform/binding-objective-c/walkthrough.md)を参照してください。
