---
タイトル: ' Xamarin.Forms マップの位置と距離 ' 説明: ' Xamarin.Forms 。Maps 名前空間には、マップとそのピンを配置するときに通常使用される位置構造体と、マップを配置するときに必要に応じて使用できる距離構造体が含まれます。
ms. 製品: ms. assetid: ms. テクノロジ: 作成者: ms. 作成者: ms. 日付: なし:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinforms-map-position-and-distance"></a>Xamarin.Formsマップの位置と距離

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

名前空間には、 [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) [`Position`](xref:Xamarin.Forms.Maps.Position) マップとそのピンを配置するときに通常使用される構造体と、 [`Distance`](xref:Xamarin.Forms.Maps.Distance) マップを配置するときに必要に応じて使用できる構造体が含まれています。

## <a name="position"></a>位置

[`Position`](xref:Xamarin.Forms.Maps.Position)構造体は、緯度と経度の値として格納されている位置をカプセル化します。 この構造体は、次の2つの読み取り専用プロパティを定義します。

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)型の `double` 。これは、小数点以下の位置の緯度を表します。
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)型の `double` 。これは、小数点以下の位置の経度を表します。

[`Position`](xref:Xamarin.Forms.Maps.Position)オブジェクトはコンストラクターを使用して作成されます。このコンストラクターには、 `Position` 値として指定された緯度と経度の引数が必要です `double` 。

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

オブジェクトを作成するとき `Position` 、緯度の値は-90.0 と90.0 の間でクランプされ、経度の値は-180.0 と180.0 の間で固定されます。

> [!NOTE]
> `GeographyUtils`クラスには、 `ToRadians` `double` 値を度数からラジアンに変換する拡張メソッドと、値を `ToDegrees` ラジアンから度に変換する拡張メソッドがあり `double` ます。

## <a name="distance"></a>Distance

[`Distance`](xref:Xamarin.Forms.Maps.Distance)構造体は、値として格納されている距離をカプセル化し `double` ます。これは、メートル単位の距離を表します。 この構造体は、次の3つの読み取り専用プロパティを定義します。

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)に `double` よってスパンされる距離 (キロメートル単位) を表す、型の `Distance` 。
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)に `double` よってスパンされるメートル単位の距離を表す、型の `Distance` 。
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)によって分散され `double` ている距離 (マイル単位) を表す、型の `Distance` 。

[`Distance`](xref:Xamarin.Forms.Maps.Distance)オブジェクトは、 `Distance` 次のように指定されたメーター引数を必要とするコンストラクターを使用して作成できます `double` 。

```csharp
Distance distance = new Distance(1450.5);
```

また、、、 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 、およびの各ファクトリメソッドを使用してオブジェクトを作成することもでき [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*) [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*) [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*) `BetweenPositions` ます。

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
