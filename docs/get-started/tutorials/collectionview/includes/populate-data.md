---
ms.openlocfilehash: ff37327e1fd942382c3eb81b8c6bcc0041347b0e
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98689965"
---
`IEnumerable` 型の [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを使用して、[`CollectionView`](xref:Xamarin.Forms.CollectionView) にデータが取り込まれます。 前の手順で、XAML の `CollectionView` に文字列の配列を取り込みました。 ただし、通常 `CollectionView` には、`IEnumerable` を実装するコードで定義されたコレクションからのデータが取り込まれます。

この演習では、`List` に保管されているオブジェクトのコレクションからのデータを [`CollectionView`](xref:Xamarin.Forms.CollectionView) に取り込むように **CollectionViewTutorial** プロジェクトを変更します。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー** の **[CollectionViewTutorial]** プロジェクトで、次のコードが含まれた `Monkey` という名前のクラスを追加します。

    ```csharp
    public class Monkey
    {
        public string Name { get; set; }
        public string Location { get; set; }
        public string ImageUrl { get; set; }

        public override string ToString()
        {
            return Name;
        }        
    }
    ```

    このコードは、サルを表すイメージの名前、場所、および URL を格納する `Monkey` オブジェクトを定義します。 また、クラスは `ToString` メソッドをオーバーライドして `Name` プロパティを返します。

1. **ソリューション エクスプローラー** の **[CollectionViewTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、 **[MainPage.xaml.cs]** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace CollectionViewTutorial
    {
        public partial class MainPage : ContentPage
        {
            public IList<Monkey> Monkeys { get; private set; }

            public MainPage()
            {
                InitializeComponent();

                Monkeys = new List<Monkey>();
                Monkeys.Add(new Monkey
                {
                    Name = "Baboon",
                    Location = "Africa & Asia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Capuchin Monkey",
                    Location = "Central & South America",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Blue Monkey",
                    Location = "Central and East Africa",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Squirrel Monkey",
                    Location = "Central & South America",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Saimiri_sciureus-1_Luc_Viatour.jpg/220px-Saimiri_sciureus-1_Luc_Viatour.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Lion Tamarin",
                    Location = "Brazil",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Golden_lion_tamarin_portrait3.jpg/220px-Golden_lion_tamarin_portrait3.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Howler Monkey",
                    Location = "South America",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/Alouatta_guariba.jpg/200px-Alouatta_guariba.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Japanese Macaque",
                    Location = "Japan",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/Macaca_fuscata_fuscata1.jpg/220px-Macaca_fuscata_fuscata1.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Mandrill",
                    Location = "Southern Cameroon, Gabon, Equatorial Guinea, and Congo",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Mandrill_at_san_francisco_zoo.jpg/220px-Mandrill_at_san_francisco_zoo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Proboscis Monkey",
                    Location = "Borneo",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Proboscis_Monkey_in_Borneo.jpg/250px-Proboscis_Monkey_in_Borneo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Red-shanked Douc",
                    Location = "Vietnam, Laos",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Portrait_of_a_Douc.jpg/159px-Portrait_of_a_Douc.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gray-shanked Douc",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Cuc.Phuong.Primate.Rehab.center.jpg/320px-Cuc.Phuong.Primate.Rehab.center.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c8/Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg/165px-Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Black Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/RhinopitecusBieti.jpg/320px-RhinopitecusBieti.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Tonkin Snub-nosed Monkey",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg/320px-Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Thomas's Langur",
                    Location = "Indonesia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Thomas%27s_langur_Presbytis_thomasi.jpg/142px-Thomas%27s_langur_Presbytis_thomasi.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Purple-faced Langur",
                    Location = "Sri Lanka",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Semnopithèque_blanchâtre_mâle.JPG/192px-Semnopithèque_blanchâtre_mâle.JPG"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gelada",
                    Location = "Ethiopia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Gelada-Pavian.jpg/320px-Gelada-Pavian.jpg"
                });

                BindingContext = this;
            }
        }
    }
    ```

    このコードは、`IList<Monkey>` 型の `Monkeys` プロパティを定義し、それをクラス コンストラクターの空白のリストに初期化します。 その後、`Monkey` オブジェクトが `Monkeys` コレクションに追加され、ページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) が `MainPage` オブジェクトに設定されます。 `BindingContext` プロパティの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」ガイドの「[Bindings with a Binding Context](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context)」(バインディング コンテキストでのバインディング) をご覧ください。

    > [!IMPORTANT]
    > [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティはビジュアル ツリーを介して継承されます。 したがって、それが [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに設定されているため、`ContentPage` の子オブジェクトは [`CollectionView`](xref:Xamarin.Forms.CollectionView) を含むその値を継承します。

1. **MainPage.xaml** で、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを `Monkeys` コレクションに設定するように [`CollectionView`](xref:Xamarin.Forms.CollectionView) 宣言を変更します。

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}" />
    ```

    このコードデータは、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを `Monkeys` コレクションにバインドします。 実行時に、[`CollectionView`](xref:Xamarin.Forms.CollectionView) は `Monkeys` コレクションの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を調べ、それにこのコレクションからのデータが取り込まれます。 データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」(Xamarin.Forms データ バインディング) をご覧ください。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でコレクションのデータが取り込まれた CollectionView のスクリーンショット](../images/populate-data.png "コレクションのデータを表示する CollectionView")](../images/populate-data-large.png#lightbox "コレクションのデータを表示する CollectionView")

    [`CollectionView`](xref:Xamarin.Forms.CollectionView) は、`Monkeys` コレクション内の各 `Monkey` の `Name` プロパティを表示しています。 これは、`CollectionView` が既定では、 コレクションのオブジェクトを表示するときに (`Name` プロパティ値を返すために `Monkey` クラスでオーバーライドされている) `ToString` メソッドを呼び出すためです。

    Visual Studio で、アプリケーションを停止します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[CollectionViewTutorial]** プロジェクトで、次のコードが含まれた `Monkey` という名前のクラスを追加します。

    ```csharp
    public class Monkey
    {
        public string Name { get; set; }
        public string Location { get; set; }
        public string ImageUrl { get; set; }

        public override string ToString()
        {
            return Name;
        }
    }
    ```

    このコードは、サルを表すイメージの名前、場所、および URL を格納する `Monkey` オブジェクトを定義します。 また、クラスは `ToString` メソッドをオーバーライドして `Name` プロパティを返します。

1. **Solution Pad** の **[CollectionViewTutorial]** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace CollectionViewTutorial
    {
        public partial class MainPage : ContentPage
        {
            public IList<Monkey> Monkeys { get; private set; }

            public MainPage()
            {
                InitializeComponent();

                Monkeys = new List<Monkey>();
                Monkeys.Add(new Monkey
                {
                    Name = "Baboon",
                    Location = "Africa & Asia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Capuchin Monkey",
                    Location = "Central & South America",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Blue Monkey",
                    Location = "Central and East Africa",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Squirrel Monkey",
                    Location = "Central & South America",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Saimiri_sciureus-1_Luc_Viatour.jpg/220px-Saimiri_sciureus-1_Luc_Viatour.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Lion Tamarin",
                    Location = "Brazil",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Golden_lion_tamarin_portrait3.jpg/220px-Golden_lion_tamarin_portrait3.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Howler Monkey",
                    Location = "South America",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/Alouatta_guariba.jpg/200px-Alouatta_guariba.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Japanese Macaque",
                    Location = "Japan",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/Macaca_fuscata_fuscata1.jpg/220px-Macaca_fuscata_fuscata1.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Mandrill",
                    Location = "Southern Cameroon, Gabon, Equatorial Guinea, and Congo",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Mandrill_at_san_francisco_zoo.jpg/220px-Mandrill_at_san_francisco_zoo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Proboscis Monkey",
                    Location = "Borneo",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Proboscis_Monkey_in_Borneo.jpg/250px-Proboscis_Monkey_in_Borneo.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Red-shanked Douc",
                    Location = "Vietnam, Laos",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Portrait_of_a_Douc.jpg/159px-Portrait_of_a_Douc.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gray-shanked Douc",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Cuc.Phuong.Primate.Rehab.center.jpg/320px-Cuc.Phuong.Primate.Rehab.center.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Golden Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c8/Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg/165px-Golden_Snub-nosed_Monkeys%2C_Qinling_Mountains_-_China.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Black Snub-nosed Monkey",
                    Location = "China",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/RhinopitecusBieti.jpg/320px-RhinopitecusBieti.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Tonkin Snub-nosed Monkey",
                    Location = "Vietnam",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg/320px-Tonkin_snub-nosed_monkeys_%28Rhinopithecus_avunculus%29.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Thomas's Langur",
                    Location = "Indonesia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Thomas%27s_langur_Presbytis_thomasi.jpg/142px-Thomas%27s_langur_Presbytis_thomasi.jpg"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Purple-faced Langur",
                    Location = "Sri Lanka",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Semnopithèque_blanchâtre_mâle.JPG/192px-Semnopithèque_blanchâtre_mâle.JPG"
                });

                Monkeys.Add(new Monkey
                {
                    Name = "Gelada",
                    Location = "Ethiopia",
                    ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Gelada-Pavian.jpg/320px-Gelada-Pavian.jpg"
                });

                BindingContext = this;
            }
        }
    }
    ```

    このコードは、`IList<Monkey>` 型の `Monkeys` プロパティを定義し、それをクラス コンストラクターの空白のリストに初期化します。 その後、`Monkey` オブジェクトが `Monkeys` コレクションに追加され、ページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) が `MainPage` オブジェクトに設定されます。 `BindingContext` プロパティの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」ガイドの「[Bindings with a Binding Context](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context)」(バインディング コンテキストでのバインディング) をご覧ください。

    > [!IMPORTANT]
    > [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) プロパティはビジュアル ツリーを介して継承されます。 したがって、それが [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに設定されているため、`ContentPage` の子オブジェクトは [`CollectionView`](xref:Xamarin.Forms.CollectionView) を含むその値を継承します。

1. **MainPage.xaml** で、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを `Monkeys` コレクションに設定するように [`CollectionView`](xref:Xamarin.Forms.CollectionView) 宣言を変更します。

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}" />
    ```

    このコードデータは、[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) プロパティを `Monkeys` コレクションにバインドします。 実行時に、[`CollectionView`](xref:Xamarin.Forms.CollectionView) は `Monkeys` コレクションの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を調べ、それにこのコレクションからのデータが取り込まれます。 データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でコレクションのデータが取り込まれた CollectionView のスクリーンショット](../images/populate-data.png "コレクションのデータを表示する CollectionView")](../images/populate-data-large.png#lightbox "コレクションのデータを表示する CollectionView")

    [`CollectionView`](xref:Xamarin.Forms.CollectionView) は、`Monkeys` コレクション内の各 `Monkey` の `Name` プロパティを表示しています。 これは、`CollectionView` が既定では、 コレクションのオブジェクトを表示するときに (`Name` プロパティ値を返すために `Monkey` クラスでオーバーライドされている) `ToString` メソッドを呼び出すためです。

    Visual Studio for Mac で、アプリケーションを停止します。
