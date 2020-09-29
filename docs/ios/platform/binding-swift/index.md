---
title: IOS Swift ライブラリをバインドする
description: このドキュメントでは、コードを Swift にする C# バインディングを作成する方法について説明します。これにより、ネイティブライブラリを使用したり、Xamarin. iOS アプリケーションで併置したりすることができます。
ms.prod: xamarin
ms.assetid: 890EFCCA-A2A2-4561-88EA-30DE3041F61D
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: f678ec02ca5b9b47f6f78eea77547b038274190f
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435516"
---
# <a name="bind-ios-swift-libraries"></a>IOS Swift ライブラリをバインドする

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタム バインディングの使用を調査しています。 今後の開発作業の発展のために、この[**アンケート**](https://www.surveymonkey.com/r/KKBHNLT)にご回答ください。

IOS プラットフォームは、そのネイティブ言語とツールと共に絶えず進化しており、最新のサービスを使用して開発されたサードパーティ製のライブラリが多数あります。 コードとコンポーネントを再利用して最大限に活用することは、クロスプラットフォーム開発の主な目標の 1 つです。 Swift によって構築されたコンポーネントを再利用する機能は、開発者が成長し続けているため、Xamarin 開発者にとってますます重要になっています。 通常の [目的 C](../binding-objective-c/walkthrough.md) ライブラリをバインドするプロセスについては、既に理解している場合があります。 [Swift フレームワークをバインドする](walkthrough.md)プロセスを説明する追加のドキュメントが提供されるようになりました。これにより、Xamarin アプリケーションで同じ方法で使用できるようになります。 このドキュメントの目的は、Xamarin の Swift バインドを作成するための高レベルなアプローチについて説明することです。

## <a name="high-level-approach"></a>上位レベルのアプローチ

Xamarin を使用すると、任意のサード パーティ製のネイティブ ライブラリを、Xamarin アプリケーションで使用できるようにバインドできます。 Swift は新しい言語であり、この言語でビルドされたライブラリのバインドを作成するには、いくつかの追加の手順とツールが必要です。 このアプローチには、次の 4 つの手順が含まれます。

1. ネイティブライブラリのビルド
1. Xamarin のメタデータを準備します。これにより、Xamarin ツールで C# クラスを生成できるようになります。
1. ネイティブライブラリとメタデータを使用した Xamarin バインドライブラリの構築
1. Xamarin アプリケーションでの Xamarin バインドライブラリの使用

以下のセクションでは、これらの手順の概要を追加の詳細情報と共に説明します。

### <a name="build-the-native-library"></a>ネイティブ ライブラリをビルドする

最初の手順として、ネイティブ Swift フレームワークの準備ができて、目的の C ヘッダーを作成します。 このファイルは、必要な Swift クラス、メソッド、およびフィールドを公開する自動生成されたヘッダーであり、Xamarin バインドライブラリを使用して、目的 C と最終的に C# の両方からアクセスできるようにします。 このファイルは、フレームワーク内の次のパスにあります: ** \<FrameworkName> . Framework/Headers/ \<FrameworkName> -Swift. h**。 公開されているインターフェイスに必要なメンバーがすべて含まれている場合は、次の手順に進むことができます。 それ以外の場合、これらのメンバーを公開するには、さらに手順を実行する必要があります。 この方法は、Swift フレームワークのソースコードにアクセスできるかどうかによって異なります。

- コードにアクセスできる場合は、必要な Swift メンバーを属性で装飾 `@objc` し、いくつかの追加ルールを適用して、これらのメンバーが Xcode ビルドツールに対して、これらのメンバーが目的の C およびヘッダーに公開されることを知らせることができます。
- ソースコードにアクセスできない場合は、プロキシ Swift フレームワークを作成する必要があります。これにより、元の Swift フレームワークがラップされ、属性を使用してアプリケーションに必要なパブリックインターフェイスが定義され `@objc` ます。

### <a name="prepare-the-xamarin-metadata"></a>Xamarin メタデータを準備する

2番目の手順では、C# クラスを生成するためにバインドプロジェクトによって使用される API 定義インターフェイスを準備します。 これらの定義は、手動で作成することも、前の手順で自動生成された[マジックペン](../../../cross-platform/macios/binding/objective-sharpie/index.md)ヘッダーファイルによって自動的に作成することもできます** \<FrameworkName> ** 。 メタデータが生成されたら、それを手動で検証して検証する必要があります。

### <a name="build-the-xamarinios-binding-library"></a>Xamarin. iOS バインドライブラリをビルドする

3番目の手順では、特別なプロジェクトである Xamarin. iOS バインドライブラリを作成します。 これは、前の手順で準備されたフレームワークとメタデータと、それぞれのフレームワークが依存している追加の依存関係を参照します。 また、使用中の Xamarin. iOS アプリケーションと共に参照されるネイティブフレームワークのリンクも処理します。

### <a name="consume-the-xamarin-binding-library"></a>Xamarin バインド ライブラリを使用する

最後の4番目の手順では、Xamarin. iOS アプリケーションでバインドライブラリを参照します。 IOS 12.2 以降を対象とする Xamarin の iOS アプリケーションでは、ネイティブライブラリの使用を有効にするだけで十分です。 下位バージョンを対象とするアプリケーションでは、いくつかの追加の手順が必要です。

- 実行時サポートの Swift .dylib 依存関係を追加します。 IOS 12.2 および Swift 5.1 から、言語が ABI (アプリケーションバイナリインターフェイス) 安定し、互換性があるようになりました。 そのため、より低い iOS バージョンをターゲットとするアプリケーションには、フレームワークによって使用される Swift 用 dylib 依存関係を含める必要があります。 [SwiftRuntimeSupport NuGet パッケージ](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)を使用すると、必要な .dylib の依存関係が、生成されるアプリケーションパッケージに自動的に追加されます。
- アップロードプロセス中に AppStore によって検証される、署名付きの dylibs を含む **SwiftSupport** フォルダーを追加します。 パッケージは、Xcode ツールを使用して署名され、AppStore connect に配布される必要があります。そうでない場合は、自動的に拒否されます。

## <a name="walkthrough"></a>チュートリアル

上記のアプローチは、Xamarin の Swift バインドを作成するために必要な手順の概要を示しています。 これらのバインドを実際に準備する際には、ネイティブのツールと言語の変更に合わせるなど、多くの下位レベルの手順が必要になり、さらに詳細な考慮事項があります。 ここでの目的は、この概念と、このプロセスに関連する手順の概要をより深く理解できるようにすることです。 詳細な手順ガイドについては、 [Xamarin Swift のバインド](walkthrough.md) に関するチュートリアルのドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Objective Sharpie](../../../cross-platform/macios/binding/objective-sharpie/index.md)
- [マジックペンメタデータの検証](../../../cross-platform/macios/binding/objective-sharpie/platform/verify.md)
- [バインディングの目的-C フレームワーク](../binding-objective-c/walkthrough.md)
- [Gigya iOS SDK (Swift フレームワーク)](https://developers.gigya.com/display/GD/Swift+SDK)
- [Swift 5.1 ABI の安定性](https://swift.org/blog/swift-5-1-released/)
- [SwiftRuntimeSupport NuGet](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)
- [Xamarin UITest automation](/appcenter/test-cloud/uitest/)
- [Xamarin. iOS UITest の構成](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [AppCenter Test Cloud](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [サンプル プロジェクト リポジトリ](https://github.com/xamcat/xamarin-binding-swift-framework)