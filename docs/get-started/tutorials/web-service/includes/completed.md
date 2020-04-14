---
ms.openlocfilehash: e0f7e89be5de282daf10f941d0f0b8d0175df81a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "71107288"
---
これでこのチュートリアルは完了です。ここでは以下の方法を学習しました。

> [!div class="checklist"]
>
> - NuGet パッケージ マネージャーを使用して、Newtonsoft.Json を Xamarin.Forms プロジェクトに追加する。
> - Web サービス クラスを作成する。
> - Web サービス クラスを使用する。

## <a name="next-steps"></a>次の手順

このチュートリアル シリーズでは、Xamarin.Forms でのモバイル アプリケーションの作成の基本について説明しました。 Xamarin.Forms 開発についてさらに学習する場合は、以下の機能を確認してください。

- Xamarin.Forms アプリケーションのユーザー インターフェイスを作成するために、主に 4 つのコントロール グループが使用されます。 詳細については、「[Controls Reference](~/xamarin-forms/user-interface/controls/index.md)」 (コントロールのリファレンス) を参照してください。
- データ バインディングは、2 つのオブジェクトのプロパティをリンクして、片方のプロパティでの変更が自動的にもう片方のプロパティに反映されるようにする手法です。 詳細については、[データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)に関するページを参照してください。
- Xamarin.Forms では、使用されるページの種類に応じて、さまざまなページ ナビゲーションのエクスペリエンスが提供されます。 詳細については、「[ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/index.md)」を参照してください。
- スタイルは、繰り返されるマークアップを減らすのに役立ち、アプリケーションの外観をより簡単に変更できるようにします。 詳しくは、「[Styling Xamarin.Forms Apps](~/xamarin-forms/user-interface/styles/index.md)」 (Xamarin.Forms アプリのスタイル設定) をご覧ください。
- XAML マークアップ拡張では、要素属性をリテラル テキスト文字列ではなく、ソースから設定できるようにすることで、XAML をより強力かつ柔軟なものにします。 詳細については、「[XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/index.md)」 (XAML マークアップ拡張) を参照してください。
- データ テンプレートでは、サポートされているビューでのデータの表現方法を定義する機能が提供されます。 詳細については、「[Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」 (データ テンプレート) を参照してください。
- 各ページ、レイアウト、およびビューは `Renderer` クラスを使用して、プラットフォームごとに異なる方法でレンダリングされます。その後、ネイティブ コントロールが作成され、画面に配置され、共有コードで指定された動作が追加されます。 開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 詳細については、「[Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (カスタム レンダラー) を参照してください。
- エフェクトでは、各プラットフォームのネイティブ コントロールのカスタマイズを可能にします。 エフェクトは、プラットフォーム固有のプロジェクトで [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) クラスをサブクラス化することによって作成され、適切な Xamarin.Forms コントロールに添付することによって使用されます。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (エフェクト) を参照してください。
- 共有コードはネイティブ機能に [`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスを介してアクセスできます。 詳細については、「[Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)」 (DependencyService を使用したネイティブ機能へのアクセス) を参照してください。

または、Charles Petzold 著の『[_Creating Mobile Apps with Xamarin.Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)』 (Xamarin.Forms でモバイル アプリを作成する) でも Xamarin.Forms の詳細を学習できます。 この書籍は、PDF またはさまざまな形式の電子ブックとして入手可能です。

## <a name="related-links"></a>関連リンク

- [WebServiceTutorial (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-tutorials-webservicetutorial/)
- [RESTful Web サービスの使用 (ガイド)](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Newtonsoft.Json NuGet パッケージ](https://www.nuget.org/packages/Newtonsoft.Json/)
