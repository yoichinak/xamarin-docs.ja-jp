---
title: NuGet へのコンポーネント参照の更新
description: このドキュメントでは、参照に置き換える、コンポーネントを将来の NuGet パッケージを使用したアプリ、Xamarin コンポーネント ストアが廃止されたための方法を説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: 70ca9a73c83bed5233b77a6f7be80a13f04f2bcb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61360798"
---
# <a name="updating-component-references-to-nuget"></a>NuGet へのコンポーネント参照の更新

> [!IMPORTANT]
> 2018 年 5 月 15 日の時点で、コンポーネント ストアが廃止されました (このクロージャがもともと[発表](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/)2017 年 11 月で)。
>
> Xamarin コンポーネントでは、Visual Studio ではサポートされなくと、NuGet パッケージで置き換える必要があります。 コンポーネントの参照をプロジェクトから手動で削除するには、以下の手順に従います。

NuGet パッケージを追加するための次の手順を参照してください[Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package)または[Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)します。

一連の一般的な Xamarin[プラグインとライブラリ](https://github.com/xamarin/XamarinComponents/blob/master/README.md)NuGet パッケージとして利用可能ではないコンポーネントに代わる方法を見つけやすくすることはできます。

## <a name="manually-removing-component-references"></a>コンポーネントの参照を手動で削除します。

15.6 リリースの Visual Studio と Visual studio for Mac 7.4 のリリースは、プロジェクトのコンポーネントをサポートしません。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio からプロジェクトを読み込む場合必要がありますから削除すること、コンポーネント プロジェクトに手動で説明する次のダイアログ ボックスが表示されます。

![アラート ダイアログの説明、コンポーネントは、プロジェクトからあるし、削除する必要があります。](component-nuget-images/component-alert-vs.png)

プロジェクトからコンポーネントを削除します。

1. 開く、 **.csproj**ファイル。 これを行うには、プロジェクト名を右クリックし、**プロジェクトのアンロード**を選択します 

2. アンロードされたプロジェクトを再度右クリックし、**{プロジェクト名には、} .csproj の編集**を選択しま。

3. ファイル内の参照`XamarinComponentReference`を探します。 次の例のようになっています。 次の例のようにする必要があります。

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

5. ファイルが保存されると、プロジェクト名を右クリックし、選択**プロジェクトの再読み込み**します。

6. ソリューション内の各プロジェクトには、上記の手順を繰り返します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac プロジェクトをロードする場合をする必要があります、コンポーネント プロジェクトから手動で削除するを説明する次のダイアログ ボックスが表示されます。

![アラート ダイアログの説明、コンポーネントは、プロジェクトからあるし、削除する必要があります。](component-nuget-images/component-alert.png)

プロジェクトからコンポーネントを削除します。

1. .cproj ファイルを開きます。 これを行うには、プロジェクト名を右クリックし、**プロジェクトのアンロード**を選択します

2. ファイル内の参照`XamarinComponentReference`を探します。 次の例のようになっています。 次の例のようにする必要があります。

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

3. 参照`XamarinComponentReference`を削除し、ファイルを保存します。 上記の例では、全体を削除する安全 `ItemGroup`

4. ソリューション内の各プロジェクトには、上記の手順を繰り返します。

-----

> [!WARNING]
> 次の手順は、Visual Studio の以前のバージョンでのみ機能します。
> **コンポーネント**ノードは、Visual Studio 2017 または Visual Studio for mac の現在のリリースではなくなりました

次のセクションでは、NuGet パッケージへのコンポーネント参照を変更する既存の Xamarin ソリューションを更新する方法を説明します。

- [NuGet パッケージを含むコンポーネント](#contain)
- [NuGet の置換文字列によるコンポーネント](#replace)

ほとんどのコンポーネントは、上記のカテゴリのいずれかに分類されます。
対応する NuGet パッケージ、読み取り表示されていないコンポーネントを使用している場合、 [NuGet の移行パスなしでコンポーネントを](#require-update)以下のセクション。

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>NuGet パッケージを含むコンポーネント

多くのコンポーネントでは、NuGet パッケージの場合は、既に含まれており、移行パスは、コンポーネント参照を削除するだけです。

ソリューションのコンポーネントをダブルクリックして、NuGet パッケージには、コンポーネントが既に含まれているかどうかを判断できます。

![コンポーネント ノードを展開](component-nuget-images/solution-sml.png)

**パッケージ** タブには、コンポーネントに含まれている NuGet パッケージが一覧表示します。

![[パッケージ] タブには、NuGet が含まれています。](component-nuget-images/packages-tab-sml.png)

なお、**アセンブリ** タブは空になります。

![[アセンブリ] タブが空](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>ソリューションを更新しています

ソリューションを更新するには、削除、**コンポーネント**ソリューションからのエントリ。

![コンポーネントを削除します。](component-nuget-images/delete-component-sml.png)

NuGet パッケージの一覧表示されたままになります、**パッケージ**ノードと、アプリがコンパイルされ、通常どおりに実行します。 今後、このパッケージの更新プログラムを使用して実行は、 **Nuget**更新機能。

![NuGet パッケージを更新します。](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>NuGet の置換文字列によるコンポーネント

場合、コンポーネントの情報ページ**アセンブリ** タブは、次に示すようにエントリは、同等の NuGet パッケージを手動で検索する必要があります。

![アセンブリが含まれます](component-nuget-images/assemblies-tab-sml.png)

なお、**パッケージ** タブは空になります可能性があります。

![](component-nuget-images/packages-tab-empty-sml.png)

_NuGet の依存関係を含めることができますが、これらは無視できます。_

検索するのには、交換用の NuGet パッケージが存在する[NuGet.org](https://www.nuget.org/packages)コンポーネント名を使用して別の方法の作成者。

例として、広く普及している見つかります**pcl-sqlite-net**を検索してパッケージ。

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) -製品名。
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) -作成者のプロファイル。

### <a name="updating-the-solution"></a>ソリューションを更新しています

コンポーネントが NuGet で使用可能な確認が、これらの手順に従います。

#### <a name="delete-the-component"></a>コンポーネントを削除します。

ソリューションのコンポーネントを右クリックし、選択**削除**:

![コンポーネントを削除します。](component-nuget-images/remove-component-sml.png)

これは、コンポーネントとの参照をすべて削除されます。 置き換えます同等の NuGet パッケージを追加するまで、ビルドが壊れます。

#### <a name="add-the-nuget-package"></a>NuGet パッケージを追加します。

1. 右クリックし、**パッケージ**ノード選択**パッケージを追加しています.**.
2. 名前または作成者 NuGet 置換を検索します。

  ![](component-nuget-images/nuget-search-sml.png)

3. キーを押して**パッケージを追加**します。

NuGet パッケージは、すべての依存関係と共に、プロジェクトに追加されます。
これは、ビルドを修正する必要があります。 ビルドのエラーが続く場合は、コンポーネントと NuGet パッケージの間の API の相違点があったかどうかに表示するには、各エラーを調査します。

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>NuGet の移行パスを持たないコンポーネント

アプリケーションで使用されるコンポーネントの置換がすぐに見つからない場合は、懸念必要はありません。 既存のコンポーネントは引き続き Visual Studio 15.5 では、機能と**コンポーネント**ソリューションにはノードが通常どおり表示されます。

ただし、Visual Studio の今後のリリースは_いない_復元またはコンポーネントを更新します。
つまりを新しいコンピューターにソリューションを開いた場合、コンポーネントがないダウンロードしてインストール;作成者は、更新プログラムを提供できません。 計画する必要があります。

* コンポーネントからアセンブリを抽出し、プロジェクト内で直接参照します。
* コンポーネントの作成者に連絡して、NuGet に移行するプランについて依頼してください。
* 別の NuGet パッケージを調査またはコンポーネントがオープン ソースの場合は、ソース コードをシークします。

多くのコンポーネント ベンダーは、引き続き、NuGet への移行に関する機能と他のユーザー (市販製品を含む) 代替配信オプションを調査する可能性があります。


## <a name="related-links"></a>関連リンク
- [人気の Xamarin プラグインとライブラリの一覧](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [インストールして使用する NuGet パッケージ (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [NuGet パッケージ (Mac) を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
