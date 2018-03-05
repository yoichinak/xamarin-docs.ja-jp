---
title: "バインディングを統一された API に移行します。"
description: "この記事では、Xamarin.IOS および Xamarin.Mac アプリケーションの統合された Api をサポートするために既存の Xamarin バインド プロジェクトの更新に必要な手順について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: b07a88f25da1ea504785ddc4a0db8db1ed0e2650
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="migrating-a-binding-to-the-unified-api"></a>バインディングを統一された API に移行します。

_この記事では、Xamarin.IOS および Xamarin.Mac アプリケーションの統合された Api をサポートするために既存の Xamarin バインド プロジェクトの更新に必要な手順について説明します。_

#<a name="overview"></a>概要

、2015 年 2 月 1 日の開始 Apple では、すべて新しいおよびへの発信、iTunes Mac App Store は 64 ビット アプリケーションである必要がある必要があります。 その結果、すべて新しい Xamarin.iOS または Xamarin.Mac アプリケーションでは、64 ビットをサポートするために、既存のクラシック MonoTouch と MonoMac Api ではなく新しい Unified API を使用する必要があります。

さらに、Xamarin バインド プロジェクトでは、64 ビット Xamarin.iOS または Xamarin.Mac プロジェクトに含まれる新しい Unified Api がサポートもする必要があります。 この記事では、Unified API を使用して既存のバインド プロジェクトを更新するための手順を説明します。

#<a name="requirements"></a>必要条件

この記事で紹介する手順を完了する、次が必要。

 -  **Visual Studio for Mac** -Visual Studio for Mac の最新バージョンがインストールされており、開発用コンピューター上で構成します。
 -  **Apple Mac** - Apple iOS 用のバインドのプロジェクトをビルドする mac が必要とファルダ

Windows コンピューター上の Visual studio では、プロジェクトのバインドはサポートされていません。

#<a name="modify-the-using-statements"></a>使用してステートメントを変更します。

Unified Api よりも簡単にこれまで Mac と iOS だけでなくできると、同じ 32 と 64 ビットのアプリケーションをサポートするバイナリ間でコードを共有します。 ドロップによって、 _MonoMac_と_MonoTouch_プレフィックスから名前空間、Xamarin.Mac と Xamarin.iOS アプリケーション プロジェクト間で簡単に共有を実現します。

バインディング、コントラクトのいずれかを変更する必要がありますが、結果として (およびその他の`.cs`バインド プロジェクト内のファイル) を削除する、 _MonoMac_と_MonoTouch_からプレフィックス、 `using`ステートメント。

たとえば、次を指定、バインドのコントラクトでステートメントを使用します。

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

取り除くことが、`MonoTouch`次の結果として得られるプレフィックス。

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

これを行う任意の必要がありますが、もう一度`.cs`バインド プロジェクト内のファイルです。 この変更は、次の手順は、新しいネイティブ データ型を使用するバインディングのプロジェクトを更新するは。

Unified API の詳細についてを参照してください、 [Unified API](~/cross-platform/macios/unified/index.md)ドキュメント。 32 と 64 ビットのアプリケーションおよびフレームワークに関する情報をサポートする背景情報については、次を参照してください。、 [32 と 64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64.md)ドキュメント。

#<a name="update-to-native-data-types"></a>ネイティブ データ型への更新

Objective C のマップ、`NSInteger`のデータ型`int32_t`32 ビット システム上に`int64_t`64 ビット システムでします。 この動作を一致するには、新しい Unified API には、前に使用が置き換えられます`int`(として常に定義されている .net `System.Int32`) を新しいデータ型:`System.nint`です。

新しいと共に`nint`データ型の場合は、Unified API が導入されています、`nuint`と`nfloat`へのマッピングの種類、`NSUInteger`と`CGFloat`同様の種類。

API を確認し、任意のインスタンスのことを確認する必要があります指定すると、上、 `NSInteger`、`NSUInteger`と`CGFloat`以前にマップされている`int`、`uint`と`float`新しいに更新される`nint`、 `nuint`と`nfloat`型です。

たとえばの OBJECTIVE-C メソッド定義を指定します。

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

場合は、以前のバインドのコントラクトには、次の定義が必要があります。

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

新しいバインドを更新します。

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
おはマッピングする場合に、新しいバージョン サード パーティ製のライブラリとおが最初に、リンクするよりも、確認する必要があります、`.h`あればライブラリとヘッダー ファイル、既存の明示的な呼び出し`int`、 `int32_t`、 `unsigned int`、`uint32_t`または`float`なるようアップグレードされて、 `NSInteger`、`NSUInteger`または`CGFloat`です。 場合は、同じ変更を`nint`、`nuint`と`nfloat`型は同様のマッピングに対して行う必要があります。

これらのデータ型の変更に関する詳細についてを参照してください、[ネイティブ型](~/cross-platform/macios/nativetypes.md)ドキュメント。

#<a name="update-the-coregraphics-types"></a>CoreGraphics 型を更新します。

使用されるポイント、サイズ、および四角形のデータ型`CoreGraphics`で実行されているデバイスに応じて 32 ビットまたは 64 ビットを使用します。 内のデータ型を一致するように発生した既存のデータ構造を使用した Xamarin iOS および Mac の Api をもともとバインドするときに`System.Drawing`(`RectangleF`など)。

要件のため、64 ビットと、新しいネイティブ データ型をサポートするために、次の調整は必要がありますを呼び出すときに、既存のコードをできる`CoreGraphic`メソッド。

- **CGRect** -使用`CGRect`の代わりに`RectangleF`四角形の領域にポイント浮動を定義する場合。
- **CGSize** -使用`CGSize`の代わりに`SizeF`浮動小数点のポイント サイズ (幅と高さ) を定義するときにします。
- **CGPoint** -使用`CGPoint`の代わりに`PointF`(X と Y 座標) の場所にポイント浮動を定義する場合。

上記の指定すると、API を確認し、任意のインスタンスのことを確認する必要が`CGRect`、`CGSize`または`CGPoint`に既にバインドされていたを`RectangleF`、`SizeF`または`PointF`ネイティブ型に変更する`CGRect`、`CGSize`または`CGPoint`直接です。

たとえば、OBJECTIVE-C 初期化子を指定します。

```csharp
- (id)initWithFrame:(CGRect)frame;
```

場合は、以前のバインドには、次のコードが含まれています。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

そのコードを更新します。

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

すべてのコードの変更の準備には、バインド プロジェクトを変更または Unified Api に対してバインド ファイルを作成する必要があります。

#<a name="modify-the-binding-project"></a>バインディングのプロジェクトを変更します。

Unified Api を使用するバインディングのプロジェクトを更新する最後の手順としていただくために変更するか、 `MakeFile` (for Mac を Visual Studio 内でバインドからおいる) 場合に、プロジェクトまたは Xamarin プロジェクトの種類をビルドし、指示を使用している_btouch_従来のものではなく Unified Api にバインドします。


##<a name="updating-a-makefile"></a>メイクファイルの更新

メイクファイル プロジェクトをビルドする、バインディングに、Xamarin を使用する場合。DLL を含める必要は、`--new-style`コマンド ライン オプションと呼び出し`btouch-native`の代わりに`btouch`です。

したがって、次を指定`MakeFile`:

```csharp
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```

呼び出し元からに切り替える必要があります`btouch`に`btouch-native`ので、次のように、マクロ定義は調整。

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

呼び出しを更新`btouch`を追加し、`--new-style`オプションを次のようにします。

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

実行できるようになりました、 `MakeFile` API の新しい 64 ビット バージョンをビルドするには通常どおりです。

##<a name="updating-a-binding-project-type"></a>バインディングのプロジェクト タイプの更新

私たちを使用している Mac プロジェクト テンプレートのバインド用の Visual Studio API を構築する場合、Unified API の新しいバージョンへのバインドのプロジェクト テンプレートを更新する必要になります。 これを行う最も簡単な方法では、すべての既存のコードと設定に対する新しい統合 API のバインド プロジェクトの追加とコピーを開始します。

次の手順で行います。

1. Visual Studio for mac を起動します。
2. 選択**ファイル** > **新しい** > **ソリューションしています.**
3. 新しいソリューション ダイアログ ボックスで選択**iOS** > **Unified API** > **iOS プロジェクトのバインド**: 

    [ ![](update-binding-images/image01new.png "新しいソリューション ダイアログ ボックスで、iOS を選択/Unified API/iOS バインド プロジェクト")](update-binding-images/image01new.png)
4. 新しいプロジェクトの構成 ダイアログ ボックスを入力してください、**名前**新しいプロジェクトのバインドと をクリック、 **ok**ボタンをクリックします。
5. バインドを作成しようとする Objective C ライブラリの 64 ビット バージョンが含まれます。
6. 32 ビット クラシックの API バインドの既存のプロジェクトからのソース コードをコピー (など、`ApiDefinition.cs`と`StructsAndEnums.cs`ファイル)。
7. ソース コード ファイルに上記に説明変更を行います。

すべての変更を行うには、32 ビット バージョンのと同様に、API の新しい 64 ビット バージョンを作成できます。

#<a name="summary"></a>まとめ

この記事の内容を既存の Xamarin のバインドのプロジェクトに新しい Unified Api と 64 ビットのデバイスをサポートするために必要な変更を示しましたし、API の新しい 64 ビット互換性のあるバージョンをビルドするために必要な手順を実行します。



## <a name="related-links"></a>関連リンク

- [Mac および iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64.md)
- [IOS アプリの既存のアップグレード](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
