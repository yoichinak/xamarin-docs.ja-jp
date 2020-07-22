---
title: Xamarin でのネイティブライブラリの参照
description: このドキュメントでは、ネイティブ C ライブラリを Xamarin iOS アプリケーションにリンクする方法について説明します。 ここでは、ユニバーサルネイティブライブラリを構築し、C# から C メソッドにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/28/2016
ms.openlocfilehash: 014c5074c6dfd9698b0a746e58da50a3f3d9ee03
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574107"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Xamarin でのネイティブライブラリの参照

Xamarin. iOS は、ネイティブ C ライブラリと目的 C ライブラリの両方を使用したリンクをサポートします。 このドキュメントでは、ネイティブ C ライブラリを Xamarin. iOS プロジェクトにリンクする方法について説明します。 目的 C ライブラリについても同様に説明します。詳細については、「[バインディングの目的-c の型](~/ios/platform/binding-objective-c/index.md)」を参照してください。

<a name="building_native"></a>

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Universal Native Library (i386、ARMv7、ARM64) の構築

多くの場合、iOS 開発用にサポートされているプラットフォームごとにネイティブライブラリを構築することをお勧めします (シミュレーターの場合は i386、デバイス自体の場合は ARMv7/ARM64)。 ライブラリ用の Xcode プロジェクトを既に持っている場合は、これは非常に簡単です。

ネイティブライブラリの i386 バージョンをビルドするには、ターミナルから次のコマンドを実行します。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

これにより、にネイティブスタティックライブラリが生成され `MyProject.xcodeproj/build/Release-iphonesimulator/` ます。 ライブラリアーカイブファイル (libMyLibrary. a) を後で使用するために安全な場所にコピー (または移動) します。これにより、次に構築するのと同じライブラリの arm64 バージョンと armv7 バージョンが競合しないようにし**ます**。

ネイティブライブラリの ARM64 バージョンをビルドするには、次のコマンドを実行します。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

今回は、ビルドされたネイティブライブラリがに配置され `MyProject.xcodeproj/build/Release-iphoneos/` ます。 ここでも、このファイルを安全な場所にコピー (または移動) します。このファイルの名前を**Libmylibrary-arm64**のように変更して、競合しないようにします。

次に、ライブラリの ARMv7 バージョンをビルドします。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

作成したライブラリファイルを他の2つのバージョンのライブラリに移動した同じ場所にコピー (または移動) します。その後、 **Libmylibrary-armv7**のように名前を変更します。

ユニバーサルバイナリを作成するには、次のようにツールを使用し `lipo` ます。

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

これに `libMyLibrary.a` より、ユニバーサル (fat) ライブラリが作成されます。これは、すべての iOS 開発ターゲットで使用するのに適しています。

### <a name="missing-required-architecture-i386"></a>必要なアーキテクチャの i386 がありません

`does not implement methodSignatureForSelector` `does not implement doesNotRecognizeSelector` IOS シミュレーターで目的の C ライブラリを使用しようとしたときに、ランタイムの出力にまたはメッセージが表示される場合は、ライブラリが i386 アーキテクチャ用にコンパイルされていない可能性があります (上記の「 [Universal ネイティブライブラリの構築](#building_native)」セクションを参照してください)。

特定のライブラリでサポートされているアーキテクチャを確認するには、ターミナルで次のコマンドを使用します。

```bash
lipo -info /full/path/to/libraryname.a
```

ここ `/full/path/to/` で、は使用されているライブラリへの完全パスであり、 `libraryname.a` は対象のライブラリの名前です。

ソースがライブラリにある場合は、iOS シミュレーターでアプリをテストする場合は、i386 アーキテクチャ用にもコンパイルしてバンドルする必要があります。

### <a name="linking-your-library"></a>ライブラリのリンク

使用するすべてのサードパーティライブラリは、アプリケーションと静的にリンクされている必要があります。 

ライブラリ "libMyLibrary. a" を静的にリンクする場合は、インターネットから入手したか、Xcode でビルドした場合、いくつかの操作を行う必要があります。

- ライブラリをプロジェクトに取り込む
- ライブラリをリンクするように Xamarin を構成する
- ライブラリからメソッドにアクセスします。

**ライブラリをプロジェクトに取り込む**には、ソリューションエクスプローラーからプロジェクトを選択し、**コマンド + オプション + a**キーを押します。 LibMyLibrary. a に移動し、プロジェクトに追加します。 プロンプトが表示されたら Visual Studio for Mac または Visual Studio でプロジェクトにコピーするように指示します。 追加した後、プロジェクトで libFoo を見つけて右クリックし、[**ビルド] アクション**を [**なし**] に設定します。

**Xamarin を構成**してライブラリをリンクするには、最終的な実行可能ファイル (ライブラリ自体ではなく、最後のプログラム) のプロジェクトオプションで、" **iOS Build**-gcc_flags" オプションの後に、プログラムに必要なすべての追加のライブラリを含む引用符で囲まれた文字列を追加する必要があります。次に例を示します。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

上の例では **、Libmylibrary をリンクしています。**

を使用すると、 `-gcc_flags` 実行可能ファイルの最終的なリンクを実行するために使用される GCC コンパイラに渡すコマンドライン引数のセットを指定できます。 たとえば、次のコマンドラインは、CFNetwork フレームワークも参照します。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

ネイティブライブラリに C++ コードが含まれている場合は、.cxx フラグを "追加の引数" に渡す必要があります。これにより、Xamarin は正しいコンパイラを使用することが認識されます。 C++ の場合、上記のオプションは次のようになります。

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#"></a>

## <a name="accessing-c-methods-from-c35"></a>C&#35; から C メソッドにアクセスする

IOS で使用できるネイティブライブラリには、次の2種類があります。

- オペレーティングシステムの一部である共有ライブラリ。

- アプリケーションに付属するスタティックライブラリ。

これらのいずれかで定義されているメソッドにアクセスするには、 [Mono の P/Invoke 機能](https://www.mono-project.com/docs/advanced/pinvoke/)を使用します。これは、.net で使用するのと同じテクノロジです。これは、ほぼ次のようになります。

- 呼び出す C 関数を決定する
- 署名を確認する
- どのライブラリが常駐しているかを判断する
- 適切な P/Invoke 宣言を記述します。

P/Invoke を使用する場合は、リンク先のライブラリのパスを指定する必要があります。 IOS 共有ライブラリを使用する場合は、パスをハードコーディングするか、「」で定義した便利な定数を使用することができ `Constants` ます。これらの定数は、ios の共有ライブラリに対応している必要があります。

たとえば、次のシグネチャを持つ Apple の UIKit ライブラリから、C: で UIRectFrameUsingBlendMode メソッドを呼び出す必要があるとします。

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

P/Invoke 宣言は次のようになります。

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

UIKitLibrary は "/System/Library/Frameworks/UIKit.framework/UIKit" として定義された定数にすぎません。エントリポイントでは、必要に応じて外部名 (UIRectFramUsingBlendMode) を指定できますが、C# では、より短い RectFrameUsingBlendMode が使用されます。

<a name="Accessing_C_Dylibs"></a>

### <a name="accessing-c-dylibs"></a>C Dylibs へのアクセス

Xamarin. iOS アプリケーションで C Dylib を使用する必要がある場合は、属性を呼び出す前に追加のセットアップが必要になります `DllImport` 。

たとえば、アプリケーションで呼び出すメソッドを持つがある場合は、 `Animal.dylib` `Animal_Version` ライブラリの使用を試みる前に、そのライブラリの場所を Xamarin に通知する必要があります。

これを行うには、ファイルを編集し、次のようにし `Main.CS` ます。

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

ここ `/full/path/to/` で、は、使用されている Dylib への完全なパスです。 このコードを配置すると、次のようにメソッドにリンクでき `Animal_Version` ます。

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries"></a>

### <a name="static-libraries"></a>スタティックライブラリ

IOS ではスタティックライブラリしか使用できないため、リンク先となる外部共有ライブラリがないため、DllImport 属性の path パラメーターでは、パス名ではなく、特別な名前を使用する必要があり `__Internal` ます (名前の先頭にある2つのアンダースコア文字に注意してください)。

これにより、DllImport は、共有ライブラリから読み込まれるのではなく、現在のプログラムで参照しているメソッドのシンボルを検索します。
