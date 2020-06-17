---
title: "Xamarin.Forms シェルの概要" の説明:"Xamarin.Formsシェルには、一般的なナビゲーション ユーザー エクスペリエンス、URI ベースのナビゲーション体系、統合された検索ハンドラーなど、ほとんどのアプリケーションが必要とする基本的な機能が用意されています。"
ms.prod: xamarin ms.assetid:4604DCB5-83DA-458A-8B02-6508A740BE0E ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:09/20/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-shell-introduction"></a>Xamarin.Forms シェルの概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms シェルでは、次に示すようなほとんどのモバイル アプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さが軽減されます。

- 1 つの場所にアプリケーションの視覚階層を示す機能。
- 一般的なナビゲーション ユーザー インターフェイス。
- アプリケーション内の任意のページへ移動できる URI ベースのナビゲーション体系。
- 統合された検索ハンドラー

さらに、シェル アプリケーションには、レンダリング速度が速くなり、メモリ使用量が減少するという利点があります。

> [!IMPORTANT]
> 既存のアプリケーションではシェルを採用することができ、ナビゲーション、パフォーマンス、および拡張性の強化をすぐに活用できます。

## <a name="platform-support"></a>プラットフォームのサポート

Xamarin.Forms シェルは、iOS および Android 上では完全に使用できますが、ユニバーサル Windows プラットフォーム (UWP) 上では一部のみを使用できます。 さらに、シェルは現在 UWP で試験段階であり、使用するには、`Forms.Init` を呼び出す前に、次のコード行を UWP プロジェクトの `App` クラスに追加する必要があります。

```csharp
global::Xamarin.Forms.Forms.SetFlags("Shell_UWP_Experimental");
```

Xamarin.Forms ソリューションに UWP プロジェクトを追加する方法の詳細については、「[Windows プロジェクトのセットアップ](~/xamarin-forms/platform/windows/installation/index.md)」を参照してください。

## <a name="shell-navigation-experience"></a>シェルのナビゲーション エクスペリエンス

シェルでは、ポップアップとタブに基づいて、厳格なナビゲーション エクスペリエンスを提供しています。 シェル アプリケーションでのナビゲーションの最上位レベルは、アプリケーションのナビゲーション要件に応じて、ポップアップまたは下部のタブ バーのいずれかです。 次の例は、ナビゲーションの最上位レベルがポップアップであるアプリケーションを示しています。

[![iOS および Android 上のシェル フライアウトのスクリーンショット](introduction-images/flyout.png "シェル ポップアップ")](introduction-images/flyout-large.png#lightbox "シェル ポップアップ")

ポップアップ項目を選択すると、項目が選択され表示された状態で、下部のタブが表示されます。

[![iOS および Android 上のシェルの下部タブのスクリーンショット](introduction-images/monkeys.png "シェルの下部タブ")](introduction-images/monkeys-large.png#lightbox "シェルの下部タブ")

> [!NOTE]
> ポップアップが開いていないときは、下部のタブ バーはアプリケーション内の上位レベルのナビゲーションと見なすことができます。

各タブには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) が表示されます。 ただし、下部のタブに複数のページが含まれる場合は、上部のタブ バーからページをナビゲートできます。

[![iOS および Android 上のシェルの上部タブのスクリーンショット](introduction-images/cats.png "シェルの上部タブ")](introduction-images/cats-large.png#lightbox "シェルの上部タブ")

各タブ内で、追加の [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに移動できます。

[![iOS および Android 上のシェル ページ ナビゲーションのスクリーンショット](introduction-images/cat-details.png "シェル アプリ ナビゲーション")](introduction-images/cat-details-large.png#lightbox "シェル アプリ ナビゲーション")

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
