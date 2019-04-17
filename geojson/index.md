---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: GeoJSON 仕様
description: GeoJSON のスタイルを定義するための仕様について紹介します。

# Author box
# author:
#     title: About Author
#     title_url: #
#     external_url: true
#     description: Author description

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    # prev:
    #     content: Previous page
    #     url: #
    # next:
    #     content: はじめに
    #     url: /tutorial/001/

breadcrumbs:
    - title: GeoJSON 仕様
      url: /geojson/
    # - title: "hello"
    #   URL: "./"
---

## GeoJSON について

GeoJSON とは JSON によって空間情報を扱うためのファイルフォーマットです。

[GeoJSON - Wikipedia](https://ja.wikipedia.org/wiki/GeoJSON)

## Simplestyle について

Simplestyle とは Mapbox 社が公開した GeoJSON にスタイル情報を埋め込むための仕様で、GitHub 等で採用されています。

[smplestyle-spec](https://github.com/mapbox/simplestyle-spec)

[![](https://www.evernote.com/l/ABWRqUPcMf1AwKFp5kH0BSZzScwRaC0TAusB/image.png)](https://github.com/tilecloud/docs.tilecloud.io/blob/master/geojson/example.geojson)

TileCloud の Embed API の GeoJSON 埋め込み機能でも同様に Simplestyle に対応しています。

```html
<div
  class="tilecloud"
  data-key="YOUR-API-KEY"
  data-lat="35.67443"
  data-lng="139.75675"
  data-zoom="15.5"
  data-geojson="https://docs.tilecloud.io/geojson/example.geojson"
  data-marker="off"
></div>
```

<div
  class="tilecloud"
  data-key="YOUR-API-KEY"
  data-lat="35.67443"
  data-lng="139.75675"
  data-zoom="15.5"
  data-geojson="https://docs.tilecloud.io/geojson/example.geojson"
  data-marker="off"
></div>

## Simplestyle のスキーマ

GeoJSON は、以下のようなルート要素を持っています。

```json
{
  "type": "FeatureCollection",
  "features": []
}
```

`features` の中には `feature` オブジェクト（地物 - マーカーなど）が配列で保存されており、それらも含めると以下の通りになります。

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": []
      },
      "properties": {}
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "Polygon",
        "coordinates": []
      },
      "properties": {}
    }
  ]
}
```

`feature` オブジェクトにはいくつかのタイプがあり、それは `geometry.type` で指定されています。

TileCloud の Embed API が対応しているタイプは以下の3種類です。

* `Point`: 特定の座標を示すためのマーカーを設置します。
* `LineString`: 複数の座標を結ぶ線を設置します。
* `Polygon`: 多角形の図形を設置します。

## タイプごとに指定可能なスタイル情報

上述した `Point` などの各 `feature` タイプに対してスタイルを指定するには、それぞれのオブジェクトの `properties` にスタイル情報を埋め込みます。

| プロパティ| 内容| Point | LineString | Polygon |
|---------|---------|-------|------------|---------|
| title          | マーカーの下に表示されるテキスト | ○ |   |   |
| description    | マーカークリック時に表示されるコンテンツ | ○ |   |   |
| marker-size    | マーカーのサイズを `small`、`medium`、`large` のいずれかで指定 | ○ |   |   |
| marker-symbol  | マーカーのアイコン。 | ○ |   |   |
| marker-color   | マーカーの色。例: `#7e7e7e` | ○ |   |   |
| stroke         | 線の色。例: `#555555` | ○ | ○ | ○ |
| stroke-opacity | 線の透視度。例: `1.0` | ○ | ○ |   |
| stroke-width   | 線の太さ。例: `2` | ○ | ○ |   |
| fill           | 塗りつぶし色。例: `#7e7e7e` |   |   | ○ |
| fill-opacity   | 塗りつぶし色の透明度。例: `0.6` | ○ |   | ○ |

* [geojson.io](http://geojson.io/) を使用するとブラウザベースの GUI でスタイル情報を設定することができます。
* `marker-symbol` については、一部のスタイルで意図したとおりのアイコンが表示されないことがあります。