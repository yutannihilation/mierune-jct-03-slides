# 自分で使う用の Slidev テンプレート

このままでは GitHub Pages にデプロイされないので以下の手順が必要。

- Settings > Pages > Build and deployment の「Source」を GitHub Actions に
- `bun install` を実行して、生成された `bun.lock` をコミットしてプッシュ

## ロゴの出典・ライセンス

スライド内で使用しているロゴの出典は以下のとおり。

- **DuckDB logo** (`src/DuckDB_icon-lightmode.svg`):
  [DuckDB Design](https://duckdb.org/design/) より取得。
  DuckDB is a trademark of the DuckDB Foundation.
  ロゴは DuckDB のソフトウェアライセンス（MIT）の対象外であり、
  [DuckDB trademark guidelines](https://duckdb.org/trademark_guidelines) に従って無改変で使用している。
- **PostGIS logo** (`src/postgis-logo-horizontal.png`):
  [postgis.net](https://postgis.net/) より取得。
  postgis.net のコンテンツは [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) でライセンスされている。
- **Apache Sedona logo** (`src/sedona_logo_symbol.png`):
  [apache/sedona-db](https://github.com/apache/sedona-db/blob/main/docs/image/sedona_logo_symbol.png) リポジトリ
  （[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)）より取得。
  Apache Sedona, Sedona, Apache, and the Apache feather logo are trademarks of
  [The Apache Software Foundation](https://www.apache.org/).
