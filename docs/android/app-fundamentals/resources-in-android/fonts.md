---
title: フォント
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/09/2018
ms.openlocfilehash: 8f732e05565c420ef28da38c0da0e61ecd595313
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025019"
---
# <a name="fonts"></a>フォント

## <a name="overview"></a>概要

API レベル26以降、Android SDK では、レイアウトや drawables 可能な場合と同様に、フォントをリソースとして扱うことができます。 [Android サポートライブラリ 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1)は、api レベル14以上を対象とするアプリに新しいフォント api をバックポートします。

API 26 をターゲットにした後、または Android サポートライブラリ v26 をインストールした後、Android アプリケーションでフォントを使用するには次の2つの方法があります。

1. この**フォントを Android &ndash; リソースとしてパッケージ化すると**、そのフォントは常にアプリケーションで使用できるようになりますが、apk のサイズは大きくなります。
2. フォント**のダウンロード**&ndash; Android では、フォント_プロバイダー_からのフォントのダウンロードもサポートされています。 フォントプロバイダーは、フォントが既にデバイス上にあるかどうかを確認します。 必要に応じて、フォントがダウンロードされ、デバイスにキャッシュされます。 このフォントは、複数のアプリケーション間で共有できます。

類似したフォント (または複数の異なるスタイルを持つフォント) は、_フォントファミリ_にグループ化されることがあります。 これにより、開発者は、太さなどのフォントの特定の属性を指定できます。 Android では、フォントファミリから適切なフォントが自動的に選択されます。

Android サポートライブラリ v26 は、フォントのサポートを API レベル26にバックポートします。 以前の API レベルを対象とする場合は、`app` XML 名前空間を宣言し、`android:` 名前空間と `app:` 名前空間を使用してさまざまなフォント属性に名前を指定する必要があります。 `android:` 名前空間のみが使用されている場合、API レベル25以下を実行しているデバイスは表示されません。 たとえば、次の XML スニペットでは、API レベル14以上で動作する新しい[_フォントファミリ_](#font_families)リソースが宣言されています。

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

適切な方法で Android アプリケーションにフォントが提供されている限り、 [`fontFamily` 属性](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)を設定することによって UI ウィジェットに適用できます。 たとえば、次のスニペットは、TextView にフォントを表示する方法を示しています。

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

このガイドでは、まず、フォントを Android リソースとして使用する方法について説明した後、実行時にフォントをダウンロードする方法について説明します。

## <a name="fonts-as-a-resource"></a>リソースとしてのフォント

Android APK にフォントをパッケージ化すると、アプリケーションで常に使用できるようになります。 フォントファイル (。または.OTF ファイル) は、他のリソースと同様に、xamarin Android プロジェクトの**Resources**フォルダー内のサブディレクトリにファイルをコピーすることによって、xamarin android アプリケーションに追加されます。 フォントリソースは、プロジェクトの**resources**フォルダーの**フォント**サブディレクトリに保持されます。

> [!NOTE]
> フォントは、 **Androidresource**の**ビルドアクション**を持つ必要があります。それ以外の場合は、最終的な apk にパッケージされません。 ビルドアクションは、IDE によって自動的に設定されます。

類似したフォントファイルが多数ある場合 (たとえば、異なる太さやスタイルを持つ同じフォント)、フォントファミリにグループ化することができます。

<a name="font_families" />

### <a name="font-families"></a>フォントファミリ

フォントファミリは、さまざまな太さとスタイルを持つ一連のフォントです。 たとえば、太字または斜体のフォントに個別のフォントファイルがある場合があります。 フォントファミリは、**リソース/フォント**ディレクトリに保持されている XML ファイル内の `font` 要素によって定義されます。 各フォントファミリには、独自の XML ファイルが必要です。

フォントファミリを作成するには、まず、すべてのフォントを**Resources/font**フォルダーに追加します。 次に、フォントファミリのフォントフォルダーに新しい XML ファイルを作成します。 XML ファイルの名前には、参照されているフォントとのアフィニティまたは関係がありません。リソースファイルには、任意の有効な Android リソースファイル名を指定できます。 この XML ファイルには、1つ以上の `font` 要素を含むルート `font-family` 要素が含まれます。 各 `font` 要素は、フォントの属性を宣言します。

次の XML は、さまざまなフォントの太さを定義する_Sources Sans Pro_フォントのフォントファミリの例です。 これは、 **sourcesanspro .xml**という名前の**Resources/font**フォルダーにファイルとして保存されます。

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

`fontStyle` 属性には、次の2つの値があります。

- 標準フォント &ndash;**通常**のフォント
- **斜体 &ndash; 斜体**のフォント

`fontWeight` 属性は、CSS `font-weight` 属性に対応し、フォントの太さを示します。 100 ~ 900 の範囲の値です。 次の一覧では、一般的なフォントの太さの値とその名前について説明します。

- **シン**&ndash; 100
- **エクストラライト**&ndash; 200
- **ライト**&ndash; 300
- **標準**&ndash; 400
- **中**&ndash; 500
- **半太字**&ndash; 600
- **太字**&ndash; 700
- **Xl 太字**&ndash; 800
- **黒**&ndash; 900

フォントファミリを定義したら、レイアウトファイルの `fontFamily`、`textStyle`、および `fontWeight` 属性を設定することによって、そのファミリを宣言によって使用できます。  たとえば、次の XML スニペットは、600 weight フォント (normal) と斜体のテキストスタイルを設定します。

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

### <a name="programmatically-assigning-fonts"></a>プログラムによるフォントの割り当て

フォントは、 [`Resources.GetFont`](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))メソッドを使用して、 [`Typeface`](https://developer.android.com/reference/android/graphics/Typeface.html)オブジェクトを取得することによって、プログラムで設定できます。 多くのビューには、ウィジェットにフォントを割り当てるために使用できる `TypeFace` のプロパティがあります。 次のコードスニペットは、TextView でプログラムによってフォントを設定する方法を示しています。

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont` メソッドは、フォントファミリ内の最初のフォントを自動的に読み込みます。  特定のスタイルに一致するフォントを読み込むには、`Typeface.Create` メソッドを使用します。 このメソッドは、指定されたスタイルに一致するフォントの読み込みを試みます。 例として、このスニペットは、 **Resources/fonts**で定義されているフォントファミリから太字の `Typeface` オブジェクトを読み込もうとします。

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>ダウンロード (フォントを)

Android では、フォントをアプリケーションリソースとしてパッケージ化する代わりに、リモートソースからフォントをダウンロードできます。 これにより、APK のサイズを小さくするという望ましい効果が得られます。

フォント_プロバイダー_をサポートするフォントがダウンロードされます。 これは、デバイス上のすべてのアプリケーションに対するフォントのダウンロードとキャッシュを管理する特殊なコンテンツプロバイダーです。 Android 8.0 には、 [Google フォントリポジトリ](https://fonts.google.com)からフォントをダウンロードするためのフォントプロバイダーが含まれています。 この既定のフォントプロバイダーは、Android サポートライブラリ v26 を使用して API レベル14に移植ます。

アプリがフォントの要求を行うと、フォントプロバイダーはまず、フォントがデバイス上に既に存在するかどうかを確認します。 そうでない場合は、フォントのダウンロードが試行されます。 フォントをダウンロードできない場合、Android では既定のシステムフォントが使用されます。 フォントがダウンロードされると、最初の要求を行ったアプリだけでなく、デバイス上のすべてのアプリケーションで使用できるようになります。

フォントのダウンロードが要求されると、アプリはフォントプロバイダーに直接クエリを実行しません。 代わりに、アプリは[`FontsContract`](https://developer.android.com/reference/android/provider/FontsContract.html) API (またはサポートライブラリ26が使用されている場合は[`FontsContractCompat`](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) ) のインスタンスを使用します。  

Android 8.0 では、2つの異なる方法でのフォントのダウンロードがサポートされています。

1. **ダウンロード可能なフォントをリソースとして宣言**する &ndash; アプリは、XML リソースファイルを使用して、ダウンロード可能なフォントを Android に宣言することができます。 これらのファイルには、アプリの起動時にフォントを非同期的にダウンロードしてデバイスにキャッシュするために Android が必要とするすべてのメタデータが含まれます。
2. Android API レベル26の Api を**プログラム**で &ndash; すると、アプリケーションは、アプリケーションの実行中にプログラムによってフォントをダウンロードできます。 アプリは、指定されたフォントの `FontRequest` オブジェクトを作成し、このオブジェクトを `FontsContract` クラスに渡します。 `FontsContract` は `FontRequest` を受け取り、フォント_プロバイダー_からフォントを取得します。 Android は、同期的にフォントをダウンロードします。 `FontRequest` を作成する例については、このガイドの後半で説明します。

どの方法を使用するかにかかわらず、フォントをダウンロードする前に、Xamarin. Android アプリケーションに追加する必要があるリソースファイルです。 まず、フォントファミリの一部として、 **Resources/font**ディレクトリ内の XML ファイルでフォントを宣言する必要があります。 このスニペットは、Android 8.0 (またはサポートライブラリ v26) に付属する既定のフォントプロバイダーを使用して、 [Google fonts のオープンソースコレクション](https://fonts.google.com)からフォントをダウンロードする方法の例です。

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

`font-family` 要素には、次の属性が含まれており、Android がフォントをダウンロードするために必要な情報を宣言します。

1. **Fontproviderauthority**は、要求に使用されるフォントプロバイダーの権限を &ndash; します。
2. **Fontpackage**は、要求に使用するフォントプロバイダーのパッケージ &ndash; します。 これは、プロバイダーの id を確認するために使用されます。
3. **Fontquery** &ndash; これは、フォントプロバイダーが要求されたフォントを検索するのに役立つ文字列です。 フォントクエリの詳細については、フォントプロバイダーに固有のものです。 ダウンロード可能な[フォント](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/)サンプルアプリの[`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)クラスは、Google Fonts のオープンソースコレクションのフォントのクエリ形式についての情報を提供します。
4. **Fontprovidercerts**は、プロバイダーに署名する必要がある証明書の一連のハッシュの一覧を使用して、リソース配列 &ndash; します。

フォントが定義されたら、ダウンロードに関連する_フォント証明書_に関する情報を提供することが必要になる場合があります。

### <a name="font-certificates"></a>フォント証明書

フォントプロバイダーがデバイスにプレインストールされていない場合、またはアプリが `Xamarin.Android.Support.Compat` ライブラリを使用している場合、Android はフォントプロバイダーのセキュリティ証明書を必要とします。 これらの証明書は、**リソース/値**ディレクトリに保持されている配列リソースファイルに一覧表示されます。

たとえば、次の XML は**Resources/values/fonts_cert**という名前で、Google フォントプロバイダーの証明書を格納します。

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

これらのリソースファイルを配置すると、アプリはフォントをダウンロードすることができます。

### <a name="declaring-downloadable-fonts-as-resources"></a>ダウンロード可能なフォントをリソースとして宣言する

Android では、ダウンロード可能なフォントを**Androidmanifest .xml**で一覧表示することにより、アプリを初めて起動したときにフォントを非同期的にダウンロードします。 フォント自体は、次のように配列のリソースファイルに一覧表示されます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

これらのフォントをダウンロードするには、`meta-data` を `application` 要素の子として追加して、 **Androidmanifest .xml**で宣言する必要があります。 たとえば、ダウンロード可能なフォントが**Resources/values/downloadable_fonts**のリソースファイルで宣言されている場合、このスニペットをマニフェストに追加する必要があります。

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>フォント Api を使用したフォントのダウンロード

[`FontRequest`](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)オブジェクトをインスタンス化し、それを `FontContractCompat.RequestFont` メソッドに渡すことによって、プログラムによってフォントをダウンロードすることができます。 `FontContractCompat.RequestFont` メソッドは、まず、フォントがデバイスに存在するかどうかを確認し、必要に応じてフォントプロバイダーを非同期に照会して、アプリのフォントのダウンロードを試みます。 `FontRequest` がフォントをダウンロードできない場合、Android では既定のシステムフォントが使用されます。

`FontRequest` オブジェクトには、フォントを検索してダウンロードするためにフォントプロバイダーによって使用される情報が含まれています。 `FontRequest` には、次の4つの情報が必要です。

1. **フォントプロバイダー機関**は、要求に使用されるフォントプロバイダーの権限を &ndash; します。
2. **フォントパッケージ**は、要求に使用されるフォントプロバイダーのパッケージ &ndash; ます。 これは、プロバイダーの id を確認するために使用されます。
3. **フォントクエリ**&ndash; これは、フォントプロバイダーが要求されたフォントを検索するのに役立つ文字列です。 フォントクエリの詳細については、フォントプロバイダーに固有のものです。 文字列の詳細は、フォントプロバイダーに固有のものです。 ダウンロード可能な[フォント](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/)サンプルアプリの[`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs)クラスは、Google Fonts のオープンソースコレクションのフォントのクエリ形式についての情報を提供します。
4. **フォントプロバイダー証明書**は、プロバイダーが署名する必要のある証明書の一連のハッシュの一覧を使用して、リソース配列 &ndash; します。

このスニペットは、新しい `FontRequest` オブジェクトをインスタンス化する例です。

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

前のスニペットで `FontToDownload` は、Google フォントのオープンソースコレクションのフォントを支援するクエリです。

`FontRequest` を `FontContractCompat.RequestFont` メソッドに渡す前に、次の2つのオブジェクトを作成する必要があります。

- **`FontsContractCompat.FontRequestCallback`** &ndash; これは拡張する必要がある抽象クラスです。 これは `RequestFont` の完了時に呼び出されるコールバックです。 Xamarin Android アプリでは、`FontsContractCompat.FontRequestCallback` をサブクラス化して `OnTypefaceRequestFailed` と `OnTypefaceRetrieved`を上書きする必要があります。これにより、ダウンロードが失敗または成功したときに実行されるアクションが提供されます。
- **`Handler`** &ndash; この `Handler` は、必要に応じて `RequestFont` がスレッドでフォントをダウンロードするために使用されます。 UI スレッドでフォントをダウンロードすることはでき**ません**。

このスニペットは、Google Fonts のC#オープンソースコレクションからフォントを非同期的にダウンロードするクラスの例です。 `FontRequestCallback` インターフェイスを実装し、`FontRequest`が終了C#したときにイベントを発生させます。

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

このヘルパーを使用するには、新しい `FontDownloadHelper` が作成され、イベントハンドラーが割り当てられます。  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) =>
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>まとめ

このガイドでは、ダウンロード可能なフォントとフォントをリソースとしてサポートするために、Android 8.0 の新しい Api について説明しました。 ここでは、APK に既存のフォントを埋め込み、レイアウトで使用する方法について説明しました。 また、プログラムによって、またはリソースファイルのフォントメタデータを宣言して、Android 8.0 がフォントプロバイダーからのフォントのダウンロードをサポートする方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [フォント Contractcompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources. GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [伴う](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android サポートライブラリ 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Android でのフォントの使用](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS のフォントの太さの指定](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google フォントオープンソースコレクション](https://fonts.google.com/)
- [ソース San Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
