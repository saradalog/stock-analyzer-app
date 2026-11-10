# AGENTS.md — saradalog/stock-analyzer-app（銘柄分析アプリの画面）

このファイルは、このリポジトリを編集するAI（Codex・Gemini・Copilot・Claude など）向けの説明です。
持ち主は日本語で指示します。画面の文言・説明・コミットメッセージは日本語で書いてください。

## これは何か

日本株の銘柄分析アプリ（PWA）の画面です。GitHub Pages（`main` ブランチのルート）で https://saradalog.github.io/stock-analyzer-app/ に配信しています。
**画面のコードはこのリポジトリだけにあります**（データを作る側は非公開リポジトリ `saradalog/analysis`）。

| ファイル | 役割 |
|---|---|
| `index.html` | アプリ本体。HTML・CSS・JavaScript を1枚にまとめている（ビルド不要） |
| `sw.js` | 画面部品のオフライン用キャッシュ。**画面を直したら `CACHE` の版（`analyzer-shell-vN`）を1つ上げる**（上げないとスマホに古い画面が残る） |
| `manifest.webmanifest`, `icons/` | ホーム画面に追加したときの名前とアイコン |

## データの受け取り方

- 設定画面（⚙）に入れた Apps Script の URL（`…/exec`）と合言葉で `fetch` する。URLと合言葉は端末の localStorage（`ma_cfg`）にだけ保存し、コードに書かない。
- `?action=list` → 全銘柄の `[コード, 名前, 業種, 市場]`、`?action=get&code=7208` → 1銘柄のJSON。
- JSON の形は `saradalog/analysis` の `AGENTS.md`（「タブ app の JSON」）に書いてある。キー名を変えるときはデータ側と同時に直す。
- 設定が無いときは `DEMO`（7208・8088 の試作データ）で動く。

## 画面の中身

- 銘柄画面のタブ：業績・指標・理論株価・競合・独自予想（`d.own` があるときだけ）。
- 独自予想の計算は `ownCalc`（モデルは `analysis/forecast/models/*.json`）。`mode:"pool"` は利益の積み上げ型。前提の変更は localStorage（`ma_own_<コード>`）に保存。
- ★ でお気に入り一覧（`favList`）。並べ替えの定義は `SORTS`。
- グラフは Chart.js 4.4.1（cdnjs）。高さは `.cw`（220px固定）で、`maintainAspectRatio=false`。

## 確かめ方

- `python3 -m http.server` などで開くと、設定なしなら試作データで確認できる。
- スマホ幅（390px前後）で崩れないか確認する。表は横スクロール（年の表は右端＝最新期に寄せる）。
- 公開すると誰でも見られるので、合言葉・URL・個人の情報をコードに入れない。
