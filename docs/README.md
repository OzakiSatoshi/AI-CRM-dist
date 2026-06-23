# AI-CRM 配布サイト

公開用ランディングページ + インストーラー配布のための静的サイト一式。

| ファイル | 役割 |
|---|---|
| `index.html` | ランディングページ。ダウンロードボタン・機能紹介・変更履歴 |
| `style.css` | スタイル（依存なしの素の CSS） |
| `version.json` | 最新バージョン・DL URL・リリースノート（アプリ／サイト双方が参照） |

## なぜ配布専用の Public リポジトリが必要か

ソース本体リポジトリ `OzakiSatoshi/AI-CRM` は **Private**。Private のままだと:

- GitHub Pages の公開には GitHub Pro 以上が必要。
- Releases の資産ダウンロードに認証が必要 → 一般ユーザーが DL できない。

そこで **配布専用の Public リポジトリ `OzakiSatoshi/AI-CRM-dist`** を新設し、
ソースは非公開のまま、サイトと配布バイナリ（インストーラー）のみを公開する。
アプリ内の更新チェック（`UpdateService`）もこの Public リポジトリの Releases を参照する。

## 初回セットアップ手順

1. GitHub で Public リポジトリ `AI-CRM-dist` を作成する。
2. このフォルダ（`site/`）の中身をリポジトリ直下の `docs/` に配置して push する。
   ```powershell
   # 例: 配布リポジトリをクローンした作業フォルダで
   #   site/ の中身を docs/ にコピーして push
   ```
3. リポジトリの **Settings → Pages** で、Source = `Deploy from a branch`、
   Branch = `main` / フォルダ = `/docs` を選択して保存。
4. 数分後、`https://ozakisatoshi.github.io/AI-CRM-dist/` で公開される。
5. 初回リリースは `/release` の手順5（`gh release create`）でインストーラーをアップロードする。

> リポジトリ名・オーナー名を変更した場合は、アプリ側 `src/AI-CRM.Core/Services/UpdateService.cs`
> の `Owner` / `Repo` 定数、本サイトの各 URL、`version.json` の URL も合わせて更新すること。

## 公開フォルダについて

GitHub Pages は `/docs` 配下を公開ルートにできる。本リポジトリ（ソース側）では編集を `site/`
で行い、リリース時に配布リポジトリの `docs/` へ反映する運用とする（`/release` の手順6）。
