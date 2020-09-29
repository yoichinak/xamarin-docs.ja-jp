---
title: Visual Basic と .NET Standard
description: このガイドでは、Visual Basic を使用して、Xamarin および Xamarin Android を対象とするソリューションで使用できるプロジェクトを .NET Standard 作成する方法について説明します。
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 8ccf96e266f9d5eae69a178cfcad3d1e48fb6962
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457532"
---
# <a name="visual-basic-and-net-standard"></a>Visual Basic と .NET Standard

Xamarin Android および iOS プロジェクトは、Visual Basic をネイティブでサポートしていません。ただし、開発者は [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) ライブラリを使用して、既存の Visual Basic コードを Android や iOS に移行したり、Visual Basic にアプリケーションロジックの重要な部分を書き込んだりすることができます。 Xamarin. Forms アプリケーションは、Visual Basic (カスタムレンダラー、依存サービス、および XAML 分離コードを除く) 全体で作成できます。

## <a name="requirements"></a>必要条件

.NET Standard ライブラリ Visual Basic を作成してコンパイルするには、Windows 上の Visual Studio (Visual Studio 2017 以降) を使用する必要があります。

> [!NOTE]
> Visual Basic ライブラリは、Visual Studio を使用して作成およびコンパイルすることしかできません。 Xamarin Android と Xamarin. iOS では、Visual Basic 言語はサポートされていません。
>
> Visual Studio だけで作業する場合は、Xamarin. Android プロジェクトと Xamarin. iOS プロジェクトから Visual Basic プロジェクトを参照できます。
>
> Android および iOS プロジェクトも Visual Studio for Mac に読み込む必要がある場合は、Visual Basic アセンブリから出力アセンブリを参照する必要があります。

## <a name="creating-a-visual-basicnet-net-standard-library"></a>Visual Basic.NET .NET Standard ライブラリの作成

このセクションでは、Visual Studio 2019 を使用して Visual Basic .NET Standard ライブラリを作成する方法について説明します。
その後、ライブラリは、Xamarin Android、Xamarin、xamarin、Xamarin などの他のプロジェクトで参照できます。

Visual Studio で Visual Basic .NET Standard ライブラリを追加する場合、適切なプロジェクトの種類を選択する必要があります。

1. Visual Studio 2019 から **、[新しいプロジェクトの作成**] を選択します。

2. **Visual Basic ライブラリ**を入力してプロジェクトオプションをフィルター処理し、[**クラスライブラリ (.NET Standard)** ] オプションを Visual Basic アイコンで選択します。

    [![Visual Basic ライブラリのフィルター](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

3. 次の画面で、プロジェクトの名前を入力し、[ **作成**] をクリックします。

4. 次のように、Visual Basic プロジェクトが  **ソリューションエクスプローラー** に表示されます。

    [![空の Visual Basic プロジェクト](images/new-library-sml.png)](images/new-library.png#lightbox)

これで、プロジェクトの Visual Basic コードを追加できるようになりました。 .NET Standard プロジェクトは、他のプロジェクト (アプリケーションプロジェクトまたはライブラリプロジェクト) で参照できます。

## <a name="summary"></a>まとめ

この記事では、Visual Studio を使用して Xamarin アプリケーションの Visual Basic コードを使用する方法について説明しました。 Xamarin は Visual Basic を直接サポートしていませんが、Visual Basic を .NET Standard ライブラリにコンパイルすることで、Visual Basic で記述されたコードを Android および iOS アプリに組み込むことができます。

次のページでは、ネイティブまたは Xamarin で Visual Basic.NET .NET Standard ライブラリを使用する方法について説明します。

- [VB を使用するネイティブ Xamarin iOS アプリと Xamarin Android アプリのビルド](native-apps.md)
- [VB での Xamarin Forms アプリのビルド](xamarin-forms.md)

## <a name="related-links"></a>関連リンク

- [TaskyVB (サンプル)](/samples/xamarin/mobile-samples/visualbasic-taskyvb/)
- [XamarinFormsVB (サンプル)](/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [.NET Standard と Xamarin](~/cross-platform/app-fundamentals/net-standard.md)
- [.NET Standard](/dotnet/standard/net-standard/)