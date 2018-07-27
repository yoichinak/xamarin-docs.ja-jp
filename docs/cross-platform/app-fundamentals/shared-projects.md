---
title: 共有コードを共有するプロジェクトを使用します。
description: 共有プロジェクトでは、複数の異なるアプリケーション プロジェクトによって参照される共通のコードを記述できます。 コードでは、各参照元のプロジェクトの一部としてコンパイルされ、プラットフォーム固有の機能を共有コード ベースに組み込むためのコンパイラ ディレクティブを含めることができます。
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 61096635cd94d0fdd0abe6fda59c4efa41eeceb1
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270216"
---
# <a name="shared-projects-code-sharing"></a>プロジェクト コードの共有の共有

_共有プロジェクトでは、複数の異なるアプリケーション プロジェクトによって参照される共通のコードを記述できます。コードでは、各参照元のプロジェクトの一部としてコンパイルされ、プラットフォーム固有の機能を共有コード ベースに組み込むためのコンパイラ ディレクティブを含めることができます。_

共有のプロジェクト (共有資産プロジェクトと呼ばれることもあります) を使用して、Xamarin アプリケーションを含む複数のターゲット プロジェクト間で共有されるコードを記述できます。

共有プロジェクトを参照しているプロジェクトのサブセットにコンパイルするプラットフォーム固有のコードは、条件付きでが含まれるようにコンパイラ ディレクティブをサポートしています。 コンパイラ ディレクティブを管理して、各アプリケーションでコードがどの視覚化に役立つ IDE のサポートもあります。

過去のファイル リンクを使用したプロジェクト間でコードを共有する場合、共有プロジェクトは大幅に改善された IDE サポートは、同様の方法では動作します。

## <a name="what-is-a-shared-project"></a>共有プロジェクトとは何ですか。

その他のほとんどのプロジェクトの種類とは異なり、共有プロジェクトにしていない任意の出力 (DLL 形式)、それを参照する各プロジェクトにコードをコンパイルする代わりにします。 次の図に示す - 共有プロジェクトの内容全体は概念的には、各参照元のプロジェクトに「コピー」と、それらの一部のようにコンパイルします。

![](shared-projects-images/sharedassetproject.png "共有プロジェクトのアーキテクチャ")

共有プロジェクト内のコードは、有効またはアプリケーションによって、プロジェクトの図に色付きのプラットフォーム ボックスで提示されると、コードで使用してコードのセクションを無効にはコンパイラ ディレクティブを含めることができます。

共有プロジェクトは独自にコンパイルされません、他のプロジェクトに含めることができるソース コード ファイルのグループ化としてのみ存在します。 コードとして効果的にコンパイルされたときに、別のプロジェクトによって参照される、*一部*そのプロジェクトの。 共有プロジェクトには、他のプロジェクト種類 (その他の共有プロジェクトを含む) を参照できません。

Android アプリケーション プロジェクトは、他の Android アプリケーション プロジェクトを参照できません - たとえば、Android の単体テスト プロジェクトのことに注意してくださいでは、Android アプリケーション プロジェクトを参照できません。 この制限の詳細については、この参照してください[フォーラム ディスカッション](http://forums.xamarin.com/discussion/comment/98092/)します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac のチュートリアル

このセクションで説明を作成して、Visual Studio for mac を使用、共有プロジェクトを使用する方法 参照してくださいに[共有プロジェクト例](#Shared_Project_Example)完全な例のセクション。

## <a name="creating-a-shared-project"></a>共有プロジェクトを作成します。

移動する新しい共有プロジェクトを作成する**ファイル > 新しいソリューション.** (または既存のソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加**):

[![新しい共有プロジェクト](shared-projects-images/xs-newsolution-sml.png "新しいソリューション")](shared-projects-images/xs-newsolution.png#lightbox)

次の画面では、プロジェクト名を選択し、をクリックして**作成**です。

新しい共有プロジェクトが次に示す - がありますには、参照されていないか、コンポーネント ノードこれらは、共有プロジェクトのサポートされていません。

![空の共有プロジェクト](shared-projects-images/xs-empty.png "空共有プロジェクト")

共有プロジェクトを使用する (iOS または Android のアプリケーションまたはライブラリを PCL プロジェクトなど) の少なくとも 1 つのビルドできるプロジェクトで参照することが必要です。 関係ありませんが、その構文 (またはその他) を参照するときに共有プロジェクトがコンパイルされないは取得エラーが強調表示されませんが、他のものによって参照されているまでです。

共有プロジェクトへの参照を追加すると、正規表現ライブラリ プロジェクトの参照と同じ方法は行われます。 このスクリーン ショットは、共有プロジェクトを参照する Xamarin.iOS プロジェクトを示しています。

![](shared-projects-images/xs-reference.png "共有プロジェクトへの参照をプロジェクト")

共有プロジェクトが別のライブラリまたはアプリケーションによって参照されているとは、ソリューションをビルドし、コードのエラーを表示します。 共有プロジェクトがによって参照されるときに_2 またはそれ以上_他のプロジェクトでは、メニューが表示されます、ソース コード エディターの左上の表示は、このファイルを参照するプロジェクト選択します。

## <a name="shared-project-options"></a>共有プロジェクトのオプション

共有プロジェクトを右クリックし、選択**オプション**他よりも少ない設定プロジェクトの種類があります。 (自身で) 共有プロジェクトがコンパイルされないため、出力またはコンパイラ オプション、プロジェクトの構成、アセンブリの署名、またはカスタム コマンドを設定することはできません。 共有プロジェクト内のコードは、すべてが参照することからこれらの値を効果的に継承します。



**オプション**- プロジェクトの下の画面が表示**名前**と**Namespace の既定の**は一般に変更する 2 つだけ設定します。


![](shared-projects-images/xs-sharedprojectoptions.png "共有プロジェクトのオプション")



# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)



## <a name="visual-studio-walkthrough"></a>Visual Studio チュートリアル


このセクションで作成して Visual Studio を使用して、共有プロジェクトを使用する方法について説明します。 参照してくださいに[共有プロジェクト例](#Shared_Project_Example)セクションの完全な実装。

### <a name="creating-a-shared-project"></a>共有プロジェクトを作成します。

移動する新しい共有プロジェクトを作成する**ファイル > 新しいソリューション.** プロジェクトとソリューションの名前を選択します。

![](shared-projects-images/vs-newsolution.png "新しいソリューション")

ソリューション ファイルを右クリックして、ソリューションに新しい共有プロジェクトを追加することも**追加 > 新しいプロジェクト.**.新しい共有のプロジェクトを検索 (クラス ファイルが追加された) 後に、次に示すように-参照されていないことを確認またはコンポーネント ノードです。これらは、共有プロジェクトのサポートされていません。

![](shared-projects-images/vs-empty.png "空の共有プロジェクト")

共有プロジェクトを使用する (iOS または Android のアプリケーションまたはライブラリを PCL プロジェクトなど) の少なくとも 1 つのビルドできるプロジェクトで参照することが必要です。 関係ありませんが、その構文 (またはその他) を参照するときに共有プロジェクトがコンパイルされないは取得エラーが強調表示されませんが、他のものによって参照されているまでです。

共有プロジェクトへの参照を追加すると、正規表現ライブラリ プロジェクトの参照と同じ方法は行われます。 このスクリーン ショットは、共有プロジェクトを参照する Xamarin.iOS プロジェクトを示しています。

![](shared-projects-images/vs-reference.png "共有プロジェクトへの参照をプロジェクト")

共有プロジェクトが別のライブラリまたはアプリケーションによって参照されているとは、ソリューションをビルドし、コードのエラーを表示します。 共有プロジェクトがによって参照されるときに_2 またはそれ以上_プロジェクトが現在のコード ファイルを参照してソース コード エディターの左上の他のプロジェクトでは、メニューが表示されます。


### <a name="shared-project-properties"></a>共有プロジェクトのプロパティ


選択すると、共有プロジェクトが設定を減らすプロパティ パネルの他のプロジェクトの種類よりもします。 (自身で) 共有プロジェクトがコンパイルされないため、出力またはコンパイラ オプション、プロジェクトの構成、アセンブリの署名、またはカスタム コマンドを設定することはできません。 共有プロジェクト内のコードは、すべてが参照することからこれらの値を効果的に継承します。

**プロパティ**パネルは、次に示します、**ルート Namespace**唯一の設定を変更することができます。

![](shared-projects-images/vs-sharedprojectproperties.png "共有プロジェクトのプロパティ")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>共有プロジェクトの例

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky)例の両方、iOS、Android、Windows Phone アプリケーションで使用される一般的なコードを含むプロジェクトの共有を使用します。 両方の`SQLite.cs`と`TaskRepository.cs`ソース コード ファイル八コンパイラ ディレクティブ (例。 `#if __ANDROID__`) を参照するアプリケーションごとに別の出力を生成します。

完全なソリューション構造を次に示します (Visual Studio for Mac と Visual Studio でそれぞれ)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Visual Studio for Mac ソリューション")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Visual Studio ソリューション")

-----

Windows Phone プロジェクトからナビゲートできます Visual studio for Mac では、場合でも、その種類のプロジェクトは Visual studio for mac のコンパイルのサポートされていません

実行中のアプリケーションは、以下に示します。

![](shared-projects-images/example.png "iOS、Android、Windows Phone の例")

## <a name="summary"></a>まとめ

このドキュメントでは、共有プロジェクトのしくみ、方法、作成できが Visual Studio for Mac と Visual Studio の両方で使用を説明し、共有プロジェクトの動作を示す単純なサンプル アプリケーションを導入します。

## <a name="related-links"></a>関連リンク

- [Tasky サンプル アプリ](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [ポータブル クラス ライブラリ (サンプル)](~/cross-platform/app-fundamentals/pcl.md)
- [コード共有のオプション (サンプル)](~/cross-platform/app-fundamentals/code-sharing.md)
