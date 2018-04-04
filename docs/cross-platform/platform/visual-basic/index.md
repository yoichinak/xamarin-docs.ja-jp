---
title: ポータブル Visual Basic.NET
description: このガイドでは、Visual Basic を使用して、Xamarin.iOS および Xamarin.Android を対象とするソリューションで使用できるポータブル クラス ライブラリ (PCL) プロジェクトを作成する方法を説明します。
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6a4ecad0b28dc4b8ba4060966ccefb678c8e6794
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="portable-visual-basicnet"></a>ポータブル Visual Basic.NET

Xamarin iOS および Android のプロジェクト ネイティブでサポートしない Visual Basic です。ただしを iOS デバイスと Android、既存の Visual Basic コードまたは Visual Basic では、アプリケーション ロジックの重要な部分を書き込むには、開発者はポータブル クラス ライブラリを使用できます。 Visual Basic (カスタム レンダラー、依存関係サービス、および XAML の分離コードを除く) すべてでは、Xamarin.Forms アプリケーションを作成できます。

## <a name="requirements"></a>要件

Xamarin.Android 4.10.1、Xamarin.iOS 7.0.4、つまり、これらのツールで作成した、Xamarin のプロジェクトが Visual Basic PCL アセンブリに組み込むことができます、Xamarin Studio 4.2 でポータブル クラス ライブラリのサポートが追加されました。

作成し、Visual Basic のポータブル クラス ライブラリをコンパイル (Visual Studio 2012 またはそれ以降) の Windows で Visual Studio を使用する必要があります。

> [!NOTE]
> Visual Basic のライブラリは、のみ作成でき、Visual Studio を使用してコンパイルします。 Xamarin.iOS および Xamarin.Android では、Visual Basic 言語はサポートされません。
>
> Visual Studio でのみ作業する場合は、Xamarin.iOS および Xamarin.Android プロジェクトから Visual Basic プロジェクトを参照できます。
>
> IOS および Android のプロジェクトも読み込まなければなりません Visual Studio で Mac の場合は、Visual Basic PCL から出力アセンブリを参照する必要があります。


## <a name="creating-a-visual-basicnet-pcl"></a>Visual Basic.NET PCL を作成します。

このセクションで Visual Studio を使用して Visual Basic のポータブル クラス ライブラリを作成する方法について説明します。
ライブラリは、Xamarin.iOS、Xamarin.Android、および Xamarin.Forms のアプリを含む、他のプロジェクトで参照できます。

### <a name="creating-a-pcl"></a>PCL を作成します。

Visual Studio で Visual Basic PCL を追加する場合は、プラットフォーム ライブラリと互換性があるかを説明するプロファイルを選択する必要があります。 プロファイルは、PCL ドキュメントの概要について説明します。

PCL を作成し、そのプロファイルを選択する手順は次のとおりです。

1.  **新しいプロジェクト**画面で、、 **Visual Basic > クラス ライブラリ (ポータブル)**オプション。

    [![](images/image1-sml.png "新しい Visual Basic のポータブル ライブラリを作成します。")](images/image1.png#lightbox)

1.  Visual Studio は、次にすぐに促します**ポータブル クラス ライブラリの追加**ダイアログ プロファイルを構成できるようにします。 サポートし、キーを押す必要のあるプラットフォームのチェック マークを付けて**OK**です。

    [![](images/image2-sml.png "プラットフォームを選択して PCL プロファイルを選択します。")](images/image2.png#lightbox)

1.  ように、Visual Basic PCL プロジェクトが表示されます、**ソリューション エクスプ ローラー**次のようにします。

    [![](images/image3-sml.png "空の Visual Studio PCL プロジェクト")](images/image3.png#lightbox)


PCL は、Visual Basic コードを追加する準備が整いました。 PCL プロジェクトは、他のプロジェクト (アプリケーション プロジェクト、ライブラリのプロジェクトおよびその他の PCL プロジェクト) で参照できます。

### <a name="editing-the-pcl-profile"></a>PCL プロファイルを編集します。

(PCL と互換性のあるプラットフォームを制御) するために PCL をプロファイルを表示し、プロジェクトを右クリックして を選択して変更できます**プロパティ > ライブラリ > 変更しています.**.このスクリーン ショットに表示されるダイアログ ボックスが表示されます。

 [![](images/image4-sml.png "プロジェクトのプロパティで PCL プロファイルを編集します。")](images/image4.png#lightbox)

コードは既に、PCL に追加された後、プロファイルが変更されると、そのコードは、新しく選択したプロファイルの一部ではない機能を参照している場合、ライブラリはコンパイルされなく可能性があります。


## <a name="summary"></a>まとめ

この記事の内容が示されている Visual Studio およびポータブル クラス ライブラリを使用して Xamarin アプリケーションで Visual Basic コードを使用する方法です。 場合でも、Xamarin で Visual Basic が直接サポートされていませんが、iOS と Android アプリに含まれる Visual Basic で記述されたコードを Visual Basic にコンパイルするために PCL をできます。

次のページでは、ネイティブ モードまたは Xamarin.Forms アプリで Visual Basic.NET Pcl を使用する方法について説明します。

- [VB を使用する、Xamarin.iOS および Xamarin.Android のネイティブ アプリの構築](native-apps.md)
- [VB と Xamarin.Forms アプリのビルド](xamarin-forms.md)


## <a name="related-links"></a>関連リンク

- [TaskyPortableVB (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework (Microsoft) を使用したクロスプラット フォーム開発](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)