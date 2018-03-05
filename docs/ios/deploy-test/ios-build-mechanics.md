---
title: "iOS ビルドのしくみ"
description: "このガイドでは、アプリの時間を調整する方法と、あらゆるビルド構成のビルドを速くする手法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b752ebdd1a98d5258cc27b2221d33e07fa04aa46
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="ios-build-mechanics"></a>iOS ビルドのしくみ

_このガイドでは、アプリの時間を調整する方法と、あらゆるビルド構成のビルドを速くする手法について説明します。_

動作するコードを記述するだけでは、優れたアプリケーションは開発できません。 優れたアプリには最適化が取り入れられています。容量が少なく、高速で動作するアプリによってビルドが短時間で完了します。 このような最適化は利用者にとって使い勝手をよくするだけでなく、プロジェクトに取り組む開発者にとっても開発を楽にします。 アプリを扱うときは、すべてにおいて頻繁に時間を調整することが不可欠です。 

既定のオプションは安全で高速ですが、あらゆる状況に対して最適ではありません。 また、オプションの多くは、プロジェクトに応じて開発サイクルを遅らせることも、早めることもあります。 たとえば、標準の削除には時間がかかりますが、サイズがあまり小さくならない場合、短時間で配置しても削除に要した時間が取り戻されることはありません。 一方で、標準の削除はアプリを大幅に圧縮できます。これにより配置が短時間になります。 結果はプロジェクトによって異なり、それを確認する唯一の方法はテストすることです。

Xamarin ビルドのスピードは、パフォーマンスに影響を与えるコンピューターのさまざまな容量や機能の影響も受けます。プロセッサ、各種機能、バス スピード、物理メモリの容量、ディスク速度、ネットワーク速度などです。 このようなパフォーマンス上の制約は本書の範囲外であり、開発者が担う部分です。


## <a name="timing-apps"></a>アプリの時間を調整する

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac 内で診断用 MSBuild 出力を有効にするには:

1. **[Visual Studio for Mac]、[環境設定]** の順にクリックします。
2. 左側のツリー ビューで、**[プロジェクト]、[ビルド]** の順に選択します。
3. 右側のパネルで、次のようにログ詳細度ドロップダウンを **[診断]** に設定します。 [ ![](ios-build-mechanics-images/image2.png "ログ詳細度を設定する")](ios-build-mechanics-images/image2.png)
4. **[OK]** をクリックします。
5. Visual Studio for Mac を再起動します。
6. パッケージから不要な要素を取り除き、再ビルドします。
7. [ビルド出力] ボタンをクリックし、エラー パッド内で診断出力を表示します ([表示]、[パッド]、[エラー])。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 内で診断 MSBuild 出力を有効にするには:

1. **[ツール]、[出力]** の順にクリックします。
2. 左側のツリー ビューで、**[プロジェクトおよびソリューション]、[ビルド/実行]** の順に選択します。
3. 右側のパネルで、*MSBuild ビルド出力の詳細度ドロップダウン*を **[診断]** に設定します。 [ ![](ios-build-mechanics-images/image2-vs.png "MSBuild ビルド出力の詳細度を設定する")](ios-build-mechanics-images/image2-vs.png)
4. **[OK]** をクリックします。
5. パッケージから不要な要素を取り除き、再ビルドします。
6. 診断出力は [出力] パネルに表示されます。

-----

## <a name="timing-mtouch"></a>mtouch の時間を調整する

mtouch ビルド プロセス固有の情報を表示するには、**[プロジェクト オプション]** の mtouch 引数に `--time --time` を渡します。 結果は [ビルド出力] にあります。`MTouch` タスクを探してください。

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>ビルド ホストで Visual Studio から接続する

Xamarin ツールは技術的には、OS X 10.10 Yosemite 以降を実行できるあらゆる Mac で動作します。 ただし、Mac の性能によってはビルドに時間がかかり、開発が遅れることがあります。

接続のない状態では、Windows の Visual Studio は C# 段階のみを実行し、リンクも AOT コンパイルも実行せず、アプリを _.app_ バンドルにパッケージ化せず、アプリ バンドルに署名しません。 (C# コンパイル段階がパフォーマンス上のボトルネックになることはまれです。)Visual Studio for Mac の Mac ビルド ホストで直接ビルドし、ビルドが遅くなっているパイプラインの場所を特定します。


また、動作を遅くする一般的な原因の 1 つに、Windows コンピューターと Mac ビルド ホストの間のネットワーク接続があります。 この原因には、ネットワーク上の物理的妨害、無線接続の使用、飽和状態のコンピューターを通過すること (Mac-in-the-cloud サービスなど) などが挙げられます。

## <a name="simulator-tricks"></a>シミュレーター関連のヒント

 
モバイル アプリケーションを開発するときは、コードをすばやく配置することが重要です。 スピードやデバイス プロビジョニング要件の不足など、さまざまな理由から、開発者は多くの場合、インストール済みのシミュレーターやエミュレーターに配置します。 開発者ツールのメーカーにとっては、シミュレーターまたはエミュレーターを提供するかどうかは、スピードと互換性の折り合いになります。 

Apple は iOS 開発用のシミュレーターを提供しており、互換性よりもスピードを奨励しています。コードを実行する環境の制約を少なくします。 環境の制約が少なければ、Xamarin では、(デバイスの [AOT](~/ios/internals/architecture.md) ではなく) シミュレーターに Just In Time (JIT) コンパイラを使用できます。つまり、ビルドが実行時にネイティブ コードにコンパイルされます。 Mac がデバイスよりずっと高速になり、パフォーマンスが向上します。

このシミュレーターは共有アプリケーション起動ツールを使用します。デバイスで要求されるように毎回ビルドするのではなく、起動ツールを再利用できます。

次の一覧では以上の内容を踏まえ、最高のパフォーマンスが得られるよう、アプリをビルドし、シミュレーターに配置するときの手順をまとめています。
 
### <a name="tips"></a>ヒント

- ビルドの場合: 
  - [プロジェクト オプション] で **[PNG 画像を最適化する]** オプションの選択を解除します。 シミュレーターでのビルドの場合、この最適化は不要です。
  - リンカーに **[リンクしない]** に設定します。 リンカーを無効にすると速くなります。リンカーの実行には長い時間がかかるためです。
  - `--nofastsim` フラグで共有アプリケーション起動ツールを無効にすると、シミュレーターのビルドがかなり遅くなります。 不要になったときにこのフラグを削除します。
  - ネイティブ ライブラリを使用すると遅くなります。共有シミュレーション起動ツールのメイン実行可能ファイルが再利用できないためです。アプリケーション固有の実行可能ファイルをビルドのたびにコンパイルする必要があります。
- 配置の場合:
  - 可能な限り、シミュレーターを常時実行します。 シミュレーターをコールドスタートするには、最大 12 秒かかります。
- その他のヒント
  - リビルドよりビルドを選んでください。リビルドの場合、ビルド前に不要な要素が削除されます。 クリーニングでは、利用できそうな参照も削除され、時間がかかります。
  - シミュレーターはサンドボックスを強制しないという事実を活かしてください。 動画やその他のアセットなど、大容量のリソースをプロジェクトに含めると、シミュレーターでアプリを起動するたびに、ファイル コピー操作に長い時間がかかります。 そのようなファイルはホーム ディレクトリに置き、アプリケーションでは、省略のないファイル パスでファイルを参照することで、そのような代償の大きい操作を回避できます。  
  - 確かではない場合は、`–time –time` フラグで変更の度合いを測ってください。

次のスクリーンショットでは、iOS オプションでシミュレーターに前述のオプションを設定している様子を確認できます。

[ ![](ios-build-mechanics-images/image3.png "オプションを設定する")](ios-build-mechanics-images/image3.png)

## <a name="device-tricks"></a>デバイス関連のヒント

デバイスへの配置はシミュレーターへの配置に似ています。シミュレーターは、iOS デバイスに使用されるビルドの小さなサブセットであるためです。 デバイスのビルドにはたくさんの手順が必要ですが、アプリを最適化する機会が増えるという利点があります。

### <a name="build-configurations"></a>ビルド構成

iOS アプリを配置するとき、さまざまなビルド構成が与えられます。 それぞれの構成をよく理解し、最適化するタイミングと理由を知ることが重要です。

 - デバッグ
  - これはメインの構成であり、アプリの開発中に使用します。そのため、可能な限り短時間で行います。
 - Release
  - リリース ビルドはユーザーに出荷されるビルドであり、パフォーマンスが非常に重視されます。 リリース構成を使用するとき、LLVM 最適化コンパイラを使用し、PNG ファイルを最適化することもできます。

 
ビルドと配置の関係を理解することも重要です。 配置時間はアプリケーション サイズに依存します。 アプリケーションが大きければ、それだけ配置に時間がかかります。 アプリのサイズを最小限に抑えることで、配置時間を短くすることができます。

アプリのサイズを最小限に抑えれば、ビルド時間も短くなります。 アプリケーションからコードを削除すると、利用されないコードの標準コンパイルより時間が短縮されるためです。 オブジェクト ファイルが小さければ、リンクがそれだけ速くなります。実行可能ファイルが小さくなり、生成するシンボルが少なくなります。 そのため、領域の節約には倍の見返りがあり、そのような理由から、すべてのデバイス ビルドで **[Link SDK]\(SDK のリンク\)**が既定になっています。 

> [!NOTE]
> **[Link SDK]\(SDK のリンク\)** オプションは、使用されている IDE に基づき、[フレームワーク SDK のみをリンクする] または [SDK アセンブリのみをリンクする] として表示されます。
 

### <a name="tips"></a>ヒント

- ビルド: 
  - 1 つのアーキテクチャ (ARM64 など) のビルドは FAT バイナリ (ARMv7 + ARM64 など) より速くなります。
  - デバッグ時は PNG ファイルの最適化を避けます。
  - すべてのアセンブリをリンクすることを検討します。 すべてのアセンブリを最適化します。 
  - `--dsym=false` を利用し、デバッグ シンボルの作成を無効にします。 ただし、これを無効にすると、アプリをビルドしたそのコンピューターでのみ、かつ、アプリが取り除かれなかった場合にのみ、クラッシュ レポートにシンボル名を付加できます。

 
避けるべきこと:

- Fat バイナリ (デバッグ) 
- リンカー `–nolink` を無効にする 
- 削除を無効にする 
  - シンボル `--nosymbolstrip` 
  - IL (リリース) `--nostrip`.  
 
その他のヒント 

- シミュレーターの場合と同様に、リビルドよりビルドを選択します。 
  - AOT のアセンブリ (オブジェクト ファイル) がキャッシュに保存されます。 
- シンボルや dsymutil の実行に起因してデバッグ ビルドには時間がかかります。最終的に大きくなるため、デバイスにアップロードするための時間が長くなります。 
- リリース ビルドは、既定では、アセンブリの IL 削除を行います。 これにはわずかな時間しかかからず、多くの場合、小さくしたアプリをデバイスに配置するとき、かかった時間を取り戻すことができます。
- すべてのビルドで大きな静的ファイルを配置することを回避します (デバッグ)。 
  - UIFileSharingEnabled (info.plist) を使用します。 
    - アセットは 1 回でアップロードできます。 
- 確かではない場合は、`–time –time` フラグで変更の度合いを測ってください。

次のスクリーンショットでは、iOS オプションでシミュレーターに前述のオプションを設定している様子を確認できます。

[ ![](ios-build-mechanics-images/image4.png "オプションを設定する")](ios-build-mechanics-images/image4.png)

## <a name="using-the-linker"></a>リンカーを使用する

アプリケーションをビルドするとき、mtouch はマネージ コードにリンカーを使用します。それによって、アプリケーションで利用されていないコードが削除されます。 理論上、これでビルドの規模が小さくなり、その分、速くなります。 リンカーの詳細については、「[iOS でのリンク](~/ios/deploy-test/linker.md)」ガイドを参照してください。

リンカーを利用するとき、次のオプションを考慮します。

- デバイス ビルドに **[リンクしない]** を選択するとビルドの時間が長くなり、アプリが大きくなります。 
  - サイズの上限を超えた場合、Apple はアプリを拒否します。 `MinimumOSVersion` にもよりますが、60 MB という少ない容量が上限となっています。 
  - ネイティブの実行可能ファイルは含まれていません。 
  - シミュレーター ビルドの場合、[リンクしない] を選択すると (デバイスの AOT ではなく) JIT コンパイルが利用され、ビルドが速くなります。
- SDK のリンクが既定のオプションです。
- [すべてをリンクする] は、NuGet やコンポーネントなど、自分で作成したものではないコードを利用する場合は特に、安全ではない可能性があります。 アセンブリをリンクしない場合、これらのサービスからのすべてのコードがアプリケーションによって追加されるため、アプリが大きくなる可能性があります。 
  - ただし、**[すべてをリンクする]** を選択した場合、特に外部コンポーネントが使用されるとき、アプリケーションがクラッシュすることがあります。 一部のコンポーネントでは、特定の種類にリフレクションが使用されるためです。
  - スタティック分析とリフレクションは一緒に機能しません。 

[`[Preserve]` 属性](~/ios/deploy-test/linker.md)を利用すれば、アプリケーション内にものを保持するようにツールに指示できます。 

ソース コードにアクセスできないか、ツールによって生成されたものを変更しない場合でも、保存する必要があるあらゆる種類とメンバーを表す XML ファイルを作成することでリンクできます。 その後、フラグ `--xml={file.name}.xml` をプロジェクト オプションに追加できます。Attributes を利用している場合とまったく同じようにコードが処理されます。


### <a name="partially-linking-applications"></a>アプリケーションを部分的にリンクする 

次のようにアプリケーションを部分的にリンクすることもできます。アプリケーションのビルド時間の最適化に役立ちます。

- `Link All` を使用して一部のアセンブリをスキップする 
  - アプリケーション サイズ最適化の一部が失われます。
  - ソース コードにアクセスする必要はありません。
  - 例を挙げると、`--linkall --linkskip=fieldserviceiOS` のようになります。
 
- 必要なアセンブリで `Link SDK` オプションと `[LinkerSafe]` 属性を使用する 
  - ソース コードにアクセスする必要があります。
  - 安全にアセンブリにリンクできることと、Xamarin SDK のように処理されることをシステムに通知します。
 
### <a name="objective-c-bindings"></a>Objective-C バインディング 

- バインディングで `[Assembly: LinkerSafe]` 属性を使用すると、時間と容量の節約になります。

- SmartLink 
  - ネイティブ側で行う 
  - `[LinkWith (SmartLink=true)]` 属性を使用する
  - リンク対象のライブラリからネイティブ コードを削除するというネイティブ リンカーの動作を支援します。 
  - シンボルの動的参照はこれと連動しないことに注意してください。 

## <a name="summary"></a>まとめ

このガイドでは、iOS アプリケーションと、プロジェクトのビルド構成とオプションによっては考慮すべき iOS オプションの時間を調整する方法について説明しました。 

<!-----
# Benchmarks

## Layer 1: building again after making modifications, but _without_ cleaning should be faster 
 
The app should build a bit more quickly if you have only made changes to a subset of the libraries and you do not clean the build before re-deploying. 
 
 
 
### Clean build time 
178 seconds 
 
 
### Build again (without cleaning) after making _no changes_ 
12.5 seconds 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
3 trials: 45 seconds, 43 seconds, 43 seconds 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
 
3 trials: 45 seconds, 45 seconds, 45 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
- Sales.Native.Core.IOS.Ext/ServiceInterfaces/AlertDialog/Dialog.cs 
- Sales.Native.Core.Tools.IOS.Ext/BaseViews/BaseNavigationViewController.cs 
- View.Common/Services/DataTransferResult.cs 
 
45 seconds 
 
 
 
 
 
 
## Layer 2: "app thinning" aka "device specific builds" 
 
The idea of "app thinning" is that the IDE will only build the 1 architecture needed for the specific device that you're deploying to (rather than _both_ 32-bit and 64-bit architectures). 
 
As of the latest "Xamarin 4" builds, you can now enable "app thinning" in Visual Studio via the "Project Options -> iOS Build -> Enable device-specific builds" setting. 
 
Or if you prefer you can achieve a similar result by changing the "Project Options -> iOS Build -> Advanced [tab] -> Supported architectures" to select just _one_ architecture (for example ARM64 if you are developing on a 64-bit device). 
 
 
 
(Caveat: I ran the following builds in Visual Studio for Mac on the Mac rather than on the command line.) 
 
### Clean build time without "device specific builds" 
177 seconds 
 
 
 
### Clean build time _with_ "device specific builds"  
2 trials: 106 seconds, 98 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 31 seconds, 31 seconds 
 
 
* * * 
 
 
## Using the same strategy, but explicitly setting "Supported architectures" to select ARM64 _only_ (rather than using "device specific builds") 
 
(These builds were again run on the command line using `xbuild`.) 
 
 
 
### Clean build time with "Supported architectures" set to ARM64 _only_ 
2 trials: 80 seconds, 91 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 26 seconds, 26 seconds 
 
 
 
 
 
[1] Mac system used for testing: MacBookAir5,2 
 
- 2.0 GHz Core i7 (I7-3667U) 
 
2 Cores with hyper-threading 
 
L2 Cache (per Core): 256 KB 
L3 Cache: 4 MB 
 
- Standard MacBook soldered-in solid-state storage 
 
- 8 GB RAM 
---->


## <a name="related-links"></a>関連リンク

- [ブログの投稿](https://blog.xamarin.com/xamarin-ios-build-improvements/)
- [iOS でのリンク](~/ios/deploy-test/linker.md)
- [カスタム リンカーの構成](~/cross-platform/deploy-test/linker.md)
