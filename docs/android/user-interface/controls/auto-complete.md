---
title: "オート コンプリート"
ms.topic: article
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f4118881272bb605607d528007064ada561cf7fc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="auto-complete"></a>オート コンプリート


## <a name="overview"></a>概要

オートコンプリートの候補を提供するテキスト エントリ ウィジェットを作成するには、使用、 [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)ウィジェット。 を介して、ウィジェットに関連付けられている文字列のコレクションから受信した提案、 [ `ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)です。

このチュートリアルでは作成、 [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)国名のための推奨事項を提供するウィジェット。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

[ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)を導入するラベル、 [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)ウィジェット。


## <a name="tutorial"></a>チュートリアル

という名前の新しいプロジェクトを開始*HelloAutoComplete*です。

という XML ファイルを作成する`list_item.xml`内で保存し、**リソース/レイアウト**フォルダーです。 設定するには、このファイルのビルド アクション`AndroidResource`です。 次のようにファイルを編集します。

```xml
<?xml version="1.0" encoding="utf-8"?>

<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp"
    android:textColor="#000">
</TextView>
```

このファイルは定義単純な[ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)修正候補の一覧に表示される各項目に対して使用されます。

開いている**Resources/Layout/Main.axml**し、次の挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

開いている**MainActivity.cs**の次のコードを挿入し、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))メソッド。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    AutoCompleteTextView textView = FindViewById<AutoCompleteTextView> (Resource.Id.autocomplete_country);
    var adapter = new ArrayAdapter<String> (this, Resource.Layout.list_item, COUNTRIES);

    textView.Adapter = adapter;
}
```

コンテンツ ビューに設定した後、`main.xml`レイアウト、 [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)ウィジェットが持つレイアウトからキャプチャされた[ `FindViewById`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/)です。 新しい[ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)はバインドに初期化し、`list_item.xml`レイアウト内の各リスト項目を`COUNTRIES`(次の手順で定義されている) 文字列の配列。 最後に、`SetAdapter()`を関連付けるために呼び出される、 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)で、 [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)ウィジェット文字列の配列が提案の一覧に表示できるようにします。

内部、`MainActivity`クラス、文字列の配列を追加します。

```csharp
static string[] COUNTRIES = new string[] {
  "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra",
  "Angola", "Anguilla", "Antarctica", "Antigua and Barbuda", "Argentina",
  "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium",
  "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia",
  "Bosnia and Herzegovina", "Botswana", "Bouvet Island", "Brazil", "British Indian Ocean Territory",
  "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi",
  "Cote d'Ivoire", "Cambodia", "Cameroon", "Canada", "Cape Verde",
  "Cayman Islands", "Central African Republic", "Chad", "Chile", "China",
  "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo",
  "Cook Islands", "Costa Rica", "Croatia", "Cuba", "Cyprus", "Czech Republic",
  "Democratic Republic of the Congo", "Denmark", "Djibouti", "Dominica", "Dominican Republic",
  "East Timor", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Ethiopia", "Faeroe Islands", "Falkland Islands", "Fiji", "Finland",
  "Former Yugoslav Republic of Macedonia", "France", "French Guiana", "French Polynesia",
  "French Southern Territories", "Gabon", "Georgia", "Germany", "Ghana", "Gibraltar",
  "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau",
  "Guyana", "Haiti", "Heard Island and McDonald Islands", "Honduras", "Hong Kong", "Hungary",
  "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
  "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
  "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg",
  "Macau", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands",
  "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia",
  "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
  "Nicaragua", "Niger", "Nigeria", "Niue", "Norfolk Island", "North Korea", "Northern Marianas",
  "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay", "Peru",
  "Philippines", "Pitcairn Islands", "Poland", "Portugal", "Puerto Rico", "Qatar",
  "Reunion", "Romania", "Russia", "Rwanda", "Sqo Tome and Principe", "Saint Helena",
  "Saint Kitts and Nevis", "Saint Lucia", "Saint Pierre and Miquelon",
  "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Saudi Arabia", "Senegal",
  "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands",
  "Somalia", "South Africa", "South Georgia and the South Sandwich Islands", "South Korea",
  "Spain", "Sri Lanka", "Sudan", "Suriname", "Svalbard and Jan Mayen", "Swaziland", "Sweden",
  "Switzerland", "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "The Bahamas",
  "The Gambia", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey",
  "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", "Virgin Islands", "Uganda",
  "Ukraine", "United Arab Emirates", "United Kingdom",
  "United States", "United States Minor Outlying Islands", "Uruguay", "Uzbekistan",
  "Vanuatu", "Vatican City", "Venezuela", "Vietnam", "Wallis and Futuna", "Western Sahara",
  "Yemen", "Yugoslavia", "Zambia", "Zimbabwe"
};
```

これは、ユーザーに入力する場合に、ドロップダウン リストで指定する提案の一覧、 [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)ウィジェット。

アプリケーションを実行します。 入力するを参照して次のようにする必要があります。

[![例オート コンプリートのスクリーン ショットを"ca"を含む名前を一覧表示します。](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>説明

ハード コーディングされた文字列配列を使用して、推奨される設計プラクティスいないコンテンツの動作を集中的に、アプリケーション コードに注意してください。 文字列などのアプリケーションのコンテンツは、コンテンツへの変更が容易し、コンテンツのローカライズを容易にするために、コードから外部化する必要があります。 ハードコーディングされた文字列を使用する簡単と重点にのみこのチュートリアルでは使用が、 [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)ウィジェット。 代わりに、アプリケーションでは、XML ファイルにそのような文字列の配列を宣言する必要があります。 これで、`<string-array>`プロジェクトのリソース`res/values/strings.xml`ファイル。 例:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="countries_array">
        <item>Bahrain</item>
        <item>Bangladesh</item>
        <item>Barbados</item>
        <item>Belarus</item>
        <item>Belgium</item>
        <item>Belize</item>
        <item>Benin</item>
    </string-array>
</resources>
```

これらのリソース文字列を使用する、 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)、置換元[ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)を次のコンス トラクター行。

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>参照

-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
-   [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)

*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、* 
 [ *クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/) *.このチュートリアルがに基づいて、* 
 [ *Android 自動の完全なチュートリアル*](http://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
*です。*
