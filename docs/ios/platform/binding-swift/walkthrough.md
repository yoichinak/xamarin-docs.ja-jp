---
title: 'チュートリアル: iOS Swift ライブラリをバインドする'
description: この記事では、既存の Swift フレームワークである Gigya の作成に関する実践的なチュートリアルを提供します。 Swift フレームワークのコンパイル、バインド、Xamarin. iOS アプリケーションでのバインドの使用などのトピックについて説明します。
ms.prod: xamarin
ms.assetid: B563ADE9-D0E3-4BC3-A288-66D2296BE706
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: 3bc6f0a95bdd01991ccea69d7917f5fabcb9f51b
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430965"
---
# <a name="walkthrough-bind-an-ios-swift-library"></a>チュートリアル: iOS Swift ライブラリをバインドする

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタム バインディングの使用を調査しています。 今後の開発作業の発展のために、この[**アンケート**](https://www.surveymonkey.com/r/KKBHNLT)にご回答ください。

Xamarin を使用すると、モバイル開発者は、Visual Studio と C# を使用してクロスプラットフォームのネイティブモバイルエクスペリエンスを作成できます。 IOS platform SDK コンポーネントは、すぐに使用できます。 ただし、多くの場合、このプラットフォーム用に開発されたサードパーティ製の Sdk を使用することもできます。この場合、Xamarin ではバインドを使用して実行できます。 サードパーティの目的 C フレームワークを Xamarin. iOS アプリケーションに組み込むには、アプリケーションで使用する前に、Xamarin の iOS バインドを作成する必要があります。

IOS プラットフォームとそのネイティブ言語およびツールは常に進化しており、Swift は iOS 開発環境における最も動的な分野の1つです。 いくつかのサードパーティ製 Sdk があります。この Sdk は、既に目標-C から Swift に移行されており、新しい課題があります。 Swift バインドプロセスは、目的 C と似ていますが、AppStore で受け入れ可能な Xamarin. iOS アプリケーションを正常にビルドして実行するには、追加の手順と構成設定が必要です。

このドキュメントの目的は、このシナリオに対処するためのアプローチの概要を示し、簡単な例を含む詳細なステップバイステップ ガイドを提供することです。

## <a name="background"></a>背景

Swift は2014の Apple によって初めて導入されましたが、現在のところ、サードパーティ製のフレームワークによる導入が急速に進んでいるバージョン5.1 です。 Swift フレームワークをバインドするためのいくつかのオプションがあります。このドキュメントでは、目的 C で生成されたインターフェイスヘッダーを使用したアプローチについて説明します。 ヘッダーは、フレームワークが作成されたときに Xcode ツールによって自動的に作成されます。このヘッダーは、マネージ環境から Swift 環境への通信方法として使用されます。

## <a name="prerequisites"></a>[前提条件]

このチュートリアルを完了するには、次のものが必要です。

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Objective Sharpie](../../../cross-platform/macios/binding/objective-sharpie/get-started.md#installing-objective-sharpie)
- [Appcenter CLI](/appcenter/test-cloud/) (省略可能)

## <a name="build-a-native-library"></a>ネイティブ ライブラリをビルドする

最初の手順として、-C ヘッダーを有効にしたネイティブ Swift フレームワークをビルドします。 フレームワークは、通常、サードパーティの開発者によって提供され、次のディレクトリにパッケージに埋め込まれたヘッダーがあります。 ** \<FrameworkName> フレームワーク/ヘッダー/ \<FrameworkName> -Swift. h**。

このヘッダーは、パブリックインターフェイスを公開します。これは、Xamarin の iOS バインディングメタデータを作成し、Swift フレームワークメンバーを公開する C# クラスを生成するために使用されます。 ヘッダーが存在しない場合、または不完全なパブリックインターフェイスがある場合 (クラスやメンバーが表示されない場合など) は、次の2つのオプションがあります。

- Swift ソースコードを更新してヘッダーを生成し、必要なメンバーを属性でマークします。 `@objc`
- パブリックインターフェイスを制御し、基になるフレームワークへのすべての呼び出しをプロキシするプロキシフレームワークを構築します。

このチュートリアルでは、2番目のアプローチについて説明します。サードパーティのソースコードに対する依存関係が少数であるため、常に使用できるとは限りません。 最初のアプローチを回避するもう1つの理由は、今後のフレームワーク変更をサポートするために必要な追加作業です。 サードパーティのソースコードに変更を追加すると、これらの変更をサポートし、今後のすべての更新でそれらをマージできるようになります。

例として、このチュートリアルでは、 [Gigya SWIFT SDK](https://developers.gigya.com/display/GD/Swift+SDK) のバインドを作成します。

1. Xcode を開き、新しい Swift フレームワークを作成します。これは、Xamarin iOS コードとサードパーティ Swift フレームワーク間のプロキシになります。 [ **ファイル > 新しい > プロジェクト** ] をクリックし、ウィザードの手順に従います。

    ![xcode framework プロジェクトの作成](walkthrough-images/xcode-create-framework-project.png)

    ![xcode name フレームワークプロジェクト](walkthrough-images/xcode-name-framework-project.png)

1. 開発者の web サイトから [Gigya framework](https://developers.gigya.com/display/GD/Swift+SDK) をダウンロードしてアンパックします。 執筆時点では、最新バージョンは[Gigya SWIFT SDK 1.0.9](https://downloads.gigya.com/predownload?fileName=Swift-Core-framework-1.0.9.zip)です。

1. プロジェクトファイルエクスプローラーから **SwiftFrameworkProxy** を選択し、[全般] タブを選択します。

1. **Gigya**パッケージを [全般] タブの [Xcode フレームワークとライブラリ] の一覧にドラッグアンドドロップします。次に、フレームワークを追加するときに、[**必要に**応じて項目をコピーする] オプションをオンにします。

    ![xcode コピーフレームワーク](walkthrough-images/xcode-copy-framework.png)

    Swift フレームワークがプロジェクトに追加されていることを確認します。そうでない場合、次のオプションは使用できません。

1. [ **埋め込みを行わない** ] オプションが選択されていることを確認します。このオプションは、後で手動で制御します。

    [![xcode donotembed オプション](walkthrough-images/xcode-donotembed-option.png)](walkthrough-images/xcode-donotembed-option.png#lightbox)

1. [ビルド設定] オプションに [ **Swift 標準ライブラリ] が常に埋め込ま**れていることを確認します 後で手動で制御され、Swift 用 dylib が最終的なパッケージに含まれるようになります。

    [![xcode always 埋め込み false オプション](walkthrough-images/xcode-alwaysembedfalse-option.png)](walkthrough-images/xcode-alwaysembedfalse-option.png#lightbox)

1. [ **Bitcode を有効に** する] オプションが [ **いいえ**] に設定されていることを確認します。 現時点では、Xamarin には、すべてのライブラリで同じアーキテクチャをサポートするためのすべてのライブラリが必要であるのに対して、iOS にビットコードは含まれていません。

    [![xcode enable bitcode false オプション](walkthrough-images/xcode-enablebitcodefalse-option.png)](walkthrough-images/xcode-enablebitcodefalse-option.png#lightbox)

    フレームワークに対して次のターミナルコマンドを実行すると、結果のフレームワークの Bitcode オプションが無効になっていることを確認できます。

    ```bash
    otool -l SwiftFrameworkProxy.framework/SwiftFrameworkProxy | grep __LLVM
    ```

    出力を空にする必要がある場合は、特定の構成のプロジェクト設定を確認する必要があります。

1. [ **目的-C で生成されたインターフェイスヘッダー名** ] オプションが有効になっていることを確認し、ヘッダー名を指定します。 既定の名前は** \<FrameworkName> -Swift. h**:

    [![xcode objectice-c ヘッダー enabled オプション](walkthrough-images/xcode-objcheaderenabled-option.png)](walkthrough-images/xcode-objcheaderenabled-option.png#lightbox)

1. 目的のメソッドを公開し、属性でマークし `@objc` 、以下で定義されている追加の規則を適用します。 この手順を実行せずにフレームワークをビルドした場合、生成された目的 C のヘッダーは空になり、Xamarin は Swift フレームワークのメンバーにアクセスできなくなります。 新しい Swift ファイル **SwiftFrameworkProxy** を作成し、次のコードを定義して、基になる GIGYA Swift SDK の初期化ロジックを公開します。

    ```swift
    import Foundation
    import UIKit
    import Gigya

    @objc(SwiftFrameworkProxy)
    public class SwiftFrameworkProxy : NSObject {

        @objc
        public func initFor(apiKey: String) -> String {
            Gigya.sharedInstance().initFor(apiKey: apiKey)
            let gigyaDomain = Gigya.sharedInstance().config.apiDomain
            let result = "Gigya initialized with domain: \(gigyaDomain)"
            return result
        }
    }
    ```

    上記のコードには、いくつかの重要な注意事項があります。

    - 元のサードパーティ製 Gigya SDK から Gigya モジュールをインポートし、フレームワークの任意のメンバーにアクセスできるようになりました。
    - 名前を指定する属性を使用して SwiftFrameworkProxy クラスをマークし `@objc` ます。それ以外の場合は、などの一意の読み取り不可能な名前が生成され `_TtC19SwiftFrameworkProxy19SwiftFrameworkProxy` ます。 型名は、後で名前によって使用されるため、明確に定義されている必要があります。
    - からプロキシクラスを継承し `NSObject` ます。それ以外の場合は、目的の C ヘッダーファイルには生成されません。
    - 公開するすべてのメンバーをとしてマーク `public` します。

1. [ビルド構成を **デバッグ** から **リリース**に変更する。 これを行うには、[スキームの編集] ダイアログボックスで [ **Xcode > ターゲット >** 開き、[ **ビルド構成** ] オプションを [ **リリース**] に設定します。

    ![xcode 編集スキーム](walkthrough-images/xcode-edit-scheme.png)

    ![xcode 編集スキームのリリース](walkthrough-images/xcode-edit-scheme-release.png)

1. この時点で、フレームワークを作成する準備ができました。 シミュレーターとデバイスアーキテクチャの両方のフレームワークをビルドし、出力を1つのフレームワークパッケージとして結合します。 **Xcodebuild**ツールを使用してソースコードをビルドするために、インストールされている SDK のバージョンを識別します。

    ```bash
    xcodebuild -showsdks
    ```

    出力は次のようになります。

    ```bash
    iOS SDKs:
            iOS 13.2                        -sdk iphoneos13.2
    iOS Simulator SDKs:
            Simulator - iOS 13.2            -sdk iphonesimulator13.2
    macOS SDKs:
            DriverKit 19.0                  -sdk driverkit.macosx19.0
            macOS 10.15                     -sdk macosx10.15
    tvOS SDKs:
            tvOS 13.2                       -sdk appletvos13.2
    tvOS Simulator SDKs:
            Simulator - tvOS 13.2           -sdk appletvsimulator13.2
    watchOS SDKs:
            watchOS 6.1                     -sdk watchos6.1
    watchOS Simulator SDKs:
            Simulator - watchOS 6.1         -sdk watchsimulator6.1
    ```

    目的の iOS SDK と iOS シミュレーター SDK バージョン (この場合はバージョン 13.2) を選択し、次のコマンドを使用してビルドを実行します。

    ```bash
    xcodebuild -sdk iphonesimulator13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    xcodebuild -sdk iphoneos13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    ```

    > [!TIP]
    > Project ではなくワークスペースがある場合は、ワークスペースをビルドし、必要なパラメーターとしてターゲットを指定します。 また、ワークスペースの場合、このディレクトリはプロジェクトビルドの場合とは異なるため、出力ディレクトリを指定することもできます。

    > [!TIP]
    > また [、ヘルパースクリプト](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L3-L14) を使用して、適用可能なすべてのアーキテクチャのフレームワークをビルドしたり、ターゲットセレクターの Xcode スイッチシミュレーターとデバイスから構築することもできます。

1. Swift フレームワークは2つあります。プラットフォームごとに1つのパッケージとして結合し、後で Xamarin. iOS バインドプロジェクトに埋め込むことができます。 両方のアーキテクチャを組み合わせた fat フレームワークを作成するには、次の手順を実行する必要があります。 フレームワークパッケージはフォルダーであるため、ファイルの追加、削除、置換など、あらゆる種類の操作を実行できます。

    - **Release-iリストで iphonesimulator os**および**release-** サブフォルダーがあるビルド出力フォルダーに移動し、最終的な出力 (fat フレームワーク) の初期バージョンとして、いずれかのフレームワークをコピーします。

        ```bash
        cp -R "Release-iphoneos" "Release-fat"
        ```

    - 別のビルドのモジュールと fat フレームワークモジュールを組み合わせる

        ```bash
        cp -R "Release-iphonesimulator/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/" "Release-fat/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/"
        ```

    - Iリストで iphonesimulator configuration を fat フレームワークとして組み合わせる

        ```bash
        lipo -create -output "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphoneos/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphonesimulator/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

    - 検証結果

        ```bash
        lipo -info "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

        出力では、フレームワークの名前と含まれるアーキテクチャを反映して、次のものが表示されます。

        ```bash
        Architectures in the fat file: Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy are: x86_64 arm64
        ```

    > [!TIP]
    > 1つのプラットフォームのみをサポートする場合 (たとえば、デバイスでのみ実行できるアプリをビルドする場合) は、この手順をスキップして、fat ライブラリを作成し、前にデバイスビルドの出力フレームワークを使用することができます。

    > [!TIP]
    > また、 [ヘルパースクリプト](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L16-L24) を使用して、上記のすべての手順を自動化する fat フレームワークを作成することもできます。

## <a name="prepare-metadata"></a>メタデータを準備する

現時点では、Xamarin. iOS バインドで使用できるように、C で生成された目的のインターフェイスヘッダーを持つフレームワークを用意する必要があります。  次の手順では、C# クラスを生成するためにバインドプロジェクトによって使用される API 定義インターフェイスを準備します。 これらの定義は、手動で作成することも、 [目的のマジックペン](../../../cross-platform/macios/binding/objective-sharpie/index.md) ツールおよび生成されたヘッダーファイルによって自動的に作成することもできます。 マジックペンを使用してメタデータを生成します。

1. 公式ダウンロード web サイトから最新の [目標マジックペン](../../../cross-platform/macios/binding/objective-sharpie/index.md) ツールをダウンロードして、ウィザードに従ってインストールします。 インストールが完了したら、マジックペンコマンドを実行して確認できます。

    ```bash
    sharpie -v
    ```

1. マジックペンと自動生成された目標 C ヘッダーファイルを使用してメタデータを生成します。

    ```bash
    sharpie bind --sdk=iphoneos13.2 --output="XamarinApiDef" --namespace="Binding" --scope="Release-fat/SwiftFrameworkProxy.framework/Headers/" "Release-fat/SwiftFrameworkProxy.framework/Headers/SwiftFrameworkProxy-Swift.h"
    ```

    出力には、生成されるメタデータファイル **ApiDefinitions.cs** と **StructsAndEnums.cs**が反映されます。 次の手順でこれらのファイルを保存して、ネイティブ参照と共に Xamarin. iOS バインドプロジェクトに追加します。

    ```bash
    Parsing 1 header files...
    Binding...
        [write] ApiDefinitions.cs
        [write] StructsAndEnums.cs
    ```

    次のコードのように、公開されている各目標 C メンバーの C# メタデータがツールによって生成されます。 ご覧のように、人間が判読できる形式と単純なメンバーのマッピングがあるため、手動で定義できます。

    ```csharp
    [Export ("initForApiKey:")]
    string InitForApiKey (string apiKey);
    ```

    > [!TIP]
    > ヘッダーファイル名は、ヘッダー名の既定の Xcode 設定を変更した場合、異なる可能性があります。 既定では、プロジェクト名には **-Swift** サフィックスが付いています。 フレームワークパッケージの headers フォルダーに移動すると、ファイルとその名前をいつでも確認できます。

    > [!TIP]
    > オートメーションプロセスの一環として、fat フレームワークを作成した後に [、ヘルパースクリプト](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L35) を使用してメタデータを自動的に生成できます。

## <a name="build-a-binding-library"></a>バインド ライブラリをビルドする

次の手順では、Visual Studio バインドテンプレートを使用して Xamarin. iOS バインドプロジェクトを作成し、必要なメタデータとネイティブ参照を追加してから、プロジェクトをビルドして使用できるライブラリを生成します。

1. Visual Studio for Mac を開き、新しい Xamarin. iOS バインドライブラリプロジェクトを作成して名前を付けます。この場合は、SwiftFrameworkProxy を実行し、ウィザードを完了します。 Xamarin. iOS バインドテンプレートは次のパスにあります: **ios > Library > Binding library**:

    ![visual studio のバインドライブラリの作成](walkthrough-images/visualstudio-create-binding-library.png)

1. 既存のメタデータファイル **ApiDefinition.cs** と **Structs.cs** は、目標マジックペンツールによって生成されたメタデータに完全に置き換えられるため、削除します。
1. マジックペンによって生成されたメタデータをコピーする前の手順のいずれかで、[プロパティ] ウィンドウで次のビルドアクションを選択します。 **ApiDefinitions.cs**ファイルの場合は**Objbindingapidefinition** 、 **StructsAndEnums.cs**ファイルの場合は**objbindingapidefinition**です。

    ![visual studio プロジェクト構造のメタデータ](walkthrough-images/visualstudio-project-structure-metadata.png)

    メタデータ自体は、公開されている各目標 C クラスと C# 言語を使用したメンバーを記述します。 C# の宣言と共に、元の目的 C のヘッダー定義を確認できます。

    ```csharp
    // @interface SwiftFrameworkProxy : NSObject
    [BaseType (typeof(NSObject))]
    interface SwiftFrameworkProxy
    {
        // -(NSString * _Nonnull)initForApiKey:(NSString * _Nonnull)apiKey __attribute__((objc_method_family("none"))) __attribute__((warn_unused_result));
        [Export ("initForApiKey:")]
        string InitForApiKey (string apiKey);
    }
    ```

    有効な C# コードであっても、このメタデータ定義に基づいて C# クラスを生成するために、そのまま使用されますが、Xamarin の iOS ツールで使用されます。 その結果、SwiftFrameworkProxy インターフェイスではなく、同じ名前の C# クラスを取得します。このクラスは、Xamarin. iOS コードでインスタンス化できます。 このクラスは、メタデータで定義されているメソッド、プロパティ、およびその他のメンバーを取得します。これは、C# の方法で呼び出されます。

1. 生成された以前の fat フレームワークに加えて、そのフレームワークの各依存関係にネイティブ参照を追加します。 この場合は、SwiftFrameworkProxy と Gigya framework の両方のネイティブ参照をバインドプロジェクトに追加します。

    - ネイティブフレームワーク参照を追加するには、finder を開き、フレームワークがあるフォルダーに移動します。 フレームワークをソリューションエクスプローラー内のネイティブ参照の場所にドラッグアンドドロップします。 または、[ネイティブ参照] フォルダーのコンテキストメニューオプションを使用して、[ **ネイティブ参照の追加** ] をクリックし、フレームワークを参照して追加することもできます。

    ![visual studio プロジェクト構造のネイティブ参照](walkthrough-images/visualstudio-project-structure-nativerefs.png)

    - すべてのネイティブ参照のプロパティを更新し、3つの重要なオプションを確認します。

        - スマートリンクの設定 = true
        - 強制読み込みの設定 = false
        - 元のフレームワークの作成に使用されるフレームワークの一覧を設定します。 この場合、各フレームワークには、Foundation と UIKit の2つの依存関係のみがあります。 [フレームワーク] フィールドに設定します。

        ![visual studio のプロキシオプション](walkthrough-images/visualstudio-nativeref-proxy-options.png)

        指定する追加のリンカーフラグがある場合は、[リンカーフラグ] フィールドに設定します。 この場合は、空のままにします。

    - 必要に応じて、追加のリンカーフラグを指定します。 バインドしているライブラリが、目的 C の Api のみを公開していても、内部で Swift が使用されている場合は、次のような問題が発生している可能性があります。

        ```Console
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftQuartzCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCoreImage
        ```

        ネイティブライブラリのバインドプロジェクトのプロパティで、次の値をリンカーフラグに追加する必要があります。

        ```Linker
        L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphonesimulator/ -L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos -Wl,-rpath -Wl,@executable_path/Frameworks
        ```

        最初の2つのオプション (1 つ) は、  `-L ...`   swift ライブラリを検索する場所をネイティブコンパイラに指示します。 ネイティブコンパイラは、正しいアーキテクチャを持たないライブラリを無視します。つまり、シミュレーターライブラリとデバイスライブラリの両方の場所を同時に渡すことができるため、シミュレーターとデバイスの両方のビルドで機能します (これらのパスは iOS に対してのみ適切で、tvOS と watchOS は更新する必要があります)。 欠点の1つとして、この方法では正しい Xcode が/Application/Xcode.app にある必要があります。これは、バインドライブラリのコンシューマーが別の場所に Xcode を持っている場合は機能しません。 別の方法として、実行可能ファイルプロジェクトの iOS ビルドオプション () の追加の mtouch 引数にこれらのオプションを追加し `--gcc_flags -L... -L...` ます。 3番目のオプションを使用すると、ネイティブリンカーは swift ライブラリの場所を実行可能ファイルに格納し、OS がそれを検出できるようにします。

1. 最後のアクションでは、ライブラリをビルドし、コンパイルエラーがないことを確認します。 多くの場合、目的のマジックペンによって生成されるバインディングメタデータには属性で注釈が付けられ  `[Verify]`   ます。 これらの属性は、バインディングを元のマジックペン宣言 (バインドされた宣言の上のコメントで提供される) と比較することによって、目標が正しいことを確認する必要があることを示します。 属性でマークされたメンバーの詳細については [、次のリンク](../../../cross-platform/macios/binding/objective-sharpie/platform/verify.md)を参照してください。 プロジェクトがビルドされると、Xamarin. iOS アプリケーションで使用できるようになります。

## <a name="consume-the-binding-library"></a>バインド ライブラリを使用する

最後の手順では、Xamarin. ios アプリケーションで Xamarin の iOS バインドライブラリを使用します。 新しい Xamarin. iOS プロジェクトを作成し、バインドライブラリへの参照を追加して、Gigya Swift SDK をアクティブにします。

1. Xamarin. iOS プロジェクトを作成します。 **シングルビューアプリ > iOS > アプリ**を開始点として使用できます。

    ![visual studio アプリの新規作成](walkthrough-images/visualstudio-app-new.png)

1. 前に作成したターゲットプロジェクトまたは .dll へのバインドプロジェクト参照を追加します。 バインドライブラリを通常の Xamarin. iOS ライブラリとして扱います。

    ![visual studio アプリの参照](walkthrough-images/visualstudio-app-refs.png)

1. アプリのソースコードを更新し、初期化ロジックをプライマリ ViewController に追加します。これにより、Gigya SDK がアクティブになります。

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
        var proxy = new SwiftFrameworkProxy();
        var result = proxy.InitForApiKey("APIKey");
        System.Diagnostics.Debug.WriteLine(result);
    }
    ```

1. " **Btnlogin** " という名前のボタンを作成し、次のボタンクリックハンドラーを追加して認証フローをアクティブにします。

    ```csharp
    private void btnLogin_Tap(object sender, EventArgs e)
    {
        _proxy.LoginWithProvider(GigyaSocialProvidersProxy.Instagram, this, (result, error) =>
        {
            // process your login result here
        });
    }
    ```

1. アプリを実行すると、デバッグ出力には次の行が表示されます `Gigya initialized with domain: us1.gigya.com` 。 認証フローをアクティブにするには、このボタンをクリックします。

    [![swift プロキシの結果](walkthrough-images/swiftproxy-result.png)](walkthrough-images/swiftproxy-result.png#lightbox)

おめでとうございます! Swift フレームワークを使用する Xamarin iOS アプリとバインドライブラリが正常に作成されました。 上記のアプリケーションは、ios 12.2 以降で正常に実行されます。これは、この iOS バージョン Apple から開始したときに [ABI の安定性が導入](https://swift.org/blog/swift-5-1-released/) され、すべての ios が 12.2 + を開始するため、swift 5.1 + でコンパイルされたアプリケーションの実行に使用できる swift ランタイムライブラリが含まれているためです。 以前のバージョンの iOS のサポートを追加する必要がある場合は、さらにいくつかの手順を実行します。

1. IOS 12.1 以前のサポートを追加するには、フレームワークをコンパイルするために使用する特定の Swift 用 dylib を発送する必要があります。 [SwiftRuntimeSupport](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/) NuGet パッケージを使用して、必要な lib を処理し、IPA にコピーします。 ターゲットプロジェクトに NuGet 参照を追加し、アプリケーションをリビルドします。 それ以上の手順は必要ありません。 NuGet パッケージは、ビルドプロセスで実行される特定のタスクをインストールし、必要な Swift 用 dylib を特定して、最終的な IPA でパッケージ化します。

1. アプリを app store に送信するには、Xcode および配付オプションを使用する必要があります。これにより、IPA ファイルと **SwiftSupport** フォルダー用 dylib が更新され、appstore によって受け入れられるようになります。

    ○アプリをアーカイブします。 [Visual Studio for Mac] メニューで、[ **発行のためのビルド > アーカイブ**] を選択します。

    ![発行のための visual studio アーカイブ](walkthrough-images/visualstudio-archiveforpublishing.png)

    この操作により、プロジェクトがビルドされ、オーガナイザーに対して実行されます。オーガナイザーは、Xcode が配布用にアクセスできます。

    ○を Xcode 経由で配布します。 Xcode を開き、[ **> オーガナイザー** ] メニューオプションのウィンドウに移動します。

    ![visual studio アーカイブ](walkthrough-images/visualstudio-archives.png)

    前の手順で作成したアーカイブを選択し、[アプリの配布] ボタンをクリックします。 ウィザードに従って、アプリケーションを AppStore にアップロードします。

1. この手順は省略可能ですが、アプリが iOS 12.1 以前と12.2 の両方で実行できることを確認することが重要です。 これは Test Cloud と UITest framework のヘルプを使用して行うことができます。 アプリを実行する UITest プロジェクトと基本的な UI テストを作成します。

    - UITest プロジェクトを作成し、Xamarin. iOS アプリケーション用に構成します。

        ![visual studio uitest の新規作成](walkthrough-images/visualstudio-uitest-new.png)

        > [!TIP]
        > UITest プロジェクトを作成し、アプリ用に構成する方法の詳細については [、次のリンク](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)を参照してください。

    - 基本テストを作成してアプリを実行し、Swift SDK 機能の一部を使用します。 このテストでは、アプリがアクティブ化され、ログインが試行され、[キャンセル] ボタンが押されます。

        ```csharp
        [Test]
        public void HappyPath()
        {
            app.WaitForElement(StatusLabel);
            app.WaitForElement(LoginButton);
            app.Screenshot("App loaded.");
            Assert.AreEqual(app.Query(StatusLabel).FirstOrDefault().Text, "Gigya initialized with domain: us1.gigya.com");

            app.Tap(LoginButton);
            app.WaitForElement(GigyaWebView);
            app.Screenshot("Login activated.");

            app.Tap(CancelButton);
            app.WaitForElement(LoginButton);
            app.Screenshot("Login cancelled.");
        }
        ```

        > [!TIP]
        > UITests framework と UI オートメーションの詳細について [は、次のリンク](/appcenter/test-cloud/uitest/)を参照してください。

    - App center で iOS アプリを作成し、テストを実行する新しいデバイスセットを使用して新しいテストの実行を作成します。

        ![visual studio app center 新規](walkthrough-images/visualstudio-appcenter-new.png)

        > [!TIP]
        > AppCenter Test Cloud の詳細について [は、次のリンク](/appcenter/test-cloud/)を参照してください。

    - Appcenter CLI のインストール

        ```bash
        npm install -g appcenter-cli
        ```

        > [!IMPORTANT]
        > Node v2.0 以降がインストールされていることを確認します。

    - 次のコマンドを使用して、テストを実行します。 また、appcenter コマンドラインが現在ログインしていることを確認します。

        ```bash
        appcenter test run uitest --app "Mobile-Customer-Advisory-Team/SwiftBinding.iOS" --devices a7e7cb50 --app-path "Xamarin.SingleView.ipa" --test-series "master" --locale "en_US" --build-dir "Xamarin/Xamarin.SingleView.UITests/bin/Debug/"
        ```

    - 結果を確認します。 AppCenter ポータルで、 **アプリ > テスト > テストの実行**に移動します。

        ![visual studio appcenter uitest の結果](walkthrough-images/visualstudio-appcenter-uitest-result.png)

        次のように、目的のテストの実行を選択し、結果を確認します。

        ![visual studio appcenter uitest の実行](walkthrough-images/visualstudio-appcenter-uitest-runs.png)

Xamarin. iOS バインドライブラリを介してネイティブ Swift フレームワークを使用する基本的な Xamarin. iOS アプリケーションを開発しました。 この例では、選択したフレームワークを使用するための単純な方法を示しています。実際のアプリケーションでは、より多くの Api を公開し、これらの Api のメタデータを準備する必要があります。 メタデータを生成するスクリプトにより、フレームワーク Api への将来の変更が簡略化されます。

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
- [サンプル プロジェクト リポジトリ](https://github.com/alexeystrakh/xamarin-binding-swift-framework)