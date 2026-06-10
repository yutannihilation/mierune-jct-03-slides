---
title: DuckDB vs SedonaDB の現在地
theme: default
background: /background.png
fonts:
  sans: Zen Kaku Gothic New
  mono: Fira Code
  weights: 700,900
class: text-center
mdc: true
favicon: /favicon.ico
# seoMeta:
#  ogImage: https://cover.sli.dev
---

# DuckDB<br/>vs<br/>SedonaDB<br/>の現在地

@yutannihilation

---
layout: image-right
image: "/icon.jpg"
backgroundSize: 70%
---

# ドーモ！

## 名前:

湯谷啓明 (@yutannihilation)

## 自己紹介:

- 札幌在住（12日目）

---
layout: section
---

# 基礎知識

---

# 今日の登場人物

<div class="flex justify-center items-center gap-16 h-4/5 text-8xl">
<v-clicks>
    <div class="h-40 w-40">🐘</div>
    <div class="h-40 w-40">🦆</div>
    <div class="h-40 w-40">⛰️</div>
</v-clicks>
</div>

---

# 今日の登場人物

<div class="flex justify-center items-center gap-16 h-4/5">
    <img src="/src/postgis-logo-horizontal.png" class="h-40 object-contain" />
    <img src="/src/DuckDB_icon-lightmode.svg" class="h-40 object-contain" />
    <img src="/src/sedona_logo_symbol.png" class="h-40 object-contain" />
</div>

---

# 🐘PostGIS

<img src="/src/postgis-logo-horizontal.png" class="absolute top-8 right-8 h-30 object-contain" />

- PostgreSQL で GIS データを扱える<br/>ようにする拡張
- 関数が豊富
  - PostGIS の挙動がデファクトスタンダード
- ベクターもラスターも扱える

---

# 🦆DuckDB

<img src="/src/DuckDB_icon-lightmode.svg" class="absolute top-8 right-8 h-30 object-contain" />

- ポータビリティに優れている
  - シングルバイナリ
  - ブラウザでも動く
  - 古いマシンでも動く
  - もちろん Python や R からも使える
- GIS 専用ではないが `spatial` 拡張がある
- v1.5 で`GEOMETRY`型が本体に入った

---

# ⛰️SedonaDB

<img src="/src/sedona_logo_symbol.png" class="absolute top-8 right-8 h-30 object-contain" />

- GIS のために設計されたクエリエンジン
- Python や R から使うのがメイン（CLI もある）
- Sedona（分散処理エンジン）と同じ API
  - ローカルでは重すぎる処理はクラウド上にスケールしたりできる
- ベクターもラスター（開発中）も扱える
- Wherobots が開発

---

# Sedona？

- Apache の大規模地理空間データ処理エンジン<br/>（旧 GeoSpark）
- Spark や Flink などの上で動く分散処理
- 何十億行ものデータをクラスタで一気に処理できる

<v-click>
<div class="text-center text-red-500 text-5xl">
→ その Sedona と同じ API を、<br/>ローカル一台で使えるようにしたのが SedonaDB
</div>
</v-click>


---
layout: section
---

# 注目点①<br/>データベースサーバーが<br/>必要かどうか

---

# セットアップ

- PostGIS:
  - データベースサーバーが必要
  - ストレージも必要
  - 一度データをインポートしないと使えない
- DuckDB・SedonaDB:
  - ダウンロードすればすぐ使える
  - Parquet や GeoPackage などをすぐ読める

---

# データの編集・保存

- PostGIS:
  - 一度インポートしてしまえば、あとは自由にデータを操作できる
- DuckDB・SedonaDB:
  - インメモリでデータを操作し、永続化のためにはファイル書き出しが必要になる

<v-click>

<div class="text-center text-red-500 text-5xl">
→ 細かい変更が頻繁にあるような<br/>ワークロードなら PostGIS！
</div>

</v-click>

---
layout: section
---

# 注目点②<br/>ラスターデータが<br/>扱えるかどうか

---

# ラスターデータ

- PostGIS:
  - 扱える
- DuckDB:
  - コミュニティ拡張で扱える
- SedonaDB:
  - 扱える（開発中）

---

# 難点

- PostGIS:
  - インポート作業が必要（out-db ラスターであっても、データ登録は必要）
  - 多次元ラスターが扱えない
- DuckDB:
  - 本体に入っていないので、pushdown などがベクターデータほど効率的ではない？

---
layout: image
image: /src/pushdown.png
---

---

# DuckDB が厳しそうな点

- pushdown の「メタデータを読んでいい感じに実行計画を立てる」というのは内部の仕組みなので、拡張では難しい...？
- 例外：RaQuet（Parquet にラスターデータを突っ込むというフォーマット）の場合は、Parquet なのでいけるかも

---
layout: section
---

# 注目点③<br/>関数の豊富さ

---

# 関数の豊富さ

- PostGIS:
  - 一番関数が多い
- DuckDB:
  - ほぼ追いついた？
  - ただ、あまりテストされてなくてやや不安...
- SedonaDB:
  - まだ少なめ

---
layout: section
---

# 使い分けまとめ

---

# 使い分けまとめ

## PostGIS

- 細かい変更が頻繁にあるとき

## DuckDB

- セットアップ不要で使えるツールが欲しいとき
- ブラウザで使いたいとき

## SedonaDB

- Python や R から巨大な GIS データを扱いたいとき

---
layout: section
---

# DuckDB の<br/>最近のアップデート

---

# DuckDB v1.5（今年3月）

- `GEOMETRY` 型が本体に入った
  - CRS が保持される
- `GEOMETRY` 型の統計情報を使って pushdown できるようになった
  - Parquet のネイティブ `GEOMETRY` 型

<div class="text-center text-red-500 text-5xl">
→ ベクターならもう DuckDB で<br/>困ることはあまりないはず
</div>

---

# DuckDB の CRS

- これまで：元データの CRS が無視されるので、クエリを書く人が覚えておく必要があった。
  ```sql
  ST_Transform(geom, 'EPSG:2451', 'OGC:CRS84')
  ```
- v1.5〜：元データの CRS が自動で使われる！
  ```sql
  ST_Transform(geom, 'OGC:CRS84')
  ```

---

# DuckDB の CRS

- これまで：`SRS`オプションで指定する必要があった。
  ```sql
  COPY foo TO 'foo.gpkg' WITH (FORMAT GDAL,
    DRIVER 'Gpkg', SRS 'EPSG:2451');
  ```
- v1.5〜：なしで大丈夫！
  ```sql
  COPY foo TO 'foo.gpkg' WITH (FORMAT GDAL,
    DRIVER 'Gpkg');
  ```

---
layout: section
---

# SedonaDB の<br/>最近のアップデート

---

# SedonaDB v0.3（今年3月）

- JOINの効率化
- LAS/LAZ をサポート（とりあえず読める）
- GPU による高速化（実質まだ使えない）

---

# SedonaDB v0.4（もうすぐ？）

- LAS/LAZ のサポートの強化（chunk statistics を使った pushdown など）
- Zarr をサポート
- GeoParquet 2.0対応

---

# SedonaDB のラスター関数（RS_*）

- ピクセル値を読む関数はまだない
  - Sedona にはあるので、そのうち追加されるはず...？
  - `RS_PixelAsPolygon()` や `RS_WorldToRasterCoord()` でピクセルを `POINT` に変換することはできる
- ラスターとベクターの JOIN はできる

---

# SedonaDB のラスター関数（RS_*）

- `RS_Intersects()`  
  `RS_Contains()`  
  `RS_Within()`:  
  CRS が違っても自動で変換して比較してくれる
- `RS_Envelope()`  
  `RS_ConvexHull()`:  
  範囲をポリゴンとして取得

---

# SedonaDB のラスター関数（RS_*）

- まだあまり面白いことはできなそう！

---

# Zarr を読み込むのはこんな感じ

```py
con = sedonadb.connect()

df = con.read_format(
    sedonadb_zarr.ZarrFormatSpec().with_options({
        # 必要なバンドだけを指定
        "arrays": ["temperature", "pressure"]
    }),
    "file:///path/to/data.zarr"
)
# my_zarr_data として SQL から参照できるようにする
df.to_view('my_zarr_data')
```

---

# Zarr を読み込むのはこんな感じ

```py
con.sql("""
    SELECT 
        RS_Width(raster) as width,
        RS_Height(raster) as height,
        RS_NumBands(raster) as num_bands,
        RS_SRID(raster) as srid
    FROM my_zarr_data
""").show()
```

---

# ベクターと JOIN したりもできる

```py
con.read_parquet("...").to_view('my_points')

con.sql("""
    SELECT 
        p.city_name,
        p.population,
        z.raster.outdb_uri as chunk_uri
    FROM my_zarr_data z, my_points p
    WHERE 
        ST_Intersects(p.geometry, RS_Envelope(z.raster))
""").show()
```

---

# 感想

- なんだかんだ DuckDB が強すぎる
- でも SedonaDB の方が開発がオープンなのでがんばってほしい

---

# References

## ロゴ

- DuckDB logo: https://duckdb.org/design/
- PostGIS logo: https://postgis.net/
- Sedona logo: https://github.com/apache/sedona-db/blob/main/docs/image/sedona_logo_symbol.png
