---
title: "Info.plist リファレンス"
ms.topic: article
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2017
ms.openlocfilehash: 7732419af22ab8bd37cbfbf828dc59e48841217f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="infoplist-reference"></a>Info.plist リファレンス

Info.Plist キーの使用の詳細については、「[Working with Security and Privacy](~/ios/app-fundamentals/security-privacy.md)」 (セキュリティとプライバシーの使用) ガイドを参照してください。 

## <a name="location"></a>場所 

ユーザーの場所にアクセスするには、Info.plist への変更も必要になります。 位置データに関連する次のキーを設定する必要があります。 

* **NSLocationWhenInUseUsageDescription**: アプリでやり取りしている間にユーザーの場所にアクセスする場合。 
* **NSLocationAlwaysUsageDescription**: アプリがバック グラウンドでユーザーの場所にアクセスする場合。

## <a name="photos"></a>写真 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>連絡先 

NSContactsUsageDescription 

## <a name="calendar-data"></a>予定表データ 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>リマインダー データ 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Bluetooth 周辺機器 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>マイク 

NSMicrophoneUsageDescription 

## <a name="camera"></a>カメラ 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>HealthKit の読み取り  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>バックグラウンド モード 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 


## <a name="related-links"></a>関連リンク

- [Apple リファレンス ガイド。](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
