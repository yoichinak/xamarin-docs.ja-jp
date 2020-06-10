---
title: "アプリケーションのテーマを Xamarin.Forms 作成する" 説明: " Xamarin.Forms アプリケーションは、各テーマの ResourceDictionary を作成し、dynamicresource マークアップ拡張機能を使用してリソースを読み込むことによって、テーマをサポートします。"
ms. 製品: xamarin ms. assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 04/22/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="theming-a-xamarinforms-application"></a>アプリケーションのテーマを Xamarin.Forms 適用する

## <a name="theme-an-application"></a>[アプリケーションのテーマを適用する](theming.md)

Xamarin.Formsテーマは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 各テーマに対してを作成し、 `DynamicResource` マークアップ拡張機能を使用してリソースを読み込むことによって、アプリケーションに実装できます。

## <a name="respond-to-system-theme-changes"></a>[システム テーマの変更に対応する](system-theme-changes.md)

通常、デバイスには明るいテーマとダークテーマが含まれており、それぞれがオペレーティングシステムレベルで設定できるさまざまな外観設定を参照しています。 アプリケーションはこれらのシステムテーマを尊重し、システムテーマが変更されたときに直ちに応答する必要があります。
