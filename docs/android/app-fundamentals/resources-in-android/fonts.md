---
title: フォント
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/09/2018
ms.openlocfilehash: c7953748e79bd43bc14601c1f0ea05d1a36adf08
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668129"
---
# <a name="fonts"></a>フォント

## <a name="overview"></a>概要

API レベル 26 以降、Android SDK により、レイアウトと同じように、リソースとして扱う場合にフォントまたはドローアブルします。 [Android サポート ライブラリ 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) API レベル 14 以上を対象とするこれらのアプリに新しいフォントの API のバックポートをされます。

ターゲット API 26 または Android サポート ライブラリ v26 をインストールした後は、Android アプリケーションでフォントを使用する 2 つの方法があります。

1. **Android のリソースとしてフォントをパッケージ化**&ndash;これにより、フォントは、アプリケーションが常には、APK のサイズが増えます。
2. **フォントをダウンロード**&ndash;も Android ではダウンロードからフォントをサポートしています、_フォント プロバイダー_します。 フォントが既にデバイスのかどうか、フォントのプロバイダーを確認します。 必要に応じて、フォントがダウンロードされ、デバイス上でキャッシュします。 このフォントは、複数のアプリケーション間で共有できます。

類似のフォント (または複数の異なるスタイルを持たない可能性のあるフォント) がグループ化することがあります_フォント ファミリ_します。 これにより開発者は、フォントの太さなどの特定の属性を指定して、Android がフォント ファミリから適切なフォントを自動的に選択されます。

Android サポート ライブラリ v26 は、API レベル 26 にフォントのバックポート サポートされます。 以前の API レベルを対象とする場合、宣言する必要が、 `app` XML 名前空間と名前を使用して、さまざまなフォント属性を`android:`名前空間と`app:`名前空間。 だけの場合、`android:`名前空間を使用すると、その後、フォントには、API レベル 25 以下を実行している表示されているデバイスはできません。 たとえば、この XML スニペットを新しい宣言[_フォント ファミリ_](#font_families) API レベル 14 以上で作業するリソース。

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular"
            android:fontStyle="normal"
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular"
            app:fontStyle="normal"
            app:fontWeight="400" />

</font-family>
```

フォントは、適切な方法で Android アプリケーションに用意されて、限りそれらに適用できる UI ウィジェットを設定して、 [ `fontFamily`属性](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)します。 たとえば、次のスニペットでは、TextView にフォントを表示する方法を示しています。

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

このガイドは、Android のリソースとしてフォントを使用する方法について説明し、実行時にフォントをダウンロードする方法について説明に移ります。

## <a name="fonts-as-a-resource"></a>リソースとしてフォント

Android APK にフォントをパッケージ化とは、アプリケーションで使用できるが常にすることを確認します。 フォント ファイル (いずれかをします。TTF またはします。OTF ファイル) がファイルをサブディレクトリにコピーして、他のリソースと同様、Xamarin.Android アプリケーションに追加、**リソース**Xamarin.Android プロジェクトのフォルダー。 フォント リソースを**フォント**のサブディレクトリ、**リソース**プロジェクトのフォルダー。

> [!NOTE]
> フォントがあります、**ビルド アクション**の**AndroidResource**または、これらは、最終的な APK をパッケージ化されません。 ビルド アクションは、IDE によって自動的に設定する必要があります。

多くの類似したフォント ファイル (たとえば、さまざまな重みやスタイルで同じフォント) がある場合に、フォント ファミリにグループ化することができます。

<a name="font_families" />

### <a name="font-families"></a>フォント ファミリ

フォント ファミリは、さまざまな重みとスタイルを持つフォントのセットです。 たとえば、太字や斜体のフォントの個別のフォント ファイルがあります。 フォント ファミリがによって定義されている`font`を保持する XML ファイル内の要素、**リソース/フォント**ディレクトリ。 各フォント ファミリ、独自の XML ファイルが必要です。

フォント ファミリを作成するには、最初にすべてのフォントを追加、**リソース/フォント**フォルダー。 [フォント ファミリのフォント] フォルダーで新しい XML ファイルを作成します。 XML ファイルの名前がありませんアフィニティまたはリレーションシップです。 参照されているフォントにリソース ファイルには、任意の有効な Android のリソース ファイル名を指定できます。 この XML ファイルは、ルートが`font-family`要素を 1 つまたは複数を含む`font`要素。 各`font`要素のフォント属性を宣言します。

次の XML のフォント ファミリの例に示します、_ソース San Pro_多くの異なるフォントの太さを定義するフォント。 これでファイルとして保存されますが、**リソース/フォント**という名前のフォルダー **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular"
          android:fontStyle="normal"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_regular"
          app:fontStyle="normal"
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold"
          android:fontStyle="normal"
          android:fontWeight="800"
          app:font="@font/sourcesanspro_bold"
          app:fontStyle="normal"
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic"
          android:fontStyle="italic"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic"
          app:fontStyle="italic"
          app:fontWeight="400" />
</font-family>
```

`fontStyle`属性が 2 つの値。

* **通常**&ndash;通常フォント
* **斜体**&ndash;斜体のフォント

`fontWeight`属性は、CSS に対応`font-weight`属性、フォントの太さを指しています。 これは、100 ~ 900 の範囲内の値です。 次の一覧には、共通のフォントの太さ値とその名前がについて説明します。

* **シン** &ndash; 100
* **余分な明るい** &ndash; 200
* **Light** &ndash; 300
* **標準** &ndash; 400
* **Medium** &ndash; 500
* **太字の半** &ndash; 600
* **太字** &ndash; 700
* **極太字** &ndash; 800
* **黒** &ndash; 900

フォント ファミリを定義した後、これで使用できる宣言によって設定、 `fontFamily`、 `textStyle`、および`fontWeight`レイアウト ファイル内の属性。  たとえば、次の XML スニペットは、600 の太さのフォント (通常) と斜体のテキスト スタイルを設定します。

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

### <a name="programmatically-assigning-fonts"></a>プログラムでフォントを割り当てる

使用して、フォントをプログラムで設定できる、 [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))を取得するメソッド、 [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html)オブジェクト。 多くのビューが、`TypeFace`プロパティが、ウィジェットを割り当て、フォントを使用できます。 このコード スニペットでは、プログラムで、TextView のフォントを設定する方法を示します。

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont`メソッドは、フォント ファミリ内の最初のフォントを自動的にロードします。  特定のスタイルに一致するフォントを読み込むには、使用、`Typeface.Create`メソッド。 このメソッドは、指定したスタイルに一致するフォントをロードしようとします。 たとえば、このスニペットを太字読み込んでみます`Typeface`オブジェクトで定義されているフォント ファミリから**リソース]、[フォント**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>フォントをダウンロードします。

アプリケーション リソースとしてのフォントのパッケージングではなく Android は、リモート ソースからフォントをダウンロードすることができます。 APK のサイズを縮小するが望ましい効果があります。

フォントのダウンロードの支援を得て、_フォント プロバイダー_します。 これは、デバイス上のすべてのアプリケーションにフォントのダウンロード、およびキャッシュを管理する特殊なコンテンツ プロバイダーです。 Android 8.0 にはからフォントをダウンロードするフォントのプロバイダーが含まれています、 [Google フォント リポジトリ](http://fonts.google.com)します。 この既定のフォントのプロバイダーは、Android サポート ライブラリ v26 と API レベル 14 に移植します。

アプリが要求を行うと、フォントのフォントのプロバイダーはまず フォントは、デバイスで既にかどうかを確認します。 それ以外の場合は、フォントをダウンロードするのには試みます。 ダウンロードした、Android、フォントができない場合は、既定のシステム フォントを使用します。 フォントがダウンロードされるは、最初の要求を作成したアプリだけでなく、デバイス上のすべてのアプリケーションで使用できます。

フォントをダウンロードする要求が行われると、アプリは、フォントのプロバイダーを直接照会できません。 アプリがのインスタンスを使用する代わりに、 [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (または[ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)サポート ライブラリの 26 が使用されている場合)。  

Android 8.0 では、2 つの方法でフォントのダウンロードをサポートします。

1. **ダウンロード可能なフォント リソースとして宣言**&ndash;アプリは、XML リソース ファイルを使用して Android へのダウンロード可能なフォントを宣言できます。 これらのファイルは、すべての Android が非同期的にフォントをアプリが開始され、デバイス上のキャッシュにダウンロードする必要があるメタ データが格納されます。
2. **プログラムで** &ndash; 26 の Android API レベルでの Api を使用すると、アプリケーションの実行中にプログラムでは、フォントをダウンロードします。 アプリを作成、`FontRequest`特定のフォントのオブジェクトし、するには、このオブジェクトを渡す、`FontsContract`クラス。 `FontsContract`は、`FontRequest`からフォントを取得し、_フォント プロバイダー_します。 Android は、フォントを同期的にダウンロードされます。 作成する例を`FontRequest`このガイドの後半で表示されます。

どちらのアプローチを使用するに関係なく、フォントの前に、Xamarin.Android アプリケーションに追加する必要がありますリソース ファイルをダウンロードできます。 最初に、フォントで XML ファイルで宣言する必要があります、**リソース/フォント**フォント ファミリの一部としてのディレクトリ。 このスニペットはからフォントをダウンロードする方法の例、 [Google フォントのオープン ソースのコレクション](https://fonts.google.com)Android 8.0 (またはサポート ライブラリ v26) に付属する既定のフォントのプロバイダーを使用します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts"
             android:fontProviderPackage="com.google.android.gms"
             android:fontProviderQuery="VT323"
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts"
             app:fontProviderPackage="com.google.android.gms"
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
>
</font-family>
```

`font-family`要素には、Android がフォントのダウンロードが必要な情報を宣言する次の属性が含まれています。

1. **fontProviderAuthority** &ndash;要求に使用するフォントのプロバイダーの機関。
2. **fontPackage** &ndash;要求に使用するフォントのプロバイダーのパッケージ。 これは、プロバイダーの id の検証に使用されます。
3. **fontQuery** &ndash;フォント プロバイダーが要求されたフォントの検索に役立つ文字列です。 フォントのクエリの詳細については、フォントのプロバイダーに固有です。 [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)クラス、[ダウンロード可能なフォント](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/)サンプル アプリは、いくつかの情報、クエリの形式でフォント Google フォントのオープン ソースのコレクションから。
4. **fontProviderCerts** &ndash;のプロバイダーを使用して署名する必要があります証明書のハッシュ セットの一覧を持つリソース配列。

フォントが定義した後の情報を提供する必要あります、_フォントの証明書_ダウンロードに含まれています。

### <a name="font-certificates"></a>証明書のフォント

フォントのプロバイダーが、デバイスにプレインストールされていない場合、またはアプリが使用されている場合、`Xamarin.Android.Support.Compat`ライブラリを Android には、フォントのプロバイダーのセキュリティ証明書が必要です。 これらの証明書を保持する配列リソース ファイル内に表示されます**リソース/値**ディレクトリ。

たとえば、次の XML の名前は**Resources/values/fonts_cert.xml**し、Google のフォントのプロバイダーの証明書を格納します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

これらの場所にリソース ファイル、アプリは、フォントをダウンロードするの。

### <a name="declaring-downloadable-fonts-as-resources"></a>ダウンロード可能なフォントをリソースとして宣言します。

ダウンロード可能なフォントの一覧を表示して、 **AndroidManifest.XML**、初回起動時に、アプリは、Android にフォントはダウンロード非同期的にします。 自体のフォントの配列のリソース ファイルに、このような一覧表示されます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

宣言するが、これらのフォントをダウンロードするには、 **AndroidManifest.XML**を追加して`meta-data`の子として、`application`要素。 リソース ファイルにダウンロード可能なフォントが宣言されている場合など、 **Resources/values/downloadable_fonts.xml**、このスニペットは、マニフェストに追加する必要があります。

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>フォントの Api を使用したフォントをダウンロード

プログラムでインスタンス化して、フォントをダウンロードすることができます、 [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)オブジェクトとを渡して、`FontContractCompat.RequestFont`メソッド。 `FontContractCompat.RequestFont`メソッドは、フォントが、デバイス上に存在するかを確認して、し、必要な場合は非同期的にクエリ、フォントのプロバイダーとアプリのフォントをダウンロードしてください。 場合`FontRequest`は Android では、既定のシステム フォントを使用して、フォントをダウンロードできません。

A`FontRequest`オブジェクトに特定して、フォントをダウンロードするフォントのプロバイダーによって使用される情報が含まれています。 A `FontRequest` 4 つの情報が必要です。

1. **フォント プロバイダー機関**&ndash;要求に使用するフォントのプロバイダーの機関。
2. **フォント パッケージ**&ndash;要求に使用するフォントのプロバイダーのパッケージ。 これは、プロバイダーの id の検証に使用されます。
3. **フォント クエリ**&ndash;フォント プロバイダーが要求されたフォントの検索に役立つ文字列です。 フォントのクエリの詳細については、フォントのプロバイダーに固有です。 文字列の詳細については、フォントのプロバイダーに固有です。 [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)クラス、[ダウンロード可能なフォント](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/)サンプル アプリは、いくつかの情報、クエリの形式でフォント Google フォントのオープン ソースのコレクションから。
4. **フォントのプロバイダーの証明書**&ndash;でプロバイダーを署名する必要があります証明書のハッシュのセットの一覧を持つリソース配列。

このスニペットのような新しいをインスタンス化した`FontRequest`オブジェクト。

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

前のスニペットで`FontToDownload`は Google のフォントのオープン ソースのコレクションから、フォントに役立つクエリです。

渡される前に、`FontRequest`を`FontContractCompat.RequestFont`メソッド、2 つのオブジェクトが作成する必要があります。

* **`FontsContractCompat.FontRequestCallback`** &ndash; これは、クラスは抽象クラスを拡張する必要があります。 できるコールバックされると呼び出されます`RequestFont`が終了します。 Xamarin.Android アプリにサブクラス化する必要があります`FontsContractCompat.FontRequestCallback`をオーバーライドし、`OnTypefaceRequestFailed`と`OnTypefaceRetrieved`ダウンロードが失敗したか、それぞれが成功したときに実行されるアクションを提供します。
* **`Handler`** &ndash; これは、`Handler`によって使用される`RequestFont`必要な場合に、スレッド上のフォントをダウンロードします。 フォントにする必要があります**いない**UI スレッドにダウンロードされます。

このスニペットの例に示します、C#クラスは、Google のフォントのオープン ソースのコレクションからフォントをダウンロードして非同期的にです。 実装、`FontRequestCallback`インターフェイス、および発生させる、C#イベントと`FontRequest`が完了します。

```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";

    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };


    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }

    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```

このヘルパーを使用する新しい`FontDownloadHelper`が作成され、イベント ハンドラーが割り当てられます。  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) =>
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>まとめ

このガイドでは、ダウンロード可能なフォントおよびリソースとしてフォントをサポートするために Android 8.0 を使用して、新しい Api について説明します。 これは、APK で既存のフォントを埋め込むと、レイアウトで使用する方法を説明します。 また、プログラム、またはリソース ファイルでフォントのメタ データを宣言することでの Android 8.0 におけるフォント プロバイダーからのダウンロードのフォントのサポートについても説明します。

## <a name="related-links"></a>関連リンク

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [タイプフェイス](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android サポート ライブラリ 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Android でのフォントを使用します。](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS のフォントの太さの仕様](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google のフォントのオープン ソースのコレクション](https://fonts.google.com/)
- [Pro San ソース](https://fonts.google.com/specimen/Source+Sans+Pro)
