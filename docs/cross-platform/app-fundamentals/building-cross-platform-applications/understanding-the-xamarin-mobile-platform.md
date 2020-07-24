---
title: パート 1-Xamarin Mobile Platform について
description: このドキュメントでは、コンパイルプロセス、プラットフォーム SDK アクセス、コード共有、ユーザーインターフェイスの作成、ビジュアルデザイナーなど、概要を示す Xamarin プラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: b010af4794c31e3dd3ccb85a81c9c05bcb6aec55
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930807"
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>パート 1-Xamarin Mobile Platform について

Xamarin プラットフォームは、iOS および Android 用のアプリケーションの開発を可能にするいくつかの要素で構成されています。

- **C# 言語**–使い慣れた構文と、ジェネリック、LINQ、並列タスクライブラリなどの高度な機能を使用できます。
- **Mono .net framework** – Microsoft .net framework の広範な機能をクロスプラットフォームで実装します。
- **コンパイラ**–プラットフォームによっては、ネイティブアプリを作成します (例: iOS) または統合された .NET アプリケーションとランタイム ( Android)。 また、コンパイラは、使用されなくなったコードのリンクなど、モバイルデプロイの多くの最適化を実行します。
- **IDE ツール**– Mac と Windows の Visual Studio を使用すると、Xamarin プロジェクトを作成、ビルド、配置できます。

また、基になる言語は .NET framework を使用した C# であるため、Windows Phone に配置できるコードを共有するようにプロジェクトを構成することもできます。

## <a name="under-the-hood"></a>しくみ

Xamarin では、C# でアプリを作成し、複数のプラットフォームで同じコードを共有することができますが、各システムの実際の実装は大きく異なります。

## <a name="compilation"></a>コンパイル

C# ソースは、プラットフォームごとに異なる方法でネイティブアプリに変換します。

- **iOS** – C# は、ARM アセンブリ言語にコンパイルされた事前 (AOT) です。 .NET framework が含まれています。リンク中に未使用のクラスを削除して、アプリケーションのサイズを小さくします。 Apple では、iOS でランタイムコードを生成することはできないため、一部の言語機能は使用できません (「 [Xamarin. iOS の制限事項](~/ios/internals/limitations.md)」を参照してください)。
- **Android** – C# は IL にコンパイルされ、モノ Vm + JIT'ing を使用してパッケージ化されます。 フレームワーク内の未使用のクラスは、リンク時に除去されます。 アプリケーションは Java/ART (Android ランタイム) とサイドバイサイドで実行され、JNI を使用してネイティブの型と対話します (「 [Xamarin の制限事項](~/android/internals/limitations.md)」を参照してください)。
- **Windows** – C# は IL にコンパイルされ、組み込みのランタイムによって実行されるため、Xamarin ツールは必要ありません。 Xamarin のガイダンスに従って Windows アプリケーションを設計すると、iOS と Android でコードを簡単に再利用できるようになります。
  ユニバーサル Windows プラットフォームには、Xamarin. iOS ' AOT のコンパイルと同様に動作する **.NET ネイティブ**オプションもあります。

[Xamarin. iOS](~/ios/deploy-test/linker.md)および[xamarin. Android](~/android/deploy-test/linker.md)のリンカードキュメントでは、コンパイルプロセスのこの部分について詳しく説明しています。

ランタイムの ' コンパイル ' –を使用してコードを動的に生成する `System.Reflection.Emit` ことは避ける必要があります。

Apple のカーネルでは、iOS デバイスでの動的なコード生成を防ぐことができます。そのため、Xamarin でコードを出力することはできません。 同様に、動的言語ランタイム機能を Xamarin ツールで使用することはできません。

一部のリフレクション機能は機能します ( Monotouch.dialog は、コード生成ではなく、リフレクション API に対して使用します。

## <a name="platform-sdk-access"></a>プラットフォーム SDK アクセス

Xamarin では、プラットフォーム固有の SDK によって提供される機能が、使い慣れた C# 構文で簡単にアクセスできるようになります。

- **ios** – Xamarin。 ios は、C# から参照できる名前空間として Apple の CocoaTouch SDK フレームワークを公開します。 たとえば、すべてのユーザーインターフェイスコントロールを含む UIKit フレームワークは、単純なステートメントに含めることができ `using UIKit;` ます。
- **Android** – Xamarin android は、Google の Android SDK を名前空間として公開します。そのため、ユーザーインターフェイスコントロールにアクセスするなど、サポートされている SDK の任意の部分を using ステートメントで参照でき `using Android.Views;` ます。
- **Windows – windows**アプリは、windows 上の Visual Studio を使用してビルドされます。 プロジェクトの種類には、Windows フォーム、WPF、WinRT、ユニバーサル Windows プラットフォーム (UWP) などがあります。

## <a name="seamless-integration-for-developers"></a>開発者のためのシームレスな統合

Xamarin の利点は、内部で違いがあっても (Microsoft の Windows Sdk と組み合わせて)、3つのすべてのプラットフォームで再利用できる C# コードをシームレスに記述できることです。

ビジネスロジック、データベースの使用状況、ネットワークアクセス、およびその他の一般的な機能は、各プラットフォームで一度記述して再利用できます。これにより、ネイティブアプリケーションとして検索して実行するプラットフォーム固有のユーザーインターフェイスの基盤が提供されます。

## <a name="integrated-development-environment-ide-availability"></a>統合開発環境 (IDE) の可用性

Xamarin の開発は、Mac または Windows の Visual Studio で行うことができます。 選択する IDE は、対象とするプラットフォームによって決定されます。

Windows アプリは Windows 上でのみ開発できるため、iOS、Android、_および_windows 用にビルドするには、Visual Studio for Windows が必要です。 ただし、Windows コンピューターと Mac コンピューターの間でプロジェクトとファイルを共有できるので、iOS アプリと Android アプリを Mac 上に作成し、共有コードを後で Windows プロジェクトに追加することができます。

各プラットフォームの開発要件については、[要件](~/cross-platform/get-started/requirements.md)ガイドで詳しく説明されています。

### <a name="ios"></a>iOS

IOS アプリケーションを開発するには、macOS を実行する Mac コンピューターが必要です。 また、visual Studio を使用して、Visual Studio で Xamarin を使用して iOS アプリケーションを作成し、デプロイすることもできます。 ただし、Mac はビルドとライセンスのために必要です。

テスト用のコンパイラとシミュレーターを提供するには、Apple の Xcode IDE がインストールされている必要があります。 独自のデバイスでのテストを[無料](~/ios/get-started/installation/device-provisioning/free-provisioning.md)で行うことができますが、配布用のアプリケーションを構築することができます (例として、 アプリストア) は、Apple の開発者プログラム (1 年あたり $99 米国ドル) に参加する必要があります。 アプリケーションを送信または更新するたびに、ユーザーがダウンロードできるようになる前に、Apple によって確認および承認される必要があります。

コードは Visual Studio IDE を使用して記述され、画面レイアウトは、プログラムでビルドするか、またはいずれかの IDE で Xamarin の iOS デザイナーを使用して編集することができます。

セットアップ方法の詳細については、 [Xamarin のインストールガイド](~/ios/get-started/installation/index.md)を参照してください。

### <a name="android"></a>Android

Android アプリケーションの開発には、Java および Android Sdk をインストールする必要があります。 これらは、コンパイラ、エミュレーター、およびビルド、配置、およびテストに必要なその他のツールを提供します。 Java、Google の Android SDK および Xamarin のツールはすべて、Windows および macOS にインストールして実行できます。 次の構成をお勧めします。

- Visual Studio 2019 を使用した Windows 10
- Visual Studio 2019 for Mac を使用した macOS Mojave (10.11 +)

Xamarin には、前提条件となる Java、Android、および Xamarin ツール (画面レイアウト用のビジュアルデザイナーを含む) を使用してシステムを構成する統合インストーラーが用意されています。 詳細な手順については、 [Xamarin のインストールガイド](~/android/get-started/installation/index.md)を参照してください。

Google のライセンスを使用せずに、実際のデバイスでアプリケーションをビルドしてテストすることができます。ただし、ストア (Google Play、Amazon、Barnes 立派など) を介してアプリケーションを配布するには、 &amp; 登録料がオペレーターに支払われる可能性があります。 Google Play は、アプリをすぐに発行します。他のストアには、Apple のと同様の承認プロセスがあります。

### <a name="windows"></a>Windows

Windows アプリ (WinForms、WPF、UWP) は、Visual Studio でビルドされます。 これらのユーザーは、Xamarin を直接使用しません。 ただし、C# コードは、Windows、iOS、および Android 間で共有できます。
Windows 開発に必要なツールの詳細については、Microsoft の[デベロッパーセンター](https://developer.microsoft.com/)を参照してください。

## <a name="creating-the-user-interface-ui"></a>ユーザー インターフェイス (UI) の作成

Xamarin を使用する主な利点は、アプリケーションのユーザーインターフェイスは、各プラットフォームでネイティブコントロールを使用し、目的が C または Java で記述されたアプリケーションと区別できないアプリを作成することです (iOS と Android 向けにそれぞれ)。

アプリで画面を作成する場合は、コードでコントロールをレイアウトするか、各プラットフォームで使用可能なデザインツールを使用して完全な画面を作成することができます。

### <a name="create-controls-programmatically"></a>プログラムによるコントロールの作成

各プラットフォームでは、コードを使用してユーザーインターフェイスコントロールを画面に追加できます。 制御位置とサイズに対してピクセル座標をハードコーディングする場合、完成したデザインを視覚化することが困難になる可能性があるため、この処理には非常に時間がかかることがあります。

プログラムを使用してコントロールを作成することには利点があります。ただし、iPhone と iPad の画面サイズによってサイズ変更や表示が異なるビューを作成する場合は特に iOS で特に便利です。

### <a name="visual-designer"></a>ビジュアルデザイナー

各プラットフォームには、画面を視覚的にレイアウトするためのさまざまな方法があります。

- **ios** : Xamarin の ios デザイナーでは、ドラッグアンドドロップ機能とプロパティフィールドを使用したビューの作成が容易になります。 これらのビュー全体がストーリーボードを構成し、でアクセスでき**ます。** プロジェクトに含まれているストーリーボードファイル。
- **Android** – Xamarin には、Visual Studio 用の android ドラッグアンドドロップ UI デザイナーが用意されています。 Android の画面レイアウトはとして保存され**ます。** Xamarin ツールを使用する場合の AXML ファイル。
- **Windows** – Microsoft では、Visual Studio と Blend でドラッグアンドドロップの UI デザイナーを提供しています。 画面レイアウトはとして格納されます。XAML ファイル。

これらのスクリーンショットは、各プラットフォームで使用できるビジュアルスクリーンデザイナーを示しています。

 [![これらのスクリーンショットは、各プラットフォームで使用できるビジュアルスクリーンデザイナーを示しています。](understanding-the-xamarin-mobile-platform-images/designer-all1.png)](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

どのような場合でも、視覚的に作成した要素をコード内で参照できます。

### <a name="user-interface-considerations"></a>ユーザーインターフェイスに関する考慮事項

Xamarin を使用してクロスプラットフォームアプリケーションを構築する主な利点は、ネイティブ UI ツールキットを利用して、使い慣れたインターフェイスをユーザーに提示できることです。 UI も、他のネイティブアプリケーションと同じように高速に実行されます。

一部の UI メタファは複数のプラットフォームで機能します (たとえば、3つのすべてのプラットフォームで同様のスクロールリストコントロールが使用されます) が、アプリケーションで適切に動作させるためには、UI はプラットフォーム固有のユーザーインターフェイス要素を適切に利用する必要があります。 プラットフォーム固有の UI メタファの例を次に示します。

- **iOS** – [ソフト戻る] ボタンを使用した階層ナビゲーション、画面の下部にタブ。
- **Android** – [ハードウェア/システム]-ソフトウェアの [戻る] ボタン、[アクション] メニュー、画面の上部にあるタブ。
- **Windows** – windows アプリはデスクトップ、タブレット (Microsoft Surface など)、携帯電話で実行できます。 Windows 10 デバイスには、ハードウェアの [戻る] ボタンとライブタイルが含まれている場合があります (たとえば、)。

ターゲットとするプラットフォームに関連するデザインガイドラインを読むことをお勧めします。

- **iOS** – [Apple のヒューマンインターフェイスガイドライン](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
- **Android** – [Google のユーザーインターフェイスガイドライン](https://developer.android.com/guide/practices/ui_guidelines/index.html)
- **Windows** – [Windows のユーザーエクスペリエンス設計ガイドライン](https://developer.microsoft.com/windows/design)

## <a name="library-and-code-re-use"></a>ライブラリとコードの再利用

Xamarin プラットフォームでは、すべてのプラットフォームで既存の C# コードを再利用できるだけでなく、プラットフォームごとにネイティブに作成されたライブラリを統合することもできます。

### <a name="c-source-and-libraries"></a>C# のソースとライブラリ

Xamarin 製品では C# と .NET framework が使用されるため、既存のソースコードの多く (オープンソースプロジェクトと社内プロジェクトの両方) を Xamarin. iOS プロジェクトまたは Xamarin Android プロジェクトで再利用できます。 多くの場合、ソースは Xamarin ソリューションに簡単に追加でき、すぐに機能します。 サポートされていない .NET framework の機能が使用されている場合は、いくつかの調整が必要になることがあります。

Xamarin. iOS または Xamarin. Android で使用できる C# ソースの例として、SQLite-NET、NewtonSoft.JSON、SharpZipLib があります。

### <a name="objective-c-bindings--binding-projects"></a>目的-C バインド + バインドプロジェクト

Xamarin には*btouch*というツールが用意されています。これは、Xamarin. iOS プロジェクトで目的の C ライブラリを使用できるようにするバインディングを作成するのに役立ちます。 この方法の詳細については、「[バインディングの目的-C の型](~/cross-platform/macios/binding/binding-types-reference.md)」のドキュメントを参照してください。

Xamarin で使用できる目的の C ライブラリの例としては、RedLaser バーコードスキャン、Google アナリティクス、PayPal 統合などがあります。 オープンソースの Xamarin. iOS バインドは、 [github](https://github.com/mono/monotouch-bindings)から入手できます。

### <a name="jar-bindings--binding-projects"></a>.jar バインディングとバインドプロジェクト

Xamarin では、xamarin Android の既存の Java ライブラリの使用をサポートしています。 の使用方法の詳細については、 [Java ライブラリのバインドに関するドキュメント](~/android/platform/binding-java-library/index.md)を参照してください。Xamarin. Android からの JAR ファイル。

オープンソースの Xamarin. Android バインドは、 [github](https://github.com/mono/monodroid-bindings)から入手できます。

### <a name="c-via-pinvoke"></a>PInvoke を使用した C

"プラットフォーム呼び出し" テクノロジ (P/Invoke) を使用すると、マネージコード (C#) でネイティブライブラリのメソッドを呼び出したり、マネージコードにコールバックするためのネイティブライブラリをサポートしたりできます。

たとえば、 [SQLite-NET](https://github.com/praeclarum/sqlite-net)ライブラリでは、次のようなステートメントを使用します。

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

これは、iOS および Android のネイティブ C 言語の SQLite 実装にバインドされます。
既存の C API を使い慣れている開発者は、C# クラスのセットを構築して、ネイティブ API にマップし、既存のプラットフォームコードを利用することができます。 Xamarin で[ネイティブライブラリをリンク](~/ios/platform/native-interop.md)するためのドキュメントがあります。 iOS でも同様の原則が適用されます。

### <a name="c-via-cppsharp"></a>CppSharp による C++

Miguel は、彼の[ブログ](https://tirania.org/blog/archive/2011/Dec-19.html)で CXXI ( [cppsharp](https://github.com/mono/CppSharp)と呼ばれる) について説明しています。 C++ ライブラリに直接バインドする代わりに、C ラッパーを作成し、P/Invoke を使用してそれにバインドすることもできます。
