---
title: "既存の Mac アプリケーションの更新"
description: "アプリを更新する既存 Xamarin.Mac Unified API を使用してこれらの手順に従います。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 46118b5879589c963898ab7f60c61bd8e38f3900
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="updating-existing-mac-apps"></a>既存の Mac アプリケーションの更新

_アプリを更新する既存 Xamarin.Mac Unified API を使用してこれらの手順に従います。_

Unified API を使用して既存のアプリを更新すると、同様に、プロジェクト ファイル自体の名前空間と、アプリケーション コードで使用される Api への変更が必要です。

## <a name="the-road-to-64-bits"></a>64 ビットへの道

新しい Unified Api が Xamarin.Mac アプリケーションからデバイスに 64 ビット アーキテクチャをサポートするために必要です。 、2015 年 2 月 1 日の時点では、Apple は、すべて新しいアプリへの発信 Mac App Store に 64 ビット アーキテクチャがサポートする必要があります。

Xamarin Mac 用の Visual Studio および Visual Studio の両方を Classic API から Unified API への移行プロセスを自動化するためのツールを提供するか、プロジェクト ファイルを手動で変換することができます。 自動のツールを使用して、強くお勧め、中に、この記事の内容は両方の方法を説明します。

### <a name="before-you-start"></a>開始する前にしています.

Unified API に、既存のコードを更新する前にすべてを除去することを強くお勧め*コンパイルの警告*です。 多く*警告*Classic API ではエラーになる統合に移行するとします。 Classic API からのメッセージのコンパイラは、更新するものにヒントを提供する多くの場合、ために、開始する前にそれらを修正することが簡単です。

## <a name="automated-updating"></a>自動更新

Mac または Visual Studio の Visual Studio で既存の Mac プロジェクトを選択して、警告が修正され後、 **Xamarin.Mac Unified API への移行**から、**プロジェクト**メニュー。 例えば:

![](updating-mac-apps-images/beta-tool1.png "[プロジェクト] メニューから Xamarin.Mac Unified API への移行を選択します。")

この警告は、自動的に移行を実行する前にすることに同意する必要があります (当然ながらようにこの adventure に着手する前にバックアップ/ソース コントロールがある)。

![](updating-mac-apps-images/migrate01.png "自動移行を実行する前に、この警告に同意します。")

Xamarin.Mac アプリケーションで、統合 API の使用時に選択できる 2 つのサポートされているターゲット フレームワーク種類があります。

- **Xamarin.Mac Mobile Framework** -これは、Xamarin.iOS と Xamarin.Android 全体のサブセットをサポートで使用される同じのチューニング対象の .NET framework**デスクトップ**フレームワークです。 これは、優れたリンク動作によるものより小さい平均バイナリを提供するための推奨されるフレームワークです。
- **Xamarin.Mac .NET 4.5 Framework** -このフレームワークは、のサブセットでは、もう一度、**デスクトップ**フレームワークです。 ただし、小さくするときに完全はるかに少なくて**デスクトップ**フレームワークよりも、**モバイル**framework する必要があります_「同じように作業」_ほとんどの NuGet パッケージまたはサード パーティ製ライブラリを持つ。 これにより、標準を使用する開発者**デスクトップ**アセンブリもサポートされているフレームワークが、このオプションを使用して大規模なアプリケーション バンドルを生成中にします。 これは、サード パーティ製の .NET アセンブリが使用されていると互換性がないことをお勧めのフレームワーク、 **Xamarin.Mac Mobile Framework**です。 サポートされているアセンブリの一覧を参照してください、[アセンブリ](~/cross-platform/internals/available-assemblies.md)ドキュメント。

ターゲット フレームワークの詳細情報と Xamarin.Mac アプリケーションの特定のターゲットを選択した場合の影響を参照してください、[ターゲット フレームワーク](~/mac/platform/target-framework.md)ドキュメント。 

ツールは基本的に記載されているすべてのステップを自動化、**手動で更新**セクションでは、次に示すし、推奨される方法を Unified API に Xamarin.Mac の既存のプロジェクトを変換します。

## <a name="steps-to-update-manually"></a>手動で更新する手順

もう一度、したら、警告が解決されて、手動でアプリを更新する Xamarin.Mac 新しい Unified API を使用してこれらの手順に従います。

### <a name="1-update-project-type--build-target"></a>1.プロジェクトの更新の種類およびビルドのターゲット

プロジェクト フレーバーの変更、 **csproj**ファイルから`42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23`に`A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`です。 編集、 **csproj**ファイルの最初の項目を置き換えて、テキスト エディターで、`<ProjectTypeGuids>`要素を示すようにします。

![](updating-mac-apps-images/csproj.png "ように、ProjectTypeGuids 要素の最初の項目を置き換えて、テキスト エディターで、csproj ファイルを編集します。")

変更、**インポート**要素を含む`Xamarin.Mac.targets`に`Xamarin.Mac.CSharp.targets`ように。

![](updating-mac-apps-images/csproj2.png "ように、Xamarin.Mac.CSharp.targets に Xamarin.Mac.targets を含むインポート要素を変更します。")

次の行の後にコードを追加、`<AssemblyName>`要素。

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

例:

![](updating-mac-apps-images/csproj3.png "< AssemblyName > 要素の後にこれらのコード行を追加します。")

### <a name="2-update-project-references"></a>2.プロジェクト参照を更新します。

Mac アプリケーション プロジェクトの展開**参照**ノード。 最初に表示、* 壊れた - **XamMac**参照 (プロジェクトの種類を変更した) ため、このスクリーン ショットに類似します。

![](updating-mac-apps-images/references.png "最初に表示されます分割 XamMac の参照をこのスクリーン ショットのような")

クリックして、**歯車アイコン**の横にある、 **XamMac**エントリを選択**削除**壊れている参照を削除します。

次に、右クリックし、**参照**内のフォルダー、**ソリューション エクスプ ローラー**選択**参照の編集**です。 参照の一覧の一番下までスクロールし、チェックだけでなく**Xamarin.Mac**です。

![](updating-mac-apps-images/references2.png "参照の一覧の一番下までスクロールし、Xamarin.Mac 以外のチェックを配置")

キーを押して**OK**プロジェクト参照の変更を保存します。

### <a name="3-remove-monomac-from-namespaces"></a>3.名前空間から MonoMac を削除します。

削除、 **MonoMac**プレフィックスで名前空間から`using`ステートメントまたは任意の場所、classname 完全認定されたなどです。 `MonoMac.AppKit` 単なる`AppKit`)。

### <a name="4-remap-types"></a>4.型を再マップします。

[ネイティブ型](~/cross-platform/macios/nativetypes.md)されている導入以前、使用されていたのインスタンスなど一部の種類を置換する`System.Drawing.RectangleF`で`CoreGraphics.CGRect`(など)。 型の完全な一覧にあります、[ネイティブ型](~/cross-platform/macios/nativetypes.md)ページ。

### <a name="5-fix-method-overrides"></a>5.メソッドのオーバーライドを修正します。

いくつか`AppKit`メソッドの署名を使用して、新しい変更があった[ネイティブ型](~/cross-platform/macios/nativetypes.md)(など`nint`)。 カスタムのサブクラスは、これらのメソッドをオーバーライドする場合、署名と一致しなくなります、エラーが発生します。 ネイティブ型を使用して新しいシグネチャと一致するサブクラスを変更することで、これらのメソッドのオーバーライドを修正します。 

## <a name="considerations"></a>注意事項

次の考慮事項は、コンポーネントまたは NuGet パッケージの 1 つまたは複数のアプリケーションが依存している場合は、新しい Unified API を Classic API から Xamarin.Mac の既存のプロジェクトを変換時に考慮する必要があります。 

### <a name="components"></a>コンポーネント

アプリケーションに含まれている任意のコンポーネントも Unified API に更新する必要があります。 またはコンパイルしようとすると、競合が発生します。 含まれるコンポーネントの Unified API をサポートする Xamarin コンポーネント ストアから新しいバージョンで現在のバージョンを置き換えるし、クリーン ビルドを実行します。 32 ビットの唯一のコンポーネント ストアで警告の作成者によって変換されていない任意のコンポーネントが表示されます。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API のサポートを使用する NuGet への変更の原因は、中にされていない、NuGet の新しいリリースを新しい Api を認識する NuGet を取得する方法を検討しているためです。 

コンポーネントと同じように、その時点までには、Unified Api をサポートするバージョンに、プロジェクトに含めるすべての NuGet パッケージを切り替えるし、その後、クリーン ビルドを実行する必要があります。

> [!IMPORTANT]
> フォームでエラーがあれば_"エラー 3 は、同じ Xamarin.Mac プロジェクトに 'monomac.dll' と 'Xamarin.Mac.dll' の両方を含めることはできません - 'Xamarin.Mac.dll' は 'monomac.dll' によって参照されますが、明示的に参照される ' xxx、バージョン = 0.0.000、カルチャ =neutral, PublicKeyToken = null'"_ Unified Api にアプリを変換した後は、通常、統合 API が更新されていないプロジェクトで、コンポーネントまたは NuGet パッケージを持つためです。 削除する既存のコンポーネント/NuGet Unified Api をサポートするバージョンに更新して、クリーン ビルドを行う必要があります。

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin.Mac アプリのビルド時に 64 ビットの有効化

Xamarin.Mac モバイル アプリケーションの Unified API に変換されている場合、開発者もする必要がありますを使用するアプリのオプションから 64 ビット コンピューターでアプリケーションの構築します。 参照してください、**を有効にする 64 ビット ビルドの Xamarin.Mac アプリ**の[32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64/index.md)64 ビットを有効にする方法の詳細な手順についてはドキュメントを作成します。
    
## <a name="finishing-up"></a>作業の完了

メソッドを使用して、自動または手動 Xamarin.Mac アプリケーションをクラシックから Unified Api に変換することを選択するかどうかをさらの手動による介入が必要とするいくつかのインスタンスがあります。 参照してください、 [Unified API にコードを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)ドキュメントの既知の問題と回避策です。

## <a name="related-links"></a>関連リンク

- [コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [従来の vs Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
