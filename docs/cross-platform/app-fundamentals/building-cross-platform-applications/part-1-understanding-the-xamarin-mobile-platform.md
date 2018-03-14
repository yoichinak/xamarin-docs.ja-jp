---
title: "パート 1: Xamarin モバイル プラットフォームを理解します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d90cd8ae620f51d7ea9bac0945ccc7ec4b83bac4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2018
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>パート 1: Xamarin モバイル プラットフォームを理解します。

Xamarin プラットフォームは、iOS および Android 用アプリケーションを開発できる要素の数で構成されます。

-   **C# 言語**– 使い慣れた構文やジェネリック、LINQ およびタスク並列ライブラリなどの高度な機能を使用することができます。
-   **.NET の mono フレームワーク**– Microsoft の .NET framework の広範な機能のクロスプラット フォームの実装を提供します。
-   **コンパイラ**–、プラットフォームによっては (のネイティブ アプリが生成されます iOS) または統合の .NET アプリケーションおよびランタイム。 Android の場合)。 コンパイラは、退席中の解除に使用されるコードをリンクなどのモバイルの展開にも多くの最適化を実行します。
-   **IDE tools** : Mac および Windows で Visual Studio では、作成、ビルド、および Xamarin のプロジェクトを展開することができます。


さらに、基になる言語は c#、.NET framework では、ために、Windows Phone に展開することもコードを共有するプロジェクトを構造化できます。

 <a name="Under_the_Hood" />


## <a name="under-the-hood"></a>実際には

Xamarin には、C# の場合、アプリを作成して、複数のプラットフォーム間で同じコードを共有することができますが、各システム上での実装は大きく異なります。

 <a name="Compilation" />


## <a name="compilation"></a>コンパイル

C# ソースにより、ネイティブなアプリに各プラットフォームでの非常に異なる方法で。

-   **iOS** -C# の場合は、先行時間 (AOT) ARM アセンブリ言語をコンパイルします。 .NET framework が含まれて、アプリケーションのサイズを削減するリンク時に除去されている未使用のクラスを使用します。 一部の言語機能が利用できないように、Apple が、iOS でランタイム コードの生成を許可しない (を参照してください[Xamarin.iOS 制限](~/ios/internals/limitations.md))。
-   **Android** – c# は IL にコンパイルおよび MonoVM + JIT'ing と共にパッケージ化します。 フレームワークで使用されていないクラスは、リンク時に削除されます。 アプリケーションの実行によってサイドで Java/クリップアートを (Android ランタイム) と対話して JNI 経由でのネイティブ型 (を参照してください[Xamarin.Android 制限](~/android/internals/limitations.md))。
-   **Windows** – c# IL にコンパイルされ、組み込みのランタイムによって実行および Xamarin ツールは必要ありません。 Windows アプリケーションの設計ガイドの次の Xamarin が簡単 iOS および Android のコードを再利用します。
  ユニバーサル Windows プラットフォームをもなお、 **.NET ネイティブ**Xamarin.iOS' AOT コンパイルと同様に動作するオプションです。


リンカーのドキュメントに[Xamarin.iOS](~/ios/deploy-test/linker.md)と[Xamarin.Android](~/android/deploy-test/linker.md)コンパイル処理のこのパートに関する詳細情報を提供します。

ランタイム 'コンパイル': 動的にコードを生成する`System.Reflection.Emit`– 避ける必要があります。

Apple のカーネルにより、iOS デバイスでの動的コード生成、Xamarin.iOS、したがってコードでの実行時の出力は動作しません。 同様に、動的言語ランタイムの機能は、Xamarin ツールでは使用できません。

リフレクション機能が動作しないでください。 MonoTouch.Dialog の使用、リフレクション API)、コード生成がないだけです。

 <a name="Platform_SDK_Access" />


## <a name="platform-sdk-access"></a>プラットフォーム SDK のアクセス

Xamarin では、使い慣れた c# 構文を使用して簡単にアクセスできるプラットフォーム固有の SDK によって提供される機能を加えます。

-   **iOS** – Xamarin.iOS は c# から参照できるように名前空間と Apple の CocoaTouch SDK フレームワークを公開します。 たとえば、すべてのユーザー インターフェイス コントロールが含まれた UIKit framework できますに含まれて、単純な`using UIKit;`ステートメントです。
-   **Android** – を使用して、サポートされている SDK の任意の部分を参照できるように、名前空間として公開する Google の Android SDK を Xamarin.Android ステートメントなど`using Android.Views;`ユーザー インターフェイス コントロールにアクセスします。
-   **Windows** – Windows で Visual Studio を使用して Windows アプリを構築します。 プロジェクトの種類には、Windows フォーム、WPF、WinRT、およびユニバーサル Windows プラットフォーム (UWP) が含まれます。


 <a name="Seamless_Integration_for_Developers" />


## <a name="seamless-integration-for-developers"></a>開発者向けのシームレスな統合

Xamarin の利点は、フードの違いは、Xamarin.iOS および Xamarin.Android (Microsoft の Windows Sdk と組み合わせると) 提供する c# コードを記述する次の 3 つすべてのプラットフォーム間で再利用できるため、シームレスなエクスペリエンスです。

ビジネス ロジック、データベースの使用率、ネットワーク アクセス、およびその他の共通の関数を 1 回だけ書き込むおよび検索し、ネイティブ アプリケーションとして実行できるプラットフォーム固有のユーザー インターフェイスの基盤を提供し、プラットフォームごとに再利用します。

 <a name="Integrated_Development_Environment_(IDE)_Availability" />


## <a name="integrated-development-environment-ide-availability"></a>統合開発環境 (IDE) の可用性

Mac または Windows のいずれかに Visual Studio では、Xamarin 開発を実行できます。 対象にする、IDE を選択する、プラットフォームによって決定されます。

Windows アプリを開発する場合は Windows では、iOS、用にビルドするため、Android_と_Windows には、Visual Studio for Windows が必要です。 ただしプロジェクトや Mac で iOS アプリと Android アプリを作成できるため、共有コードは、Windows プロジェクトに後から追加でした、Windows と Mac のコンピューター間でファイルを共有することができます。

各プラットフォームの開発要件がで詳しく説明されている、[要件](~/cross-platform/get-started/requirements.md)ガイドです。


<a name="iOS" />

### <a name="ios"></a>iOS

IOS アプリケーションを開発するには、Mac コンピューター、macOS を実行している必要があります。 また、Visual Studio を使用して、記述して、Visual Studio で Xamarin を使用した iOS アプリケーションを展開することができます。 ただし、Mac では、ビルドとライセンス目的に必要なままです。

コンパイラとテスト用のシミュレーターを利用するには、Apple の Xcode IDE をインストールしてください。 独自のデバイスでテストできます[無料](~/ios/get-started/installation/device-provisioning/free-provisioning.md)がの配布 (アプリケーションを構築するには App Store) Apple の開発者プログラム (99 $USD/1 年) に参加する必要があります。 送信したり、アプリケーションを更新するたびに、必要がありますを確認および承認 Apple によってダウンロードして入手が行われる前にします。

Visual Studio IDE でコードを記述および画面レイアウトをプログラムによって構築されるか、Xamarin の iOS のいずれかの IDE でデザイナーを使用して編集します。

参照してください、 [Xamarin.iOS インストール ガイド](~/ios/get-started/installation/index.md)詳細な手順についての設定を取得します。

<a name="Android" />

### <a name="android"></a>Android

Android アプリケーションの開発では、Java と Android Sdk をインストールする必要があります。 これらは、コンパイラ、エミュレーターおよびビルド、配置とテストに必要なその他のツールを提供します。 Java、Google の Android SDK Xamarin のツールがすべてインストールして、次の構成で実行します。

-  Mac OS X 許可されて以降 (10.11 +) Visual studio for Mac
-  Windows 7 と上記の Visual Studio 2015 または 2017


Xamarin では、統一されたインストーラーを構成する、システム前提の Java と Android と Xamarin ツールは、画面のレイアウトをビジュアル デザイナー) を提供します。 参照してください、 [Xamarin.Android インストール ガイド](~/android/get-started/installation/index.md)詳細な手順についてです。

ビルドおよびただし、Google からのライセンスがなくても実際のデバイスでアプリケーションをテストするストアを使用してアプリケーションを配布する (Google Play、Amazon Barnes など&amp;かなった) 登録料がオペレーターに payable 可能性があります。 Google Play は、他の店舗がある Apple のような承認プロセス中に、アプリをすぐに発行されます。

 <a name="Windows_Phone" />


### <a name="windows"></a>Windows

WinForms、WPF では、(UWP) の Windows アプリは、Visual Studio で構築されます。 Xamarin を直接使用しません。 ただし、Windows、iOS および Android での c# コードを共有することができます。
Microsoft を参照してください[デベロッパー センター](https://developer.microsoft.com/)に、Windows 開発に必要なツールについて説明します。

 <a name="Creating_the_User_Interface_(UI)" />


## <a name="creating-the-user-interface-ui"></a>ユーザー インターフェイス (UI) の作成

Xamarin を使用する主な利点は、アプリケーションのユーザー インターフェイスが、OBJECTIVE-C または Java で記述されたアプリケーションと区別されているアプリを作成して、各プラットフォームでネイティブ コントロールを使用する (iOS および Android 用それぞれ)。

アプリの画面を作成するときにコード内のコントロールをレイアウトか、プラットフォームごとに使用できるデザイン ツールを使用して完全な画面を作成します。

 <a name="Programmatically_Create_Controls" />


### <a name="create-controls-programmatically"></a>コントロールをプログラムで作成します。

各プラットフォームは、コードを使用して画面に追加するインターフェイスのコントロールをユーザーにできます。 これを指定できます非常に時間がかかるように完成した設計を視覚化するが困難であることができますをハード コーディング ピクセルのコントロールの位置とサイズの座標が場合。

コントロールをプログラムで作成するには利点は、iOS のサイズを変更または iPhone と iPad の画面サイズを異なる方法でレンダリングするビューを構築するために特に。

 <a name="Visual_Designer" />


### <a name="visual-designer"></a>ビジュアルなデザイナー

各プラットフォームには、画面を視覚的にレイアウトするための別の方法があります。

-   **iOS** : Xamarin の iOS デザイナーでは、ドラッグ アンド ドロップ機能とプロパティ フィールドを使用してビューの構築が容易になります。 総称してこれらのビューは、ストーリー ボードを構成してでアクセスできる、**です。ストーリー ボード**プロジェクトに含まれているファイル。
-   **Android** – Xamarin には、Visual Studio 用の Android のドラッグ アンド ドロップ UI デザイナーが用意されています。 Android 画面レイアウトとして保存されます**です。AXML** Xamarin ツールを使用する場合のファイルです。
-   **Windows** – Microsoft には、Visual Studio と Blend でのドラッグ アンド ドロップ UI デザイナーが用意されています。 画面のレイアウトとしてを格納されます。XAML ファイル。


これらのスクリーン ショットは、各プラットフォームで利用可能な visual 画面デザイナーを表示します。

 [ ![](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png "これらのスクリーン ショットが各プラットフォームで利用可能な visual 画面デザイナーを表示します。")](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

常に視覚的に作成された要素は、コードで参照できます。

 <a name="User_Interface_Considerations" />


### <a name="user-interface-considerations"></a>ユーザー インターフェイスに関する考慮事項

クロス プラットフォーム アプリケーションを構築する Xamarin を使用する主な利点が利用できるようにユーザーに使い慣れたインターフェイスを提供するネイティブの UI ツールキットのです。 その他のすべてのネイティブ アプリケーションほど高速では、UI も実行します。

一部の UI 要素が複数のプラットフォーム間で作業 (たとえば、3 つすべてのプラットフォームを使用して同様のスクロール リスト コントロール) 'と思われる' 右側に、アプリケーションの順序で UI を活用してプラットフォーム固有のユーザー インターフェイス要素の該当する場合は。 プラットフォーム固有の UI 要素の例については、次のとおりです。

-   **iOS** – ソフトの戻るボタンを使用した階層のナビゲーションは画面の下部にあるタブします。
-   **Android** – ハードウェア/システムとソフトウェア ボタン、[操作] メニューの画面の上部にタブをバックアップします。
-   **Windows** – Windows アプリは、デスクトップ、(Microsoft Surface) などのタブレットと携帯電話で実行できます。 Windows 10 デバイスでは、バックアップを作成 ボタンとライブ タイルは、たとえばハードウェアがあります。


対象とするプラットフォームに関連のデザイン ガイドラインを読むことをお勧めします。

-   **iOS** – [Apple のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
-   **Android** – [Google のユーザー インターフェイスのガイドライン](http://developer.android.com/guide/practices/ui_guidelines/index.html)
-   **Windows** – [Windows のユーザー エクスペリエンスの設計ガイドライン](https://developer.microsoft.com/en-us/windows/design)


 <a name="Library_and_Code_Re-use" />


## <a name="library-and-code-re-use"></a>ライブラリとコードの再利用

Xamarin プラットフォームにより、既存の c# コードの再利用をすべてのプラットフォームだけでなく、各プラットフォームのネイティブで記述された、ライブラリの統合できます。

 <a name="C#_Source_and_Libraries" />


### <a name="c-source-and-libraries"></a>C# ソースおよびライブラリ

Xamarin の製品では、c# と .NET framework を使用するため、多数の (両方ともソースおよびプロジェクトを開く社内) 既存のソース コードは Xamarin.iOS または Xamarin.Android プロジェクトで再利用できます。 多くの場合、ソースできます単に追加 Xamarin ソリューションをされ、すぐに動作します。 サポートされていない .NET framework の機能を使用すると場合、によって調整が必要な場合があります。

Xamarin.iOS、Xamarin.Android で使用できる c# ソースの例を示します。 SQLite NET、NewtonSoft.JSON および SharpZipLib です。

 <a name="Objective-C_Bindings_+_Binding_Projects" />


### <a name="objective-c-bindings--binding-projects"></a>Objective C のバインディングとバインド プロジェクト

Xamarin と呼ばれるツールを提供する*btouch* Objective C Xamarin.iOS プロジェクトで使用するライブラリを許可するバインドを簡単に作成します。 参照してください、 [Objective C 型のバインド ドキュメント](~/cross-platform/macios/binding/binding-types-reference.md)これを行う方法の詳細。

Xamarin.iOS で使用できる Objective C ライブラリの例: RedLaser バーコードのスキャンでは、Google アナリティクス、PayPal の統合。 使用可能なオープン ソース Xamarin.iOS バインド[github](https://github.com/mono/monotouch-bindings)です。

 <a name=".jar_Bindings_+_Binding_Projects" />


### <a name="jar-bindings--binding-projects"></a>.jar バインド + バインド プロジェクト

Xamarin では、Xamarin.Android で既存の Java ライブラリの使用をサポートします。 参照してください、 [Java Library のドキュメントをバインド](~/android/platform/binding-java-library/index.md)を使用する方法の詳細についてはします。Xamarin.Android から JAR ファイルです。

使用可能なオープン ソース Xamarin.Android バインディング[github](https://github.com/mono/monodroid-bindings)です。

 <a name="C_via_PInvoke" />


### <a name="c-via-pinvoke"></a>PInvoke を使用して C

"プラットフォーム Invoke"技術 (P/invoke) は、マネージ コードにマネージ コード (c#) をネイティブ ライブラリでメソッドを呼び出すだけでなくネイティブ ライブラリを呼び出すためのサポートを使用できます。

たとえば、 [SQLite NET](https://github.com/praeclarum/sqlite-net)ライブラリは、次のようにステートメントを使用します。

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

これは、iOS および Android のネイティブの C 言語 SQLite 実装にバインドします。
既存の C API に慣れている開発者は、ネイティブの API にマップし、既存のプラットフォーム コードを利用する c# クラスのセットを構築できます。 ドキュメントがある[ネイティブ ライブラリとリンク](~/ios/platform/native-interop.md)Xamarin.iOS で、同じ原則が Xamarin.Android に適用します。

 <a name="C++_via_Cxxi" />


### <a name="c-via-cppsharp"></a>CppSharp 経由での C++

ある Miguel CXXI をについて説明します (と呼ばれるようになりました[CppSharp](https://github.com/mono/CppSharp)) で彼[ブログ](http://tirania.org/blog/archive/2011/Dec-19.html)です。 C++ ライブラリに直接バインドする代わりには、C ラッパーを作成し、P/invoke を使用してバインドするにです。

