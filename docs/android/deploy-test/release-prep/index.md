---
title: リリースに向けてアプリケーションを準備する
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8145B3-FCF1-4649-8C6A-49672DDA4159
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2018
ms.openlocfilehash: baaa40bc89a1ca6728189563c8350f9c9f011762
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="preparing-an-application-for-release"></a>リリースに向けてアプリケーションを準備する


アプリケーションがコード化され、テストされたら、配信のためにパッケージを用意する必要があります。 このパッケージ準備における最初の作業は、リリース用のアプリケーションをビルドすることです。中心的な作業は、いくつかのアプリケーション属性を設定することです。

次の手順で、リリースに向けてアプリをビルドします。

-   **[アプリケーション アイコンを指定する](#Specify_the_Application_Icon)** &ndash; Xamarin.Android アプリケーションごとにアプリケーション アイコンを指定する必要があります。 厳密には必須ではありませんが、Google Play など、一部のマーケットでは要求されます。

-   **[アプリケーションのバージョン管理](#Versioning)** &ndash; この手順では、バージョン管理情報を初期化または更新します。 これは将来、アプリケーションを更新するために、また、インストールしているアプリケーションのバージョンをユーザーに知らせるために重要です。

-   **[APK を圧縮する](#shrink_apk)** &ndash; マネージ コードで Xamarin.Android リンカーを使用し、Java バイトコードで ProGuard を使用することで、最終的な APK のサイズを大幅に縮小できます。

-   **[アプリケーションを保護する](#protect_app)** &ndash; ユーザーや攻撃者によるアプリケーションのデバッグ、改ざん、またはリバース エンジニアリングを防ぐために、マネージ コードを難読化し、アンチデバッグとおよび改ざん防止を追加し、ネイティブ コンパイルを使用します。

-   **[パッケージング プロパティを設定する](#Set_Packaging_Properties)** &ndash; パッケージ プロパティは、Android アプリケーション パッケージ (APK) の作成を制御します。 この手順は APK を最適化し、そのアセットを保護し、必要に応じてパッケージをモジュール化します。

-   **[コンパイル](#Compile)** &ndash; この手順はコードとアセットをコンパイルし、リリース モードでビルドすることを確認します。

-   **[発行のためのアーカイブ](#archive)** &ndash;この手順は、アプリをビルドし、署名および発行のためにアーカイブに配置します。

各手順について以下で詳しく説明します。

<a name="Specify_the_Application_Icon" />

## <a name="specify-the-application-icon"></a>アプリケーション アイコンを指定する

Xamarin.Android アプリケーションそれぞれでアプリケーション アイコンを指定することを強くお勧めします。 一部のアプリケーション マーケットプレースでは、これがないと、Android アプリケーションを公開することができません。 `Application` 属性の `Icon` プロパティは、Xamarin.Android プロジェクトのアプリケーション アイコンを指定するために使用されます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2015 以降では、アプリケーション アイコンは、次のスクリーンショットで示すように、プロジェクトの **[プロパティ]** の **[Android マニフェスト]** セクションから指定します。

[![アプリケーションのアイコンを設定する](images/vs/01-application-icon-sml.png)](images/vs/01-application-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac では、アプリケーション アイコンは、次のスクリーンショットで示すように、**[プロジェクト オプション]** の **[Android アプリケーション]** セクションから指定することもできます。

[![アプリケーションのアイコンを設定する](images/xs/01-application-icon-sml.png)](images/xs/01-application-icon.png#lightbox)

-----

これらの例では、`@drawable/icon` は **Resources/drawable/icon.png** にあるアイコン ファイルを示します (リソース名には **.png** 拡張子が含まれないことに注意してください)。 また、この属性は、このサンプルのスニペットに示されているように、ファイル **Properties\AssemblyInfo.cs** で宣言することができます。

```csharp
[assembly: Application(Icon = "@drawable/icon")]
```

通常、`using Android.App` は **AssemblyInfo.cs** の先頭で宣言されています (`Application` 属性の名前空間は `Android.App`)。ただし、`using` ステートメントがまだ存在しない場合は、追加する必要があります。


<a name="Versioning" />

## <a name="version-the-application"></a>アプリケーションのバージョン

Android アプリケーションの保守と配布には、バージョン管理が重要です。 何らかのバージョン管理がなければ、アプリケーションが更新される必要があるかどうか、またはその方法を決定することが困難です。 バージョン管理に役立てるために、Android は次の 2 つの異なる種類の情報を認識します。 

-   **バージョン番号** &ndash; アプリケーションのバージョンを表す (Android とアプリケーションによって内部使用される) 整数値。 ほとんどのアプリケーションは、この値を 1 に設定して開始し、ビルドごとにインクリメントされます。 この値は、バージョン名の属性とは関係ありません (下記参照)。 アプリケーションおよび発行サービスで、この値がユーザーに表示されることはありません。 この値は、**AndroidManifest.xml** ファイルに `android:versionCode` として保存されます。 

-   **バージョン名** &ndash; (特定のデバイスにインストールされる) アプリケーションのバージョンに関する情報をユーザーに知らせるためにのみ使用される文字列。 バージョン名は、ユーザーまたは Google Play 内に表示されることを意図しています。 この文字列は、Android によって内部的に使用されることはありません。 バージョン名は、ユーザーが自分のデバイスにインストールされているビルドを識別するために役立つ文字列の値になります。 この値は、**AndroidManifest.xml** ファイルに `android:versionName` として保存されます。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio では、次のスクリーンショットで示すように、これらの値は、プロジェクトの **[プロパティ]** の **[Android マニフェスト]** セクションで設定できます。

[![バージョン番号を設定する](images/vs/02-versioning-sml.png)](images/vs/02-versioning.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

以上の値は **[プロジェクト オプション]** の **[ビルド]、[Android アプリケーション]** セクションで設定できます。下のスクリーンショットをご覧ください。

[![バージョン番号を設定する](images/xs/02-versioning-sml.png)](images/xs/02-versioning.png#lightbox)

-----

<a name="shrink_apk" />

## <a name="shrink-the-apk"></a>APK を圧縮する

不要な*マネージ* コードを削除する Xamarin.Android リンカーと、使用しない *Java バイトコード*を削除する Android SDK の *ProGuard* ツールを組み合わせることで、Xamarin.Android APK を小さくすることができます ビルド プロセスでは最初に Xamarin.Android リンカーを使用してマネージ (C#) コード レベルでアプリを最適化し、次に ProGuard (有効になっている場合) を使用して Java バイトコード レベルで APK を最適化します。


### <a name="configure-the-linker"></a>リンカーを構成する

リリース モードでは、アプリケーションがランタイムで必要な Xamarin.Android の要素のみを送付するように、共有ランタイムはオフにし、リンクをオンにします。 Xamarin.Android の*リンカー*では、スタティック分析を使用して、Xamarin.Android アプリケーションによって使用または参照されるアセンブリ、型、および型メンバーを決定します。 次に、リンカーは、使用 (または参照) されないすべての未使用のアセンブリ、型、メンバーを破棄します。 この操作を行うと、パッケージ サイズが大幅に削減されます。 たとえば、[HelloWorld](~/android/deploy-test/linker.md) の例を検討します。この例では、APK の最終的なサイズが 83% 削減されます。 

-   構成: なし &ndash; Xamarin.Android 4.2.5 サイズ = 17.4 MB。

-   構成: SDK アセンブリのみ &ndash; Xamarin.Android 4.2.5 サイズ = 3.0 MB。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

プロジェクトの **[プロパティ]** の **[Android オプション]** セクションを使用してリンカー オプションを設定します。

[![リンカーのオプション](images/vs/03-linking-sml.png)](images/vs/03-linking.png#lightbox)

**[リンク中]** プルダウン メニューには、リンカーを制御するために、次のオプションが用意されています。

-   **[なし]** &ndash; リンカーをオフにします。リンクは行われません。

-   **[SDK アセンブリのみ]** &ndash; [Xamarin.Android で必要になる](~/cross-platform/internals/available-assemblies.md)アセンブリのみがリンクされます。 
    その他のアセンブリはリンクされません。

-   **[SDK およびユーザー アセンブリ]** &ndash; Xamarin.Android で必要になるアセンブリだけでなく、アプリケーションで必要になるすべてのアセンブリがリンクされます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**[プロジェクト オプション]** の **[Android のビルド]** セクションの **[リンカー]** タブでリンカー オプションを設定します。次のスクリーンショットをご覧ください。

[![リンカーのオプション](images/xs/03-linking-sml.png)](images/xs/03-linking.png#lightbox)

リンカーの制御オプションは次のようになります。

-   **[リンクしない]** &ndash; リンカーをオフにします。リンクは行われません。

-   **[SDK アセンブリのみをリンクする]** &ndash; [Xamarin.Android で必要になる](~/cross-platform/internals/available-assemblies.md)アセンブリのみがリンクされます。 その他のアセンブリはリンクされません。

-   **[すべてのアセンブリをリンクする]** &ndash; Xamarin.Android で必要になるアセンブリだけでなく、アプリケーションで必要になるすべてのアセンブリがリンクされます。

-----

リンクは意図しない副作用を起すことがあります。そのため、リリース モードで、かつ、物理デバイス上でアプリケーションを再テストすることが重要です。


### <a name="proguard"></a>ProGuard

*ProGuard* は、Java コードをリンクし、難読化する Android SDK ツールです。 ProGuard は通常、APK の大きな組み込みライブラリ (Google Play サービスなど) のフットプリントを少なくすることで小さなアプリケーションを作成するために使用します。 ProGuard では、未使用の Java バイトコードが削除されるため、結果としてアプリが小さくなります。 たとえば、小さな Xamarin.Android アプリで ProGuard を使用すると、24% のサイズ縮小が実現します。ライブラリ依存関係が複数ある大きなアプリで ProGuard を使用すると、通常はさらにサイズが縮小します。 

ProGuard は Xamarin.Android リンカーの代替ではありません。 Xamarin.Android リンカーは*マネージ* コードをリンクし、ProGuard は Java バイトコードをリンクします。 ビルド プロセスでは最初に Xamarin.Android リンカーを使用してアプリのマネージ (C#) コードを最適化し、次に ProGuard (有効になっている場合) を使用して Java バイトコード レベルで APK を最適化します。 

**[ProGuard を有効にする]** がオンになっていると、Xamarin.Android が結果として得られる APK で ProGuard ツールを実行します。 ProGuard 構成ファイルが生成され、ビルド時に ProGuard で使用されます。 Xamarin.Android は、カスタムの *ProguardConfiguration* ビルド アクションもサポートしています。 下の例で示すように、カスタムの ProGuard 構成ファイルをプロジェクトに追加し、これを右クリックして、ビルド アクションとして選択することができます。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Proguard ビルド アクション](images/vs/05-proguard-build-action-sml.png)](images/vs/05-proguard-build-action.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Proguard ビルド アクション](images/xs/05-proguard-build-action-sml.png)](images/xs/05-proguard-build-action.png#lightbox)

-----

ProGuard は既定では無効です。 **[ProGuard を有効にする]** オプションは、プロジェクトが**リリース** モードに設定されている場合にのみ使用できます。 **[ProGuard を有効にする]** がオンになっていない限り、すべての ProGuard ビルド アクションは無視されます。 Xamarin.Android の ProGuard 構成によって APK が難読化されることはなく、さらにはカスタム構成ファイルでも難読化されることはありません。 難読化を使用する場合は「[Application Protection with Dotfuscator](~/android/deploy-test/release-prep/index.md#dotfuscator)」(Dotfuscator を使用したアプリケーションの保護) を参照してください。 

ProGuard ツールの使用方法については、「[ProGuard](~/android/deploy-test/release-prep/proguard.md)」を参照してください。

<a name="protect_app" />

## <a name="protect-the-application"></a>アプリケーションを保護する

<a name="Disable_Debugging" />

### <a name="disable-debugging"></a>デバッグを無効にする

Android アプリケーションの開発時、デバッグは *Java Debug Wire Protocol* (JDWP) を使用して実行されます。 これは、デバッグ目的のために、Java 仮想マシンと通信する **adb** などのツールを許可するテクノロジです。 JDWP は、Xamarin.Android アプリケーションのデバッグ ビルドに対して、既定で有効にされています。 JDWP は開発時に重要ですが、リリースされたアプリケーションに対してセキュリティの問題を引き起こす可能性があります。 

> [!IMPORTANT]
> (JDWP によって) Java プロセスへの完全なアクセスを取得できるように、リリースされたアプリケーションのデバッグ状態を常に無効にします。このデバッグ状態が無効にされない場合は、アプリケーションのコンテキストで任意のコードを実行します。

Android マニフェストには、アプリケーションをデバッグするかどうかを制御する、`android:debuggable` 属性が含まれます。 `android:debuggable` 属性は、`false` に設定することが推奨されます。 この設定の最も簡単な方法は、次のように **AssemblyInfo.cs** に条件付きコンパイル ステートメントを追加することです。 

```csharp
#if DEBUG
[assembly: Application(Debuggable=true)]
#else
[assembly: Application(Debuggable=false)]
#endif
```

デバッグ ビルドでは、デバッグしやすいように、自動的にアクセス許可が設定されることに注意してください (**Internet**、**ReadExternalStorage** など)。 ただし、リリース ビルドでは、明示的に設定したアクセス許可のみを使用します。 リリース ビルドへの切り替えによって、使用しているアプリがデバッグ ビルドで使用可能であったアクセス許可を失う場合は、[アクセス許可](~/android/app-fundamentals/permissions.md)で示されているように、**必要なアクセス許可**リストのアクセス許可を明示的に有効にしていることを確認します。 
 

<a name="dotfuscator" id="dotfuscator" />

### <a name="application-protection-with-dotfuscator"></a>Dotfuscator によるアプリケーションの保護

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[デバッグが無効](#Disable_Debugging)になっていても、攻撃者が、アプリケーションを再パッケージ化し、構成オプションまたはアクセス許可を追加または削除することは可能です。 これにより、攻撃者がアプリケーションをリバース エンジニアリング、デバッグ、または改ざんすることができます。
[Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) を使用して、マネージ コードを難読化し、ビルド時にランタイムのセキュリティ状態検出コードを Xamarin.Android アプリに注入することができます。

Dotfuscator CE は、Visual Studio に含まれていますが、Visual Studio 2015 Update 3 (およびそれ以降) にのみ Xamarin.Android で機能する適切なバージョンが含まれています。 Dotfuscator を使用するには、**[ツール]、[PreEmptive Protection - Dotfuscator]** の順にクリックします。

Dotfuscator CE を構成するには、「[Using Dotfuscator Community Edition with Xamarin](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator)」(Xamarin での Dotfuscator Community Edition の使用) を参照してください。
構成されると、Dotfuscator CE は、自動的に作成される各ビルドを保護します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[デバッグが無効](#Disable_Debugging)になっていても、攻撃者が、アプリケーションを再パッケージ化し、構成オプションまたはアクセス許可を追加または削除することは可能です。 これにより、攻撃者がアプリケーションをリバース エンジニアリング、デバッグ、または改ざんすることができます。
Visual Studio for Mac はサポートされていませんが、[Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) と Visual Studio を使用して、マネージ コードを難読化し、ビルド時にランタイムのセキュリティ状態検出コードを Xamarin.Android アプリに挿入できます。

Dotfuscator CE を構成するには、「[Using Dotfuscator Community Edition with Xamarin](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator)」(Xamarin での Dotfuscator Community Edition の使用) を参照してください。
構成されると、Dotfuscator CE は、自動的に作成される各ビルドを保護します。

-----

<a name="bundle" />

### <a name="bundle-assemblies-into-native-code"></a>アセンブリをネイティブ コードにバンドルする

このオプションが有効になっていると、アセンブリがネイティブの共有ライブラリにまとめられます。 このオプションによってコードの安全性が保持されます。コードがネイティブ バイナリに埋め込まれることでマネージ アセンブリが保護されます。

このオプションは Enterprise ライセンスを必要とし、**[Fast Deployment の使用]** が無効になっている場合にのみ使用できます。 **[ネイティブ コードへのアセンブリのバンドル]** は既定では無効です。

**[Bundle into Native Code]\(ネイティブ コードへのバンドル\)** オプションは、アプリケーションがネイティブ コードにコンパイルされることを意味するわけ*ではありません*。 [**[AOT コンパイル]**](#aot) を使用してネイティブ コードにアセンブリをコンパイルすることはできません (現在は実験的な機能にすぎず、運用環境向けではありません)。

<a name="aot" />

### <a name="aot-compilation"></a>AOT コンパイル

**[AOT コンパイル]** オプション ([[パッケージング プロパティ]](#Set_Packaging_Properties) ページ) により、アセンブリの Ahead Of Time (AOT) コンパイルが有効になります。 このオプションを有効にすると、ランタイム前にアセンブリがプリコンパイルされることで Just In Time (JIT) スタートアップのオーバーヘッドが最小化します。 生成されたネイティブ コードは、コンパイルされていないアセンブリと共に APK に含まれています。 この結果、アプリケーションの起動時間が短縮しますが、APK のサイズが少し大きくなります。

**[AOT コンパイル]** オプションには、Enterprise 以上のライセンスが必要です。 **[AOT コンパイル]** は、プロジェクトがリリース モードで構成されていて、これが既定で無効になっている場合にのみ使用できます。 AOT コンパイルの詳細については、「[AOT](http://www.mono-project.com/docs/advanced/aot/)」を参照してください。


#### <a name="llvm-optimizing-compiler"></a>LLVM 最適化コンパイラ

_LLVM 最適化コンパイラ_では、より小さく高速なコンパイル済みコードを作成し、AOT コンパイルによるアセンブリをネイティブ コードに変換しますが、その分ビルド時間がかかります。 LLVM コンパイラは既定では無効です。 LLVM コンパイラを使用するには、最初に ([[パッケージング プロパティ]](#Set_Packaging_Properties) ページで) **[AOT コンパイル]** オプションを有効にする必要があります。


> [!NOTE]
> **LLVM 最適化コンパイラ** オプションには、ビジネス ライセンスが必要です。  

<a name="Set_Packaging_Properties" />

## <a name="set-packaging-properties"></a>パッケージング プロパティを設定する

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

パッケージング プロパティは、次のスクリーンショットで示すように、プロジェクトの**[プロパティ]** の **[Android オプション]** セクションで設定できます。

[![パッケージング プロパティ](images/vs/04-packaging-sml.png)](images/vs/04-packaging.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

パッケージング プロパティは **[プロジェクト オプション]** で設定できます。次のスクリーンショットをご覧ください。

[![パッケージング プロパティ](images/xs/04-packaging-sml.png)](images/xs/04-packaging.png#lightbox)

-----

**[共有ランタイムの使用]** や **[Fast Deployment の使用]** など、プロパティの多くはデバッグ モードを意図しています。 ただし、リリース モードのためにアプリケーションを構成するとき、[サイズと実行速度に関してアプリを最適化する](#shrink_apk)方法、[改ざんを防止する方法](#protect_app)、さまざまなアーキテクチャやサイズ制約に対応できるようにパッケージ化する方法を決定する設定が他にあります。


### <a name="specify-supported-architectures"></a>サポートされているアーキテクチャを指定する

リリースに向けて Xamarin.Android アプリを準備しているときに、サポートされる CPU アーキテクチャを指定する必要があります。 1 つの APK に、複数の異なるアーキテクチャをサポートするためのマシン コードを含めることができます。 複数の CPU アーキテクチャのサポートに関する詳細については、「[CPU アーキテクチャ](~/android/app-fundamentals/cpu-architectures.md)」を参照してください。


### <a name="generate-one-package-apk-per-selected-abi"></a>選択した ABI ごとに 1 つのパッケージ (.APK) を生成する

このオプションが有効になっていると、サポートされるすべての ABI を対象とする単一の大きな APK ではなく、サポートされるそれぞれの ABI (**[詳細設定]** タブで選択。「[CPU Architectures](~/android/app-fundamentals/cpu-architectures.md)」(CPU アーキテクチャ) に説明あり) に対して 1 つの APK が作成されます。 このオプションは、プロジェクトがリリース モードで構成されていて、これが既定で無効になっている場合にのみ使用できます。


### <a name="multi-dex"></a>Multi-Dex

**[Multi-Dex を有効にする]** オプションが有効になっていると、Android SDK ツールを使用して **.dex** ファイル形式の 65K のメソッド制限が回避されます。 65K のメソッド制限は、アプリが_参照する_ Java メソッドの数に基づいています (アプリが依存しているすべてのライブラリのメソッドも含む)。_ソース コードに記述されている_メソッドの数には基づいていません。 アプリケーションで定義しているメソッドの数が 2、3 個で、使用するメソッドが多い (またはライブラリが大きい) 場合、65K の制限を超過する可能性があります。

参照されるすべてのライブラリのすべてのメソッドをアプリが使用していない場合があるため、ProGuard (上記参照) などのツールを使用して、未使用のメソッドをコードから削除することができます。 **[Multi-Dex を有効にする]** は本当に必要な場合にのみ有効にすることをお勧めします。たとえば、ProGuard を使用してもアプリが 65K を超える Java メソッドを参照する場合などです。

Multi-Dex の詳細については、「[64K を超えるメソッドを使用するアプリの設定](http://developer.android.com/tools/building/multidex.html)」を参照してください。

<a name="Compile" />

## <a name="compile"></a>Compile

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

上記の手順のすべてが完了したら、アプリのコンパイルの準備ができます。 **[ビルド]、[ソリューションのリビルド]** の順に選択し、リリース モードで正常にビルドされることを確認します。 この手順ではまだ APK が生成されません。

[アプリ パッケージの署名](~/android/deploy-test/signing/index.md)に関するセクションでは、パッケージ化と署名についてさらに詳しく説明します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

上記のすべての手順が完了したら、アプリケーションをコンパイルし (**[ビルド]、[すべてビルド]** の順に選択)、リリース モードで正しくビルドされることを確認できます。 この手順ではまだ APK が生成されません。

-----


<a name="archive" />

## <a name="archive-for-publishing"></a>発行のためのアーカイブ

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

発行プロセスを開始するには、**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[アーカイブ]** コンテキスト メニュー項目を選択します。

[![アプリのアーカイブ](images/vs/07-archive-for-publishing-sml.png)](images/vs/07-archive-for-publishing.png#lightbox)

**[アーカイブ...]** を選択すると、**アーカイブ マネージャー**が起動し、このスクリーン ショットに示すように、アプリ バンドルをアーカイブするプロセスが開始されます。

[![アーカイブ マネージャー](images/vs/08-archive-manager-sml.png)](images/vs/08-archive-manager.png#lightbox)

アーカイブを作成する別の方法として、**ソリューション エクスプローラー**でソリューションを右クリックし、**[すべてアーカイブ]** を選択します。これにより、ソリューションがビルドされ、アーカイブを生成できるすべての Xamarin プロジェクトがアーカイブされます。

[![すべてアーカイブ](images/vs/09-archive-all-sml.png)](images/vs/09-archive-all.png#lightbox)


**[アーカイブ]** と **[すべてアーカイブ]** のどちらも、**アーカイブ マネージャー**を自動的に起動します。 **アーカイブ マネージャー**を直接起動するには、**[ツール]、[アーカイブ マネージャー]** メニュー項目の順に選択します。

[![アーカイブ マネージャーを起動する](images/vs/10-launch-archive-manager-sml.png)](images/vs/10-launch-archive-manager.png#lightbox)

**[ソリューション]** ノードを右クリックして **[アーカイブの表示]** を選択することで、いつでもソリューションをアーカイブすることができます。

[![アーカイブの表示](images/vs/11-view-archives-sml.png)](images/vs/11-view-archives.png#lightbox)

### <a name="the-archive-manager"></a>アーカイブ マネージャー

**アーカイブ マネージャー**は、**ソリューション一覧**ウィンドウ、**アーカイブ一覧**、および**詳細パネル**で構成されます。

[![アーカイブ マネージャーのウィンドウ](images/vs/12-archive-manager-detail-sml.png)](images/vs/12-archive-manager-detail.png#lightbox)

**ソリューション一覧**には、アーカイブされた少なくとも 1 つのプロジェクトを持つすべてのソリューションが表示されます。 **ソリューション一覧**には、次のセクションが含まれます。

* **現在のソリューション** &ndash; 現在のソリューションが表示されます。 現在のソリューションに既存のアーカイブがない場合は、この領域は空になる可能性があることに注意してください。
* **すべてのアーカイブ** &ndash; アーカイブがあるすべてのソリューションが表示されます。
* **[検索]** テキストボックス (上部) &ndash; テキストボックスに入力した検索文字列に従って **[すべてのアーカイブ]** の一覧に表示されるソリューションにフィルターを適用します。

**アーカイブ一覧**には、選択したソリューションのすべてのアーカイブの一覧が表示されます。 **アーカイブ一覧**には、次のセクションが含まれます。

* **選択したソリューションの名前** &ndash; **ソリューション一覧**で選択したソリューションの名前が表示されます。 **アーカイブ一覧**に表示されるすべての情報は、この選択したソリューションを参照しています。
* **プラットフォーム フィルター** &ndash; このフィールドにより、プラットフォームの種類 (iOS、Android など) によってアーカイブをフィルター処理することができます。
* **アーカイブ項目** &ndash; 選択したソリューションのアーカイブの一覧です。 この一覧内の各項目には、プロジェクト名、作成日、およびプラットフォームが含まれています。 項目がアーカイブまたは発行されているときの進行状況などの追加情報も表示できます。

**詳細パネル**には、各アーカイブに関する追加情報が表示されます。 ユーザーが、配布ワークフローを開始したり、またはディストリビューションが作成されたフォルダーを開いたりすることもできます。 **[ビルドのコメント]** セクションにより、アーカイブにビルドのコメントを含めることができます。

### <a name="distribution"></a>配布

アーカイブ済みのバージョンのアプリケーションを発行する準備ができたら、**アーカイブ マネージャー**でアーカイブを選択し、**[配布]** ボタンをクリックします。

[![[配布] ボタン](images/vs/13-distribute-sml.png)](images/vs/13-distribute.png#lightbox)

**[配布チャネル]** ダイアログには、アプリに関する情報、配布ワークフローの進行状況を示す値、および配布チャネルの選択肢が表示されます。 最初の実行時に、2 つの選択肢が表示されます。

[![配布チャネルの選択](images/vs/14-distribution-channel-sml.png)](images/vs/14-distribution-channel.png#lightbox)

次の配布チャネルのいずれかを選択できます。

* **アドホック** &ndash; Android デバイスにサイドロードできるように、署名済み APK をディスクに保存します。 引き続き[アプリ パッケージの署名](~/android/deploy-test/signing/index.md)に関するセクションに進み、Android の署名 ID を作成する方法、Android アプリケーション用の新しい署名証明書を作成する方法、アプリの_アドホック_ バージョンをディスクに発行する方法を学習してください。 これは、テスト用の APK を作成するための効果的な方法です。

* **Google Play** &ndash; 署名済み APK を Google Play に発行します。 引き続き「[Google Play に公開する](~/android/deploy-test/publishing/publishing-to-google-play/index.md)」に進み、APK を署名して Google Play ストアに発行する方法について学習してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

発行プロセスを開始するには、**[ビルド]、[発行のためのアーカイブ]** の順に選択します。

[![発行のためのアーカイブ](images/xs/07-archive-for-publishing-sml.png)](images/xs/07-archive-for-publishing.png#lightbox)

**[発行のためのアーカイブ]** では、プロジェクトをビルドし、アーカイブ ファイルにバンドルします。 **[すべてアーカイブ]** メニューの選択肢は、ソリューション内のすべてのアーカイブ可能プロジェクトをアーカイブします。 どちらのオプションも、ビルドおよびバンドル操作が完了したときに **[アーカイブ マネージャー]** が自動的に開きます。

[![ビューのアーカイブ](images/xs/08-archives-view-sml.png)](images/xs/08-archives-view.png#lightbox)

この例では、**アーカイブ マネージャー**が、1 つのアーカイブされたアプリケーション **MyApp** のみを表示します。 コメント フィールドを使用して、アーカイブと共に短いコメントを保存することができます。 Xamarin.Android アプリケーションのアーカイブ済みのバージョンを発行するには、上に示すように **[アーカイブ マネージャー]** でアプリを選択し、**[署名と配布...]** をクリックします。 結果として得られる **[署名と配布]**ダイアログには、2 つの選択肢が表示されます。

[![署名と配布](images/xs/09-sign-and-distribute-sml.png)](images/xs/09-sign-and-distribute.png#lightbox)


ここでは、配布チャンネルを選択することができます。

-   **アドホック** &ndash; Android デバイスにサイドロードできるように、署名済み APK をディスクに保存します。 引き続き[アプリ パッケージの署名](~/android/deploy-test/signing/index.md)に関するセクションに進み、Android の署名 ID を作成する方法、Android アプリケーション用の新しい署名証明書を作成する方法、アプリの&ldquo;アドホック&rdquo; バージョンをディスクに発行する方法を学習してください。 これは、テスト用の APK を作成するための効果的な方法です。


-   **Google Play** &ndash; 署名済み APK を Google Play に発行します。
    引き続き「[Google Play に公開する](~/android/deploy-test/publishing/publishing-to-google-play/index.md)」に進み、APK を署名して Google Play ストアに発行する方法について学習してください。

-----


## <a name="related-links"></a>関連リンク

- [マルチコア デバイスと Xamarin.Android](~/android/deploy-test/multicore-devices.md)
- [CPU アーキテクチャ](~/android/app-fundamentals/cpu-architectures.md)
- [AOT](http://www.mono-project.com/docs/advanced/aot/)
- [コードとリソースを圧縮する](http://developer.android.com/tools/help/proguard.html)
- [64K を超えるメソッドを使用するアプリの設定](http://developer.android.com/tools/building/multidex.html)
