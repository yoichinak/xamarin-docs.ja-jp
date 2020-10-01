---
title: Xamarin.Forms での文字列とイメージのローカライズ
description: Xamarin.Forms アプリは、.NET リソース ファイルを使用してローカライズできます。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fac41e57d13815dcd202521d16ae4730e1d99cfa
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555867"
---
# <a name="no-locxamarinforms-string-and-image-localization"></a>Xamarin.Forms の文字列とイメージのローカライズ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)

ローカライズは、ターゲット市場の特定の言語または文化の要件を満たすようにアプリケーションを適合させるプロセスです。 ローカライズを実現するには、アプリケーションのテキストとイメージを複数の言語に翻訳する必要がある場合があります。 ローカライズされたアプリケーションでは、モバイル デバイスのカルチャ設定に基づいて、翻訳されたテキストが自動的に表示されます。

![iOS および Android のローカライズ アプリケーションのスクリーンショット](text-images/localizationdemo-screenshots.png)

.NET Framework には、[Resx リソース ファイル](/dotnet/framework/resources/creating-resource-files-for-desktop-apps)を使用してアプリケーションをローカライズするための組み込みメカニズムが用意されています。 リソース ファイルには、指定されたキーのコンテンツをアプリケーションで取得できるように、テキストやその他のコンテンツが名前と値のペアとして格納されます。 リソース ファイルを使用すると、ローカライズされたコンテンツをアプリケーション コードから分離できます。

リソース ファイルを使用して Xamarin.Forms アプリケーションをローカライズするには、次の手順を実行する必要があります。

1. 翻訳されたテキストを含む [Resx ファイルを作成する](#create-resx-files)。
1. 共有プロジェクトで[既定のカルチャを指定する](#specify-the-default-culture)。
1. [Xamarin.Forms のテキストをローカライズします](#localize-text-in-xamarinforms)。
1. 各プラットフォームのカルチャ設定に基づいて[イメージをローカライズする](#localize-images)。
1. 各プラットフォームで[アプリケーション名をローカライズする](#localize-the-application-name)。
1. 各プラットフォームで[ローカライズをテストする](#test-localization)。

## <a name="create-resx-files"></a>Resx ファイルを作成する

リソース ファイルは、ビルド処理中にバイナリ リソース (.resources) ファイルにコンパイルされた、 **.resx** 拡張子を持つ XML ファイルです。 Visual Studio 2019 では、リソースの取得に使用される API を提供するクラスが生成されます。 ローカライズされたアプリケーションには、通常、アプリケーションで使用されるすべての文字列を含む既定のリソース ファイル、およびサポートされている各言語のリソース ファイルが含まれています。 このサンプル アプリケーションには、リソース ファイルを含む共有プロジェクト内の **Resx** フォルダー、および **AppResources.resx** という名前の既定のリソース ファイルがあります。

リソース ファイルには、各項目に関する次の情報が含まれています。

- **[名前]** では、コード内のテキストへのアクセスに使用されるキーが指定されます。
- **[値]** では、翻訳済みテキストが指定されます。
- **[コメント]** は、追加情報を含む省略可能なフィールドです。

::: zone pivot="windows"

Visual Studio 2019 では、 **[新しい項目の追加]** ダイアログを使用してリソース ファイルを追加します。

![Visual Studio 2019 で新しいリソースを追加する](text-images/pc-add-resource-file.png)

ファイルが追加されると、各テキスト リソースに対して行を追加できるようになります。

![.resx ファイルで既定のテキスト リソースを指定する](text-images/pc-default-strings.png)

**[アクセス修飾子]** ドロップダウン設定では、リソースへのアクセスに使用されるクラスが Visual Studio でどのように生成されるかが決定されます。 [アクセス修飾子] を **[公開]** または **[内部]** に設定すると、指定したアクセシビリティ レベルを持つクラスが生成されます。 [アクセス修飾子] を **[コード生成なし]** に設定すると、クラス ファイルは生成されません。 既定のリソース ファイルは、クラス ファイルを生成するように構成する必要があります。これにより、 **.designer.cs** 拡張子を持つファイルがプロジェクトに追加されます。

既定のリソース ファイルが作成されたら、アプリケーションでサポートされるカルチャごとに追加のファイルを作成できます。 追加の各リソース ファイルでは、ファイル名に翻訳カルチャを含める必要があります。また、 **[アクセス修飾子]** を **[コード生成なし]** に設定する必要があります。

実行時に、アプリケーションではリソース要求を特異性の順に解決することが試行されます。 たとえば、デバイスのカルチャが **en-US** である場合、アプリケーションでは次の順序でリソース ファイルが検索されます。

1. AppResources.en-US.resx
1. AppResources.en.resx
1. AppResources.resx (既定)

次のスクリーンショットは、**AppResources.es.cs** という名前のスペイン語の翻訳ファイルを示しています。

![.resx ファイルで既定のスペイン語のテキスト リソースを指定する](text-images/pc-spanish-strings.png)

翻訳ファイルでは、既定のファイルに指定されているのと同じ **[名前]** 値が使用されますが、 **[値]** 列にはスペイン語の文字列が含まれています。 また、 **[アクセス修飾子]** は **[コード生成なし]** に設定されています。

::: zone-end
::: zone pivot="macos"

Visual Studio 2019 for Mac では、 **[新しい項目の追加]** ダイアログを使用してリソース ファイルを追加します。

![Visual Studio 2019 for Mac で新しいリソースを追加する](text-images/mac-add-resource-file.png)

既定のリソース ファイルが作成されたら、リソース ファイルの `root` 要素内に `data` 要素を作成することで、テキストを追加できます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="AddButton" xml:space="preserve">
        <value>Add Note</value>
    </data>
    <data name="NotesLabel" xml:space="preserve">
        <value>Notes:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>e.g. Get Milk</value>
    </data>
</root>
```

**.designer.cs** クラス ファイルを作成するには、リソース ファイルのオプションで **[カスタム ツール]** プロパティを設定します。

![リソース ファイルのプロパティで指定された [カスタム ツール]](text-images/mac-resx-properties.png)

**[カスタム ツール]** を **[PublicResXFileCodeGenerator]** に設定すると、`public` アクセスを持つクラスが生成されます。 **[カスタム ツール]** を **[InternalResXFileCodeGenerator]** に設定すると、`internal` アクセスを持つクラスが生成されます。 **[カスタム ツール]** 値が空の場合、クラスは生成されません。 生成されたクラス名は、リソース ファイル名と一致します。 たとえば、**AppResources.resx** ファイルによって、**AppResources.designer.cs** という名前のファイルに `AppResources` クラスが作成されます。

サポートされているカルチャごとに、追加のリソース ファイルを作成できます。 各言語ファイルでは、ファイル名に翻訳カルチャを含める必要があるため、**es-MX** をターゲットとするファイルには **AppResources.es-MX.resx** という名前を付ける必要があります。

実行時に、アプリケーションではリソース要求を特異性の順に解決することが試行されます。 たとえば、デバイスのカルチャが **en-US** である場合、アプリケーションでは次の順序でリソース ファイルが検索されます。

1. AppResources.en-US.resx
1. AppResources.en.resx
1. AppResources.resx (既定)

言語翻訳ファイルには、既定のファイルと同じ **[名前]** 値が指定されている必要があります。 次の XML は、**AppResources.es.resx** という名前のスペイン語翻訳ファイルを示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="NotesLabel" xml:space="preserve">
        <value>Notas:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>por ejemplo . comprar leche</value>
    </data>
    <data name="AddButton" xml:space="preserve">
        <value>Agregar nuevo elemento</value>
    </data>
</root>
```

::: zone-end

## <a name="specify-the-default-culture"></a>既定のカルチャを指定する

リソース ファイルが正常に機能するには、アプリケーションに `NeutralResourcesLanguage` が指定されている必要があります。 共有プロジェクトでは、**AssemblyInfo.cs** ファイルをカスタマイズして既定のカルチャを指定する必要があります。 次のコードは、**AssemblyInfo.cs** ファイルで `NeutralResourcesLanguage` を **en-US** に設定する方法を示しています。

```csharp
using System.Resources;

// The resources from the neutral language .resx file are stored directly
// within the library assembly. For that reason, changing en-US to a different
// language in this line will not by itself change the language shown in the
// app. See the discussion of UltimateResourceFallbackLocation in the
// documentation for additional information:
// https://docs.microsoft.com/dotnet/api/system.resources.neutralresourceslanguageattribute
[assembly: NeutralResourcesLanguage("en-US")]
```

> [!WARNING]
> `NeutralResourcesLanguage` 属性を指定しない場合、`ResourceManager` クラスにより、特定のリソース ファイルを持たないすべてのカルチャに対して `null` 値が返されます。 既定のカルチャを指定した場合、`ResourceManager` により、サポートされていないカルチャに対して既定の Resx ファイルから結果が返されます。 そのため、サポートされていないカルチャに対してテキストが表示されるように、常に `NeutralResourcesLanguage` を指定することをお勧めします。

既定のリソース ファイルが作成され、**AssemblyInfo.cs** ファイルで既定のカルチャが指定されると、実行時にアプリケーションでは、ローカライズされた文字列を取得できます。

リソース ファイルの詳細については、「[Create resource files for .NET apps](/dotnet/framework/resources/creating-resource-files-for-desktop-apps)」(.NET アプリでのリソース ファイルの作成) を参照してください。

## <a name="specify-supported-languages-on-ios"></a>iOS でサポートされる言語を指定する

iOS では、サポートされるすべての言語をプロジェクトの **Info.plist** ファイルで宣言する必要があります。 **Info.plist** ファイルで、 **[Source]\(ソース\)** ビューを使用して `CFBundleLocalizations` キーの配列を設定し、Resx ファイルに対応する値を指定します。 さらに、`CFBundleDevelopmentRegion` キーを使用して、予期される言語が設定されていることを確認します。

![ローカライズのセクションを示す Info.plist エディターのスクリーンショット](text-images/info-plist.png)

または、XML エディターで **Info.plist** ファイルを開き、次を追加します。

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

> [!NOTE]
> Apple では、ポルトガル語の扱いが予期したものとは若干異なる可能性があります。 詳細については、developer.apple.com の「[Adding Languages](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2)」(言語の追加) を参照してください。

詳細については、「[Info.plst での既定の言語とサポートされる言語の指定](~/ios/app-fundamentals/localization/index.md#specifying-default-and-supported-languages-in-infoplist)」を参照してください。

## <a name="localize-text-in-no-locxamarinforms"></a>Xamarin.Forms のテキストをローカライズする

Xamarin.Forms では、テキストは、生成される `AppResources` クラスを使用してローカライズされます。 このクラスには、既定のリソース ファイル名に基づいた名前が付けられます。 サンプル プロジェクトのリソース ファイルは **AppResources.cs** という名前であるため、Visual Studio では、`AppResources` という名前の一致するクラスが生成されます。 静的プロパティは、リソース ファイル内の行ごとに `AppResources` クラスで生成されます。 サンプル アプリケーションの `AppResources` クラスでは、次の静的プロパティが生成されます。

- AddButton
- NotesLabel
- NotesPlaceholder

これらの値に [x:Static](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md#the-xstatic-markup-extension) プロパティとしてアクセスすると、ローカライズされたテキストを XAML で表示できます。

```xaml
<ContentPage ...
             xmlns:resources="clr-namespace:LocalizationDemo.Resx">
    <Label Text="{x:Static resources:AppResources.NotesLabel}" />
    <Entry Placeholder="{x:Static resources:AppResources.NotesPlaceholder}" />
    <Button Text="{x:Static resources:AppResources.AddButton}" />
</ContentPage>
```

ローカライズされたテキストは、次のようにコードで取得することもできます。

```csharp
public LocalizedCodePage()
{
    Label notesLabel = new Label
    {
        Text = AppResources.NotesLabel,
        // ...
    };

    Entry notesEntry = new Entry
    {
        Placeholder = AppResources.NotesPlaceholder,
        //...
    };

    Button addButton = new Button
    {
        Text = AppResources.AddButton,
        // ...
    };

    Content = new StackLayout
    {
        Children = {
            notesLabel,
            notesEntry,
            addButton
        }
    };
}
```

`AppResources` クラスのプロパティでは、`System.Globalization.CultureInfo.CurrentUICulture` の現在の値を使用して、値を取得するカルチャ リソース ファイルが決定されます。

## <a name="localize-images"></a>イメージをローカライズする

Resx ファイルには、テキストを格納するだけでなく、テキスト以外のイメージやバイナリ データを格納することもできます。 ただし、モバイル デバイスにはさまざまな画面サイズと密度があり、各モバイル プラットフォームには、密度に依存したイメージを表示する機能があります。 そのため、リソース ファイルにイメージを格納する代わりに、プラットフォーム イメージのローカライズ機能を使用する必要があります。

### <a name="localize-images-on-android"></a>Android でイメージをローカライズする

Android の場合、ローカライズされたドローアブル (イメージ) は、**Resources** ディレクトリ内のフォルダーの名前付け規則を使用して格納されます。 フォルダーには、ターゲット言語のサフィックスの付いた **drawable** という名前が付けられます。 たとえば、スペイン語の言語のフォルダーには **drawable-es** という名前が付けられます。

4 文字のロケール コードが必要な場合、Android では、ダッシュの後に **r** を追加する必要があります。 たとえば、メキシコ ロケール (es-MX) フォルダーには、**drawable-es-rMX** という名前を付ける必要があります。 各ロケール フォルダー内のイメージ ファイル名は同じである必要があります。

![Android プロジェクト内のローカライズされたイメージ](text-images/pc-android-images.png)

詳細については、「[Android Localization](~/android/app-fundamentals/localization.md)」(Android のローカライズ) を参照してください。

### <a name="localize-images-on-ios"></a>iOS でイメージをローカライズする

iOS の場合、ローカライズされたイメージは、**Resources** ディレクトリ内のフォルダーの名前付け規則を使用して格納されます。 既定のフォルダーには、**Base.lproj** という名前が付けられます。 言語固有のフォルダーの名前としては、言語またはロケールの名前の後に **lproj** が付けられます。 たとえば、スペイン語のフォルダーには **es.lproj** という名前が付けられます。

4 文字のローカル コードは、2 文字の言語コードと同じように動作します。 たとえば、メキシコ ロケール (es-MX) フォルダーには、**es-MX.lproj** という名前を付ける必要があります。 各ロケール フォルダー内のイメージ ファイル名は同じである必要があります。

![iOS プロジェクト内のローカライズされたイメージ](text-images/pc-ios-images.png)

> [!NOTE]
> iOS では、.lproj フォルダー構造を使用する代わりに、ローカライズされたアセット カタログの作成がサポートされています。 ただし、これらは Xcode で作成および管理する必要があります。

詳細については、「[iOS Localization](~/ios/app-fundamentals/localization/index.md)」(iOS のローカライズ) を参照してください。

### <a name="localize-images-on-uwp"></a>UWP でイメージをローカライズする

UWP の場合、ローカライズされたイメージは、**Assets/Images** ディレクトリ内のフォルダーの名前付け規則を使用して格納されます。 フォルダーには、言語またはロケールの名前が付けられます。 たとえば、スペイン語のフォルダーには **es**、メキシコ ロケール フォルダーには **es-MX** という名前を付ける必要があります。 各ロケール フォルダー内のイメージ ファイル名は同じである必要があります。

![UWP プロジェクト内のローカライズされたイメージ](text-images/pc-uwp-images.png)

詳細については、[UWP のローカライズ](/windows/uwp/design/globalizing/globalizing-portal/)に関するページを参照してください。

### <a name="consume-localized-images"></a>ローカライズされたイメージを使用する

各プラットフォームでは一意のファイル構造を持つイメージが格納されるため、XAML では `OnPlatform` クラスを使用して、現在のプラットフォームに基づいて `ImageSource` プロパティが設定されます。

```xaml
<Image>
    <Image.Source>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="flag.png" />
            <On Platform="UWP" Value="Assets/Images/flag.png" />
        </OnPlatform>
    </Image.Source>
</Image>
```

> [!NOTE]
> `OnPlatform` マークアップ拡張機能では、プラットフォーム固有の値をより簡潔に指定する方法が提供されます。 詳細については、「[OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」(OnPlatform マークアップ拡張機能) を参照してください。

イメージ ソースは、コードの `Device.RuntimePlatform` プロパティに基づいて設定できます。

```csharp
string imgSrc = Device.RuntimePlatform == Device.UWP ? "Assets/Images/flag.png" : "flag.png";
Image flag = new Image
{
    Source = ImageSource.FromFile(imgSrc),
    WidthRequest = 100
};
```

## <a name="localize-the-application-name"></a>アプリケーション名をローカライズする

アプリケーション名はプラットフォームごとに指定され、Resx リソース ファイルは使用されません。 Android でアプリケーション名をローカライズする方法については、[Android でのアプリ名のローカライズ](~/android/app-fundamentals/localization.md#stringsxml-file-format)に関するページを参照してください。 iOS でアプリケーション名をローカライズする方法については、[iOS でのアプリ名のローカライズ](~/ios/app-fundamentals/localization/index.md#app-name)に関するページを参照してください。 UWP でアプリケーション名をローカライズする方法については、[UWP パッケージ マニフェストでの文字列のローカライズ](/windows/uwp/app-resources/localize-strings-ui-manifest)に関するページを参照してください。

## <a name="test-localization"></a>ローカライズをテストする

ローカライズのテストは、デバイスの言語を変更して実行することをお勧めします。 コードで `System.Globalization.CultureInfo.CurrentUICulture` の値を設定することはできますが、プラットフォーム間で動作が一貫しないため、テストにはお勧めできません。

iOS では、設定アプリで、デバイスの言語を変更せずに、アプリごとに特定の言語を設定できます。

Android では、アプリケーションの起動時に言語設定が検出され、キャッシュされます。 言語を変更した場合は、アプリケーションを終了してから再起動し、変更が適用されていることを確認する必要があります。

## <a name="related-links"></a>関連リンク

- [ローカライズのサンプル プロジェクト](/samples/xamarin/xamarin-forms-samples/usingresxlocalization)
- [.NET アプリ用のリソース ファイルを作成する](/dotnet/framework/resources/creating-resource-files-for-desktop-apps)
- [クロスプラットフォームのローカライズ](~/cross-platform/app-fundamentals/localization.md)
- [CultureInfo クラスの使用 (MSDN)](/dotnet/api/system.globalization.cultureinfo)
- [Android のローカライズ](~/android/app-fundamentals/localization.md)
- [iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)
- [UWP のローカライズ](/windows/uwp/design/globalizing/globalizing-portal/)
- [固有カルチャのリソースの検索と使用 (MSDN)](/previous-versions/visualstudio/visual-studio-2008/s9ckwb4b(v=vs.90))