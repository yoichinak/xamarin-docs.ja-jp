---
title: 移植可能な Visual Basic.NET
description: このガイドでは、Visual Basic を使用して、Xamarin.iOS と Xamarin.Android を対象としたソリューションで使用できるポータブル クラス ライブラリ (PCL) プロジェクトを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
---

# <a name="portable-visual-basicnet"></a>移植可能な Visual Basic.NET

Xamarin iOS、Android プロジェクト ネイティブでサポートしない Visual Basic です。ただし開発者は iOS と Android では、既存の Visual Basic コードに移行する、または Visual Basic では、アプリケーション ロジックの大部分を記述するポータブル クラス ライブラリを使用できます。 Xamarin.Forms アプリケーションは、Visual Basic の (カスタム レンダラー、依存関係サービスでは、XAML 分離コードを除く) で完全作成できます。

## <a name="requirements"></a>必要条件

Xamarin.Android 4.10.1、Xamarin.iOS 7.0.4、つまり、これらのツールで作成したすべての Xamarin プロジェクトが Visual Basic の PCL のアセンブリに組み込むことができます、Xamarin Studio 4.2 でポータブル クラス ライブラリのサポートが追加されました。

作成および Visual Basic のポータブル クラス ライブラリをコンパイルするには、Windows (Visual Studio 2012 以降) で Visual Studio を使用する必要があります。

> [!NOTE]
> Visual Basic ライブラリのみ作成でき、Visual Studio を使用してコンパイルします。 Xamarin.iOS および Xamarin.Android では、Visual Basic 言語はサポートされません。
>
> Visual Studio でのみ作業する場合は、Xamarin.iOS および Xamarin.Android プロジェクトから Visual Basic プロジェクトを参照できます。
>
> IOS と Android プロジェクトする必要がありますも読み込む場合に Visual Studio for Mac は、Visual Basic PCL から出力アセンブリを参照してください。


## <a name="creating-a-visual-basicnet-pcl"></a>Visual Basic.NET PCL を作成します。

このセクションで、Visual Studio を使用して Visual Basic のポータブル クラス ライブラリを作成する方法を説明します。
ライブラリは、Xamarin.iOS、Xamarin.Android、および Xamarin.Forms アプリを含む、他のプロジェクトで参照できます。

### <a name="creating-a-pcl"></a>PCL を作成します。

Visual Studio で Visual Basic の PCL を追加するときに、どのようなプラットフォーム、ライブラリと互換性があるかを説明するプロファイルを選択する必要があります。 プロファイルは PCL のドキュメントの概要について説明します。

PCL を作成し、そのプロファイルを選択する手順は次のとおりです。

1.  **新しいプロジェクト**画面で、、 **Visual Basic > クラス ライブラリ (ポータブル)** オプション。

    [![](images/image1-sml.png "新しい Visual Basic のポータブル ライブラリを作成します。")](images/image1.png#lightbox)

1.  Visual Studio は、次のすぐに促します**ポータブル クラス ライブラリの追加**ダイアログ、プロファイルを構成できるようにします。 キーを押すし、サポートする必要のあるプラットフォームのチェック マークを付けて**OK**します。

    [![](images/image2-sml.png "プラットフォームを選択して、PCL プロファイルを選択します")](images/image2.png#lightbox)

1.  ように、Visual Basic の PCL プロジェクトが表示されます、**ソリューション エクスプ ローラー**次のようにします。

    [![](images/image3-sml.png "空の Visual Studio の PCL プロジェクト")](images/image3.png#lightbox)


PCL は、Visual Basic コードを追加する準備ができました。 PCL プロジェクトは、他のプロジェクト (アプリケーション プロジェクト、ライブラリ プロジェクトとその他の PCL プロジェクト) で参照できます。

### <a name="editing-the-pcl-profile"></a>PCL プロファイルの編集

(つまり、PCL と互換性のあるプラットフォームを制御するには)、PCL プロファイルを表示および変更でプロジェクトを右クリックし、選択できる**プロパティ > ライブラリ > 変更しています.**.表示されるダイアログでは、このスクリーン ショットに示されます。

 [![](images/image4-sml.png "プロジェクトのプロパティで PCL プロファイルを編集します。")](images/image4.png#lightbox)

コードは既に、PCL に追加した後、プロファイルが変更されると、そのコードは、新しく選択したプロファイルの一部ではない機能を参照している場合、ライブラリがコンパイルされなく可能性があります。


## <a name="summary"></a>まとめ

この記事で説明した方法: Visual Studio とポータブル クラス ライブラリを使用して Xamarin アプリケーションでの Visual Basic コードを使用します。 IOS および Android アプリに含まれる Visual Basic で記述されたコードを PCL に Visual Basic のコンパイルのにより、場合でも、Xamarin で Visual Basic が直接サポートされていません。

次のページには、ネイティブ モードまたは Xamarin.Forms アプリで Visual Basic.NET Pcl を使用する方法について説明します。

- [VB を使用する Xamarin.iOS および Xamarin.Android のネイティブ アプリのビルド](native-apps.md)
- [VB で Xamarin.Forms アプリの構築](xamarin-forms.md)


## <a name="related-links"></a>関連リンク

- [TaskyPortableVB (サンプル)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework (マイクロソフト) を使用したクロス プラットフォーム開発](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
