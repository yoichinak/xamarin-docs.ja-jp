---
title: IOS Swift ライブラリをバインドする
description: このドキュメントでは、Swift C#コードへのバインドを作成する方法について説明します。これにより、ネイティブライブラリを使用したり、Xamarin. iOS アプリケーションで併置したりすることができます。
ms.prod: xamarin
ms.assetid: 890EFCCA-A2A2-4561-88EA-30DE3041F61D
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: 9a683f31016a9db4271e3909e421f27ef83c2080
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497960"
---
# <a name="bind-ios-swift-libraries"></a>IOS Swift ライブラリをバインドする

IOS プラットフォームは、そのネイティブ言語とツールと共に絶えず進化しており、最新のサービスを使用して開発されたサードパーティ製のライブラリが多数あります。 コードとコンポーネントの再利用を最大化することは、クロスプラットフォーム開発の主な目標の1つです。 Swift によって構築されたコンポーネントを再利用する機能は、開発者が成長し続けているため、Xamarin 開発者にとってますます重要になっています。 通常の[目的 C](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough)ライブラリをバインドするプロセスについては、既に理解している場合があります。 [Swift フレームワークをバインドする](walkthrough.md)プロセスを説明する追加のドキュメントが提供されるようになりました。これにより、Xamarin アプリケーションで同じ方法で使用できるようになります。 このドキュメントの目的は、Xamarin の Swift バインドを作成するための高レベルなアプローチについて説明することです。

## <a name="high-level-approach"></a>高レベルのアプローチ

Xamarin を使用すると、任意のサードパーティ製のネイティブライブラリを、Xamarin アプリケーションで使用できるようにバインドできます。 Swift は新しい言語であり、この言語でビルドされたライブラリのバインドを作成するには、いくつかの追加の手順とツールが必要です。 このアプローチには、次の4つの手順が含まれます。

1. ネイティブライブラリのビルド
1. Xamarin のメタデータを準備して、Xamarin ツールC#でクラスを生成できるようにする
1. ネイティブライブラリとメタデータを使用した Xamarin バインドライブラリの構築
1. Xamarin アプリケーションでの Xamarin バインドライブラリの使用

以下のセクションでは、これらの手順の概要を説明します。

### <a name="build-the-native-library"></a>ネイティブライブラリをビルドする

最初の手順として、ネイティブ Swift フレームワークの準備ができて、目的の C ヘッダーを作成します。 このファイルは、必要な Swift クラス、メソッド、およびフィールドを公開する自動生成されたヘッダーで、目的のC# Swift クラス、メソッド、およびフィールドを公開しています。 このファイルは、フレームワーク内の次のパスにあります: **\<frameworkname >。フレームワーク/ヘッダー/\<frameworkname >-Swift. h**。 公開されているインターフェイスに必要なメンバーがすべて含まれている場合は、次の手順に進むことができます。 それ以外の場合、これらのメンバーを公開するには、さらに手順を実行する必要があります。 この方法は、Swift フレームワークのソースコードにアクセスできるかどうかによって異なります。

- コードにアクセスできる場合は、`@objc` 属性を使用して必要な Swift メンバーを装飾し、いくつかの追加のルールを適用して、これらのメンバーが Xcode ビルドツールに対して、それらのメンバーが目的の C およびヘッダーに公開されることを知らせることができます。
- ソースコードにアクセスできない場合は、プロキシ Swift フレームワークを作成する必要があります。これにより、元の Swift フレームワークがラップされ、`@objc` 属性を使用して、アプリケーションに必要なパブリックインターフェイスが定義されます。

### <a name="prepare-the-xamarin-metadata"></a>Xamarin メタデータを準備する

2番目の手順では、クラスを生成C#するためにバインドプロジェクトによって使用される API 定義インターフェイスを準備します。 これらの定義は、手動で作成することも、[マジックペン](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/)ツールによって自動的に作成することもできます。これにより、前述のように**frameworkname >-Swift. h**ヘッダーファイルが\<生成されます。 メタデータが生成されたら、それを手動で検証して検証する必要があります。

### <a name="build-the-xamarinios-binding-library"></a>Xamarin. iOS バインドライブラリをビルドする

3番目の手順では、特別なプロジェクトである Xamarin. iOS バインドライブラリを作成します。 これは、前の手順で準備されたフレームワークとメタデータと、それぞれのフレームワークが依存している追加の依存関係を参照します。 また、使用中の Xamarin. iOS アプリケーションと共に参照されるネイティブフレームワークのリンクも処理します。

### <a name="consume-the-xamarin-binding-library"></a>Xamarin バインドライブラリを使用する

最後の4番目の手順では、Xamarin. iOS アプリケーションでバインドライブラリを参照します。 IOS 12.2 以降を対象とする Xamarin の iOS アプリケーションでは、ネイティブライブラリの使用を有効にするだけで十分です。 下位バージョンを対象とするアプリケーションでは、いくつかの追加の手順が必要です。

- 実行時サポートの Swift .dylib 依存関係を追加します。 IOS 12.2 および Swift 5.1 から、言語が ABI (アプリケーションバイナリインターフェイス) 安定し、互換性があるようになりました。 そのため、より低い iOS バージョンをターゲットとするアプリケーションには、フレームワークによって使用される Swift 用 dylib 依存関係を含める必要があります。 [SwiftRuntimeSupport NuGet パッケージ](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)を使用すると、必要な .dylib の依存関係が、生成されるアプリケーションパッケージに自動的に追加されます。
- アップロードプロセス中に AppStore によって検証される、署名付きの dylibs を含む**SwiftSupport**フォルダーを追加します。 パッケージは、Xcode ツールを使用して署名され、AppStore connect に配布される必要があります。そうでない場合は、自動的に拒否されます。

## <a name="walkthrough"></a>チュートリアル

上記のアプローチは、Xamarin の Swift バインドを作成するために必要な手順の概要を示しています。 ネイティブのツールと言語の変更に合わせて、これらのバインディングを実際に準備する際には、多くの下位レベルの手順が必要になり、さらに詳細な考慮事項があります。 この目的は、この概念と、このプロセスに関連する大まかな手順を深く理解できるようにすることです。 詳細な手順ガイドについては、 [Xamarin Swift のバインド](walkthrough.md)に関するチュートリアルのドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Objective Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/)
- [マジックペンメタデータの検証](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/platform/verify)
- [バインディングの目的-C フレームワーク](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough)
- [Gigya iOS SDK (Swift フレームワーク)](https://developers.gigya.com/display/GD/Swift+SDK)
- [Swift 5.1 ABI の安定性](https://swift.org/blog/swift-5-1-released/)
- [SwiftRuntimeSupport NuGet](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)
- [Xamarin UITest automation](https://docs.microsoft.com/appcenter/test-cloud/uitest/)
- [Xamarin. iOS UITest の構成](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [AppCenter Test Cloud](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [サンプルプロジェクトリポジトリ](https://github.com/xamcat/xamarin-binding-swift-framework)
