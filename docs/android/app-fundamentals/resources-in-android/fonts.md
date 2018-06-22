---
title: フォント
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/13/2018
ms.openlocfilehash: 086576ea7d806bb0768fbe4563df7fca99244ccb
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044822"
---
# <a name="fonts"></a>フォント

## <a name="overview"></a>概要

API レベル 26 以降、Android SDK により、レイアウトと同じように、リソースとして扱われるフォントまたはドロウアブルです。 [Android のサポート ライブラリ 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1)新しいフォントの API の API レベルを 14 以上を対象とするそれらのアプリを移植をされます。

を対象とする API 26 または Android のサポート ライブラリ v26 をインストールした後は、Android アプリケーションで複数のフォントを使用する 2 つの方法があります。

1. **Android のリソースとしてフォントをパッケージ化**&ndash;こうこと、フォントは常に、アプリケーションで使用できる、APK のサイズが増えます。 
2. **フォントをダウンロード** &ndash; Android サポートからフォントをダウンロード、_フォント プロバイダー_です。 フォントのプロバイダーは、フォントが既にデバイス上のかどうかを確認します。 必要に応じて、フォントをダウンロードして、デバイス上でキャッシュします。 このフォントは、複数のアプリケーション間で共有できます。

類似のフォント (または複数の異なるスタイルを持たない可能性のあるフォント) にグループ化する_フォント ファミリ_です。 これにより開発者はフォントの太さなどの特定の属性を指定して、Android では適切なフォントをフォント ファミリから自動的に選択されます。

Android のサポート ライブラリ v26 は、API レベル 26 にフォントのサポートを移植します。 古い API レベルを対象にする場合、宣言する必要が、 `app` XML 名前空間を使用してさまざまなフォント属性の名前を付けると、`android:`名前空間および`app:`名前空間。 だけの場合、`android:`名前空間を使用し、フォントには、25 個以下に、API レベルを実行している表示されているデバイスはできません。 たとえば、この XML スニペットを新しい宣言[_フォント ファミリ_](#font_families) 14 以降の API レベルで機能するリソース。

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

フォントは、適切な方法で Android のアプリケーションに用意されて、限り、それらに適用できる UI ウィジェットを設定して、 [ `fontFamily`属性](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)です。 たとえば、次のスニペットは、TextView にフォントを表示する方法を示しています。

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

このガイドは、Android のリソースとしてフォントを使用する方法について説明し、その後、実行時にフォントをダウンロードする方法について説明します。

## <a name="fonts-as-a-resource"></a>フォント リソースとして

Android APK にフォントをパッケージ化とは、アプリケーションで使用できるが常にすることを確認します。 フォント ファイルは、(どちらか、します。TTF またはします。OTF ファイル) は、ファイルをサブディレクトリにコピーして、他のリソースと同じように Xamarin.Android アプリケーションに追加は、**リソース**Xamarin.Android プロジェクトのフォルダーです。 フォント リソースが保持されており、**フォント**のサブディレクトリ、**リソース**プロジェクトのフォルダーです。 

> [!NOTE]
> フォントが必要、**ビルド アクション**の**AndroidResource**最終的な APK にパッケージ化されませんされますか。 ビルド アクションは、IDE によって自動的に設定する必要があります。

多くの類似したフォント ファイル (たとえば、異なる重みまたはスタイルと同じフォント) がある場合は、フォント ファミリにグループ化することができます。

<a name="font_families" />

### <a name="font-families"></a>フォント ファミリ

フォント ファミリは、異なる重みとスタイルを持つフォントのセットです。 たとえば、太字や斜体のフォントの別のフォント ファイルが存在する可能性があります。 フォント ファミリがによって定義された`font`で保持されている XML ファイル内の要素、**リソース/フォント**ディレクトリ。 各フォント ファミリには、独自の XML ファイルです。

作成する、フォント ファミリでは、最初にすべてのフォントを追加、**リソース/フォント**フォルダーです。 [フォント ファミリのフォント] フォルダーで、新しい XML ファイルを作成します。 XML ファイルの名前がありませんアフィニティまたはリレーションシップです。 参照されているフォントにリソース ファイルには、任意の有効な Android リソース ファイル名を指定できます。 この XML ファイルは、ルートが`font-family`を 1 つまたは複数を含む要素`font`要素。 各`font`要素は、フォントの属性を宣言します。

次の XML のフォント ファミリの例では、_ソース San Pro_多数の異なるフォント重みを定義するフォントです。 これは内のファイルとして保存、**リソース/フォント**という名前のフォルダー **sourcesanspro.xml**:

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

`fontWeight`属性は CSS に対応`font-weight`属性し、フォントの太さを指します。 これは、100 ~ 900 の範囲内の値です。 次の一覧には、共通のフォントの太さ値とその名前がについて説明します。

* **シン** &ndash; 100
* **余分なライト** &ndash; 200
* **ライト** &ndash; 300
* **標準** &ndash; 400
* **Medium** &ndash; 500
* **太字 semi** &ndash; 600
* **太字** &ndash; 700
* **極太字** &ndash; 800
* **黒** &ndash; 900

フォント ファミリを定義すると、使用できます宣言を設定して、 `fontFamily`、 `textStyle`、および`fontWeight`レイアウト ファイル内の属性です。  たとえば次の XML スニペットには、400 の太さのフォント (標準) と斜体スタイルが設定します。

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

使用して、フォントをプログラムで設定することができます、 [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))を取得する方法、 [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html)オブジェクト。 多くのビューが、`TypeFace`ウィジェットに割り当て、フォントに使用できるプロパティです。 このコード スニペットは、プログラムによって、TextView 上のフォントを設定する方法を示しています。

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont`メソッドは自動的に読み込ま、フォント ファミリ内の最初のフォント。  特定のスタイルに一致するフォントを読み込むを使用して、`Typeface.Create`メソッドです。 このメソッドは、指定したスタイルと一致するフォントをロードしようとします。 例として、このスニペットを太字読み込んでみます`Typeface`オブジェクトで定義されているフォント ファミリから**リソース]、[フォント**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>フォントをダウンロードします。

アプリケーション リソースとしてのフォントをパッケージ化、代わりに Android は、リモート ソースからフォントをダウンロードすることができます。 APK のサイズを小さくすることをお勧めの効果があります。 

フォントをダウンロードするの支援を受けて、_フォント プロバイダー_です。 これは、ダウンロードと、デバイス上のすべてのアプリケーションにフォントのキャッシュを管理する特殊なコンテンツ プロバイダーです。 Android 8.0 にからフォントをダウンロードするフォントのプロバイダーが含まれています、 [Google フォント リポジトリ](http://fonts.google.com)です。 この既定のフォントのプロバイダーは、Android のサポート ライブラリ v26 と API レベル 14 には移植です。

要求すると、アプリのフォント、フォント プロバイダーが最初に確認する、フォントが既にデバイス上のかどうか。 それ以外の場合は、フォントのダウンロードを試みます。 ダウンロードした、し、Android、フォントができない場合は、既定のシステム フォントに使用されます。 フォントをダウンロードすると、使用可能になる最初の要求を行ったアプリだけでなく、デバイス上のすべてのアプリケーションにします。

フォントをダウンロードする要求が行われると、アプリは、フォントのプロバイダーを直接照会できません。 アプリがのインスタンスを使用する代わりに、 [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (または[ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)サポート ライブラリ 26 が使用されている場合)。  

Android 8.0 では、2 つの方法でダウンロードのフォントをサポートします。

1. **ダウンロード可能なフォント リソースとしてを宣言**&ndash;アプリが Android XML リソース ファイル形式でダウンロード可能なフォントを宣言できます。 これらのファイルは、すべての Android が非同期的に、アプリを起動し、デバイスにキャッシュし、フォントをダウンロードする必要があるメタ データが格納されます。
2. **プログラムによって** &ndash; Android API レベル 26 の Api は、アプリケーションの実行中にプログラムでは、フォントをダウンロードするアプリケーションを使用します。 アプリの作成は、`FontRequest`特定のフォントのオブジェクトをこのオブジェクトに渡す、`FontsContract`クラスです。 `FontsContract`受け取り、`FontRequest`からフォントを取得し、_フォント プロバイダー_です。 Android には、同期的にフォントがダウンロードされます。 作成する例を`FontRequest`このガイドの後半で表示されます。

どの方法を使用するに関係なくフォントの前に、Xamarin.Android アプリケーションに追加する必要がありますのリソース ファイルをダウンロードできます。 XML ファイルで最初に、フォントを宣言する必要があります、**リソース/フォント**フォント ファミリの一部としてのディレクトリ。 このスニペットはからフォントをダウンロードする方法の例、 [Google フォントのオープン ソース コレクション](https://fonts.google.com)Android 8.0 (またはサポート ライブラリ v26) に付属している既定のフォントのプロバイダーを使用します。

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

`font-family`要素には、Android がフォントをダウンロードする必要がある情報を宣言する次の属性が含まれています。

1. **fontProviderAuthority** &ndash;要求に使用するフォントのプロバイダーの機関です。
2. **fontPackage** &ndash;要求に使用するフォントのプロバイダーのパッケージです。 プロバイダーの id の検証に使用されます。
3. **fontQuery** &ndash;フォント プロバイダーが要求されたフォントを検索できる文字列です。 フォントのクエリの詳細については、フォントのプロバイダーに固有です。 [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)クラス内で、[ダウンロード可能なフォント](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/)サンプル アプリいくつかの情報を提供クエリ形式でフォントから Google フォントのオープン ソース コレクション。
4. **fontProviderCerts** &ndash;プロバイダーを使用して署名する必要があります、証明書のハッシュのセットの一覧を持つリソース配列。

フォントを定義すると、に関する情報を提供する必要あります、_フォント証明書_ダウンロードに含まれています。

### <a name="font-certificates"></a>証明書のフォント

フォントのプロバイダーが、デバイスにプリインストールされていませんか、アプリが使用されている場合、`Xamarin.Android.Support.Compat`ライブラリ、Android には、フォントのプロバイダーのセキュリティ証明書が必要です。 これらの証明書を保持する配列のリソース ファイルに一覧表示されます**リソース/値**ディレクトリ。 

たとえば、次の XML が名前付き**Resources/values/fonts_cert.xml**し、Google フォント プロバイダーの証明書を格納します。 

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

これらのリソース ファイルの場所で、アプリは、フォントをダウンロードできます。

### <a name="declaring-downloadable-fonts-as-resources"></a>ダウンロード可能なフォントをリソースとして宣言します。

ダウンロード可能なフォントをリストすることによって、 **AndroidManifest.XML**Android に非同期的にフォントをダウンロード、アプリが初めて起動したとき。 自体のフォントの配列のリソース ファイルに、このような一覧表示されます。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

宣言するが、これらのフォントをダウンロードする**AndroidManifest.XML**を追加して`meta-data`の子として、`application`要素。 たとえば、ダウンロード可能なフォントがリソース ファイルで宣言されている場合**Resources/values/downloadable_fonts.xml**、このスニペットは、マニフェストに追加する必要があります。 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>フォントの Api を使用したフォントをダウンロードします。

プログラムによってインスタンス化して、フォントをダウンロードすることは、 [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)オブジェクトを渡すことを`FontContractCompat.RequestFont`メソッドです。 `FontContractCompat.RequestFont`メソッドは、フォントが、デバイス上に存在するかを確認してし、し、必要な場合は非同期的にクエリ、フォント プロバイダーしようとして、アプリのフォントをダウンロードします。 場合`FontRequest`Android が既定のシステム フォントを使用して、フォントをダウンロードすることができます。 

A`FontRequest`オブジェクトを探して、フォントをダウンロード フォント プロバイダーによって使用される情報を格納します。 A `FontRequest` 4 つの情報が必要です。

1. **フォント プロバイダー機関**&ndash;要求に使用するフォントのプロバイダーの機関です。
2. **フォント パッケージ**&ndash;要求に使用するフォントのプロバイダーのパッケージです。 プロバイダーの id の検証に使用されます。
3. **フォント クエリ**&ndash;フォント プロバイダーが要求されたフォントを検索できる文字列です。 フォントのクエリの詳細については、フォントのプロバイダーに固有です。 文字列の詳細については、フォントのプロバイダーに固有です。 [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)クラス内で、[ダウンロード可能なフォント](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/)サンプル アプリいくつかの情報を提供クエリ形式でフォントから Google フォントのオープン ソース コレクション。
4. **フォントのプロバイダーの証明書**&ndash;でプロバイダーを署名する必要があります、証明書のハッシュのセットの一覧を持つリソース配列。 

このスニペットは、新しいをインスタンス化した例`FontRequest`オブジェクト。

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

前のスニペットに`FontToDownload`Google フォントのオープン ソースのコレクションからフォントを支援するクエリです。 

渡される前に、`FontRequest`を`FontContractCompat.RequestFont`メソッドを作成する必要がある 2 つのオブジェクトは。

* **`FontsContractCompat.FontRequestCallback`** &ndash; これは、抽象クラスを拡張する必要があります。 なるコールバックであるときに呼び出されました`RequestFont`が終了します。 Xamarin.Android アプリにサブクラス化する必要があります`FontsContractCompat.FontRequestCallback`をオーバーライドし、`OnTypefaceRequestFailed`と`OnTypefaceRetrieved`ダウンロードが失敗したか、それぞれが成功したときに実行されるアクションを提供します。
* **`Handler`** &ndash; これは、`Handler`によって使用される`RequestFont`を必要な場合、スレッド上のフォントをダウンロードします。 フォントにする必要があります**いない**UI スレッドにダウンロードします。

このスニペットは、Google のフォントのオープン ソースのコレクションからフォントをダウンロードして非同期的に c# クラスの例を示します。 実装する、`FontRequestCallback`インターフェイス、および c# イベントを発生させるときに`FontRequest`が完了します。 

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

このガイドでは、ダウンロード可能なフォントとフォントをリソースとしてをサポートするために Android 8.0 での新しい Api について説明します。 これは、方法、APK で既存のフォントを埋め込むと、レイアウトで使用する方法について説明します。 Android 8.0 の仕組みダウンロードのフォント、フォント プロバイダーから、プログラム、またはリソース ファイル内のフォントのメタ データを宣言することによっても説明します。

## <a name="related-links"></a>関連リンク

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [タイプフェイス](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android のサポート ライブラリ 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Android でのフォントを使用します。](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS のフォントの太さの仕様](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google のフォントのオープン ソースのコレクション](https://fonts.google.com/)
- [Pro San ソース](https://fonts.google.com/specimen/Source+Sans+Pro)
