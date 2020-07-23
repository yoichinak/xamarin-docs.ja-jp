---
title: コンポーネント参照を NuGet に更新しています
description: このドキュメントでは、Xamarin コンポーネントストアが廃止されたため、コンポーネント参照を NuGet パッケージに置き換える方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: davidortinau
ms.author: daortin
ms.date: 04/18/2018
ms.openlocfilehash: b9b771efe338fbcc250aa6e7a83b73f35269d3bf
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996813"
---
# <a name="updating-component-references-to-nuget"></a>コンポーネント参照を NuGet に更新しています

> [!IMPORTANT]
> 2018年5月15日の時点で、コンポーネントストアは廃止されました (このクロージャは、当初は2017年11月に[発表](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/)されました)。
>
> Xamarin コンポーネントは Visual Studio ではサポートされなくなったため、NuGet パッケージで置き換える必要があります。 プロジェクトからコンポーネント参照を手動で削除するには、次の手順に従います。

[Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package)または[Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)で NuGet パッケージを追加する方法については、こちらの手順を参照してください。

一般的な Xamarin[プラグインとライブラリ](https://github.com/xamarin/XamarinComponents/blob/master/README.md)の一覧を使用して、NuGet パッケージとして使用できないコンポーネントの代替を見つけることができます。

## <a name="manually-removing-component-references"></a>手動によるコンポーネント参照の削除

Visual Studio for Mac の 15.6 7.4 リリースでは、プロジェクトのコンポーネントがサポートされなくなりました。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

プロジェクトを Visual Studio に読み込むと、次のダイアログが表示されます。これは、プロジェクトからコンポーネントを手動で削除する必要があることを示しています。

![コンポーネントがプロジェクト内で見つかったため、削除する必要があることを示すアラートダイアログ](component-nuget-images/component-alert-vs.png)

プロジェクトからコンポーネントを削除するには、次のようにします。

1. **.csproj** ファイルを開きます。 これを行うには、プロジェクト名を右クリックし、[**プロジェクトのアンロード**] を選択します。

2. アンロードしたプロジェクトでもう一度右クリックし、[ **{プロジェクト名} の編集**] を選択します。

3. ファイル内のすべての参照をに検索 `XamarinComponentReference` します。 これは、次の例のようになります。

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. への参照を削除 `XamarinComponentReference` し、ファイルを保存します。 上記の例では、全体を削除するのは安全 `ItemGroup` です。

5. ファイルが保存されたら、プロジェクト名を右クリックし、[**プロジェクトの再読み込み**] を選択します。

6. ソリューション内のプロジェクトごとに上記の手順を繰り返します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

プロジェクトを Visual Studio for Mac に読み込むと、次のダイアログが表示されます。これは、プロジェクトからコンポーネントを手動で削除する必要があることを示しています。

![コンポーネントがプロジェクト内で見つかったため、削除する必要があることを示すアラートダイアログ](component-nuget-images/component-alert.png)

プロジェクトからコンポーネントを削除するには、次のようにします。

1. .csproj ファイルを開きます。 これを行うには、プロジェクト名を右クリックし、[**ツール] > [ファイルの編集**] の順に選択します。

2. ファイル内のすべての参照をに検索 `XamarinComponentReference` します。 これは、次の例のようになります。

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. への参照を削除 `XamarinComponentReference` し、ファイルを保存します。 上記の例では、全体を削除するのは安全です。`ItemGroup`

4. ソリューション内のプロジェクトごとに上記の手順を繰り返します。

-----

> [!WARNING]
> 以下の手順は、以前のバージョンの Visual Studio でのみ使用できます。
> [ **Components (コンポーネント**) ノードは、Visual Studio 2017 または Visual Studio for Mac の現在のリリースでは使用できなくなりました。

以下のセクションでは、既存の Xamarin ソリューションを更新して、NuGet パッケージへのコンポーネント参照を変更する方法について説明します。

- [NuGet パッケージを含むコンポーネント](#contain)
- [NuGet の置換を含むコンポーネント](#replace)

ほとんどのコンポーネントは、上記のいずれかのカテゴリに分類されます。
同等の NuGet パッケージがないと思われるコンポーネントを使用している場合は、以下の「 [nuget 移行パスのないコンポーネント](#require-update)」セクションを参照してください。

<a name="contain"></a>

## <a name="components-that-contain-nuget-packages"></a>NuGet パッケージを含むコンポーネント

多くのコンポーネントには既に NuGet パッケージが含まれており、移行パスは単にコンポーネント参照を削除するためのものです。

ソリューション内のコンポーネントをダブルクリックすることで、コンポーネントに NuGet パッケージが既に含まれているかどうかを確認できます。

![展開されたコンポーネントノード](component-nuget-images/solution-sml.png)

[**パッケージ**] タブには、コンポーネントに含まれている NuGet パッケージが一覧表示されます。

![NuGet を含む [パッケージ] タブ](component-nuget-images/packages-tab-sml.png)

[**アセンブリ**] タブは空になることに注意してください。

![[アセンブリ] タブが空です](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>ソリューションを更新しています

ソリューションを更新するには、ソリューションから**コンポーネント**のエントリを削除します。

![コンポーネントの削除](component-nuget-images/delete-component-sml.png)

NuGet パッケージは [**パッケージ**] ノードに表示されたままになり、アプリは通常どおりコンパイルして実行されます。 今後、このパッケージの更新は**NuGet**の更新機能を使用して実行されます。

![NuGet パッケージの更新](component-nuget-images/nuget-update-sml.png)

<a name="replace"></a>

## <a name="components-with-nuget-replacements"></a>NuGet の置換を含むコンポーネント

[コンポーネント情報ページ**アセンブリ**] タブに次のようなエントリが含まれている場合は、対応する NuGet パッケージを手動で検索する必要があります。

![アセンブリを含む](component-nuget-images/assemblies-tab-sml.png)

[**パッケージ**] タブは、必ず空になっていることに注意してください。

![[パッケージ] タブ](component-nuget-images/packages-tab-empty-sml.png)

_NuGet の依存関係が含まれている場合がありますが、これらは無視してかまいません。_

置換 NuGet パッケージが存在することを確認するには、 [NuGet.org](https://www.nuget.org/packages)で、コンポーネント名を使用するか、または作成者で検索します。

例として、次のように検索することで、一般的な**sqlite-net pcl**パッケージを見つけることができます。

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl)–製品名。
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum)–作成者のプロファイル。

### <a name="updating-the-solution"></a>ソリューションを更新しています

NuGet でコンポーネントが使用可能であることを確認したら、次の手順を実行します。

#### <a name="delete-the-component"></a>コンポーネントを削除します。

ソリューション内のコンポーネントを右クリックし、[**削除**] を選択します。

![コンポーネントの削除](component-nuget-images/remove-component-sml.png)

これにより、コンポーネントとすべての参照が削除されます。 これにより、同等の NuGet パッケージを追加して置き換えられるまで、ビルドが中断されます。

#### <a name="add-the-nuget-package"></a>NuGet パッケージを追加する

1. [**パッケージ**] ノードを右クリックし、[**パッケージの追加**] を選択します。
2. 名前または作成者で NuGet の置換を検索します。

    ![NuGet 検索](component-nuget-images/nuget-search-sml.png)

3. [**パッケージの追加**...

NuGet パッケージは、すべての依存関係と共にプロジェクトに追加されます。
これにより、ビルドが修正されます。 ビルドが引き続き失敗する場合は、各エラーを調査して、コンポーネントと NuGet パッケージ間の API の違いがあるかどうかを確認します。

<a name="require-update"></a>

## <a name="components-without-a-nuget-migration-path"></a>NuGet 移行パスのないコンポーネント

アプリケーションで使用されているコンポーネントの代わりに代替が見つからない場合は、心配しないでください。 既存のコンポーネントは引き続き Visual Studio 15.5 で動作し、**コンポーネント**ノードはソリューションに通常どおり表示されます。

ただし、今後の Visual Studio のリリースでは、コンポーネントの復元や更新は_行われません_。
つまり、新しいコンピューターでソリューションを開くと、コンポーネントがダウンロードおよびインストールされません。また、作成者は更新プログラムを提供できません。 次のことを計画する必要があります。

- コンポーネントからアセンブリを抽出し、プロジェクト内で直接参照します。
- コンポーネントの作成者に連絡し、NuGet への移行計画について確認してください。
- 別の NuGet パッケージを調査するか、コンポーネントがオープンソースの場合はソースコードをシークします。

多くのコンポーネントベンダは NuGet への移行に取り組んでいますが、他のベンダー (市販製品を含む) は、別の配信オプションを調査している可能性があります。

## <a name="related-links"></a>関連リンク

- [一般的な Xamarin プラグインとライブラリの一覧](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [NuGet パッケージをインストールして使用する (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [NuGet パッケージ (Mac) を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
