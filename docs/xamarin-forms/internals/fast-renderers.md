---
title: " Xamarin.Forms Fast レンダラー" の説明: "この記事では、 Xamarin.Forms 結果として得られるネイティブコントロール階層をフラット化することにより、Android でのコントロールのインフレとレンダリングのコストを削減する高速レンダラーを紹介します。"
ms. 製品: xamarin ms. assetid: 095287 f2-d891-4f3c-be02-fb7d195a481a ms. テクノロジ: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 05/28/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms 高速レンダラー

従来、Android での元のコントロールレンダラーのほとんどは、次の2つのビューで構成されています。

- やなどのネイティブコントロール `Button` `TextView` 。
- `ViewGroup`一部のレイアウト作業、ジェスチャ処理、およびその他のタスクを処理するコンテナー。

ただし、この方法では、論理コントロールごとに2つのビューが作成され、より多くのメモリを必要とするより複雑なビジュアルツリーが生成され、画面に表示される処理が増えます。

[高速レンダラー] を指定すると、コントロールのインフレとレンダリングのコストを Xamarin.Forms 1 つのビューに削減できます。 したがって、2つのビューを作成してビューツリーに追加するのではなく、1つだけを作成します。 これにより、より少ないオブジェクトを作成することによってパフォーマンスが向上します。これは、より複雑なビューツリーで、メモリ使用量が少なくなります (これにより、ガベージコレクションの一時停止も少なくなります)。

Android 上のでは、次のコントロールに対して高速レンダラーを使用でき Xamarin.Forms ます。

- [`Button`](xref:Xamarin.Forms.Button)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`MediaElement`](xref:Xamarin.Forms.MediaElement)

機能的には、これらの高速レンダラーは従来のレンダラーとは異なります。 Xamarin.Forms4.0 以降では、を対象 `FormsAppCompatActivity` とするすべてのアプリケーションは、これらの高速レンダラーを既定で使用します。 やなど、すべての新しいコントロールのレンダラーでは、 [`ImageButton`](xref:Xamarin.Forms.ImageButton) [`CollectionView`](xref:Xamarin.Forms.CollectionView) 高速レンダラーアプローチを使用します。

高速レンダラーを使用する場合のパフォーマンスの向上は、レイアウトの複雑さに応じて、アプリケーションごとに異なります。 たとえば、何千ものデータを含むをスクロールするときに、x2 のパフォーマンスを向上させることができます。この場合、各行 [`ListView`](xref:Xamarin.Forms.ListView) のセルは高速レンダラーを使用するコントロールによって作成されるため、スクロールが滑らかになります。

> [!NOTE]
> 従来のレンダラーと同じ方法を使用して、高速レンダラー用にカスタムレンダラーを作成できます。 詳細については、「[Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (カスタム レンダラー) を参照してください。

## <a name="backwards-compatibility"></a>下位互換性

高速レンダラーは、次の方法でオーバーライドできます。

1. を呼び出す前に、次のコード行をクラスに追加して、従来のレンダラーを有効にし `MainActivity` `Forms.Init` ます。

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. レガシレンダラーを対象とするカスタムレンダラーを使用します。 既存のカスタムレンダラーは、従来のレンダラーと引き続き機能します。
1. さまざまなレンダラーを使用する、などの別のを指定し `View.Visual` `Material` ます。 素材の詳細については、「 [ Xamarin.Forms 素材ビジュアル](~/xamarin-forms/user-interface/visual/material-visual.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
