---
title: より高度な Xamarin Android サポート v4/v13 NuGet パッケージ
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: c74cac6a6d669385855999a565711a3fdc85f3b7
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019539"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>より高度な Xamarin Android サポート v4/v13 NuGet パッケージ

## <a name="about-the-android-support-libraries"></a>Android サポート ライブラリについて

Google は、古いバージョンの Android でも新しい機能を使用できるようにするためのサポート ライブラリを作成しています。 通常、サポート ライブラリには、対応する最も低い Android API レベルであるバージョン番号が名前に含まれています (例: Support-v4 は API レベル 4 以降でのみサポートされます。 詳細については、こちらの [Stack Overflow のディスカッション](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)を参照してください)。 

2 つのサポートライブラリ (`Support-v4` と `Support-v13`) を同じアプリで同時に使用することはできません。つまり、それらは相互に排他的です。 これは、`Support-v13` には実際には `Support-v4` のすべての型と実装が含まれているためです。 同じプロジェクト内で両方を参照しようとすると、型の重複エラーが発生します。

## <a name="problems-with-referencing"></a>参照に関する問題

`Support-v4` は非常に普及しているため、多数のサード パーティ製のライブラリがそれに依存しています。 Support-v13 への依存が選択されている可能性もありますが、サード パーティ製のライブラリを使用しているアプリに 4 に至るまでの API レベルをサポートするオプションが与えられるため、_v4_ に依存するほうが一般的です。

Xamarin のサード パーティ製のライブラリで `Support-v4` にバインドされている `Xamarin.Android.Support.v4.dll` が参照される場合、このライブラリを使用するアプリも、`Xamarin.Android.Support.v4.dll` を参照する必要があります。 これは、同じアプリで、`Support-v13` にバインドされている `Xamarin.Android.Support.v13.dll` の機能の一部も使用したい場合に問題になります。 両方のバインドを参照すると、型重複エラーが発生します。

## <a name="type-forwarded-v4-binding-assembly"></a>型転送される v4 バインド アセンブリ

この問題を避けるために、特別な `Xamarin.Android.Support.v4.dll` アセンブリが作成されています。これには実装はありませんが、すべての `Support-v4` 型を `Xamarin.Android.Support.v13.dll` アセンブリ内の実装に転送する `[assembly: TypeForwardedTo (..)]` 属性があります。

これは、開発者がアプリ内でこの "_型転送_" アセンブリを参照すると、サード パーティ製のライブラリによる `Xamarin.Android.Support.v4.dll` への参照に対応できることに加え、アプリ内で `Xamarin.Android.Support.v13.dll` も使用できることを意味します。

## <a name="nuget-assistance"></a>NuGet のサポート

開発者は、必要な適切な参照を手動で追加できますが、NuGet パッケージのインストール時に、NuGet を使用して、適切なアセンブリ (通常の _v4_ のバインドまたは型転送 _v4_ アセンブリ) を選択できます。

このため、`Xamarin.Android.Support.v4` NuGet パッケージには、次のロジックが含まれるようになりました。

アプリのターゲットが API レベル 13 (Gingerbread 3.2) 以降の場合:

* 依存関係として `Xamarin.Android.Support.v13` NuGet が自動的に追加されます
* プロジェクトで "_型転送_" `Xamarin.Android.Support.v4.dll` が参照されます

アプリのターゲットが API レベル 13 よりも低い場合は、プロジェクトで通常の `Xamarin.Android.Support.v4.dll` のバインドが参照されます。

## <a name="do-i-have-to-use-support-v13"></a>Support-v13 を使用する必要があるか

アプリのターゲットが API レベル 13 以降であるときに、`Xamarin Android Support-v4` NuGet パッケージを使用することを選択した場合、`Xamarin Android Support v13` NuGet パッケージは必須の依存関係になります。

アプリのサイズの非常にわずかな増加 (2 つの .jar ファイルの違いは 17 KB) は互換性にとって十分価値のあるものであり、それによって発生する問題はほとんどありません。

ターゲットが API レベル 13 以降のアプリで `Support-v4` をどうしても使用したければ、いつでも `.nupkg` を手動でダウンロードして展開し、そのアセンブリを参照できます。
