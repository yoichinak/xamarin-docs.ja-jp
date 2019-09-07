---
title: Xamarin Android のオートコンプリート
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/31/2018
ms.openlocfilehash: 575235569351d0856c7fbffbf38a981ede1a35ce
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762436"
---
# <a name="auto-complete-for-xamarinandroid"></a>Xamarin Android のオートコンプリート

`AutoCompleteTextView`は編集可能なテキストビュー要素で、ユーザーの入力中に自動的に入力候補を表示します。 修正候補の一覧がドロップダウンメニューに表示されます。このメニューから、編集ボックスの内容を置き換える項目をユーザーが選択できます。

![オートコンプリートの例](images/auto-complete.png)

## <a name="overview"></a>概要

オートコンプリートの候補を提供するテキスト入力ウィジェットを作成するには、[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェット. 候補は、を[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)通じてウィジェットに関連付けられている文字列のコレクションから受け取ります。

このチュートリアルでは、[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
国名の候補を提供するウィジェット。

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

はラベルであり、 [`TextView`](xref:Android.Widget.TextView)[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェット.

## <a name="tutorial"></a>チュートリアル

*HelloAutoComplete*という名前の新しいプロジェクトを開始します。

という名前`list_item.xml`の XML ファイルを作成し、 **Resources/Layout**フォルダー内に保存します。 このファイルのビルドアクションをに`AndroidResource`設定します。 次のようにファイルを編集します。

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

このファイルは、候補[`TextView`](xref:Android.Widget.TextView)の一覧に表示される各項目に使用される単純なを定義します。

**Resources/Layout/Main**を開き、次のように挿入します。

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

**MainActivity.cs**を開き、次のコードを挿入します。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

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

コンテンツビューが`main.xml`レイアウトに設定された後、[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェットは、を使用し[`FindViewById`](xref:Android.App.Activity.FindViewById*)てレイアウトからキャプチャされます。 次に[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) 、新しいを初期化して`list_item.xml` 、レイアウトを`COUNTRIES`文字列配列内の各リスト項目にバインドします (次の手順で定義)。 最後に、を呼び出して、 [`ArrayAdapter`を](xref:Android.Widget.ArrayAdapter) `SetAdapter()`[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェット。文字列の配列によって候補の一覧が設定されます。

`MainActivity`クラス内に、文字列配列を追加します。

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

これは、ユーザーが次のように入力したときにドロップダウンリストに表示される候補の一覧です。[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェット.

アプリケーションを実行します。 入力すると、次のように表示されるはずです。

[!["Ca" を含む名前を一覧表示する例のオートコンプリートスクリーンショット](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)

## <a name="more-information"></a>説明

ハードコーディングされた文字列配列を使用することは推奨されません。これは、アプリケーションコードがコンテンツではなく動作を重視する必要があるためです。 コンテンツを簡単に変更し、コンテンツのローカライズを容易にするために、文字列などのアプリケーションコンテンツをコードから外部化する必要があります。 このチュートリアルでは、ハードコーディングされた文字列を単純なものにして、[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェット. 代わりに、アプリケーションでは、このような文字列配列を XML ファイルで宣言する必要があります。 これは、 `<string-array>`プロジェクト`res/values/strings.xml`ファイル内のリソースを使用して行うことができます。 例:

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

これらのリソース文字列[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)をに使用するには、元の[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
次のようなコンストラクター行。

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```

### <a name="references"></a>リファレンス

- [AutoCompleteTextView レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input)&ndash;の Xamarin サンプルプロジェクト`AutoCompleteTextView`
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。このチュートリアルは、Android の[オートコンプリートチュートリアル *](https://developer.android.com/resources/tutorials/views/hello-autocomplete.html)を基にしています。_
