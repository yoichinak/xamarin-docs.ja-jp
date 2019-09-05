---
title: WatchOS で Xamarin のローカライズの操作
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリをローカライズする方法について説明します。 Watch アプリ、ウォッチ拡張機能について説明してストーリー ボードのテキスト、テスト、およびその他のコードでは、文字列。
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 10f240a8e245f24d4b8f646eb972cbe21d28b75c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289937"
---
# <a name="working-with-watchos-localization-in-xamarin"></a>WatchOS で Xamarin のローカライズの操作

_複数の言語、watchOS アプリを適合させる_

![](localization-images/both-languages-sml.png "Apple Watch のローカライズされたコンテンツを表示します。")

watchOS アプリをローカライズするには、標準的な iOS メソッドを使用します。

- 使用して**ローカリゼーション ID**上、ストーリー ボード要素
- **.strings** 、ストーリー ボードに関連付けられているファイルと
- **Localizable.strings**ファイルのコードで使用されるテキスト。

既定のストーリー ボードとリソースにある、**ベース**ディレクトリ、および言語固有の翻訳およびその他のリソースに格納されている **.lproj**ディレクトリ。
iOS および OS の監視が自動的に使用、ユーザーの言語の選択を正しい文字列とリソースを読み込めません。

Apple Watch アプリの Watch App と Watch Extension での使用方法に応じて、2 つの場所でリソースが必要にローカライズされた文字列の 2 つの部分であるため、

ローカライズされたテキストとリソース*異なる*watch アプリでウォッチ拡張機能。

## <a name="watch-app"></a>Watch アプリ

Watch アプリには、アプリのユーザー インターフェイスを記述するストーリー ボードが含まれています。 すべてのコントロール (など`Label`と`Image`) ローカライズのサポートがあること、**ローカリゼーション ID**します。

各言語固有 **.lproj**ディレクトリを含める必要があります **.strings**の各要素に対して変換を含むファイル (を使用して、**ローカリゼーション ID**)、およびイメージストーリー ボードによって参照されます。

## <a name="watch-extension"></a>拡張機能をご覧ください。

ウォッチ拡張機能は、アプリ コードが実行されます。 コードからユーザーに表示される任意のテキストは、拡張機能では、watch アプリではなくローカライズする必要があります。

拡張機能では、特定の言語を含める必要があります **.lproj**ディレクトリが、 **.strings**ファイルは、コードで使用されるテキストの翻訳しか必要とします。

## <a name="globalizing-the-watch-solution"></a>監視ソリューションのグローバル化

グローバリゼーションとローカライズ可能なアプリケーションを作成するプロセスです。
Watch アプリの設計を考慮してテキストの長さが異なるとストーリー ボードつまり場合ように各画面のレイアウトを調整します。 適切にによってどのようなテキストが表示されます。 使用してウォッチ拡張機能のコードで参照されている任意の文字列を変換できることを確認する必要があります、`LocalizedString`メソッド。

### <a name="watch-app"></a>Watch アプリ

既定では、watch アプリはローカリゼーションは構成されていません。 既定のストーリー ボード ファイルを移動し、翻訳の他のいくつかのディレクトリを作成する必要があります。

1. 作成**Base.lproj**ディレクトリと移動、 **Interface.storyboard**にします。

2. サポートする言語ごとに、  **\<言語 >** を作成します。

3. **.Lproj**ディレクトリを含める必要があります、 **Interface.strings** (ファイル名は storboard の名前を一致する必要があります)、テキスト ファイル。 必要に応じて、これらのディレクトリでのローカライズを必要とするすべてのイメージを配置できます。

Watch アプリのプロジェクトは、(言語だけで英語とスペイン語のファイルが追加されました) これらの変更が行われた後に、ようになります。

  ![](localization-images/watchapp-solution.png "英語とスペイン語の言語ファイルと watch アプリ プロジェクト")

#### <a name="storyboard-text"></a>ストーリー ボードのテキスト

ストーリー ボードを編集するときに、各要素と通知を選択、**ローカリゼーション ID**に表示される、**プロパティ**パッド。

  [![](localization-images/storyboard-sml.png "[プロパティ] タブに表示されるローカリゼーション ID")](localization-images/storyboard.png#lightbox)

**Base.lproj**フォルダーによって、キーを形成する、次に示すようにキーと値のペアを作成、**ローカリゼーション ID**ドットで、コントロールのプロパティ名が参加していると (`.`)。

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

この例では通知を**ローカリゼーション ID**できる単純な数値文字列 (例。 「0」、「1」など) または ("AgC-eL-Hgc") などのより複雑な文字列。 `Label` コントロールが、`Text`プロパティと`Button`s が、`Title`プロパティのローカライズされた値が設定の方法で反映されていますが上記の例で示すように小文字のプロパティ名を使用してください。

Watch でストーリー ボードが表示されると、正しい値に自動的に抽出され、ユーザーが選択した言語に従って表示されます。

#### <a name="storyboard-images"></a>ストーリー ボードの画像

ソリューションの例も含まれています、 **gradient@2x.png** 各言語のフォルダー内のイメージ。 この画像はあるが言語ごとに異なる (例です。 、変換に必要なテキストが埋め込まれている可能性があります。 またはローカライズされたリモコンを使用)。

イメージの設定**イメージ**ストーリー ボードと適切なイメージのプロパティは、ユーザーが選択した言語に従ってウォッチに表示されます。

![](localization-images/storyboard-image.png "ストーリー ボードにイメージ イメージのプロパティを設定します")

注: Apple のすべてのウォッチがだけ Retina ディスプレイ、 **@2x** イメージのバージョンが必要です。 指定する必要はありません **@2x** したストーリー ボードにします。

### <a name="watch-extension"></a>拡張機能をご覧ください。

ウォッチ拡張機能では、ローカライズ、ただしはストーリー ボードをサポートするようなディレクトリ構造が必要です。 拡張機能のローカライズされた文字列は参照されているもののみC#コード。

![](localization-images/watchextension-solution.png "ローカリゼーションをサポートするには、ウォッチ拡張機能ディレクトリ構造")

#### <a name="strings-in-code"></a>コード内の文字列

**Localizable.strings**ファイルは、ストーリー ボードに関連付けられていた場合よりも若干異なる構造体を持ちます。 ここで任意の「キー」文字列を選択できます。Apple の推奨事項は、既定の言語で表示される実際のテキストを反映するキーを使用します。

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString`の次のコードに示すように、翻訳済みの対応する文字列を解決するメソッドを使用します。

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>コード内のイメージ

コードによって設定されたイメージは、2 つの方法で設定できます。

1. `Image`コントロールを変更するには、その値を Watch アプリに既に存在するイメージの文字列名に設定します。たとえば、

    ```csharp
    displayImage.SetImage("gradient"); // image in Watch App (as shown above)
    ```

2. 拡張機能からイメージを移動するには、watch を使用する`FromBundle`と、アプリは、ユーザーの言語の選択の適切なイメージを自動的に選択します。 ソリューションの例では、イメージ **language@2x.png** 各言語で、フォルダーとそのに表示される`DetailController`次のコードを使用します。

    ```csharp
    using (var image = UIImage.FromBundle ("language")) {
        displayImage.SetImage (image);
    }
    ```

    イメージのファイル名を参照するときは **@2x** 、を指定する必要はありません。

2 番目のメソッドも watch; で表示するためにリモート サーバーからイメージをダウンロードする場合は、適用できます。ただしここで行う必要があります、ユーザーの設定に従い、イメージをダウンロードすることが正しくローカライズされています。

## <a name="localization"></a>ローカリゼーション

翻訳者は、処理する必要があるソリューションを構成した後、 **.strings**ファイルとイメージをサポートする言語ごとにします。

数だけ作成できます **.lproj**をディレクトリでは、(サポートされている言語ごとに 1 つ) 必要があります。 などの言語のコードを使用してその名前が**en**、 **es**、 **de**、 **ja**、 **PT-BR**、(英語のなど、スペイン語、ドイツ語、日本語、ポルトガル語 (ブラジル)、それぞれ)。

付属のサンプルでは、翻訳の (コンピューターによって生成された) を使用して、watchOS アプリをローカライズする方法を示します。

### <a name="watch-app"></a>Watch アプリ

これらの値は、watch アプリのストーリー ボードで定義されているユーザー インターフェイスの変換に使用されます。 *キー*値は、各ストーリー ボード コントロールの組み合わせ**ローカリゼーション ID**および変換されるプロパティ。

翻訳者が翻訳にする必要がありますを認識できるように、ファイルを元のテキストを含むコメントを追加することをお勧めします。

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

ストーリー ボードの (機械翻訳されたもの) のスペイン語の文字列は、以下に示します。 何かを認識することは困難であるために、各行にコメントを追加することをお勧め、**ローカリゼーション ID**それ以外の場合を参照しています。

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>拡張機能をご覧ください。

これらの値は、ユーザーに表示される前に情報を変換するコードで使用されます。 *キー*が開発者によって選択されているが、コードを記述しているときに、通常、実際の文字列に変換します。

#### <a name="eslprojlocalizablestrings-file"></a>es.lproj/Localizable.strings file

(機械翻訳されたもの) Spansish 言語文字列:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>テスト

言語設定を変更するには、メソッドは、シミュレーターと物理デバイスによって異なります。

### <a name="simulator"></a>シミュレーター

シミュレーターで iOS を使用してテストする言語を選択**設定**アプリ (シミュレーターのホーム画面で灰色の歯車アイコン)。

  ![](localization-images/sim-settings-sml.png "IOS 設定アプリのローカライズ設定")

### <a name="watch-device"></a>ウォッチ デバイス

ウォッチでテストするときは、ウォッチの言語を変更、 **Apple Watch**ペアになっている iPhone 上のアプリ。

  ![](localization-images/phone-settings-sml.png "ペアになっている iphone、Apple Watch アプリで、ウォッチの言語を変更します。")



## <a name="related-links"></a>関連リンク

- [WatchLocalization (サンプル)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchLocalization/)
