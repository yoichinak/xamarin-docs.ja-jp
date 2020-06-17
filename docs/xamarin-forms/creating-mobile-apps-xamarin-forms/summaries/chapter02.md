---
title:"第 2 章の概要: アプリの構造" の説明: "Xamarin.Forms でモバイル アプリを作成する: 第 2 章の概要 アプリの構造" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB author: davidbritch ms.author: dabritch ms.date:07/17/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>第 2 章の概要 アプリの構造

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> このページの注記では、Xamarin.Forms が書籍に記載されている資料と異なる部分が示されています。

Xamarin.Forms アプリケーションでは、画像の領域を占有するオブジェクトは、"*ビジュアル要素*" と呼ばれ、[`VisualElement`](xref:Xamarin.Forms.VisualElement) クラスでカプセル化されています。 ビジュアル要素は、以下のクラスに対応する 3 つのカテゴリに分割できます。

- [ページ](xref:Xamarin.Forms.Page)
- [レイアウト](xref:Xamarin.Forms.Layout)
- [表示](xref:Xamarin.Forms.View)

`Page` の派生物は、画面全体、または画面のほとんどを占有します。 ページの子はしばしば、子のビジュアル要素を整理する `Layout` の派生物です。 `Layout` の子としては、他の `Layout` クラス、またはテキスト、ビットマップ、スライダー、ボタン、リストボックスなどの使い慣れたオブジェクトである `View` の派生物 (しばしば "*要素*" と呼ばれます) が可能です。

この章では、テキストを表示する `View` の派生物である [`Label`](xref:Xamarin.Forms.Label) に焦点を当てたアプリケーションの作成方法について説明します。

## <a name="say-hello"></a>Hello を表示する

Xamarin プラットフォームをインストールしたら、新しい Xamarin.Forms ソリューションを Visual Studio または Visual Studio for Mac で作成できます。 [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) ソリューションでは、共通コードにポータブル クラス ライブラリを使用しています。

> [!NOTE]
> ポータブル クラス ライブラリは、.NET Standard ライブラリに置き換えられています。 本のすべてのサンプル コードは、.NET Standard ライブラリを使用するように変換されています。

このサンプルでは、Visual Studio で作成される Xamarin.Forms ソリューションを、修正を加えずに示します。 このソリューションは、次の 4 つのプロジェクトで構成されています。

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello): 他のプロジェクトで共有されているポータブル クラス ライブラリ (PCL)
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid): Android 用のアプリケーション プロジェクト
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS): iOS 用のアプリケーション プロジェクト
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP): ユニバーサル Windows プラットフォーム (Windows 10 および Windows 10 Mobile) 用のアプリケーション プロジェクト

> [!NOTE]
> Xamarin.Forms では Windows 8.1、Windows Phone 8.1、Windows 10 Mobile がサポートされなくなりましたが、Xamarin.Forms アプリケーションは Windows 10 デスクトップで実行されます。

これらのアプリケーション プロジェクトのいずれもスタートアップ プロジェクトにすることができ、その後デバイスまたはシミュレーターでプログラムをビルドして実行することができます。

お使いの多くの Xamarin.Forms プログラムでは、アプリケーション プロジェクトを変更しません。 これらは通常は、プログラムを起動するのみの小さなスタブであり続けます。 ユーザーが着目するのは、多くの場合すべてのアプリケーションに共通するライブラリです。

## <a name="inside-the-files"></a>ファイルの内部

**Hello** プログラムで表示されるビジュアルは、[`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) クラスのコンストラクターで定義されています。 `App` は、Xamarin.Forms のクラス [`Application`](xref:Xamarin.Forms.Application) から派生しています。

> [!NOTE]
> Xamarin.Forms 用の Visual Studio ソリューション テンプレートでは、XAML ファイルが使用されたページが作成されます。 この本では、XAML については[第 7 章](chapter07.md)まで扱いません。

**Hello** PCL プロジェクトの **References** セクションには、次の Xamarin.Forms アセンブリが含まれています。

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

5 つのアプリケーション プロジェクトの **References** セクションには、次の個々のプラットフォーム用のアセンブリが追加であります。

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

> [!NOTE]
> これらのプロジェクトの **References** セクションのアセンブリ一覧はなくなりました。 代わりに、このプロジェクト ファイルには、Xamarin.Forms NuGet パッケージを参照する **PackageReference** タグが含まれています。 Visual Studio の **References** セクションには、Xamarin.Forms アセンブリではなく、 **Xamarin.Forms** パッケージがあります。

各アプリケーション プロジェクトには、`Xamarin.Forms` 名前空間の静的 `Forms.Init` メソッドに対する呼び出しが含まれています。 これによって Xamarin.Forms ライブラリが初期化されます。 `Forms.Init` は、プラットフォームごとに異なるバージョンが定義されています。 このメソッドに対する呼び出しは、次のクラスにあります。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`App` クラス、`OnLaunched` メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

これに加え、各プラットフォームでは、共有ライブラリ内の `App` クラスの場所をインスタンス化する必要があります。 これは、次のクラスでの `LoadApplication` に対する呼び出しで発生します。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

これがない場合、これらのアプリケーション プロジェクトはただの "何もしない" プログラムです。

## <a name="pcl-or-sap"></a>PCL か SAP か

Xamarin.Forms ソリューションは、ポータブル クラス ライブラリ (PCL) または共有アセット プロジェクト (SAP) のいずれかの共通コードを使用して作成できます。 SAP ソリューションを作成するには、Visual Studio で [共有] オプションを選択します。 [**HelloSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) ソリューションは、変換されていない SAP テンプレートのデモです。

> [!NOTE]
> ポータブル クラス ライブラリは、.NET Standard ライブラリに置き換えられています。 本のすべてのサンプル コードは、.NET Standard ライブラリを使用するように変換されています。 これを除き、PCL ライブラリと .NET Standard ライブラリは概念的に非常に似ています。

このライブラリの手法では、プラットフォーム アプリケーション プロジェクトによって参照されるすべての共通コードがライブラリ プロジェクトにバンドルされています。 SAP の手法では、すべてのプラットフォーム アプリケーション プロジェクトに共通コードが実際に存在し、それらで共有されます。

Xamarin.Forms 開発者の多くは、ライブラリの手法を好んでいます。 この本では、ほとんどのソリューションでライブラリを使用しています。 SAP を使用しているものでは、プロジェクト名に **Sap** の接尾辞が含まれています。

SAP の手法では、共有プロジェクト内のコードで、C# プリプロセッサ ディレクティブ (`#if`、#`elif`、および `#endif`) と次の事前定義された識別子を使用して、さまざまなプラットフォームに対して異なるコードを実行できます。

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

共有ライブラリでは、この章で後述しますが、実行時に実行するプラットフォームを指定できます。

## <a name="labels-for-text"></a>テキストのラベル

[**Greetings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) ソリューションでは、**Greetings** プロジェクトに新しい C# ファイルを追加する方法を示しています。 このファイルは、`ContentPage` から派生する `GreetingsPage` という名前のクラスを定義しています。 この本のほとんどのプロジェクトには、`Page` の接尾辞が追加されたプロジェクト名を持つ、`ContentPage` の派生物が 1 つが含まれています。

`GreetingsPage` コンストラクターでは、テキストを表示する Xamarin.Forms ビューである [`Label`](xref:Xamarin.Forms.Label) ビューがインスタンス化されます。 [`Text`](xref:Xamarin.Forms.Label.Text) プロパティは、`Label` によって表示されるテキストに設定されています。 このプログラムで `Label` は、`ContentPage` の `Content` プロパティに設定されています。 `App` クラスのコンストラクターは、次いで `GreetingsPage` をインスタンス化し、それをその `MainPage` プロパティに設定します。

テキストはページの左上端に表示されます。 iOS では、これがページの状態バーに重なることを意味します。 この問題には複数の解決方法があります。

### <a name="solution-1-include-padding-on-the-page"></a>解決方法 1 ページにパディングを含める

ページに [`Padding`](xref:Xamarin.Forms.Page.Padding) プロパティを設定します。 `Padding` は、次の 4 つのプロパティを持つ [`Thickness`](xref:Xamarin.Forms.Thickness) 型の構造体です。

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` では、ページからコンテンツを除外する領域を定義できます。 これにより、`Label` が iOS の状態バーに重ならなくなります。

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>解決方法 2 iOS のみでパディングを含める (SAP のみ)

SAP で C# プリプロセッサ ディレクティブを使用して、iOS のみに対し 'Padding' プロパティを設定します。 これのデモは、[**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) ソリューションにあります。

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>解決方法 3 iOS のみでパディングを含める (PCL または SAP)

書籍で使用されているバージョンの Xamarin.Forms では、PCL または SAP のいずれかで iOS に固有の `Padding` プロパティは、[`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) または [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*) 静的メソッドを使用して選択できます。 これらのメソッドは現在非推奨です。

`Device.OnPlatform` メソッドは、プラットフォーム固有のコードを実行したり、プラットフォーム固有の値を選択したりするために使用します。 内部的には、[`Device.OS`](xref:Xamarin.Forms.Device.OS) 静的読み取り専用プロパティを使用し、次の [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform) 列挙型のメンバーを返します。

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- UWP デバイスの場合の [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows)。

`Device.OnPlatform` メソッド、`Device.OS` プロパティ、および `TargetPlatform` 列挙型は現在すべて非推奨です。 代わりに、[`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) プロパティを使用し、`string` の戻り値と次の静的フィールドを比較します。

- [`iOS`](xref:Xamarin.Forms.Device.iOS)、文字列 "iOS"
- [`Android`](xref:Xamarin.Forms.Device.Android)、文字列 "Android"
- [`UWP`](xref:Xamarin.Forms.Device.UWP)、ユニバーサル Windows プラットフォームを示す文字列 "UWP"、

静的な読み取り専用プロパティ [`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom) が関連付けられています。 これにより、次のメンバーを持つ [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) のメンバーが返されます。

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) は使用されていません

iOS と Android の場合、`Tablet` と `Phone` 間のカットオフは 600 ユニットで縦長の向きです。 Windows プラットフォームの場合、`Desktop` は Windows 10 で実行されている UWP アプリケーションを示し、`Phone` は Windows 10 アプリケーションで実行されている UWP アプリケーションを示します。

## <a name="solution-3a-set-margin-on-the-label"></a>解決方法 3a ラベルに余白を設定する

[`Margin`](xref:Xamarin.Forms.View.Margin) プロパティは実装が遅く、本に含めることはできませんでしたが、これも `Thickness` の型であり、これを `Label` に設定して、ビューのレイアウトの計算に含まれるビューの外側の領域を定義することもできます。

`Padding` プロパティは、[`Layout`](xref:Xamarin.Forms.Layout) および [`Page`](xref:Xamarin.Forms.Page) の派生物でのみ使用できます。 `Margin` プロパティは、[`View`](xref:Xamarin.Forms.View) のすべての派生物で使用できます。

## <a name="solution-4-center-the-label-within-the-page"></a>解決方法 4 ページ中央にラベルを配置する

`Label` の [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティを [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 型の値に設定すると、`Page` 内で `Label` を中央 (または、他の 8 つの場所の 1 つ) に配置することができます。 `LayoutOptions` 構造体では、次の 2 つのプロパティを定義します。

- [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) 型の [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) プロパティ。これは、向きに応じて左または上を意味する [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start)、[`Center`](xref:Xamarin.Forms.LayoutAlignment.Center)、および向きに応じて右または下を意味する [`End`](xref:Xamarin.Forms.LayoutAlignment.End)、[`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill) の 4 つのメンバーを持つ列挙体です。

- `bool` 型の [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) プロパティ。

一般に、これらのプロパティは直接使用しません。 代わりに、これら 2 つのプロパティを組み合わせが、`LayoutOptions` 型の 8 つの静的な読み取り専用プロパティで用意されています。

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Xamarin.Forms レイアウトで最も重要なプロパティは `HorizontalOptions` と `VerticalOptions` であり、「[**第 4 章:スタックをスクロール**](chapter04.md)」で詳細に説明されています。

`Label` の `HorizontalOptions` と `VerticalOptions` のプロパティが両方とも `LayoutOptions.Center` に設定されている結果は次のとおりです。

[![greetings プログラムのトリプル スクリーンショット](images/ch02fg05-small.png "水平および垂直方向に中央揃えされたラベル")](images/ch02fg05-large.png#lightbox "水平および垂直方向に中央揃えされたラベル")

## <a name="solution-5-center-the-text-within-the-label"></a>解決方法 5 ラベル中央にテキストを配置する

`Label` の [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) プロパティおよび [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) プロパティを [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 列挙型のメンバーに設定して、テキストを中央揃えに (または、ページの他の 8 つの場所に配置) することもできます。

- (向きに応じて) 左または上を意味する [`Start`](xref:Xamarin.Forms.TextAlignment.Start)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- (向きに応じて) 右または下を意味する [`End`](xref:Xamarin.Forms.TextAlignment.End)

`HorizontalAlignment` プロパティと `VerticalAlignment` プロパティが `View` で定義され、すべての `View` の派生物によって継承されるのに対し、これら 2 つのプロパティは、`Label` によってのみ定義されます。 結果のビジュアルは似て見えますが、次の章で示すとおり大きく異なります。

## <a name="related-links"></a>関連リンク

- [第 2 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [第 2 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [第 2 章の F# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin.Forms の概要](~/get-started/index.yml)
