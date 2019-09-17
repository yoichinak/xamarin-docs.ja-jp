---
title: 代替リソース
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 3f1e2ef06fb439f4b3ef290b1a7f80856b126a8d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755198"
---
# <a name="alternate-resources"></a>代替リソース

代替リソースとは、現在の言語、特定の画面サイズ、ピクセル密度など、特定のデバイスまたは実行時の構成を対象とするリソースのことです。 Android が、既定のリソースよりも特定のデバイスまたは構成に固有のリソースと一致する場合は、代わりにそのリソースが使用されます。 現在の構成と一致する代替リソースが見つからない場合は、既定のリソースが読み込まれます。 Android では、アプリケーションで使用されるリソースを決定する方法について、セクション「リソースの場所」で詳しく説明します。

代替リソースは、既定のリソースと同様に、リソースの種類に応じて、リソースフォルダー内のサブディレクトリとして構成されます。 代替リソースサブディレクトリの名前は次の形式になります。_ResourceType_修飾子-

*修飾子*は、特定のデバイス構成を識別する名前です。
名前には複数の修飾子があり、それぞれがダッシュで区切られている場合があります。 たとえば、次のスクリーンショットは、ロケール、画面の密度、画面のサイズ、向きなど、さまざまな構成の代替リソースを含む単純なプロジェクトを示しています。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![代替リソース](alternate-resources-images/alternate-resources-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![代替リソース](alternate-resources-images/alternate-resources-xs.png)

-----

リソースの種類に修飾子を追加するときは、次の規則が適用されます。

1. 各修飾子はダッシュで区切られた複数の修飾子が存在する場合があります。

2. 修飾子は1回だけ指定できます。

3. 修飾子は、次の表に示されている順序で指定する必要があります。

次に、使用可能な修飾子を示します。

- **MCC と MNC** [モバイル国コード](https://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) と、必要に応じて[モバイル ネットワーク コード](https://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC)。&ndash; SIM カードによって MCC が提供され、デバイスが接続されているネットワークによって MNC が提供されます。 携帯電話の国コードを使用してロケールをターゲットにすることもできますが、推奨される方法は、以下に示す言語の修飾子を使用することです。 たとえば、リソースをドイツにターゲット設定`mcc262`する場合、修飾子はになります。 米国での T-sql のターゲットリソースについては、修飾子は`mcc310-mnc026`です。
  モバイルの国コードとモバイルネットワークのコードの完全な一覧<http://mcc-mnc.com/>については、「」を参照してください。

- **言語**2文字の[iso 639-1 言語コード](https://en.wikipedia.org/wiki/ISO_639-1)。必要に応じて、2文字の[iso-3166-alpha-2 地域コード](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)を続けます。 &ndash; 
  両方の修飾子が指定されている場合は、 `-r`によって区切られます。 たとえば、フランス語圏のロケールを対象とする場合、 `fr`の修飾子が使用されます。 フランス語 (カナダ) のロケールを`fr-rCA`対象とするには、を使用します。 言語コードと地域コードの完全な一覧については、「言語の名前と国の[名前とコード要素](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm)の[表記のコード](http://www.loc.gov/standards/iso639-2/php/English_list.php)」を参照してください。

- **最小の幅**&ndash;アプリケーションが実行される最小の画面幅を指定します。 [さまざまな画面のリソースを作成する方法](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)について詳しく説明します。 
  API レベル 13 (Android 3.2) 以降で使用できます。 たとえば、修飾子`sw320dp`は、高さと幅が320dp 以上のデバイスを対象とするために使用されます。

- **使用可能な幅**画面の幅の最小値 (w*n*dp)。ここで、n は密度に依存しないピクセル単位の幅です。 &ndash;
  この値は、ユーザーがデバイスを回転させると変化することがあります。 [さまざまな画面のリソースを作成する方法](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)について詳しく説明します。 
  API レベル 13 (Android 3.2) 以降で使用できます。 例: 修飾子 w720dp は、幅が 720 dp よりも小さいデバイスを対象として使用されます。

- **使用可能な高さ**H n dp 形式の画面の最小の高さ。 *N*は dp の高さです。 &ndash; この値は、ユーザーがデバイスを回転させると変化することがあります。 [さまざまな画面のリソースを作成する方法](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)について詳しく説明します。 
  API レベル 13 (Android 3.2) 以降で使用できます。 たとえば、qualifier h720dp を使用して、少なくとも720dp の高さを持つデバイスを対象とします。

- **画面のサイズ**&ndash;この修飾子は、これらのリソースのための画面サイズを一般化したものです。 [さまざまな画面のリソースを作成する方法](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)について詳しく説明します。 
  指定できる値は、`small`、`normal`、`large`、および `xlarge` です。 API レベル9で追加 (Android 2.3/Android 2.3.1/Android 2.3.2)

- **画面の側面**&ndash;これは、画面の向きではなく、縦横比に基づいています。 長い画面は広くなっています。 API レベル 4 (Android 1.6) で追加されました。 指定できる値は long と notlong です。

- **画面の向き**&ndash;縦または横の画面の向き。 これは、アプリケーションの有効期間中に変更される可能性があります。
  有効な値は、`port` と `land` です。

- **ドッキングモード**&ndash;車ドックまたは机ドック内のデバイスの場合。 API レベル 8 (Android 2.2. x) で追加されました。 有効な値は、`car` と `desk` です。

- **夜間モード**&ndash;アプリケーションが夜間または日中に実行されているかどうか。 これは、アプリケーションの有効期間中に変更される可能性があり、開発者が、夜間により濃いバージョンのインターフェイスを使用できるようにすることを目的としています。 API レベル 8 (Android 2.2. x) で追加されました。 有効な値は、`night` と `notnight` です。

- **画面のピクセル密度 (dpi)** &ndash;物理画面上の特定の領域のピクセル数。 通常、ドット/インチ (dpi) として表現されます。 指定できる値は次のとおりです。

  - `ldpi`&ndash;低密度画面。

  - `mdpi`&ndash;中密度画面

  - `hdpi`&ndash;高密度画面

  - `xhdpi`&ndash;高密度画面

  - `nodpi`&ndash;スケールされないリソース

  - `tvdpi`&ndash; Mdpi と hdpi の間の画面用 API レベル 13 (Android 3.2) で導入されました。

- **タッチスクリーンの種類**&ndash;デバイスが持つことができるタッチスクリーンの種類を指定します。 使用できる値`notouch`は、(タッチスクリーンなし`stylus` )、(スタイラスに適した抵抗膜方式のタッチスクリーン`finger` )、および (タッチスクリーン) です。

- **キーボードの可用性**&ndash;使用できるキーボードの種類を指定します。 これは、ユーザーがハードウェアキーボードを開い&ndash;たときなどに、アプリケーションの有効期間中に変更される可能性があります。 指定できる値は次のとおりです。

  - `keysexposed`&ndash;デバイスには、使用可能なキーボードがあります。 ソフトウェアキーボードが有効になっていない場合は、ハードウェアキーボードを開いたときにのみ使用されます。

  - `keyshidden`&ndash;デバイスはハードウェアキーボードを持っていますが、非表示になっており、ソフトウェアキーボードが有効になっていません。

  - `keyssoft`&ndash;デバイスのソフトウェアキーボードが有効になっている。

- **プライマリテキスト入力方法**&ndash;入力に使用できるハードウェアキーの種類を指定するには、を使用します。 指定できる値は次のとおりです。

  - `nokeys`&ndash;入力するハードウェアキーがありません。

  - `qwerty`&ndash; Qwerty キーボードを使用できます。

  - `12key`&ndash; 12 キーのハードウェアキーボードがある

- **ナビゲーションキーの可用性**&ndash; 5 方向または d パッド (方向パッド) のナビゲーションを使用できる場合は。 これは、アプリケーションの有効期間中に変更される可能性があります。 指定できる値は次のとおりです。

  - `navexposed`&ndash;ナビゲーションキーはユーザーが使用できます

  - `navhidden`&ndash;ナビゲーションキーは使用できません。

- **主要な非タッチナビゲーション方法**&ndash;デバイスで使用できるナビゲーションの種類。 指定できる値は次のとおりです。

  - `nonav`&ndash;使用できるナビゲーション機能はタッチスクリーンのみです

  - `dpad`&ndash; d パッド (指向性パッド) は、ナビゲーションに使用できます。

  - `trackball`&ndash;デバイスは、ナビゲーションのためのトラックボールを持っています

  - `wheel`&ndash; 1 つまたは複数の方向ホイールが使用できる一般的ではないシナリオ

- **プラットフォームのバージョン (API レベル)** デバイスでサポートされている api レベルは v*N*です。ここで、N は対象となる api レベルです。 &ndash; たとえば、v11 は API レベル 11 (Android 3.0) デバイスを対象とします。

リソース修飾子の詳細については、「Android 開発者向け web サイトに[リソースを提供](https://developer.android.com/guide/topics/resources/providing-resources.html)する」を参照してください。

## <a name="how-android-determines-what-resources-to-use"></a>Android で使用するリソースを決定する方法

多くの場合、Android アプリケーションには多くのリソースが含まれている可能性があります。 Android がデバイスで実行されているときにアプリケーションのリソースをどのように選択するかを理解することが重要です。

Android では、次の規則のテストを繰り返してリソースのベースを決定します。

- **矛盾する修飾子の排除**&ndash;たとえば、デバイスの向きが縦の場合、すべての横リソースディレクトリは拒否されます。

- **無視修飾子はサポートされていません**&ndash;すべての API レベルですべての修飾子を使用できるわけではありません。 リソースディレクトリにデバイスでサポートされていない修飾子が含まれている場合、そのリソースディレクトリは無視されます。

- **次に高い優先順位の修飾子を識別する**&ndash;上の表を参照すると、次に高い優先度修飾子 (上から下) が選択されます。

- **修飾子のリソースディレクトリを保持する**&ndash;上の表に示す修飾子に一致するリソースディレクトリがある場合は、次に高い優先順位修飾子 (上から下) を選択します。

これらのルールは、次のフローチャートにも示されています。

[![リソースフローチャート](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

システムが密度固有のリソースを探して見つからない場合は、その他の密度固有のリソースを検索してスケールします。 Android は、必ずしも既定のリソースを使用するとは限りません。
たとえば、低密度のリソースを検索するときに、使用できない場合、Android では、既定または中密度のリソースに対して高密度バージョンのリソースを選択することがあります。 高密度リソースは0.5 の係数によってスケールダウンできるので、これを実行します。これにより、密度の高いリソースをスケールダウンするよりも、表示の問題が減少します。これには、0.75 の係数が必要になります。

例として、次のようなリソースディレクトリが用意されているアプリケーションについて考えてみます。

```
drawable
drawable-en
drawable-fr-rCA
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
drawable-port-ldpi
drawable-port-notouch-12key
```

これで、アプリケーションは次のように構成されたデバイスで実行されるようになりました。

- **ロケール**&ndash; en GB
- **向き**&ndash;ポート
- **画面の密度**&ndash; hdpi
- **タッチスクリーンの種類**&ndash; notouch
- **プライマリ入力方法**&ndash; 12key

まず、フランス語のリソースはの`en-GB`ロケールと競合するため、削除されます。

```
drawable
drawable-en
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
drawable-port-ldpi
drawable-port-notouch-12key
```

次に、上記の修飾子テーブルから最初の修飾子を選択します。MCC と MNC。 この修飾子を含むリソースディレクトリは存在しないため、MCC/MNC コードは無視されます。

次の修飾子が選択されています。これは言語です。 言語コードに一致するリソースがあります。 の言語コードに一致しないすべての`en`リソースディレクトリは拒否されるため、リソースの一覧は次のようになります。

```
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
```

次の修飾子は画面の向きを示すものであるため、の画面の向きに一致しないすべて`port`のリソースディレクトリは削除されます。

```
drawable-en-port
drawable-en-port-ldpi
```

次に、画面の密度`ldpi`を示す修飾子を示します。これにより、もう1つのリソースディレクトリが除外されます。

```
drawable-en-port-ldpi
```

このプロセスの結果として、Android はデバイスのリソースディレクトリ`drawable-en-port-ldpi`にあるリソースを使用します。

> [!NOTE]
> 画面サイズ修飾子は、この選択プロセスに1つの例外を提供します。 Android では、現在のデバイスよりも小さい画面用に設計されたリソースを選択できます。 たとえば、サイズの大きい画面デバイスでは、標準サイズの画面用に提供されているリソースを使用できます。 ただし、この逆は当てはまりません。同じ大きな画面デバイスは、特大画面用に提供されたリソースを使用しません。 指定された画面サイズに一致するリソースセットが Android で見つからない場合、アプリケーションはクラッシュします。
