---
title: "APK を手動でアップロードする"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1309C251-ABF0-4412-B1F5-200DC8321A9D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: c09dcefb97a5edafcd03394e5ae3146b69a40745
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="manually-uploading-the-apk"></a>APK を手動でアップロードする

<a name="Uploading_the_APK" />

APK を Google Play に初めて送信するときは (または、古いバージョンの Xamarin.Android が使われている場合は)、[Google Play Developer Console](https://play.google.com/apps/publish) を使って APK を手動でアップロードする必要があります。 このガイドでは、このプロセスに必要な手順について説明します。 

<a name="devconsole" />

## <a name="google-play-developer-console"></a>Google Play Developer Console

APK のコンパイルとプロモーション アセットの準備が済んだら、アプリケーションを Google Play にアップロードする必要があります。 これは、[Google Play Developer Consol](https://play.google.com/apps/publish) (次図) にログインして行います。 **[Publish an Android App on Google Play]\(Android アプリを Google Play に公開する\)** ボタンをクリックして、アプリケーション配布プロセスを初期化します。

[ ![Google Play Developer Console](manually-uploading-the-apk-images/00-google-play-developer-console-sml.png)](manually-uploading-the-apk-images/00-google-play-developer-console.png)

Google Play に登録されたアプリが既にある場合は、**[Add new application]\(新しいアプリを追加する\)** ボタンをクリックします。

[ ![[Add new application]\(新しいアプリを追加する\) ボタン](manually-uploading-the-apk-images/01-existing-app-sml.png)](manually-uploading-the-apk-images/01-existing-app.png)

**[ADD NEW APPLICATION]\(新しいアプリケーションの追加\)** ダイアログが表示されたら、アプリの名前を入力して、**[Upload APK]\(APK のアップロード\)** をクリックします。

[ ![[Upload APK]\(APK のアップロード\) ボタン](manually-uploading-the-apk-images/02-add-new-application-sml.png)](manually-uploading-the-apk-images/02-add-new-application.png)

次の画面では、アルファ テスト用、ベータ テスト用、または運用用にアプリを公開できます。 次の例では、**[ALPHA TESTING]\(アルファ テスト\)** タブが選ばれています。 **[MyApp]\(マイ アプリ\)** ではライセンス サービスは使われないので、この例では **[Get license key]\(ライセンス キーの取得\)** ボタンをクリックする必要はありません。 ここで、**[Upload your first APK to Alpha]\(最初の APK をアルファにアップロードする\)** ボタンをクリックしてアルファ チャネルに公開します。

[ ![[Upload your first APK to Alpha]\(最初の APK をアルファにアップロードする\) ボタン](manually-uploading-the-apk-images/03-upload-to-alpha-sml.png)](manually-uploading-the-apk-images/03-upload-to-alpha.png)

**[UPLOAD NEW APK TO ALPHA]\(新規 APK をアルファにアップロード\)** ダイアログが表示されます。 **[Browse files]\(ファイルの参照\)** ボタンをクリックするか、ドラッグ アンド ドロップすることで、APK をアップロードできます。 

[ ![[UPLOAD NEW APK TO ALPHA]\(新規 APK をアルファにアップロード\) ダイアログ](manually-uploading-the-apk-images/04-upload-dialog-sml.png)](manually-uploading-the-apk-images/04-upload-dialog.png)

配布するリリース準備ができた APK をアップロードしてください。
次のダイアログ ボックスでは、APK のアップロードの進行状況が示されます。

[![アップロードの進行状況の表示](manually-uploading-the-apk-images/05-upload-progress-sml.png)](manually-uploading-the-apk-images/05-upload-progress.png)

APK をアップロードした後は、テスト方法を選ぶことができます。

[ ![[Choose a Testing Method]\(テスト方法の選択\) ダイアログ](manually-uploading-the-apk-images/06-select-testing-method-sml.png)](manually-uploading-the-apk-images/06-select-testing-method.png)

アプリのテストの詳細については、Google の「[Set up alpha/beta tests](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en)」(アルファ/ベータ テストのセットアップ) ガイドをご覧ください。

アップロードされた APK は、下書きとして保存されます。 次に説明するように Google Play に詳細を提供するまで、公開することはできません。

<a name="Listing_Details" />

## <a name="store-listing"></a>ストアの一覧

**Google Play Developer Console** で **[Store Listing]\(ストアの一覧\)** をクリックして、アプリケーションの潜在ユーザーに対して Google Play で表示される情報を入力します。 

[ ![[Store Listing]\(ストアの一覧\) ダイアログ](manually-uploading-the-apk-images/07-store-listing-sml.png)](manually-uploading-the-apk-images/07-store-listing.png)

<a name="Upload_Assets" />

### <a name="graphics-assets"></a>グラフィックス アセット

**[Store Listing]\(ストアの一覧\)** ページの **[GRAPHICS ASSETS]\(グラフィックス アセット\)** セクションまで下にスクロールします。

[ ![[Graphic Assets]\(グラフィックス アセット\) セクション](manually-uploading-the-apk-images/08-graphic-assets-sml.png)](manually-uploading-the-apk-images/08-graphic-assets.png)

それまでに準備されているすべてのプロモーション アセットがこのセクションにアップロードされます。 提供する必要のあるプロモーション アセットとそ形式に関するガイダンスが表示されます。

<a name="categorization" />

### <a name="categorization"></a>分類

**[GRAPHICS ASSETS]\(グラフィックス アセット\)** セクションの次の **[CATEGORIZATION]\(分類\)** セクションでは、アプリケーションの種類とカテゴリを選びます。

[ ![[Categorization]\(分類\) セクション](manually-uploading-the-apk-images/09-categorization-sml.png)](manually-uploading-the-apk-images/09-categorization.png)

コンテンツのレーティングについては次のセクションの後で説明します。

<a name="contact_details" />

### <a name="contact-details"></a>連絡先の詳細

このページの最後のセクションは **[CONTACT DETAILS]\(連絡先の詳細\)** セクションです。 このセクションでは、アプリケーション開発者の連絡先情報を収集します。

[ ![[Contact Details]\(連絡先の詳細\) セクション](manually-uploading-the-apk-images/10-contact-details-sml.png)](manually-uploading-the-apk-images/10-contact-details.png)

アプリのプライバシー ポリシーの URL を **[PRIVACY POLICY]\(プライバシー ポリシー\)** セクションで指定できます (上図)。

<a name="content_rating" />

## <a name="content-rating"></a>コンテンツのレーティング

**Google Play Developer Console** で **[Content Rating]\(コンテンツ レーティング\)** をクリックします。 このページでは、アプリのコンテンツのレーティングを指定します。 Google Play では、すべてのアプリケーションでコンテンツのレーティングを指定する必要があります。 **[Continue]\(続行\)** ボタンをクリックして、コンテンツ レーティングの質問に答えます。

[ ![[Content rating]\(コンテンツ レーティング\) セクション](manually-uploading-the-apk-images/11-content-rating-sml.png)](manually-uploading-the-apk-images/11-content-rating.png)

Google Play のすべてのアプリケーションは、Google Play レーティング システムに従ってレーティングされる必要があります。 コンテンツのレーティングに加えて、すべてのアプリケーションは Google の[開発者コンテンツ ポリシー](http://www.android.com/us/developer-content-policy.html)に従う必要があります。

Google Play レーティング システムの 4 つのレベルと、レーティング レベルが必要とされるか適用される機能またはコンテンツのガイドラインを次に示します。 

-   **[Everyone]\(全員\)** &ndash; 場所データのアクセス、公開、共有を行うことはできません。 ユーザーが生成したコンテンツをホストすることはできません。 ユーザー間の通信を有効にできません。 

-   **[Low maturity]\(成熟度低\)** &ndash; 場所データにアクセスするが共有しないアプリケーション。 軽度または漫画の暴力の表現。 

-   **[Medium maturity]\(成熟度中\)** &ndash; ドラッグ、アルコール、タバコに対する言及。 ギャンブルのテーマまたはギャンブルのシミュレーション。 扇情的コンテンツ。 冒涜的または下品なユーモア。 挑発的または性的な言及。 
    激しい空想的な暴力。 現実的な暴力。 ユーザーの相互検索が可能。 ユーザーの相互通信が可能。 
    ユーザーの場所データの共有。 

-   **[High maturity]\(成熟度高\)** &ndash; アルコール、タバコ、ドラッグの消費または販売が中心。 挑発的または性的な言及が中心。 写実的な暴力。 

成熟度中のリストの項目は主観的であり、成熟度中のレーティングを示すように思われるガイドラインが成熟度高のレーティングに該当するのに十分な激しさである可能性があります。 

<a name="pricing_and_distribution" />

## <a name="pricing-amp-distribution"></a>価格と配布

**Google Play Developer Console** と **[Pricing and Distribution]\(価格と配布\)** をクリックします。 このページでは、アプリが有料の場合に価格を設定します。
全ユーザーに無料でアプリケーションを配布することもできます。 いったん無料として指定したアプリケーションは、ずっと無料のままにする必要があります。
Google Play では、無料のアプリケーションを有料に変更することはできません (ただし、無料アプリでアプリ内課金によりコンテンツを販売することはできます)。 有料アプリを無料アプリに変更することはいつでもできます。

有料アプリを公開するには、事前に販売者アカウントが必要です。そのためには、**[set up a merchant account]\(販売者アカウントのセットアップ\)** をクリックして、指示に従います。

[![[Pricing and Distribution]\(価格と配布\) ダイアログ](manually-uploading-the-apk-images/12-pricing-sml.png)](manually-uploading-the-apk-images/12-pricing.png)

<a name="manage_countries" />

### <a name="manage-countries"></a>国の管理

次のセクションの **[Manage Countries]\(国の管理\)** では、アプリを配布できる国を設定します。

[ ![[Manage Countries]\(国の管理\) ダイアログ](manually-uploading-the-apk-images/13-manage-countries-sml.png)](manually-uploading-the-apk-images/13-manage-countries.png)

<a name="other_information" />

### <a name="other-information"></a>その他の情報

さらに下にスクロールして、アプリに広告が含まれるかどうかを指定します。 また、**[DEVICE CATEGORIES]\(デバイス カテゴリ\)** セクションには、オプションで Android Wear、Android TV、または Android Auto にアプリを配布するオプションがあります。

[ ![[Contains Ads]\(広告の有無\) セクション](manually-uploading-the-apk-images/14-contains-ads-sml.png)](manually-uploading-the-apk-images/14-contains-ads.png)

このセクションの後には、**[Designed for Families]\(ファミリー向けデザイン\)** や Google Play for Education 経由のアプリ配布など、選択可能な追加オプションがあります。

<a name="consent" />

### <a name="consent"></a>同意

**[Pricing &amp; Distribution]\(価格と配布\)** ページの下部には **[CONSENT]\(同意\)** セクションがあります。
これは必須セクションであり、アプリケーションが [Android コンテンツ ガイドライン](http://www.android.com/market/terms/developer-content-policy.html#hl=us)を満たしていることの宣言と、米国輸出法の対象であることの確認に使われます。

[ ![[Consent]\(同意\) セクション](manually-uploading-the-apk-images/15-consent-sml.png)](manually-uploading-the-apk-images/15-consent.png)

Xamarin.Android アプリの公開に関しては、このガイドでは説明しきれません。
Google Play でのアプリ公開の詳細については、「[Welcome to the Google Play Developer Console Help Center](https://support.google.com/googleplay/android-developer#topic=3450769)」(Google Play Developer Console ヘルプ センターへようこそ) をご覧ください。


<a name="Google_Play_Filters" />

## <a name="google-play-filters"></a>Google Play フィルター

Google Play Web サイトでアプリケーションを参照するときは、公開されているすべてのアプリケーションを検索できます。 Google Play を Android デバイスから参照すると、結果が若干異なります。 結果は、使われているデバイスとの互換性に従ってフィルター処理されます。 たとえば、アプリケーションが SMS メッセージを送信する必要がある場合、SMS メッセージを送信できないデバイスにはそのアプリケーションは表示されません。 検索に適用されるフィルターは以下から作成されます。

1.  デバイスのハードウェア構成。
2.  アプリケーション マニフェスト ファイルでの宣言。
3.  使われている通信事業者 (該当する場合)。
4.  デバイスの場所。

アプリのマニフェストに要素を追加して、Google Play ストアでのアプリのフィルター方法を制御できます。 次の一覧は、アプリケーションのフィルター処理に使うことができるマニフェストの要素と属性です。

-   [supports-screen](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) &ndash; Google Play はこの属性を使って、画面サイズに基づいてデバイスにアプリケーションを展開できるかどうかを判断します。 
    Google Play は、Android が小さいレイアウトを大きい画面に対応させることはできても、その逆はできないものと想定します。 したがって、通常画面のサポートを宣言するアプリケーションは、大きい画面の検索には表示されますが、小さい画面には表示されません。 Xamarin.Android アプリケーションのマニフェスト ファイルに `<supports-screen>` 要素がない場合、Google Play はすべての属性の値が true であり、アプリケーションがすべての画面サイズをサポートするものと想定します。 この要素は、**AndroidManifest.xml** に手動で追加する必要があります。 

-   [uses-configuration](http://developer.android.com/guide/topics/manifest/uses-configuration-element.html) &ndash; このマニフェスト要素は、キーボードの種類、ナビゲーション デバイス、タッチ スクリーンなど、特定のハードウェア機能を要求するために使われます。この要素は、**AndroidManifest.xml** に手動で追加する必要があります。 

-   [uses-feature](http://developer.android.com/guide/topics/manifest/uses-feature-element.html) &ndash; このマニフェスト要素は、アプリケーションが機能するためにデバイスが備えている必要のあるハードウェアまたはソフトウェアの機能を宣言します。 この属性は情報提供のみです。 Google Play は、このフィルターを満たしていないデバイスにアプリケーションを表示しません。 それでも、他の方法 (手動やダウンロード) でアプリケーションをインストールすることはできます。 この要素は、**AndroidManifest.xml** に手動で追加する必要があります。 

-   [uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) &ndash; この要素は、特定の共有ライブラリ (Google Maps など) がデバイス上に存在する必要があることを指定します。 この要素は、`Android.App.UsesLibaryAttribute` で指定することもできます。 例: 

    ```csharp
    [assembly: UsesLibrary("com.google.android.maps", true)]
    ```

-   [uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) &ndash; この要素は、アプリケーションの実行に必要な特定のハードウェア機能が `<uses-feature>` 要素で適切に宣言されていない可能性があることを示すために使われます。 たとえば、アプリケーションがカメラを使うアクセス許可を要求している場合、Google Play は、カメラを宣言する `<uses-feature>` 要素がない場合でも、デバイスはカメラを備えている必要があるものと見なします。 この要素は、`Android.App.UsesPermissionsAttribute` で設定できます。 例: 

    ```csharp
    [assembly: UsesPermission(Manifest.Permission.Camera)]
    ```

-   [uses-sdk](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html) &ndash; この要素は、アプリケーションに必要な最低限の Android API レベルを宣言するために使われます。 この要素は、Xamarin.Android プロジェクトの Xamarin.Android オプションで設定できます。 

-   [compatible-screens](http://developer.android.com/guide/topics/manifest/compatible-screens-element.html) &ndash; この要素は、この要素で指定されている画面サイズおよび密度と一致しないアプリケーションを除外するために使われます。 ほとんどのアプリケーションではこのフィルターを使わないでください。 これは、アプリケーションの配布を厳密に制御する必要がある特定の高パフォーマンスのゲームまたはアプリケーション用です。 上で説明した `<support-screen>` 属性をお勧めします。 

-   [supports-gl-texture](http://developer.android.com/guide/topics/manifest/supports-gl-texture-element.html) &ndash; この要素は、アプリケーションで必要な GL テクスチャ圧縮フォーメーションを宣言するために使われます。 ほとんどのアプリケーションではこのフィルターを使わないでください。 これは、アプリケーションの配布を厳密に制御する必要がある特定の高パフォーマンスのゲームまたはアプリケーション用です。 

アプリ マニフェストの構成方法の詳細については、Android の「[App Manifest](https://developer.android.com/guide/topics/manifest/manifest-intro.html)」(アプリ マニフェスト) トピックをご覧ください。
