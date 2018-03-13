---
title: "Hello, Android マルチスクリーン: クイック スタート"
description: "この 2 部構成のガイドでは、Phoneword アプリケーションを拡張して 2 番目の画面を処理します。 その過程で、基本的な Android アプリケーションの構成要素と Android アーキテクチャの詳細を紹介します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 159bd2435a1d2b5252e0fd1b9d525cdf6cfa7207
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="hello-android-multiscreen-quickstart"></a>Hello, Android マルチスクリーン: クイック スタート

_この 2 部構成のガイドでは、Phoneword アプリケーションを拡張して 2 番目の画面を処理します。その過程で、基本的な Android アプリケーションの構成要素と Android アーキテクチャの詳細を紹介します。_

## <a name="hello-android-multiscreen-quickstart"></a>Hello, Android マルチスクリーンのクイック スタート

このガイドのチュートリアル部分では、[Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) アプリケーションに 2 つ目の画面を追加して、アプリを使用して変換した番号の履歴を追跡し続けます。 [最終的なアプリケーション](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen/)には、右のスクリーンショットのように、"変換された" 番号を表示する 2 つ目の画面が表示されます。

[![例のアプリのスクリーンショット](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

付随する[深い分析](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md)では、ビルドされたものを確認し、アーキテクチャ、ナビゲーション、およびその過程で発生したその他の新しい Android 概念について説明します。


## <a name="requirements"></a>必要条件

このガイドは [Hello, Android](~/android/get-started/hello-android/index.md) の続きのため、[Hello, Android クイック スタート](~/android/get-started/hello-android/hello-android-quickstart.md)を完了する必要があります。
以下のチュートリアルに直接ジャンプしたい場合は、(Hello, Android クイック スタートから) [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) の完全版をダウンロードして、それを使用してチュートリアルを開始できます。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルでは、**Phoneword** アプリケーションに**変換履歴**画面を追加します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

まず、Visual Studio で **Phoneword** アプリケーションを開き、**ソリューション エクスプローラー**で **Main.axml** ファイルを編集します。

### <a name="updating-the-layout"></a>レイアウトの更新

**[ボタン]** を **[ツールボックス]** からデザイン サーフェスにドラッグし、**[TranslatedPhoneWord]** TextView の下に配置します。 **[プロパティ]** ウィンドウで、ボタン **ID** を `@+id/TranslationHistoryButton` に変更します。 

[![新しいボタンをドラッグ](hello-android-multiscreen-quickstart-images/vs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/vs/02-new-button.png#lightbox)

ボタンの **[テキスト]** プロパティを `@string/translationHistory` に設定します。 Android Designer はこれを文字どおりに解釈しますが、ボタンのテキストが正しく表示されるようにいくつかの変更を行います。

[![変換履歴ボタンのテキストを設定する](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string-sml.png)](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string.png#lightbox)

**ソリューション エクスプローラー**で **[リソース]** フォルダーの下の**値**ノードを展開し、文字列リソース ファイル **Strings.xml** をダブルクリックします。

[![Strings.xml を開く](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file.png#lightbox)

`translationHistory` 文字列の名前と値を **Strings.xml** ファイルに追加して保存します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**変換履歴**ボタンのテキストが、新しい文字列値を反映するように更新される必要があります。

[![新しい文字列値が反映されたボタン](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png#lightbox)

デザイン サーフェスで**変換履歴**ボタンを選択して、**[プロパティ]** ウィンドウで `enabled` 設定を見つけ、その値を `false` に設定してボタンを無効にします。 これによりデザイン サーフェイス上でボタンが暗くなります。

[![変換履歴ボタンを無効にする](hello-android-multiscreen-quickstart-images/vs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/vs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>2 つ目のアクティビティの作成

2 番目の画面の電源を投入する 2 番目のアクティビティを作成します。 **ソリューション エクスプローラー**で **Phoneword** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

[![新しいアイテムの追加](hello-android-multiscreen-quickstart-images/vs/07-add-new-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/07-add-new-file.png#lightbox)

**[新しい項目の追加]** ダイアログで、**[Visual C#]、[アクティビティ]** の順に選択し、アクティビティ ファイルに **TranslationHistoryActivity.cs** という名前を付けます。

**TranslationHistoryActivity.cs** 内のテンプレート コードを次と置き換えます。

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

このクラスでは、プログラムによって `ListActivity` が作成されるため、このアクティビティ用に新しいレイアウト ファイルを作成する必要はありません。 これについては、「[Hello, Android マルチスクリーンの詳細](~/android/get-started/hello-android/hello-android-deepdive.md)」で詳しく説明しています。

### <a name="adding-translation-history-code"></a>変換履歴コードの追加

このアプリは (最初の画面でユーザーが変換した) 電話番号を収集し、2 番目の画面に渡します。 電話番号は、文字列のリストとして格納されます。 リストをサポートするには、次の `using` ディレクティブを `MainActivity` クラスの先頭に追加します。

```csharp
using System.Collections.Generic;
```

次に、電話番号を入力できる空のリストを作成します。
`MainActivity` クラスは次のようになります。

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

`MainActivity` クラスで、次のコードを追加して**変換履歴**ボタンを登録します (`translationHistory` 宣言の後にこの行を配置します)。

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

`OnCreate` メソッドの末尾に次のコードを追加して、**変換履歴**ボタンを接続します。

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

`phoneNumbers` の一覧に電話番号を追加するように、**[変換]** ボタンを更新します。 `TranslateHistoryButton` の `Click` ハンドラーは次のコードのようになります。

```csharp
// Add code to translate number
string translatedNumber = string.Empty;
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

アプリケーションを保存してビルドし、エラーがないことを確認します。

### <a name="running-the-app"></a>アプリの実行

エミュレーターまたはデバイスにアプリケーションを展開します。 次のスクリーンショットは、実行中の **Phoneword** アプリケーションを示しています。

[![例のスクリーンショット](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

まず、Visual Studio for Mac で **Phoneword** プロジェクトを開き、**Solution Pad** で **Main.axml** ファイルを編集します。

### <a name="updating-the-layout"></a>レイアウトの更新

**[ボタン]** を **[ツールボックス]** からデザイン サーフェスにドラッグし、**[TranslatedPhoneWord]** TextView の下に配置します。 **[プロパティ]** パッドで、ボタン **ID** を `@+id/TranslationHistoryButton` に変更します。 

[![新しいボタンをドラッグ](hello-android-multiscreen-quickstart-images/xs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/xs/02-new-button.png#lightbox)

ボタンの **[テキスト]** プロパティを `@string/translationHistory` に設定します。 Android Designer はこれを文字どおりに解釈しますが、ボタンのテキストが正しく表示されるようにいくつかの変更を行います。

[![変換履歴ボタンのテキストを設定する](hello-android-multiscreen-quickstart-images/xs/03-call-history-string-sml.png)](hello-android-multiscreen-quickstart-images/xs/03-call-history-string.png#lightbox)


**Solution Pad** で **[リソース]** フォルダーの下の**値**ノードを展開し、文字列リソース ファイル **Strings.xml** をダブルクリックします。

[![文字列を開く](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file.png#lightbox)


`translationHistory` 文字列の名前と値を **Strings.xml** ファイルに追加して保存します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**変換履歴**ボタンのテキストが、新しい文字列値を反映するように更新される必要があります。

[![新しい文字列値が反映されたボタン](hello-android-multiscreen-quickstart-images/xs/05-new-string-value-sml.png)](hello-android-multiscreen-quickstart-images/xs/05-new-string-value.png#lightbox)


デザイン サーフェスで**変換履歴**ボタンを選択して、**Properties Pad** で **[動作]** タブを開き、**[有効]** チェック ボックスをダブルクリックして、ボタンを無効にします。 これによりデザイン サーフェイス上でボタンが暗くなります。

[![変換履歴ボタンを無効にする](hello-android-multiscreen-quickstart-images/xs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/xs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>2 つ目のアクティビティの作成

2 番目の画面の電源を投入する 2 番目のアクティビティを作成します。 **Solution Pad** で、**Phoneword** プロジェクトの横の灰色の歯車アイコンをクリックして、**[追加]、[新しいファイル]** の順に選択します。

**[新しいファイル]** ダイアログで、**[Android]、[アクティビティ]** の順に選択し、アクティビティに `TranslationHistoryActivity` と名前を付け、**[追加]** をクリックします。

`TranslationHistoryActivity` のテンプレート コードを次と置き換えます。

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

このクラスでは、プログラムによって `ListActivity` が作成されるため、このアクティビティ用に新しいレイアウト ファイルを作成する必要はありません。 これについては、「[Hello, Android マルチスクリーンの詳細](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md)」で詳しく説明しています。

### <a name="adding-translation-history-code"></a>変換履歴コードの追加

このアプリは (最初の画面でユーザーが変換した) 電話番号を収集し、2 番目の画面に渡します。 電話番号は、文字列のリストとして格納されます。 リストをサポートするには、次の `using` ディレクティブを `MainActivity` クラスの先頭に追加します。

```csharp
using System.Collections.Generic;
```

次に、電話番号を入力できる空のリストを作成します。 `MainActivity` クラスは次のようになります。

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

`MainActivity` クラスで、次のコードを追加して**変換履歴**ボタンを登録します (`TranslationHistoryButton` 宣言の後にこの行を配置します)。

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

`OnCreate` メソッドの末尾に次のコードを追加して、**変換履歴**ボタンを接続します。

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

`phoneNumbers` の一覧に電話番号を追加するように、**[変換]** ボタンを更新します。 `TranslateHistoryButton` の `Click` ハンドラーは次のコードのようになります。

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

### <a name="running-the-app"></a>アプリの実行

エミュレーターまたはデバイスにアプリケーションを展開します。 次のスクリーンショットは、実行中の **Phoneword** アプリケーションを示しています。

[![例のスクリーンショット](hello-android-multiscreen-quickstart-images/screenshot.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

-----

おつかれさまでした。これで最初のマルチスクリーン Xamarin.Android アプリケーションが完成しました。 次は、学習したツールとスキルを詳しく分析します &ndash; [Hello, Android マルチスクリーンの詳細](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md)


## <a name="related-links"></a>関連リンク

- [Xamarin App Icons & Launch Screens (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (サンプル)](https://developer.xamarin.com/samples/monodroid/Phoneword)
- [PhonewordMultiscreen (サンプル)](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen)
