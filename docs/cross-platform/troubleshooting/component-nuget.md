---
title: NuGet コンポーネント参照の更新
description: アプリを将来の NuGet パッケージで、コンポーネントが参照に置き換えます。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: e3adee1b56b833442a8c927672cf903d45d03e84
ms.sourcegitcommit: f52aa66de4d07bc00931ac8af791d4c33ee1ea04
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
---
# <a name="updating-component-references-to-nuget"></a>NuGet コンポーネント参照の更新

> [!NOTE]
> Xamarin コンポーネントは、Visual Studio ではサポートされなくと NuGet パッケージで置き換える必要があります。 コンポーネントの参照をプロジェクトから手動で削除するには、以下の手順に従います。

NuGet パッケージを追加するための次の手順を参照してください[Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package)または[Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)です。

## <a name="manually-removing-component-references"></a>コンポーネントの参照を手動で削除します。

2017 年 11 月、でした[発表](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/)Xamarin コンポーネント ストアが終了するということです。 コンポーネントの sunsetting で前方に移動するために、15.6 リリースの Visual Studio と 7.4 リリースの Visual Studio for Mac はサポートしなくなりましたプロジェクト内のコンポーネントです。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio からプロジェクトを読み込む場合は、次のダイアログ ボックスが表示されます、説明する必要があります、コンポーネント プロジェクトから手動で削除します。

![警告ダイアログことの説明、コンポーネントが見つかった後、プロジェクトで削除する必要があります。](component-nuget-images/component-alert-vs.png)

プロジェクトから、コンポーネントを削除します。

1. 開く、 **.csproj**ファイル。 これを行うには、プロジェクト名を右クリックし、**プロジェクトのアンロード**を選択します 

2. アンロードされたプロジェクトを再度右クリックし、**{プロジェクト名には、} .csproj の編集**を選択しま。

3. ファイル内の参照`XamarinComponentReference`を探します。 次の例のようになっています。 次の例のようになります。

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

4. 参照`XamarinComponentReference`を削除し、ファイルを保存します。 上記の例の`ItemGroup`全体を削除しても大丈夫です。

5. ファイルが保存されると、プロジェクト名を右クリックし **プロジェクトの再読み込み**です。

6. ソリューション内の各プロジェクトには、上記の手順を繰り返します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

読み込まれる場合、プロジェクトを Visual Studio for Mac、する必要があります、コンポーネント プロジェクトから手動で削除するを説明する次のダイアログ ボックスが表示されます。

![警告ダイアログことの説明、コンポーネントが見つかった後、プロジェクトで削除する必要があります。](component-nuget-images/component-alert.png)

プロジェクトから、コンポーネントを削除します。

1. .cproj ファイルを開きます。 これを行うには、プロジェクト名を右クリックし、**プロジェクトのアンロード**を選択します

2. ファイル内の参照`XamarinComponentReference`を探します。 次の例のようになっています。 次の例のようになります。

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

3. 参照`XamarinComponentReference`を削除し、ファイルを保存します。 上記の例では安全に全体の削除 `ItemGroup`

4. ソリューション内の各プロジェクトには、上記の手順を繰り返します。

-----

> [!WARNING]
> 次の手順は、Visual Studio の旧バージョンでのみ機能します。
> **コンポーネント**ノードは Visual Studio 2017 または Visual Studio for mac の現在のリリースで利用できなく

次のセクションでは、NuGet パッケージへのコンポーネントの参照を変更する既存の Xamarin ソリューションを更新する方法を説明します。

- [NuGet パッケージを含むコンポーネント](#contain)
- [NuGet の置換コンポーネント](#replace)

ほとんどのコンポーネントは、上記のカテゴリのいずれかに分類されます。
同等の NuGet パッケージ、読み取り表示されていないコンポーネントを使用している場合、 [NuGet 移行パスを持たないコンポーネント](#require-update)以下のセクションです。

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>NuGet パッケージを含むコンポーネント

多くのコンポーネントでは、NuGet パッケージの場合は、既に含まれてし、移行パスを単にコンポーネントの参照を削除します。

ソリューション内のコンポーネントをダブルクリックすると、NuGet パッケージをコンポーネントが既に含まれるかどうかを判断できます。

![コンポーネント ノードの展開](component-nuget-images/solution-sml.png)

**パッケージ** タブがコンポーネントに含まれるすべての NuGet パッケージを一覧表示します。

![[パッケージ] タブには、NuGet が含まれています。](component-nuget-images/packages-tab-sml.png)

なお、**アセンブリ** タブは空になります。

![[アセンブリ] タブが空](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>ソリューションの更新

ソリューションを更新するには、削除、**コンポーネント**ソリューションからのエントリ。

![コンポーネントを削除します。](component-nuget-images/delete-component-sml.png)

NuGet パッケージがで表示されたままになり、**パッケージ**ノードと、アプリがコンパイルされ、通常どおり実行します。 経由でこのパッケージの更新を行うは将来的に、 **Nuget**更新機能。

![NuGet パッケージを更新します。](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>NuGet の置換コンポーネント

場合、コンポーネントの情報ページ**アセンブリ** タブは、次に示すようにエントリは、同等の NuGet パッケージを手動で検索する必要があります。

![アセンブリが含まれます](component-nuget-images/assemblies-tab-sml.png)

なお、**パッケージ** タブはおそらく空になります。

![](component-nuget-images/packages-tab-empty-sml.png)

_NuGet の依存関係を含めることがありますが、これらは無視できます。_

代わりに NuGet パッケージが存在することを確認、検索して[NuGet.org](https://www.nuget.org/packages)コンポーネント名を使用してまたは作成者によってです。

たとえば、表示、人気の高い**sqlite net pcl**を検索してパッケージ。

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) -製品名。
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – 作成者のプロファイルです。

### <a name="updating-the-solution"></a>ソリューションの更新

コンポーネントは NuGet で使用できることを確認、したら、次の手順を行います。

#### <a name="delete-the-component"></a>コンポーネントを削除します。

ソリューション内のコンポーネントを右クリックし、選択**削除**:

![コンポーネントを削除します。](component-nuget-images/remove-component-sml.png)

これは、コンポーネントと参照をすべて削除されます。 置き換えると同等の NuGet パッケージを追加するまで、ビルドが壊れます。

#### <a name="add-the-nuget-package"></a>NuGet パッケージを追加します。

1. 右クリックし、**パッケージ**ノードを選択して**パッケージを追加しています.**.
2. 名前または作成者によって NuGet 置換を検索します。

  ![](component-nuget-images/nuget-search-sml.png)

3. キーを押して**パッケージを追加**です。

NuGet パッケージは、任意の依存関係と共に、プロジェクトに追加されます。
これは、問題を修正、ビルドします。 ビルドが失敗が続く場合は、コンポーネントと、NuGet パッケージの間の API の相違点があったかどうかに表示するには、各エラーを調査します。

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>NuGet の移行パスを持たないコンポーネント

アプリケーションで使用されるコンポーネントの代わりにすぐに見つからない場合は、心配必要はありません。 既存のコンポーネントは引き続き Visual Studio 15.5 で動作し、**コンポーネント**ソリューションにはノードが通常どおりに表示されます。

ただし、Visual Studio の今後のリリースは_いない_復元またはコンポーネントを更新します。
つまりを新しいコンピューターにソリューションを開いた場合、コンポーネントはいないダウンロードしてインストールです。作成者は、更新プログラムを提供できません。 計画する必要があります。

* コンポーネントからアセンブリを抽出し、それらをプロジェクトに直接参照します。
* コンポーネントの作成者に連絡して、NuGet に移行するプランに関する依頼してください。
* 代替の NuGet パッケージを調査またはコンポーネントがオープン ソースの場合は、ソース コードをシークします。

コンポーネントの多くのベンダーは、NuGet への移行で引き続き作業して、他のユーザー (商習慣に基づく利用可能な製品など) その他の配信オプションが調査可能性があります。


## <a name="related-links"></a>関連リンク

- [インストールして、NuGet パッケージ (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [NuGet パッケージ (Mac) を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
