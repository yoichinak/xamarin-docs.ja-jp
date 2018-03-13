---
title: "ローカリゼーションの使用"
description: "複数の言語、watchOS アプリを適応させる"
ms.topic: article
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9ad3499a232e5f2b2ef362f772ed0197e71e6bee
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-localization"></a>ローカリゼーションの使用

_複数の言語、watchOS アプリを適応させる_

![](localization-images/both-languages-sml.png "Apple Watch のローカライズされたコンテンツを表示します。")

watchOS アプリをローカライズするには、標準的な iOS メソッドを使用します。

- 使用して**ローカリゼーション ID**ストーリー ボード要素
- **.strings**ストーリー ボードに関連付けられているファイルと
- **Localizable.strings**コードで使用されるテキストのファイルです。

既定のストーリー ボードとリソースにある、**ベース**ディレクトリ、および特定言語翻訳およびその他のリソースに格納されている**.lproj**ディレクトリ。
iOS および OS のウォッチが自動的に使用、ユーザーの言語の選択を正しい文字列とリソースを読み込めません。

Apple Watch アプリがあるため 2 つの部分の Watch アプリ ウォッチ拡張機能のローカライズされた文字列の使用方法に応じて、2 つの場所でリソースが必要です。

ローカライズされたテキストおよびリソースなります*異なる*watch アプリおよびウォッチ拡張機能でします。

## <a name="watch-app"></a>Watch アプリ

Watch アプリには、アプリのユーザー インターフェイスについて説明するストーリー ボードが含まれています。 すべてのコントロール (など`Label`と`Image`) ローカライズのサポートがあること、**ローカリゼーション ID**です。

各言語固有**.lproj**ディレクトリを含めることは**.strings**の各要素に対して変換を含むファイル (を使用して、**ローカリゼーション ID**)、イメージだけでなくストーリー ボードを参照しました。

## <a name="watch-extension"></a>ウォッチ拡張機能

ウォッチ拡張機能は、アプリ コードが実行されます。 コードからユーザーに表示される任意のテキストは、拡張機能では、watch アプリではなくをローカライズする必要があります。

拡張機能では、言語固有の仕様を含める必要があります**.lproj**ディレクトリ、ですが、 **.strings**ファイルでは、コードで使用されるテキストの翻訳が必要なだけです。

## <a name="globalizing-the-watch-solution"></a>ウォッチ ソリューションのグローバル化

グローバリゼーションとは、ローカライズ可能なアプリケーションを作成するプロセスです。
つまり、注意のテキストの長さが異なるとストーリー ボードのデザイン、ウォッチ アプリの各画面のレイアウトを確保するを調整します。 適切にによっては、どのようなテキストが表示されます。 使用して、ウォッチ拡張機能のコードで参照されている任意の文字列を変換できることを確認する必要があります、`LocalizedString`メソッドです。

### <a name="watch-app"></a>Watch アプリ

既定のローカライズ用 watch アプリは構成されていません。 既定のストーリー ボード ファイルを移動して、翻訳の他のいくつかのディレクトリを作成する必要があります。

1. 作成**Base.lproj**ディレクトリと移動、 **Interface.storyboard**にします。

2. 作成** <language>.lproj**サポートする各言語用のディレクトリ。

3. **.Lproj**ディレクトリを含める必要があります、 **Interface.strings**テキスト ファイル (ファイル名は storboard の名前を一致する必要があります)。 必要に応じて、これらのディレクトリでのローカライズを必要とするすべてのイメージを配置できます。

これらの変更が完了したら (英語とスペイン語言語だけファイルを追加した) 次のような watch アプリ プロジェクト。

  ![](localization-images/watchapp-solution.png "英語とスペイン語の言語ファイルをウォッチ アプリ プロジェクト")

#### <a name="storyboard-text"></a>ストーリー ボードのテキスト

ストーリー ボードを編集するときに、各要素と通知を選択、**ローカリゼーション ID**に表示される、**プロパティ**パッド。

  [![](localization-images/storyboard-sml.png "プロパティのパッドに表示されるローカリゼーション ID")](localization-images/storyboard.png#lightbox)

**Base.lproj**フォルダーによって、キーを形成する、次のように、キーと値のペアを作成、**ローカリゼーション ID**コントロールで、プロパティ名は、ドットでに参加していると (`.`)。

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

この例では通知を**ローカリゼーション ID**単純な数値文字列 (を指定できます 「0」、「1」など) または ("AgC-eL-Hgc") などのより複雑な文字列。 `Label` コントロールがある、`Text`プロパティおよび`Button`s が、`Title`プロパティで、そのローカライズされた値が設定の方法で反映する上記の例で示すように小文字のプロパティ名を使用することを確認します。

ストーリー ボードがウォッチでレンダリングされると、適切な値に自動的に抽出し、する、ユーザーが選択した言語に従って表示されます。

#### <a name="storyboard-images"></a>ストーリー ボードの画像

ソリューションの例も含まれています、 ** gradient@2x.png **各言語のフォルダー内のイメージです。 このイメージを指定できます言語ごとに異なる (たとえばです。 変換に必要なテキストが埋め込まれている可能性があります。 またはローカライズされた物の使用)。

単にセットのイメージの**イメージ**ストーリー ボードと適切なイメージのプロパティは、ユーザーが選択した言語に従って watch でレンダリングされます。

![](localization-images/storyboard-image.png "ストーリー ボードの画像イメージのプロパティを設定します。")

注: すべての Apple ウォッチがあるのみ Retina ディスプレイのため、 ** @2x **イメージのバージョンが必要です。 指定する必要はありません** @2x **ストーリー ボードにします。

### <a name="watch-extension"></a>ウォッチ拡張機能

ウォッチ拡張機能には、ローカリゼーション、ただしがないストーリー ボードをサポートするために、同様のディレクトリ構造が必要です。 拡張機能のローカライズされた文字列では、c# コードによって参照されているもののみです。

![](localization-images/watchextension-solution.png "ローカライズをサポートするには、ウォッチ拡張機能ディレクトリ構造")

#### <a name="strings-in-code"></a>コード内の文字列

**Localizable.strings**ファイルには、ストーリー ボードに関連付けられていた場合よりも若干異なる構造体。 ここでは任意の「キー」文字列を選択できます。Apple の推奨設定では、既定の言語で表示される実際のテキストを反映するキーを使用します。

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString`次のコードに示すように、翻訳済みの対応する文字列を解決するのにはメソッドを使用します。

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>コード内のイメージ

コードによって設定されるイメージは、2 つの方法で設定できます。

1. 変更することができます、 `Image` Watch アプリでその値に設定して、イメージの文字列名を既にコントロールがリフレッシュ レートが存在します。

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. 使用して、監視する、拡張機能からイメージを行うことができます`FromBundle`し、アプリは、ユーザーの言語の選択のための適切なイメージを自動的に選択します。 ソリューションの例では、イメージ** language@2x.png **各言語で、フォルダーとそのに表示される`DetailController`次のコードを使用します。

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  指定する必要はありません、 ** @2x **画像のファイル名を参照する場合。

2 番目のメソッドも watch; で表示するためにリモート サーバーからイメージをダウンロードする場合は、適用できます。ただしここで行う必要があります、イメージをダウンロードするが、ユーザーの設定に従って正しくローカライズされています。

## <a name="localization"></a>ローカリゼーション

翻訳者が処理する必要があります、ソリューションを構成した後、 **.strings**ファイルと、サポートする各言語のイメージです。

数だけ作成できます**.lproj**するとしてディレクトリには、(サポートされている言語ごとに 1 つ) が必要があります。 などの言語コードを使用してその名前が**en**、 **es**、 **de**、**日本**、 **PT-BR**、(英語のなど、スペイン語、ドイツ語、日本語、ポルトガル語 (ブラジル)、それぞれ)。

付属のサンプルでは、(コンピューターによって生成された) の翻訳を使用して、watchOS アプリをローカライズする方法を示します。

### <a name="watch-app"></a>Watch アプリ

これらの値は、watch アプリのストーリー ボードで定義されたユーザー インターフェイスを変換に使用されます。 *キー*値は、ストーリー ボード コントロールごとの組み合わせ**ローカリゼーション ID**および変換されるプロパティです。

翻訳者に通知する必要があります、翻訳ファイルを元のテキストを含むコメントを追加することをお勧めします。

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

ストーリー ボードの (機械翻訳されたもの) のスペイン語の言語識別文字列は、以下に示します。 各行にコメントを追加すると便利で何かを認識するが困難であるため、**ローカリゼーション ID**それ以外の場合を参照しています。

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>ウォッチ拡張機能

これらの値は、ユーザーに表示される前に情報を変換するコードで使用されます。 *キー*が開発者によって選択されているコードを記述するときは、通常、実際の文字列に変換します。

#### <a name="eslprojlocalizablestrings-file"></a>es.lproj/Localizable.strings file

(機械翻訳されたもの) Spansish 言語文字列:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>テスト中

言語設定を変更するメソッドは、シミュレーターと物理デバイスによって異なります。

### <a name="simulator"></a>シミュレーター

シミュレーターで、iOS を使用してテストする言語を選択**設定**アプリ (シミュレーターのホーム画面で灰色の歯車アイコン)。

  ![](localization-images/sim-settings-sml.png "IOS アプリのローカライズ設定")

### <a name="watch-device"></a>ウォッチ デバイス

ウォッチ式をテストする場合は、ウォッチの言語を変更、 **Apple Watch**ペア iPhone 上のアプリです。

  ![](localization-images/phone-settings-sml.png "ペアの iPhone、Apple Watch アプリで、ウォッチの言語を変更します。")



## <a name="related-links"></a>関連リンク

- [WatchLocalization (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
