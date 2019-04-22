---
title: パート 1-Xamarin モバイル プラットフォームを理解します。
description: このドキュメントでは、コンパイル プロセス、プラットフォーム SDK へのアクセス、コードの共有、ユーザー インターフェイスの作成、ビジュアル デザイナー、および詳細を見て、大まかに言えば、Xamarin プラットフォームについて説明します。
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f5008d4986baa0575030e077b66b69ec0a4fad00
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854419"
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>パート 1-Xamarin モバイル プラットフォームを理解します。

Xamarin プラットフォームは、iOS および Android 用アプリケーションを開発するための要素数だけで構成されます。

-   **C#言語**– 使い慣れた構文やジェネリック、LINQ、およびタスク並列ライブラリなどの高度な機能を使用することができます。
-   **.NET の mono フレームワーク**– の広範な機能は、Microsoft .NET framework のクロスプラット フォームの実装を提供します。
-   **コンパイラ**–、プラットフォームによって (例: ネイティブ アプリを生成します。 iOS) または統合された .NET アプリケーション、およびランタイム (例。 Android の場合)。 コンパイラは、退席中に使用されていないコードをリンクなどのモバイルの展開のさまざまな最適化を実行します。
-   **IDE ツール**– Mac と Windows で Visual Studio では、作成、ビルド、および Xamarin プロジェクトを配置することができます。

さらに、基になる言語は C#、.NET framework では、ために、Windows Phone に展開することもコードを共有するプロジェクトを構造化できます。

## <a name="under-the-hood"></a>内部的には

Xamarin では、アプリで作成できます。 C#、各システム上での実装は非常に異なる、複数のプラットフォームで同じコードを共有します。

## <a name="compilation"></a>コンパイル

C#ソースは、各プラットフォームでさまざまな方法でネイティブ アプリにその方法を使用します。

-   **iOS** –C#は先行 of time (AOT) ARM アセンブリ言語にコンパイルします。 .NET framework は、アプリケーションのサイズを小さくリンク中に削除されている未使用のクラスと、含まれています。 一部の言語機能がないように、Apple が ios では、実行時コード生成を許可しない (を参照してください[Xamarin.iOS 制限](~/ios/internals/limitations.md))。
-   **Android** – C# は IL にコンパイルおよび MonoVM + JIT'ing と共にパッケージ化します。 フレームワークで使用されていないクラスは、リンク時に削除されます。 アプリケーションの実行によってサイドで Java/クリップアートを (Android ランタイム) と対話して JNI 経由でのネイティブ型 (を参照してください[Xamarin.Android 制限](~/android/internals/limitations.md))。
-   **Windows** – C# IL にコンパイルされ、組み込みのランタイムによって実行および Xamarin ツールは必要ありません。 Windows アプリケーションの設計ガイドの次の Xamarin が簡単 iOS および Android のコードを再利用します。
  ユニバーサル Windows プラットフォームがありますに注意してください、 **.NET ネイティブ**オプションは Xamarin.iOS の AOT コンパイルと同様に動作します。


リンカーのドキュメントを[Xamarin.iOS](~/ios/deploy-test/linker.md)と[Xamarin.Android](~/android/deploy-test/linker.md)コンパイル プロセスのこの部分について詳しく説明します。

'コンパイル': 動的にコードを生成するランタイム`System.Reflection.Emit`– 避ける必要があります。

Apple のカーネルは、iOS デバイスでの動的なコード生成を防止が Xamarin.iOS ではそのためのコードにその場での出力は機能しません。 同様に、Xamarin ツールを使用して、動的言語ランタイム機能を使用することはできません。

一部のリフレクション機能は機能 (例。 MonoTouch.Dialog を使用しますが、リフレクション API)、コード生成がないだけです。

## <a name="platform-sdk-access"></a>プラットフォーム SDK へのアクセス

Xamarin では、使い慣れた C# 構文を使用して簡単にアクセスできるプラットフォーム固有の SDK によって提供される機能を加えます。

-   **iOS** – Xamarin.iOS は C# から参照できるように名前空間と Apple の CocoaTouch SDK フレームワークを公開します。 たとえば、すべてのユーザー インターフェイス コントロールが含まれた UIKit framework できますに含まれて、単純な`using UIKit;`ステートメントです。
-   **Android** – を使用して、サポートされている SDK の任意の部分を参照できるように、名前空間として公開する Google の Android SDK を Xamarin.Android ステートメントなど`using Android.Views;`ユーザー インターフェイス コントロールにアクセスします。
-   **Windows** – Windows アプリは、Windows で Visual Studio を使用して構築されます。 プロジェクトの種類には、Windows フォーム、WPF、WinRT、およびユニバーサル Windows プラットフォーム (UWP) が含まれます。

## <a name="seamless-integration-for-developers"></a>開発者のためのシームレスな統合

Xamarin の利点は、フードの違いは、Xamarin.iOS および Xamarin.Android (Microsoft の Windows Sdk と組み合わせると) 提供する C# コードを記述する次の 3 つすべてのプラットフォーム間で再利用できるため、シームレスなエクスペリエンスです。

ビジネス ロジック、データベースの使用状況、ネットワーク アクセス、およびその他の共通の関数を 1 回だけ書き込むし、検索してネイティブ アプリケーションとして実行するには、プラットフォーム固有のユーザー インターフェイスの基礎を提供する各プラットフォームで再利用します。

## <a name="integrated-development-environment-ide-availability"></a>統合開発環境 (IDE) の可用性

Mac または Windows のいずれかの Visual Studio では、Xamarin 開発を実行できます。 対象にする、IDE は、プラットフォームによって決定されますを選択します。

Windows、iOS、用にビルドする Windows アプリを開発することができますのみであるため、Android_と_Windows には、Windows の Visual Studio が必要です。 プロジェクトと Mac で iOS および Android アプリを構築でき、共有コード後で Windows プロジェクトに追加できるように、Windows と Mac のコンピューター間でファイルを共有することができます。

各プラットフォームの開発要件がで詳しく説明されている、[要件](~/cross-platform/get-started/requirements.md)ガイド。

### <a name="ios"></a>iOS

IOS アプリケーションの開発には、Mac コンピューター、macOS を実行する必要があります。 記述して Visual Studio で Xamarin で iOS アプリケーションを展開する Visual Studio を使用することもできます。 ただし、Mac は引き続きビルドとライセンスの目的で必要です。

コンパイラとシミュレーターをテストするために提供する、Apple の Xcode IDE をインストールする必要があります。 独自のデバイスでテストできます[無料](~/ios/get-started/installation/device-provisioning/free-provisioning.md)がの配布 (例: アプリケーションを構築するには App Store) Apple の Developer Program ($99 USD 1 年あたり) を結合する必要があります。 送信したり、アプリケーションを更新するたびに、必要があるレビューおよび承認した Apple をダウンロードして入手が行われる前にします。

Visual Studio IDE でコードを記述し、画面レイアウトをプログラムによって構築または Xamarin の iOS のいずれかの IDE のデザイナーで編集できます。

参照してください、 [Xamarin.iOS インストール ガイド](~/ios/get-started/installation/index.md)詳細な手順について設定を取得します。

### <a name="android"></a>Android

Android アプリケーション開発では、Java と Android Sdk をインストールする必要があります。 これらは、コンパイラ、エミュレーターおよびビルド、配置およびテストに必要なその他のツールを提供します。 Java、Google の Android SDK および Xamarin のツールはすべてインストールして Windows、macOS で実行します。 次の構成が推奨されます。

- Windows 10 と Visual Studio 2019
- macOS で Mac を Visual Studio 2019 Mojave (10.11 以降)

Xamarin では、統合インストーラーは、システムを構成前提の Java、Android と Xamarin のツール (画面のレイアウトのビジュアル デザイナーを含む) を提供します。 参照してください、 [Xamarin.Android インストール ガイド](~/android/get-started/installation/index.md)詳しい手順についてはします。

ビルドおよびただし、Google からのライセンスがなくても実際のデバイスでアプリケーションをテストするストアを使用してアプリケーションを配布する (Google Play、Amazon Barnes など&amp;Noble) 登録料、演算子に支払う場合があります。 その他のストアには、Apple のような承認プロセスがありますが、Google Play は、アプリをすぐに、発行されます。

### <a name="windows"></a>Windows

WinForms、WPF では、(UWP) の Windows アプリは、Visual Studio で構築されます。 Xamarin を直接使用しません。 ただし、Windows、iOS および Android での C# コードを共有することができます。
Microsoft を参照してください。[デベロッパー センター](https://developer.microsoft.com/) Windows 開発に必要なツールについて学習します。

## <a name="creating-the-user-interface-ui"></a>ユーザー インターフェイス (UI) の作成

Xamarin を使用しての主な利点は、アプリケーション ユーザー インターフェイスが、OBJECTIVE-C または Java で記述されたアプリケーションと区別することはないアプリを作成する各プラットフォームのネイティブ コントロールを使用する (iOS と Android のそれぞれ)。

アプリの画面を構築するときにコード内のコントロールをレイアウトか、またはプラットフォームごとに使用できるデザイン ツールを使用して完全な画面を作成することができます。

### <a name="create-controls-programmatically"></a>コントロールをプログラムで作成します。

各プラットフォームでは、ユーザー インターフェイス コントロールにコードを使用して画面に追加できます。 完成した設計を視覚化することは難しいと非常に時間がかかるこのできるコントロールの位置とサイズをハード コーディングするピクセルを調整する場合。

コントロールを作成するプログラムでのサイズを変更または iPhone と iPad の画面サイズを異なる方法でレンダリングするビューを構築するための iOS に対する特にメリットにします。

### <a name="visual-designer"></a>ビジュアル デザイナー

各プラットフォームには、画面を視覚的にレイアウトするための別の方法があります。

- **iOS** – Xamarin の iOS Designer では、ドラッグ アンド ドロップ機能とプロパティ フィールドを使用してビューの構築が容易になります。 これらのビューがまとめて、ストーリー ボードを構成してでアクセスできる、**します。ストーリー ボード**プロジェクトに含まれているファイル。
- **Android** – Xamarin for Visual Studio Android のドラッグ アンド ドロップ UI デザイナーを提供します。 Android の画面のレイアウトとして保存されます**します。AXML** Xamarin ツールを使用する場合のファイルします。
- **Windows** – Microsoft には、Visual Studio と Blend でのドラッグ アンド ドロップ UI デザイナーが用意されています。 画面のレイアウトとしてを格納されます。XAML ファイル。

これらのスクリーン ショットは、各プラットフォームで利用可能な画面のビジュアル デザイナーを示します。

 [ ![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "これらのスクリーン ショットは、各プラットフォームで使用可能な visual 画面デザイナーを示します")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

すべてのケースでは、コードで視覚的に作成された要素を参照できます。

### <a name="user-interface-considerations"></a>ユーザー インターフェイスに関する考慮事項

Xamarin を使用して、クロス プラットフォーム アプリケーションを構築するの主な利点は、ユーザーに使い慣れたインターフェイスを提供するネイティブ UI ツールキットの長所を生かすことです。 その他のすべてのネイティブ アプリケーションとして高速では、UI も実行します。

複数のプラットフォームでいくつかの UI 要素が作業 (たとえば、3 つすべてのプラットフォームが同様のスクロール リスト コントロールを使用する) 'と' 右に、アプリケーションの順序で UI を活用してプラットフォーム固有のユーザー インターフェイス要素の適切な場合がします。 プラットフォーム固有の UI のメタファの例は次のとおりです。

- **iOS** – ソフトの戻るボタンでは、階層型ナビゲーションは画面の下部にあるタブします。
- **Android** – ハードウェア/システム ソフトウェア ボタン、[操作] メニューの画面の上部にタブをバックアップします。
- **Windows** – Windows アプリはデスクトップ、Microsoft Surface) などのタブレットとスマート フォンで実行できます。 Windows 10 デバイスでは、ハードウェア ボタンをクリックし、ライブ タイルをたとえばバックアップがあります。

対象とするプラットフォームに関連するデザインのガイドラインを確認することをお勧めします。

- **iOS** – [Apple のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
- **Android** – [Google のユーザー インターフェイス ガイドライン](https://developer.android.com/guide/practices/ui_guidelines/index.html)
- **Windows** – [Windows のユーザー エクスペリエンスのデザイン ガイドライン](https://developer.microsoft.com/windows/design)

## <a name="library-and-code-re-use"></a>ライブラリとコードの再利用

Xamarin プラットフォームにより、既存の C# コードの再利用をすべてのプラットフォームだけでなく、各プラットフォームのネイティブで記述された、ライブラリの統合できます。

### <a name="c-source-and-libraries"></a>C#ソースとライブラリ

Xamarin 製品を使用しているためC#と .NET framework では、(どちらもオープン ソース プロジェクトの社内) 既存のソース コードの多くは Xamarin.iOS または Xamarin.Android プロジェクトで再利用できます。 多くの場合、ソースできますだけとに追加 Xamarin ソリューションがただちに動作します。 サポートされていない .NET framework の機能が使用されている場合は、いくつかの修正が必要な場合があります。

例のC#Xamarin.iOS または Xamarin.Android で使用できるソースが含まれます。SQLite NET、NewtonSoft.JSON、および SharpZipLib です。

### <a name="objective-c-bindings--binding-projects"></a>OBJECTIVE-C バインディングとバインド プロジェクト

Xamarin と呼ばれるツールを提供する*btouch* Xamarin.iOS プロジェクトで使用する OBJECTIVE-C ライブラリのバインドの作成を支援します。 参照してください、 [OBJECTIVE-C の種類のバインドについてのドキュメント](~/cross-platform/macios/binding/binding-types-reference.md)これを行う方法の詳細について。

Xamarin.iOS で使用できる OBJECTIVE-C ライブラリの例は次のとおりです。RedLaser バーコードをスキャン、Google アナリティクスや PayPal の統合。 オープン ソースの Xamarin.iOS バインディングで使用可能な[github](https://github.com/mono/monotouch-bindings)します。

### <a name="jar-bindings--binding-projects"></a>.jar のバインドとバインド プロジェクト

Xamarin では、Xamarin.Android での既存の Java ライブラリを使用してサポートしています。 参照してください、 [Java ライブラリのドキュメントをバインド](~/android/platform/binding-java-library/index.md)を使用する方法の詳細についてはします。Xamarin.Android から JAR ファイルです。

Xamarin.Android のオープン ソースのバインドは[github](https://github.com/mono/monodroid-bindings)します。

### <a name="c-via-pinvoke"></a>PInvoke を使用して C

"プラットフォーム Invoke"技術 (P/invoke) は、マネージ コードにマネージ コード (C#) をネイティブ ライブラリでメソッドを呼び出すだけでなくネイティブ ライブラリを呼び出すためのサポートを使用できます。

たとえば、 [SQLite NET](https://github.com/praeclarum/sqlite-net)ライブラリは、このようなステートメントを使用します。

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

これは、iOS と Android のネイティブ C 言語の SQLite 実装にバインドします。
既存の C API に慣れている開発者は、ネイティブの API にマップし、既存のプラットフォーム コードを利用する C# クラスのセットを構築できます。 ドキュメントがある[ネイティブ ライブラリとリンク](~/ios/platform/native-interop.md)Xamarin.iOS で、同じ原則が Xamarin.Android に適用します。

### <a name="c-via-cppsharp"></a>CppSharp 経由での C++

Miguel CXXI をについて説明します (と呼ばれるようになりました[CppSharp](https://github.com/mono/CppSharp)) で彼[ブログ](https://tirania.org/blog/archive/2011/Dec-19.html)します。 C ライブラリに直接バインドする代わりには、C ラッパーを作成し、P/invoke を使用してバインドをすることです。
