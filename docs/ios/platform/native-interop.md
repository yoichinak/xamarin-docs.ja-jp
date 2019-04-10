---
title: Xamarin.iOS でネイティブ ライブラリを参照します。
description: このドキュメントでは、Xamarin.iOS アプリケーションにネイティブの C ライブラリにリンクする方法について説明します。 ユニバーサルのネイティブ ライブラリと C のメソッドへのアクセスをビルドする方法を説明C#します。
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/28/2016
ms.openlocfilehash: 7ed8fc18624f46abd4a9fc293d8c33a1722da7dd
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870262"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Xamarin.iOS でネイティブ ライブラリを参照します。

Xamarin.iOS では、ネイティブの C ライブラリと OBJECTIVE-C ライブラリの両方とのリンクをサポートしています。 このドキュメントでは、Xamarin.iOS プロジェクト、ネイティブの C ライブラリにリンクする方法について説明します。 OBJECTIVE-C ライブラリの作業を行うことについては、次を参照してください。 この[Objective-c のバインド型](~/ios/platform/binding-objective-c/index.md)ドキュメント。

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>(I386、ARMv7、および ARM64) ユニバーサルのネイティブ ライブラリの構築

IOS 開発のサポートされているプラットフォームごとに、ネイティブ ライブラリを構築することがしばしば (シミュレーターと ARMv7/ARM64 デバイス自体の i386)。 ライブラリの Xcode プロジェクトが既に場合は実際に行うは簡単です。

I386 バージョンのネイティブ ライブラリをビルドするには、ターミナルから、次のコマンドを実行します。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

これは、結果、ネイティブのスタティック ライブラリ `MyProject.xcodeproj/build/Release-iphonesimulator/`します。 後で使用する一意の名前を付けることの安全な場所にライブラリ アーカイブ ファイル (libMyLibrary.a) をコピー (または移動) (など**libMyLibrary i386.a**)、arm64 および armv7 バージョンは、同じライブラリの使用が衝突しないように次にビルドします。

ARM64 バージョンのネイティブ ライブラリをビルドするには、次のコマンドを実行します。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

この時点でビルドされたネイティブ ライブラリが配置されます`MyProject.xcodeproj/build/Release-iphoneos/`します。 もう一度、コピー (移動) のように名前を変更する、安全な場所にこのファイル**libMyLibrary arm64.a**競合が発生しないようにします。

ARMv7 バージョンのライブラリを今すぐビルドするには。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

結果として得られるライブラリ ファイルのコピー (または移動) を同じ場所に移動して、ライブラリの他の 2 つのバージョンのように名前を変更する**libMyLibrary armv7.a**します。

汎用をバイナリにするために、使用することができます、`lipo`ツールようになります。

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

これにより作成されます`libMyLibrary.a`これは、iOS 開発のすべてのターゲットの使用に適したとなる universal (fat) ライブラリとなります。


### <a name="missing-required-architecture-i386"></a>不足しているに必要なアーキテクチャ i386

取得する場合、`does not implement methodSignatureForSelector`または`does not implement doesNotRecognizeSelector`i386 アーキテクチャ、おそらく、ios シミュレーターでは、ライブラリ、Objective C ライブラリを使用しようとするとき、ランタイムの出力内のメッセージはコンパイルされませんでした (を参照してください、[ユニバーサル構成ネイティブ ライブラリ](#building_native)前のセクション)。

特定のライブラリでサポートされるアーキテクチャを確認するには、ターミナルで次のコマンドを使用します。

```bash
lipo -info /full/path/to/libraryname.a
```

場所`/full/path/to/`が消費されるライブラリへの完全パスと`libraryname.a`ライブラリの名前に問題が。

をライブラリにソースがある場合は、iOS シミュレーターでアプリをテストする場合コンパイルし、同様に、i386 アーキテクチャのバンドルする必要あります。

### <a name="linking-your-library"></a>ライブラリをリンク

使用する任意のサード パーティ製のライブラリは、アプリケーションに静的にリンクする必要があります。 

インターネットまたは Xcode でビルドから取得した"libMyLibrary.a"ライブラリが静的にリンクする場合は、いくつかの作業を行う必要があります。

-  プロジェクトにライブラリを取り込む
-  ライブラリをリンクする Xamarin.iOS を構成します。
-  ライブラリのメソッドにアクセスします。


**プロジェクトにライブラリを取り込む**、キーを押して、ソリューション エクスプ ローラーからプロジェクトを選択**コマンド + オプション + a**します。 LibMyLibrary.a に移動し、プロジェクトに追加します。 メッセージが表示されたら、プロジェクトにコピーするには、Visual Studio for Mac または Visual Studio に伝えます。 これを追加した後、プロジェクト内、libFoo.a を探すを右クリックし、設定、**ビルド アクション**に**none**します。

**構成 Xamarin.iOS ライブラリをリンクする**、最終的な実行可能ファイル (ライブラリそのものではありませんが、最終的なプログラム) プロジェクト オプションで追加する必要があります**iOS ビルド**の (これらは、余分な引数。プロジェクトのオプションの一部)、"-gcc_flags"オプションの後にたとえば、プログラムでは、必要なすべての余分なライブラリが含まれている引用符で囲まれた文字列。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

上記の例ではリンク**libMyLibrary.a**

使用することができます`-gcc_flags`実行可能ファイルの最後のリンクを実行するために使用、GCC コンパイラに渡すコマンドライン引数のセットを指定します。 たとえば、このコマンドラインも CFNetwork フレームワークを参照します。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

ネイティブ ライブラリに C++ コードが含まれている場合、正しいコンパイラを使用する Xamarin.iOS を認識するように、「余分な引数」で - cxx であるフラグも渡す必要があります。 C++ のように、前のオプションになります。

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>C から C のメソッドにアクセスします。&#35;

ネイティブ ライブラリの 2 つの種類に使用できるある iOS:

-  オペレーティング システムの一部であるライブラリを共有します。

-  アプリケーションに付属するスタティック ライブラリ。


使用するもののいずれかで定義されているメソッドにアクセスする[Mono の P/invoke 機能](https://www.mono-project.com/docs/advanced/pinvoke/)はほぼは .net では、使用するのと同じテクノロジであります。

-  C 関数を呼び出したいを決定します。
-  その署名を確認します。
-  ライブラリ内の特定します。
-  適切な P/invoke 宣言を書き込む

P/invoke を使用する場合は、リンクするライブラリのパスを指定する必要があります。 ライブラリを共有する iOS を使用して、ハードコーディングするか、パスまたはで定義していますが、利便性定数を使用するときに、 `Constants`、これらの定数は、iOS の共有ライブラリをカバーする必要があります。

たとえば、c: このシグネチャのある Apple の UIKit ライブラリから UIRectFrameUsingBlendMode メソッドを呼び出す必要がある場合

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

P/invoke 宣言は、次のようになります。

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary 定数として定義されているだけでは、"/System/Library/Frameworks/UIKit.framework/UIKit"、エントリ ポイントにより、必要に応じて外部名 (UIRectFramUsingBlendMode) を指定で別の名前を公開するときC#、短い RectFrameUsingBlendMode します。

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>Dylib を C へのアクセス

呼び出しの前に必要な追加のセットアップのビットがある場合は、Xamarin.iOS アプリケーションで C Dylib を利用する必要がある場合は、`DllImport`属性。

たとえば、ある場合、`Animal.dylib`で、`Animal_Version`を通じ、アプリケーションでメソッドを使用する前に、ライブラリの場所の Xamarin.iOS を通知する必要があります。

これを行うには、編集、`Main.CS`ファイルを開き、次のようになります。

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

場所`/full/path/to/`Dylib を読み取り中に完全パスを指定します。 このコードでできますし、リンクできるように、`Animal_Version`メソッドとして、次のとおりです。

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>スタティック ライブラリ

スタティック ライブラリは、iOS でのみ使用できます、ためにがない外部の共有ライブラリを使用すると、リンクため DllImport 属性で、path パラメーターは、特別な名前を使用する必要があります`__Internal`(名前の先頭に二重アンダー スコア文字に注意してください) ではなくパス名です。

これにより、共有ライブラリから読み込むことがなく、現在のプログラムで参照するメソッドのシンボルを検索する DllImport が強制されます。

