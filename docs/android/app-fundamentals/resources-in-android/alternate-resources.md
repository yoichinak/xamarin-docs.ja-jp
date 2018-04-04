---
title: 代替のリソース
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: f7833e238afe41d4a5ac7b8dde4c168f298752fc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="alternate-resources"></a>代替のリソース

代替のリソースは、特定のデバイスや、現在の言語、特定の画面サイズ、またはのピクセルの密度などの実行時構成をターゲットとするそれらのリソースです。 Android では、既定のリソースよりも特定のデバイスまたは構成のより具体的なリソースを一致させる場合、は、そのリソースを代わりに使用されます。 現在の構成に一致する代替のリソースが見つからない場合は既定のリソースが読み込まれます。 Android の決定方法、アプリケーションで使用されるどのようなリソースについては、説明セクションのリソースの場所で、以下の詳細

代替のリソースは、既定のリソースと同じように、リソースの種類に応じて、[リソース] フォルダー内のディレクトリをサブディレクトリとして編成されます。 フォームでは、代替のリソースのサブディレクトリの名前: _ResourceType_-_修飾子_

*修飾子*を特定のデバイス構成を識別する名前を指定します。
ダッシュで区切られたそれらの各名前に 2 つ以上の修飾子が可能性があります。 たとえば、次のスクリーン ショットは、ロケール、画面の密度、画面のサイズ、および印刷の向きなどのさまざまな構成を代替のリソースを含む単純なプロジェクトを示しています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![代替のリソース](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![代替のリソース](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

リソースの種類に修飾子を追加するときに、次の規則が適用されます。

1. ダッシュで区切られた各修飾子を持つ 2 つ以上の修飾子がある可能性があります。

2. 状況によって異なるに 1 回しか指定修飾子を指定します。

3. 修飾子は、次の表に表示される順序でなければなりません。

可能な修飾子は、リファレンスについては、次に示します。

- **MCC と mnc も** &ndash; 、[モバイル国コード](http://en.wikipedia.org/wiki/List_of_mobile_country_codes)(MCC) し、必要に応じて、[モバイル ネットワーク コード](http://en.wikipedia.org/wiki/Mobile_Network_Code)(mnc も)。 SIM カードは、デバイスが接続先ネットワークは、mnc も提供しています、MCC を提供します。 モバイル国コードを使用するターゲット ロケールをすることはできますが、推奨手法では、以下に指定された、Language 修飾子の使用です。 たとえば、ドイツのターゲット リソースへの修飾子になります`mcc262`です。 T モバイル、米国内のターゲット リソースに、修飾子が`mcc310-mnc026`です。
  モバイルの国コードとモバイル ネットワーク コードの完全な一覧については、次を参照してください。<http://mcc-mnc.com/>です。

- **言語** &ndash; 2 文字[ISO 639-1 言語コード](http://en.wikipedia.org/wiki/ISO_639-1)、必要に応じて、2 文字[ISO 3166-アルファ 2 地域コード](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)です。 
  両方の修飾子を指定しないかどうかで区切られます、`-r`です。 たとえば、しターゲット フランス語を話すロケールの修飾子`fr`を使用します。 カナダのロケールを対象とする、`fr-rCA`が使用されます。 言語コードと地域コードの一覧については、次を参照してください。 [、の形式の名前の言語コード](http://www.loc.gov/standards/iso639-2/php/English_list.php)と[国の名前とコード要素](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm)です。

- **最小の幅**&ndash;アプリケーションで実行するものでは最小の画面幅を指定します。 詳細については、「[さまざまな画面を作成するリソース](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)です。 
  以上で利用できる API レベル 13 (Android 3.2)。 修飾子など`sw320dp`の高さと幅は、少なくともターゲット デバイスに使用される 320dp です。

- **使用可能な幅**&ndash;形式 w で画面の最小幅*N*dp、場所*N*非依存のピクセル密度の幅は、します。
  ユーザーがデバイスを回転させると、この値を変更できます。 詳細については、「[さまざまな画面を作成するリソース](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)です。 
  以上で利用できる API レベル 13 (Android 3.2)。 例: 修飾子 w720dp は最低 720dp の幅があるデバイスを対象に使用されます。

- **使用可能な高さ**&ndash;形式 h での画面の高さの最小値*N*dp、場所*N*は、配布ポイントの高さ。 ユーザーがデバイスを回転させると、この値を変更できます。 詳細については、「[さまざまな画面を作成するリソース](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)です。 
  以上で利用できる API レベル 13 (Android 3.2)。 たとえば、720dp の最低の高さのあるデバイスを対象に修飾子 h720dp を使用します。

- **画面のサイズ**&ndash;この修飾子は、画面のサイズは、これらのリソースを汎化します。 詳細については説明[さまざまな画面を作成するリソース](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)です。 
  指定できる値は、`small`、`normal`、`large`、および `xlarge` です。 API レベル 9 (で、Android 2.3/Android 2.3.1/Android 2.3.2) の追加

- **画面の縦横**&ndash;これは、縦横比を画面の向きに基づきます。 長い画面より広いです。 API レベル 4 (Android 1.6) に追加されます。 使用可能な値は、時間の長い notlong です。

- **画面の向き**&ndash;画面の向きを縦または横。 これは、アプリケーションの有効期間中に変更できます。
  有効な値は、`port` と `land` です。

- **ドッキング モード**&ndash;車のデバイスがドッキングするか、デスクにドッキングします。 API レベル 8 (Android 2.2.x) に追加されます。 有効な値は、`car` と `desk` です。

- **夜間モード**&ndash;かどうか、アプリケーションが実行されている夜間や、1 日にします。 これは、アプリケーションの有効期間中に変更され、開発者は夜間にインターフェイスの濃いほうのバージョンを使用する機会を提供するものでは。 API レベル 8 (Android 2.2.x) に追加されます。 有効な値は、`night` と `notnight` です。

- **画面のピクセルの密度 (dpi)** &ndash;物理的な画面上の特定領域のピクセルの数。 一般ドット/インチ (dpi) として表されます。 指定できる値は次のとおりです。

    - `ldpi` &ndash; 低密度画面。

    - `mdpi` &ndash; 中程度の密度画面

    - `hdpi` &ndash; 高密度の画面

    - `xhdpi` &ndash; 余分な高密度の画面

    - `nodpi` &ndash; リソースをスケーリングするのには

    - `tvdpi` &ndash; Mdpi ~ hdpi 画面の API レベル 13 (Android 3.2) で導入されました。

- **タッチ スクリーン型**&ndash;デバイスが持つタッチ スクリーンの種類を指定します。 指定できる値は`notouch`(タッチ画面がない)、 `stylus` (抵抗膜方式タッチ スクリーン スタイラスに適した)、および`finger`(タッチ スクリーン)。

- **キーボードの可用性**&ndash;キーボードの種類が使用可能なを指定します。 これは、アプリケーションの有効期間中に変更&ndash;ユーザーが、ハードウェア キーボードを開いたときに例を示します。 指定できる値は次のとおりです。

    - `keysexposed` &ndash; デバイスでは、使用可能なキーボードがあります。 有効になっているソフトウェアのキーボードがない場合、これはのみ使用のハードウェア キーボードを開いたときにします。

    - `keyshidden` &ndash; デバイスのハードウェア キーボードが非表示にされてし、ソフトウェアのキーボードが有効でないです。

    - `keyssoft` &ndash; デバイスでは、有効になっているソフトウェアのキーボードがあります。

- **プライマリのテキスト入力方式**&ndash;を使用してどのような種類のハードウェア キーが入力の使用可能なを指定します。 指定できる値は次のとおりです。

    - `nokeys` &ndash; ハードウェア キーの入力はありません。

    - `qwerty` &ndash; 使用可能な qwerty キーボードがします。

    - `12key` &ndash; 12 キー ハードウェア キーボードがあります。


- **ナビゲーション キー可用性** &ndash; 5 方法またはパッド (方向パッド) ナビゲーションが使用可能な場合にします。 これは、アプリケーションの有効期間中に変更できます。 指定できる値は次のとおりです。

    - `navexposed` &ndash; ナビゲーション キーをユーザーに提供されます。

    - `navhidden` &ndash; ナビゲーション キーは使用できません。

-  **主要な非タッチ ナビゲーション方法**&ndash;デバイスで利用可能なナビゲーションの種類。 指定できる値は次のとおりです。

    - `nonav` &ndash; 使用可能な唯一のナビゲーション機能は、タッチ スクリーンをします。

    - `dpad` &ndash; ナビゲーションの方向パッド (方向パッド) があります。

    - `trackball` &ndash; ナビゲーションのボールは、デバイスが

    - `wheel` &ndash; 1 つまたは複数方向ホイール使用可能な場所は、一般的ではないシナリオ

-  **プラットフォームのバージョン (API レベル)** &ndash; v の形式で、デバイスでサポートされている API は、レベル*N*ここで、 *N*が対象 API レベルです。 たとえば、v11 が API レベル 11 (Android 3.0) を対象デバイス。


完全なリソース修飾子について[を提供するリソース](http://developer.android.com/guide/topics/resources/providing-resources.html)Android 開発者の web サイトです。


## <a name="how-android-determines-what-resources-to-use"></a>Android で使用するには、どのようなリソースを決定する方法

非常に多くのリソースが Android アプリケーションに含まれる可能性があります。 Android は選択方法、アプリケーションのリソース、デバイスでの実行時を理解しておく必要があります。

Android では、ルールの次のテストを反復処理して基本のリソースを指定できます。

- **矛盾する修飾子を排除**&ndash;など、デバイスの向きが縦の場合は、し、すべてのランドス ケープ リソース ディレクトリは拒否されます。

- **サポートされていない修飾子を無視する**&ndash;すべての修飾子は、すべての API レベルを使用できます。 リソース ディレクトリに、デバイスでサポートされていない修飾子が含まれている場合はそのリソース ディレクトリは無視されます。

- **[次へ] の最高の優先度型修飾子を識別**&ndash;上の表を参照する、[次へ] 最高の優先順位の修飾子を選択 (上から下)。

- **修飾子の任意のリソース ディレクトリを保持**&ndash;上の表に、修飾子に一致するすべてのリソース ディレクトリがある場合は、(上から下へ)、[次へ] の最高優先度型修飾子を選択します。

これらの規則にも次のフローチャートで示します。

[![リソースのフローチャート](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

システムでは、密度固有のリソースは検索と、それらを見つけることができません、他の密度の特定のリソースを見つけて、それらのスケールを試みます。 Android では、既定のリソースは必ずしも使用ことはできません。
たとえば、低密度リソースや、検索を使用できないときに Android 可能性があります高密度のバージョンのリソース既定のインスタンスまたはメディア密度のリソースを選択します。 これは、高密度のリソースを 0.75 係数を必要となります中密度リソースを縮小する場合よりも少ないの可視性問題の原因となる 0.5 の率をスケール ダウンできるためです。

たとえば、ドロウアブル リソースの次のディレクトリのあるアプリケーションを考えてみます。

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

次の構成を持つデバイスにアプリケーションを実行するようになりました。

- **ロケール** &ndash; EN-GB
- **印刷の向き**&ndash;ポート
- **画面の密度** &ndash; hdpi
- **タッチ スクリーン型** &ndash; notouch
- **プライマリの入力方式** &ndash; 12key

ロケールと競合するようにフランス語のリソースを除去する第一に、`en-GB`とお客様から。

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

上記の修飾子のテーブルから最初の識別子を次に、選択: MCC と mnc もします。 MCC/mnc もコードが無視されるため、この修飾子を含むリソース ディレクトリはありません。

言語は、次の修飾子が選択されます。 言語コードに一致するリソースがあります。 言語コードと一致しないすべてのリソース ディレクトリ`en`拒否されると、リソースのリストができるようにします。

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

存在する次の修飾子は画面の向きの画面の向きに一致しないすべてのリソース ディレクトリ`port`が除外されます。

    drawable-en-port
    drawable-en-port-ldpi

次は、画面の密度の修飾子`ldpi`、その結果は、排他領域の 1 つ以上のリソース ディレクトリ。

    drawable-en-port-ldpi

このプロセスの結果として Android リソースが使用されます、ドロウアブル リソース ディレクトリに`drawable-en-port-ldpi`デバイス用です。

> [!NOTE]
> 画面サイズ修飾子では、この選択プロセスの 1 つの例外を提供します。 For Android は、現在のデバイスより小さい画面のように設計されているリソースを選択することができます。 たとえば、大型画面デバイスは可能性があります、リソースを使用して通常のサイズが設定された画面を提供します。 ただしこの逆はできません。 同じ大きな画面デバイスは xlarge 画面に対して提供されるリソースを使用しません。 Android では、特定の画面サイズに一致するリソース セットを見つけることはできません、アプリケーションがクラッシュします。
