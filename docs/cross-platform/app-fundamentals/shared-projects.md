---
title: 共有プロジェクトを使用してコードを共有する
description: 共有プロジェクトを使用すると、さまざまなアプリケーションプロジェクトによって参照される共通のコードを記述できます。 コードは、参照している各プロジェクトの一部としてコンパイルされ、プラットフォーム固有の機能を共有コード ベースに組み込むのに役立つコンパイラ ディレクティブを含めることができます。
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: davidortinau
ms.author: daortin
ms.date: 07/18/2018
ms.openlocfilehash: eee76c056d05edccd1e039bc5e4cb8107d1aceb5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016696"
---
# <a name="shared-projects-code-sharing"></a>共有プロジェクトコード共有

_共有プロジェクトを使用すると、さまざまなアプリケーションプロジェクトによって参照される共通のコードを記述できます。コードは、参照している各プロジェクトの一部としてコンパイルされ、プラットフォーム固有の機能を共有コードベースに組み込むのに役立つコンパイラディレクティブを含めることができます。_

共有プロジェクト (共有アセットプロジェクトとも呼ばれます) を使用すると、Xamarin アプリケーションを含む複数のターゲットプロジェクト間で共有されるコードを作成できます。

これらはコンパイラディレクティブをサポートしているため、共有プロジェクトを参照しているプロジェクトのサブセットにコンパイルするプラットフォーム固有のコードを条件付きで含めることができます。 また、コンパイラディレクティブを管理し、各アプリケーションでコードがどのように表示されるかを視覚化するための IDE サポートも用意されています。

プロジェクト間でコードを共有するために過去にファイルリンクを使用した場合、共有プロジェクトは同様の方法で動作しますが、IDE のサポートは大幅に改善されています。

## <a name="what-is-a-shared-project"></a>共有プロジェクトとは

他のほとんどのプロジェクトの種類とは異なり、共有プロジェクトには (DLL 形式の) 出力がありません。代わりに、コードを参照する各プロジェクトにコンパイルされます。 これを次の図に示します。概念的には、共有プロジェクトのコンテンツ全体が "コピー先" に "コピー" され、それらのプロジェクトの一部としてコンパイルされます。

![](shared-projects-images/sharedassetproject.png "Shared Project architecture")

共有プロジェクトのコードには、コードを使用しているアプリケーションプロジェクトに応じて、コードのセクションを有効または無効にするコンパイラディレクティブを含めることができます。このコードは、図の色分けされたプラットフォームのボックスによって提案されます。

共有プロジェクト自体はコンパイルされません。これは、他のプロジェクトに含めることができるソースコードファイルのグループとして純粋に存在します。 他のプロジェクトによって参照されている場合、コードはそのプロジェクトの*一部*として効果的にコンパイルされます。 共有プロジェクトは他のプロジェクトの種類 (他の共有プロジェクトを含む) を参照することはできません。

Android アプリケーションプロジェクトは他の Android アプリケーションプロジェクトを参照できないことに注意してください。たとえば、android 単体テストプロジェクトは Android アプリケーションプロジェクトを参照できません。 この制限の詳細については、こちらの[フォーラムの説明](https://forums.xamarin.com/discussion/comment/98092/)を参照してください。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac のチュートリアル

このセクションでは、Visual Studio for Mac を使用して共有プロジェクトを作成して使用する方法について説明します。 完全な例については、「[共有プロジェクトの例](#Shared_Project_Example)」を参照してください。

## <a name="creating-a-shared-project"></a>共有プロジェクトの作成

新しい共有プロジェクトを作成するには、 **[ファイル > 新しいソリューション...]** に移動します (または、既存のソリューションを右クリックし、[**追加] > [新しいプロジェクトの追加**] の順に選択します)。

[![新しい共有プロジェクト](shared-projects-images/xs-newsolution-sml.png "新しいソリューション")](shared-projects-images/xs-newsolution.png#lightbox)

次の画面で、プロジェクト名を選択し、 **[作成]** をクリックします。

新しい共有プロジェクトが表示されます。参照またはコンポーネントノードがありません。これらは共有プロジェクトではサポートされていません。

![空の共有プロジェクト](shared-projects-images/xs-empty.png "空の共有プロジェクト")

共有プロジェクトを使用するには、少なくとも1つのビルド可能なプロジェクト (iOS、Android のアプリケーション、ライブラリ、PCL プロジェクトなど) によって参照される必要があります。 共有プロジェクトは、それを参照するものがない場合はコンパイルされません。そのため、構文 (またはその他の) エラーは、他のものによって参照されるまで強調表示されません。

共有プロジェクトへの参照の追加は、通常のライブラリプロジェクトを参照する場合と同じように行われます。 このスクリーンショットは、共有プロジェクトを参照している Xamarin の iOS プロジェクトを示しています。

![](shared-projects-images/xs-reference.png "Project reference to Shared Project")

共有プロジェクトが別のライブラリまたはアプリケーションによって参照されたら、ソリューションをビルドし、コード内のエラーを表示することができます。 共有プロジェクトが他の_2 つ_以上のプロジェクトによって参照されている場合、ソースコードエディターの左上にメニューが表示され、このファイルを参照するプロジェクトを選択できます。

## <a name="shared-project-options"></a>共有プロジェクトのオプション

共有プロジェクトを右クリックし、 **[オプション]** を選択すると、他のプロジェクトの種類よりも設定が多くなります。 共有プロジェクトはコンパイルされないため (独自に)、出力またはコンパイラのオプション、プロジェクト構成、アセンブリの署名、またはカスタムコマンドを設定することはできません。 共有プロジェクトのコードは、これらの値を参照しているすべての値から効率的に継承します。

**[オプション]** 画面を次に示します。プロジェクト**名**と**既定の名前空間**は、一般的に変更される2つの設定のみです。

![](shared-projects-images/xs-sharedprojectoptions.png "Shared Project Options")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio チュートリアル

このセクションでは、Visual Studio を使用して共有プロジェクトを作成して使用する方法について説明します。 完全な実装については、「[共有プロジェクトの例](#Shared_Project_Example)」セクションを参照してください。

### <a name="creating-a-shared-project"></a>共有プロジェクトの作成

新しい共有プロジェクトを作成するには、 **[ファイル]**  >  [**新しい** > **プロジェクト**] の順に移動します。

Visual Studio 2019 で、**新しいプロジェクトの作成** ページの 検索 ボックスに「 **shared** 」と入力します。 **[共有プロジェクト]** テンプレートを選択し、 **[次へ]** を選択します。 プロジェクトの名前を入力し、 **[作成]** を選択します。

Visual Studio 2017 で、 **[共有プロジェクト]** テンプレートを選択し、プロジェクトの名前を選択します。

![Visual Studio 2017 の共有プロジェクトテンプレート](shared-projects-images/vs-newsolution.png)

ソリューションファイルを右クリックし、 **[新しいプロジェクトの追加 >]** を選択して、既存のソリューションに新しい共有プロジェクトを追加することもできます。 新しい共有プロジェクトは、次のようになります (クラスファイルが追加された後)。 参照またはコンポーネントノードがないことに注意してください。これらは共有プロジェクトではサポートされていません。

![](shared-projects-images/vs-empty.png "Empty Shared Project")

共有プロジェクトを使用するには、少なくとも1つのビルド可能なプロジェクト (iOS、Android のアプリケーション、ライブラリ、PCL プロジェクトなど) によって参照される必要があります。 共有プロジェクトは、それを参照するものがない場合はコンパイルされません。そのため、構文 (またはその他の) エラーは、他のものによって参照されるまで強調表示されません。

共有プロジェクトへの参照の追加は、通常のライブラリプロジェクトを参照する場合と同じように行われます。 このスクリーンショットは、共有プロジェクトを参照している Xamarin の iOS プロジェクトを示しています。

![](shared-projects-images/vs-reference.png "Project reference to Shared Project")

共有プロジェクトが別のライブラリまたはアプリケーションによって参照されたら、ソリューションをビルドし、コード内のエラーを表示することができます。 共有プロジェクトが他の_2 つ_以上のプロジェクトによって参照されている場合は、ソースコードエディターの左上にメニューが表示され、現在のコードファイルを参照しているプロジェクトを確認できます。

### <a name="shared-project-properties"></a>共有プロジェクトのプロパティ

共有プロジェクトを選択すると、[プロパティ] パネルに他のプロジェクトの種類よりも多くの設定が表示されます。 共有プロジェクトはコンパイルされないため (独自に)、出力またはコンパイラのオプション、プロジェクト構成、アセンブリの署名、またはカスタムコマンドを設定することはできません。 共有プロジェクトのコードは、これらの値を参照しているすべての値から効率的に継承します。

**[プロパティ]** パネルは次のようになります。**ルート名前空間**は、変更できる唯一の設定です。

![](shared-projects-images/vs-sharedprojectproperties.png "Shared Project Properties")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>共有プロジェクトの例

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky)の例では、共有プロジェクトを使用して、IOS、Android、および Windows Phone アプリケーションの両方で使用される共通コードを格納します。 `SQLite.cs` と `TaskRepository.cs` ソースコードファイルの両方 utilise コンパイラディレクティブ ( `#if __ANDROID__`) を使用して、それらを参照する各アプリケーションに対して異なる出力を生成します。

完全なソリューションの構造は次のとおりです (Visual Studio for Mac と Visual Studio でそれぞれ)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Visual Studio for Mac solution")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Visual Studio solution")

-----

Windows Phone プロジェクトは、Visual Studio for Mac でのコンパイルでサポートされていない場合でも、Visual Studio for Mac 内からナビゲートできます。

実行中のアプリケーションは次のようになります。

![](shared-projects-images/example.png "iOS, Android, Windows Phone examples")

## <a name="summary"></a>まとめ

このドキュメントでは、共有プロジェクトの動作について説明し、Visual Studio for Mac と Visual Studio の両方で共有プロジェクトを作成して使用する方法について説明します。また、操作中の共有プロジェクトを示す単純なサンプルアプリケーションも導入しました。

## <a name="related-links"></a>関連リンク

- [Tasky サンプルアプリ](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [ポータブルクラスライブラリ (サンプル)](~/cross-platform/app-fundamentals/pcl.md)
- [コード共有のオプション (サンプル)](~/cross-platform/app-fundamentals/code-sharing.md)
