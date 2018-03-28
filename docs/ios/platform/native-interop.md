---
title: ネイティブ ライブラリを参照します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: 99e565c2268bec6d80c4976e604333cbd2f160a3
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="referencing-native-libraries"></a>ネイティブ ライブラリを参照します。

Xamarin.iOS では、ネイティブの C ライブラリと Objective C ライブラリの両方を持つリンクをサポートします。 このドキュメントでは、Xamarin.iOS プロジェクトを含む、ネイティブの C ライブラリをリンクする方法について説明します。 Objective C ライブラリの作業を行うことについては、次を参照してください。 この[Objective C 型のバインド](~/ios/platform/binding-objective-c/index.md)ドキュメント。

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>ユニバーサルのネイティブ ライブラリ (i386、常に ARMv7、および ARM64) の構築

各 iOS 開発のサポートされているプラットフォームのネイティブ ライブラリをビルドすると便利です (シミュレーターと常に ARMv7/ARM64 デバイス自体の i386)。 ライブラリの Xcode プロジェクトを既に取得したら、これが非常に簡単です。

I386 バージョンのネイティブ ライブラリをビルドするには、端末から次のコマンドを実行します。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

ネイティブ スタティック ライブラリと、これが`MyProject.xcodeproj/build/Release-iphonesimulator/`です。 コピーまたは移動後で使用する一意の名前を付けることの安全な場所にライブラリ アーカイブ ファイル (libMyLibrary.a) (など**libMyLibrary i386.a**) は、同じライブラリのバージョン arm64 と常に armv7 バージョンとが衝突しないように次にビルドします。

ARM64 バージョンのネイティブ ライブラリをビルドするには、次のコマンドを実行します。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

この時点でビルドされたネイティブ ライブラリが見つかることが`MyProject.xcodeproj/build/Release-iphoneos/`です。 もう一度コピーまたは移動のように名前を変更する、安全な場所にこのファイル**libMyLibrary arm64.a**が衝突しないようにします。

ライブラリの ARMv7 バージョンを今すぐビルドするには。

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

結果として得られるライブラリ ファイルをコピー (または移動) と同じ場所に移動したライブラリの他の 2 つのバージョンのように名前を変更する**libMyLibrary armv7.a**です。

汎用のバイナリを作成、使用することができます、`lipo`ツール次のようにします。

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

これにより作成`libMyLibrary.a`ユニバーサル (fat) ライブラリの iOS 開発のすべてのターゲットの使用に適したとなるされます。


### <a name="missing-required-architecture-i386"></a>不足しているに必要なアーキテクチャ i386

取得する場合、`does not implement methodSignatureForSelector`または`does not implement doesNotRecognizeSelector`可能性があります、Objective C のライブラリを ios シミュレーターで、ライブラリを使用しようとするとき、ランタイム出力内のメッセージは、i386 アーキテクチャ用コンパイルされませんでした (を参照してください、[構築ユニバーサルネイティブ ライブラリ](#building_native)前のセクション)。

指定したライブラリでサポートされるアーキテクチャを確認するには、端末で次のコマンドを使用します。

```bash
lipo -info /full/path/to/libraryname.a
```

ここで`/full/path/to/`が消費されて、ライブラリへの完全パスと`libraryname.a`の問題が、ライブラリの名前。

ソース ライブラリにある場合は場合、は、iOS シミュレーターでアプリをテストする場合コンパイルし、同様に、i386 アーキテクチャにバンドルする必要あります。

### <a name="linking-your-library"></a>ライブラリをリンク

使用する任意のサードパーティのライブラリは、アプリケーションを使用して静的にリンクする必要があります。 

インターネットまたは Xcode のビルドから取得した"libMyLibrary.a"ライブラリを静的にリンクする場合は、いくつかの作業を行う必要があります。

-  プロジェクトにライブラリを取り込む
-  ライブラリをリンクする Xamarin.iOS を構成します。
-  ライブラリのメソッドにアクセスします。


**ライブラリをプロジェクトに取り込む**、キーを押して、ソリューション エクスプ ローラーからプロジェクトを選択**コマンド + オプション + a**です。 LibMyLibrary.a に移動し、プロジェクトに追加します。 メッセージが表示されたら、プロジェクトにコピーするには、Visual Studio for Mac または Visual Studio に指示します。 これを追加した後、プロジェクト内、libFoo.a を探すを右クリックし、設定、**ビルド アクション**に**none**です。

**ライブラリをリンクする構成 Xamarin.iOS**、最終的な実行可能ファイル (ライブラリそのものではありませんが、最終的なプログラム) プロジェクトのオプションで追加する必要があります**iOS ビルド**の (これらは、余分な引数プロジェクトのオプションの一部)、"-gcc_flags"オプションの後にたとえば、プログラムに必要なすべての余分なライブラリが含まれている引用符で囲まれた文字列。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

上記の例ではリンク**libMyLibrary.a**

使用することができます`-gcc_flags`を任意の使用を実行可能ファイルの最後のリンクを行うには、GCC コンパイラに渡すコマンドライン引数のセットを指定します。 たとえば、次のコマンドラインでは、CFNetwork フレームワークも参照します。

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

ネイティブ ライブラリに C++ コードが含まれている場合も渡す必要があります - cxx であるフラグ「余分な引数」Xamarin.iOS を正しいコンパイラを使用して確認できるようにします。 C++ の以前のオプションをようになります。

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>C から C メソッドにアクセスします。&#35;

2 種類がありますネイティブ ライブラリの使用可能な iOS で。

-  オペレーティング システムの一部であるライブラリを共有します。

-  アプリケーションを使用して出荷するスタティック ライブラリ。


使用するもののいずれかで定義されているメソッドにアクセスする[モノラルの P/invoke 機能](http://www.mono-project.com/docs/advanced/pinvoke/)はほぼ同じである .NET で使用すると同じテクノロジ。

-  起動する C 関数を決定します。
-  その署名を確認します。
-  存在するためのライブラリを決定します。
-  適切な P/invoke 宣言を書き込む


P/invoke を使用する場合は、リンクするライブラリのパスを指定する必要があります。 共有ライブラリの iOS を使用して、パス、ハードコードすることができます、またはで定義しています利便性のための定数を使用するとき、 [Constants クラス](https://developer.xamarin.com/api/type/Constants/)、これらの定数は、iOS の共有ライブラリを取り扱う必要があります。

たとえば、c: でこの署名のある Apple の UIKit ライブラリから UIRectFrameUsingBlendMode メソッドを呼び出す必要がある場合

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

P/invoke 宣言は、次のようになります。

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary として定義された定数だけでは、"/System/Library/Frameworks/UIKit.framework/UIKit"、エントリ ポイントにより必要に応じて外部名 (UIRectFramUsingBlendMode) C# の場合、短い方で別の名前の公開中にRectFrameUsingBlendMode です。

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>C Dylib へのアクセス

呼び出しの前に必要な追加の設定のビットがある場合は、Xamarin.iOS アプリケーションで C Dylib を利用する必要がある場合は、`DllImport`属性。

たとえば、ある場合、`Animal.dylib`で、`Animal_Version`例のアプリケーションを呼び出すことがメソッド、それを利用する前に、ライブラリの場所の Xamarin.iOS に通知する必要があります。

これを行うには、編集、`Main.CS`ファイルし、次のようになります。

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

ここで`/full/path/to/`消費されて Dylib を完全なパスです。 配置でこのコードにし、リンクするのには`Animal_Version`次のようにメソッド。

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>スタティック ライブラリ

スタティック ライブラリは、iOS でのみ使用できます、ためにがない外部の共有ライブラリをリンクすると、ため、DllImport 属性で、path パラメーターは、特別な名前を使用する必要がある`__Internal`(名前の先頭に二重アンダー スコア文字に注意してください) はなくパス名です。

DllImport 共有ライブラリを読み込もうとしているのではなく、現在のプログラムで参照するメソッドのシンボルを検索するようにします。

