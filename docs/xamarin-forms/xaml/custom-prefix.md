---
title: 「xaml 名前空間で推奨されるプレフィックス Xamarin.Forms 」「説明:」では、XmlnsPrefixAttribute クラスを使用して、xaml 名前空間に関連付ける推奨プレフィックスを xaml の使用のためにコントロールの作成者が指定することができます。
ms. 製品: xamarin ms. assetid: 7B315BEC-7A35-48F4-A9C7-EF40255E95FF: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 02/28/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>XAML 名前空間で推奨されるプレフィックスXamarin.Forms

クラスは、xaml の `XmlnsPrefixAttribute` 使用のために xaml 名前空間に関連付ける推奨プレフィックスを指定するために、コントロールの作成者が使用できます。 このプレフィックスは、XAML へのオブジェクトツリーのシリアル化をサポートする場合や、XAML 編集機能を持つデザイン環境と対話する場合に役立ちます。 次に例を示します。

- XAML テキストエディターでは、 `XmlnsPrefixAttribute` 初期の xaml 名前空間マッピングのヒントとしてを使用でき `xmlns` ます。
- XAML デザイン環境では、を使用して、 `XmlnsPrefixAttribute` オブジェクトをツールボックスからビジュアルデザインサーフェイスにドラッグするときに、xaml にマッピングを追加できます。

推奨される名前空間プレフィックスは、コンストラクターを使用してアセンブリレベルで定義する必要があります。コンストラクターには、 `XmlnsPrefixAttribute` XAML 名前空間の識別子を指定する文字列と、推奨プレフィックスを指定する文字列の2つの引数が必要です。

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

プレフィックスは通常、XAML 名前空間から取得したすべてのシリアル化された要素に適用されるため、プレフィックスは短い文字列を使用する必要があります。 したがって、プレフィックス文字列の長さは、シリアル化された XAML 出力のサイズに大きな影響を与える可能性があります。

> [!NOTE]
> 1つの `XmlnsPrefixAttribute` アセンブリに複数のを適用できます。 たとえば、複数の XAML 名前空間の型を定義するアセンブリがある場合、各 XAML 名前空間に対して異なるプレフィックス値を定義できます。

## <a name="related-links"></a>関連リンク

- [XAML カスタム名前空間スキーマ](custom-namespace-schemas.md)
- [での XAML 名前空間Xamarin.Forms](namespaces.md)
