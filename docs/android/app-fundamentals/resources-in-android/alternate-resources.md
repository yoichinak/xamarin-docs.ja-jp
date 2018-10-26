---
title: 代替のリソース
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 0384d96ddc96f8d0b16a42f691305f26ea25881d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108757"
---
# <a name="alternate-resources"></a>代替のリソース

代替のリソースは、特定のデバイスまたは現在の言語や特定の画面サイズ、ピクセル密度などの実行時構成を対象とします。 Android は、既定のリソースよりも、特定のデバイスまたは構成の固有のリソースと一致できる場合、は、そのリソースを代わりに使用されます。 現在の構成と一致する代替のリソースが見つからない場合は、既定のリソースが読み込まれます。 リソースの場所のセクションでは、以下の詳細で、アプリケーションで使用されるどのようなリソースを説明は Android の決定方法

代替のリソースは、既定のリソースと同様、リソースの種類に従ってリソース フォルダー内のサブディレクトリとして編成されます。 代替のリソースのサブディレクトリの名前は、フォーム: _ResourceType_-_修飾子_

*修飾子*特定のデバイス構成を識別する名前です。
それぞれのダッシュで区切られた、名前の 1 つ以上の修飾子が可能性があります。 たとえば、次のスクリーン ショットは、ロケール、画面密度、画面サイズ、および印刷の向きなど、さまざまな構成を代替のリソースが含まれる単純なプロジェクトを示しています。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![代替のリソース](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![代替のリソース](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

リソースの種類に修飾子を追加するときに、次の規則が適用されます。

1. ダッシュで区切られた各修飾子で、1 つ以上の修飾子である可能性があります。

2. おそらく 1 回だけ指定された修飾子。

3. 修飾子は、次の表に表示される順序である必要があります。

可能な修飾子は、参照を以下に示します。

- **MCC と mnc も** &ndash; 、[モバイルの国コード](http://en.wikipedia.org/wiki/List_of_mobile_country_codes)(MCC) と、必要に応じて、[モバイル ネットワーク コード](http://en.wikipedia.org/wiki/Mobile_Network_Code)(mnc も)。 SIM カードは、ネットワークにデバイスが接続されているが、mnc もを提供します、MCC を提供します。 ターゲット ロケールが、モバイルの国コードを使用することはできますが、推奨されるアプローチでは、以下で指定した言語修飾子を使用します。 たとえば、ドイツ、ターゲット リソースを修飾子になります`mcc262`します。 T モバイル米国内のターゲット リソースに、修飾子が`mcc310-mnc026`します。
  モバイルの国コードとモバイル ネットワーク コードの完全な一覧については、次を参照してください。<http://mcc-mnc.com/>します。

- **言語** &ndash; 2 文字[ISO 639-1 言語コード](http://en.wikipedia.org/wiki/ISO_639-1)し、必要に応じてその後に 2 文字[ISO 3166-alpha-2 地域コード](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)します。 
  両方の修飾子を指定すると場合によって区切られます、`-r`します。 ターゲットのフランス語を話すロケールの修飾子をたとえば、`fr`使用されます。 カナダのロケールを対象に、`fr-rCA`は使用されます。 言語コードと地域コードの完全な一覧を参照してください。[言語のコードの表現の名前の](http://www.loc.gov/standards/iso639-2/php/English_list.php)と[国の名前とコード要素](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm)します。

- **最小の幅**&ndash;アプリケーション上で実行するものでは最小の画面幅を指定します。 詳細については、「[画面のさまざまなリソースを作成する](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)します。 
  以上で利用できる API レベル 13 (Android 3.2)。 たとえば、修飾子`sw320dp`の高さと幅は、少なくとも、ターゲット デバイスに使用 320dp します。

- **使用可能な幅**&ndash;形式 w では、画面の最小幅*N*dp、場所*N*に依存しないピクセル密度の幅は、します。
  ユーザーがデバイスを回転すると、この値は変わる可能性があります。 詳細については、「[画面のさまざまなリソースを作成する](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)します。 
  使用できる API レベル 13 (Android 3.2) 以降。 例: 修飾子 w720dp は幅が最小 720dp のデバイスを対象に使用されます。

- **使用可能な高さ**&ndash;形式 h での画面の高さの最小値*N*dp、場所*N*は配布ポイントの高さです。 ユーザーがデバイスを回転すると、この値は変わる可能性があります。 詳細については、「[画面のさまざまなリソースを作成する](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)します。 
  使用できる API レベル 13 (Android 3.2) 以降。 720dp の最小の高さのあるデバイスを対象に修飾子 h720dp を使用する例。

- **画面サイズ**&ndash;この修飾子は、これらのリソースは画面のサイズの汎化します。 詳細については説明[画面のさまざまなリソースを作成する](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)します。 
  指定できる値は、`small`、`normal`、`large`、および `xlarge` です。 API レベル 9 (で、Android 2.3/Android 2.3.1/Android 2.3.2) の追加

- **画面の縦横**&ndash;縦横比を画面の向きに基づきます。 長い画面は。 API レベル 4 (Android 1.6) に追加されます。 使用可能な値には、時間の長いおよび notlong です。

- **画面の向き**&ndash;画面の向きを縦または横。 これは、アプリケーションの有効期間中に変更できます。
  有効な値は、`port` と `land` です。

- **ドッキング モード**&ndash;車でのデバイスがドッキングするか、デスクにドッキングします。 API レベル 8 (Android 2.2.x) に追加されます。 有効な値は、`car` と `desk` です。

- **ナイト モード**&ndash;かどうか、アプリケーションが実行されている夜間または 1 日にします。 これは、アプリケーションの有効期間中に変更され、夜に暗いインターフェイス バージョンを使用する機会を開発者に提供するものでは。 API レベル 8 (Android 2.2.x) に追加されます。 有効な値は、`night` と `notnight` です。

- **ピクセル密度 (dpi) を画面**&ndash;物理画面上の特定領域のピクセル数。 通常、ドット/インチ (dpi) で表されます。 指定できる値は次のとおりです。

    - `ldpi` &ndash; 低密度の画面。

    - `mdpi` &ndash; 中程度の密度の画面

    - `hdpi` &ndash; 高密度の画面

    - `xhdpi` &ndash; 超高密度の画面

    - `nodpi` &ndash; リソースをスケーリングするのには

    - `tvdpi` &ndash; Mdpi と hdpi 間画面の API レベル 13 (Android 3.2) で導入されました。

- **タッチ スクリーン型**&ndash;必要があります、デバイスのタッチ スクリーンの種類を指定します。 指定できる値は`notouch`(タッチ スクリーン表示のない)、 `stylus` (抵抗膜方式タッチ スクリーン スタイラスに適した)、および`finger`(タッチ スクリーン上)。

- **可用性のキーボード**&ndash;キーボードの種類を指定します。 アプリケーションの有効期間中に変更の可能性あり&ndash;など、ユーザーがハードウェア キーボードを開くとします。 指定できる値は次のとおりです。

    - `keysexposed` &ndash; デバイスでは、使用可能なキーボードがあります。 有効になっているソフトウェアのキーボードがない場合、これはのみ使用ハードウェア キーボードを開いたときにします。

    - `keyshidden` &ndash; デバイスのハードウェア キーボードが非表示になっていてソフトウェア キーボードは有効になりません。

    - `keyssoft` &ndash; デバイスでは、有効になっているソフトウェアのキーボードがあります。

- **プライマリのテキスト入力方式**&ndash;を使用してどのような種類のハードウェア キーが入力の使用可能なを指定します。 指定できる値は次のとおりです。

    - `nokeys` &ndash; 入力用のハードウェア キーがありません。

    - `qwerty` &ndash; Qwerty キーボードが使用できます。

    - `12key` &ndash; 12 キー キーボードがあります。


- **ナビゲーション キー可用性** &ndash; 5 ウェイや方向パッド (方向パッド) ナビゲーションが使用可能な場合にします。 これは、アプリケーションの有効期間中に変更できます。 指定できる値は次のとおりです。

    - `navexposed` &ndash; ナビゲーション キーは、ユーザーが利用できます。

    - `navhidden` &ndash; ナビゲーション キーは使用できません。

-  **主要な非タッチ ナビゲーション方法**&ndash;デバイスで使用可能なナビゲーションの種類。 指定できる値は次のとおりです。

    - `nonav` &ndash; 使用可能な唯一のナビゲーション機能は、タッチ画面です。

    - `dpad` &ndash; ナビゲーションの方向パッド (方向パッド) があります。

    - `trackball` &ndash; デバイスがナビゲーションのトラック ボール

    - `wheel` &ndash; 1 つまたは複数の方向車輪使用可能な位置は、一般的ではないシナリオ

-  **プラットフォームのバージョン (API レベル)** &ndash; v の形式でのデバイスでサポートされる API レベル*N*ここで、 *N*対象となっている API レベルです。 たとえば、v11 が API レベル 11 (Android 3.0) を対象デバイス。


詳細については、リソースは修飾子を参照してください[リソースを提供する](http://developer.android.com/guide/topics/resources/providing-resources.html)Android 開発者 web サイト。


## <a name="how-android-determines-what-resources-to-use"></a>Android を使用するには、どのようなリソースを決定する方法

非常に多くのリソースを Android アプリケーションが含まれる可能性があります。 Android が選択する方法、アプリケーションのリソースをデバイスで実行時に理解しておく必要があります。

Android では、ルールの次のテストを反復して基本のリソースを決定します。

- **矛盾する修飾子を排除**&ndash;など、デバイスの向きが縦向きの場合は、し、ランドス ケープのすべてのリソース ディレクトリは拒否されます。

- **サポートされていない修飾子を無視する**&ndash;すべて修飾子は、すべての API レベルを使用できます。 リソース ディレクトリにデバイスでサポートされていない修飾子が含まれている場合はそのリソースのディレクトリは無視されます。

- **[次へ] の最高の優先度の修飾子を識別する**&ndash;上の表を参照する (上から下) から [次へ] の最高優先度の修飾子を選択します。

- **修飾子の任意のリソース ディレクトリを保持**&ndash;上の表に、修飾子に一致するすべてのリソース ディレクトリがある場合は、(上から下) から [次へ] の最高優先度の修飾子を選択します。

これらの規則は、次のフローチャートにも示します。

[![リソースのフローチャート](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

システムでは、密度に固有のリソースが検索し、見つけることができません、他の密度の特定のリソースを見つけて、スケールすることを試みます。 Android では、既定のリソースは必ずしも使用可能性があります。
たとえば、低密度のリソースを探してができない場合 Android 可能性があります、リソースの高密度のバージョン既定または中密度のリソースを選択します。 これは、高密度のリソースをスケール ダウン 0.75 倍しなければ中密度リソースよりも少ないの可視性に関する問題が発生する、0.5 倍でスケール ダウンできるためです。

例として、次の描画可能なリソースのディレクトリを含むアプリケーションを検討してください。

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

次の構成を使用したデバイスでアプリケーションを実行するようになりました。

- **ロケール** &ndash; EN-GB
- **印刷の向き**&ndash;ポート
- **画面の密度** &ndash; hdpi
- **タッチ スクリーン型** &ndash; notouch
- **主要な入力方法** &ndash; 12key

フランス語のリソースの削除のロケールとの競合と最初に、 `en-GB`、残してくれました。

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

上記の修飾子のテーブルから最初の識別子を次に、選択します。 MCC と mnc もします。 MCC/mnc もコードは無視されますので、この修飾子を含むリソース ディレクトリはありません。

次の修飾子を選択した言語であります。 言語コードに一致するリソースがあります。 言語コードと一致しないすべてのリソース ディレクトリ`en`は拒否され、リソースのリストができるようにします。

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

画面の向きの画面の向きと一致しないすべてのリソース ディレクトリに存在する次の修飾子は、`port`は除外されます。

    drawable-en-port
    drawable-en-port-ldpi

次は、画面密度、修飾子`ldpi`、複数のリソース ディレクトリのいずれかになります。

    drawable-en-port-ldpi

このプロセスでは、Android は、リソース ディレクトリに描画可能なリソースを使用が`drawable-en-port-ldpi`デバイス。

> [!NOTE]
> 画面のサイズ修飾子では、この選択プロセスの 1 つの例外を提供します。 For Android は、現在のデバイスを提供するよりも小さな画面のように設計されたリソースを選択することができます。 大型画面デバイスがリソースを使用するなどの通常サイズの画面を提供します。 ただしこの逆は true はありません: 同じの大型画面デバイスは、特大の画面に指定されたリソースを使用しません。 Android では、特定の画面サイズに一致するリソース セットを見つけることができません、アプリケーションがクラッシュします。
