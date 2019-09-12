---
title: Xamarin Live Player のトラブルシューティング
description: このドキュメントでは、Xamarin Live Player および潜在的な修正に関する既知の問題について説明します。 接続の問題、構成に関する問題などについて説明します。
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: conceptdev
ms.author: crdun
ms.date: 06/13/2019
ms.openlocfilehash: 04a377bad42ff680247759036327035d61757b42
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290178"
---
# <a name="troubleshooting-xamarin-live-player"></a>Xamarin Live Player のトラブルシューティング

![プレビュー機能](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player プレビューが終了しました。 アプリは使用できなくなりました。 以下の手順は、Visual Studio 2017 でプレビューの使用を継続しているお客様向けに提供されています。

> [!TIP]
> Visual Studio 2019 または Visual Studio for Mac の[XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)を使用して、編集時に画面のデザインを表示できます。

この記事では、ライブプレーヤーの制限事項と、それらを修正するための手順に関する一般的な問題について説明します。

## <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player の制限事項

### <a name="ide-requirements"></a>IDE の要件

Live Player Preview は、Visual Studio 2017 でのみ使用できます。

### <a name="device-requirements"></a>デバイスの要件

Xamarin Live Player アプリは、次の Android デバイスをサポートしています。

- Android 4.2 以降。
- ARM-armeabi-v7a、arm64-v8a、ARM64、arm64-v8a、x86、または x86_64 processor。

### <a name="ios-limitations"></a>iOS の制限事項

Live Player は iOS では使用できません。

### <a name="xamarinforms-limitations"></a>Xamarin. Forms の制限事項

- カスタムレンダラーはサポートされていません。
- エフェクトはサポートされていません。
- カスタムバインド可能なプロパティを持つカスタムコントロールはサポートされていません。
- 埋め込みリソースはサポートされていません (つまり、イメージや PCL 内の他のリソースを埋め込む)。
- サードパーティの MVVM フレームワークはサポートされていません (ie。Prism、Mvvm、Mvvm など)。

### <a name="other-project-type-limitations"></a>その他のプロジェクトの種類の制限

- Live Player は、ユーザーインターフェイスに Android XML を使用するネイティブの Android プロジェクトを対象としていません。

### <a name="miscellaneous-limitations"></a>その他の制限事項

- リフレクションのサポートが制限されています (現在、SQLite や Json.NET などの一般的な Nuget には影響します)。 その他の Nuget はまだサポートされている可能性があります。
- 一部のシステムクラス (たとえば、サブクラスを実装することはできません) をオーバーライドすることはできません。
- プロビジョニングが必要なプラットフォーム機能の中には、Xamarin Live Player アプリでは機能しないものがあります (ただし、フォトギャラリーへのアクセスなどの一般的な操作用に構成されています)。
- カスタムターゲットとビルドステップは無視されます。 たとえば、"AutoFac"、"フィット"、""、"AutoMapper" などのツールを組み込むことはできません。
- F#プロジェクトはサポートされていません
- カスタムジェネリッククラスおよびインターフェイスを使用した高度なシナリオはサポートされない場合があります。

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>バーコードをスキャンした後 (またはコードを入力した後)、モバイルデバイスが接続しない

Xamarin Live Player を実行しているモバイルデバイスが、IDE を実行しているコンピューターと同じネットワーク上にない場合に発生します。 次のことを確認してください。

- デバイスとコンピューターの両方が同じ Wi-fi ネットワーク上にあることを確認します。
  - コンピューターがワイヤード (有線) ネットワークにも接続されている場合は、ワイヤード (有線) 接続を抜いてみてください。
- ネットワークがセキュリティで保護されている (一部の企業ネットワークなど) と、Xamarin Live Player に必要なポートをブロックしている可能性があります。
- Xamarin Live Player アプリを閉じて再起動します。

## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE で "メッセージの展開中にエラーが発生しました" というメッセージが表示される

**"IOException: トランスポート接続からデータを読み取ることができません:非ブロッキングソケットに対する操作は "**

このエラーは、Xamarin Live Player を実行しているモバイルデバイスが、Visual Studio を実行しているコンピューターと同じネットワーク上にない場合に発生することがよくあります。これは、以前に正常にペアリングされたデバイスに接続するときによく発生します。

- デバイスとコンピューターの両方が同じ Wi-fi ネットワーク上にあることを確認します。
- ネットワークがセキュリティで保護されている (一部の企業ネットワークなど) と、Xamarin Live Player に必要なポートをブロックしている可能性があります。 Xamarin Live Player には、次のポートが必要です。
  - 37847–内部ネットワークアクセス 
  - 8090–外部ネットワークアクセス

## <a name="manually-configure-device"></a>デバイスを手動で構成する

Wi-fi 経由でデバイスに接続できない場合は、次の手順に従って、構成ファイルを使用して手動でデバイスを構成することができます。

**ステップ 1: 構成ファイルを開く**

アプリケーションデータフォルダーに移動します。

- Windows: **%userprofile%\AppData\Roaming**
- macOS: **~/Users/$USER/.config**

このフォルダーには、 **PlayerDeviceList**が存在しない場合は、作成する必要があります。

**手順 2:IP アドレスの取得**

Xamarin Live Player アプリで **> 接続テストの概要 > 接続テストを開始**します。

IP アドレスをメモしておきます。デバイスを構成するときに、IP アドレスが表示されます。

**手順 3:ペアリングコードを取得する**

Xamarin Live Player 内で、**ペアリング**または**ペアをもう一度**タップし、Enter キーを**手動で**押します。 数値のコードが表示されます。構成ファイルを更新する必要があります。

**手順 4:GUID の生成**

移動: https://www.guidgenerator.com/online-guid-generator.aspx 新しい guid を生成して、大文字のことを確認します。

**手順 5: デバイスの構成**

Visual Studio や Visual Studio Code などのエディターで**PlayerDeviceList**を開きます。 このファイルでデバイスを手動で構成する必要があります。 既定では、ファイルには、次の`Devices`空の XML 要素が含まれている必要があります。

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Android デバイスの追加:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Visual Studio を閉じて再度開きます。** デバイスが一覧に表示されます。

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE で "型または名前空間が見つかりません" というメッセージが表示される

デバイスの種類に一致する**スタートアッププロジェクト**が選択されていることを確認します (例 Android) と、構成がそのデバイスの種類と一致している (例 Android 用に**デバッグ**)。

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>"型のコンストラクターが見つかりません" というメッセージが表示されます。

一部のシステムクラスはオーバーライドできません。次に例を示します。

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs:' Resource. Layout ' に ' Main ' の定義が含まれていません

このエラーは、AXML ファイルで定義されたユーザーインターフェイスを持つ Android プロジェクトに対して発生します。
現在、Xamarin Live Player では、AXML ファイルはサポートされていません。

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android のツールバーとタブが、Xamarin. フォームを使用して正しく表示されない

Xamarin. Forms Android プロジェクトでは、関連するレイアウトファイルの名前に "Toolbar. axml" と "Tabbar. axml" を使用する必要があります。 既定のテンプレートでは、これらの名前が使用されます。名前を変更すると、レンダリングの問題が発生します。

## <a name="related-links"></a>関連リンク

- [セットアップ](~/tools/live-player/install.md)
