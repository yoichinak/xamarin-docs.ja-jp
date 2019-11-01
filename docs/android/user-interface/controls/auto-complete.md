---
title: Xamarin Android のオートコンプリート
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/31/2018
ms.openlocfilehash: 5a11ab06321b890d8f1f5a35a8456b06a900fbcc
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029374"
---
# <a name="auto-complete-for-xamarinandroid"></a>Xamarin Android のオートコンプリート

`AutoCompleteTextView` は、ユーザーが入力している間に自動的に入力候補を表示する、編集可能なテキストビュー要素です。 修正候補の一覧がドロップダウンメニューに表示されます。このメニューから、編集ボックスの内容を置き換える項目をユーザーが選択できます。

![オートコンプリートの例](images/auto-complete.png)

## <a name="overview"></a>概要

オートコンプリートの候補を提供するテキスト入力ウィジェットを作成するには、 [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)を使用します。
ウィジェット. 候補は、 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)を通じてウィジェットに関連付けられている文字列のコレクションから受け取ります。

このチュートリアルでは、 [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)を作成します。
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

[`TextView`](xref:Android.Widget.TextView)は、 [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)を導入するラベルです。
ウィジェット.

## <a name="tutorial"></a>チュートリアル

*HelloAutoComplete*という名前の新しいプロジェクトを開始します。

`list_item.xml` という名前の XML ファイルを作成し、 **Resources/Layout**フォルダー内に保存します。 このファイルのビルドアクションを `AndroidResource`に設定します。 次のようにファイルを編集します。

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

このファイルは、候補の一覧に表示される各項目に使用される単純な[`TextView`](xref:Android.Widget.TextView)を定義します。

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

**MainActivity.cs**を開き、次のコードを挿入して[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
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

コンテンツビューが `main.xml` レイアウトに設定されると、 [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェットは[`FindViewById`](xref:Android.App.Activity.FindViewById*)を使用してレイアウトからキャプチャされます。 次に、新しい[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)を初期化して、`list_item.xml` レイアウトを `COUNTRIES` 文字列配列内の各リスト項目にバインドします (次の手順で定義)。 最後に、`SetAdapter()` が呼び出され、 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)が[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)に関連付けられます。
ウィジェット。文字列の配列によって候補の一覧が設定されます。

`MainActivity` クラス内に、文字列配列を追加します。

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

これは、ユーザーがに入力したときにドロップダウンリストに表示される候補の一覧です[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェット.

アプリケーションを実行します。 入力すると、次のように表示されるはずです。

["ca" を含む名前を一覧表示するオートコンプリートスクリーンショットの例を![](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)

## <a name="more-information"></a>説明

ハードコーディングされた文字列配列を使用することは推奨されません。これは、アプリケーションコードがコンテンツではなく動作を重視する必要があるためです。 コンテンツを簡単に変更し、コンテンツのローカライズを容易にするために、文字列などのアプリケーションコンテンツをコードから外部化する必要があります。 このチュートリアルでは、ハードコーディングされた文字列を単純なものにするためだけに使用して[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ウィジェット. 代わりに、アプリケーションでは、このような文字列配列を XML ファイルで宣言する必要があります。 これは、プロジェクト `res/values/strings.xml` ファイル内の `<string-array>` リソースを使用して行うことができます。 (例:

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

これらのリソース文字列を[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)に使用するには、元の[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)を置き換えます。
次のようなコンストラクター行。

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```

### <a name="references"></a>関連項目

- [AutoCompleteTextView レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input)&ndash; `AutoCompleteTextView` 用の Xamarin Android サンプルプロジェクト
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。このチュートリアルは、Android の[オートコンプリートチュートリアル *](https://developer.android.com/resources/tutorials/views/hello-autocomplete.html)を基にしています。_
