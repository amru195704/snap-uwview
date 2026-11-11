# Snap Store: uwview

[English](#english) · [日本語](#日本語)

## English

> **Main repository** (source, releases, docs): https://github.com/amru195704/UwView — this repo only holds the packaging. UwDiff: https://github.com/amru195704/UwDiff

Makes UwView installable from Ubuntu's "Software" app (Snap Store). The snap simply packs the Linux builds (x86_64 / aarch64) from GitHub Releases; nothing is built from source. Building and uploading to the Snap Store run on GitHub Actions (Ubuntu amd64 / arm64 runners), so the whole flow can be driven from a Mac.

What you get: the "UwView" app (menu entry and icon) and the command `uwview.uvf` (`sudo snap alias uwview.uvf uvf` to call it as `uvf`).

### First release (once)

1. **Account**: create an Ubuntu One account at https://snapcraft.io and accept the developer terms.
2. **Register the name**: register `uwview` at https://snapcraft.io/register-snap.
3. **Repository**: create `amru195704/snap-uwview` (public) on GitHub. **Move `workflow-snap.yml` to `.github/workflows/snap.yml`** first (it is kept outside `.github` because our content tooling cannot write there), then push the contents of this folder.
   ```bash
   mkdir -p .github/workflows && git mv -f workflow-snap.yml .github/workflows/snap.yml 2>/dev/null || mv workflow-snap.yml .github/workflows/snap.yml
   ```
4. **Store credentials**: on a Linux box with snapcraft (an Ubuntu VM is fine), run the command below and store the output as the GitHub Actions secret `SNAPCRAFT_STORE_CREDENTIALS` (Settings → Secrets and variables → Actions).
   ```bash
   snapcraft export-login --snaps=uwview \
     --acls package_access,package_push,package_update,package_release -
   ```
5. **Build and upload**: GitHub → Actions → snap → Run workflow. Two snaps (amd64, arm64) are built and published to the **edge** channel.
6. **Test**: on Ubuntu, `sudo snap install uwview --edge`, launch from the menu, and try `uwview.uvf ~/some.log ERROR -open`.
7. **Store listing**: in the snapcraft.io dashboard (My snaps → uwview → Listing) add screenshots, categories (Utilities / Development) and a contact.
8. **Publish**: in Releases, promote the edge revision to **stable**. From here it appears in public search.

### Worth asking for later (Snapcraft forum, store-requests)

- Auto-alias for `uvf` (so `uwview.uvf` can be called as `uvf`).
- Auto-connect of `removable-media` (read external drives without a manual `connect`).

### Bumping the version

Update `version` and the two `source` URLs in `snap/snapcraft.yaml`, commit, run the workflow, test on edge, then promote to stable.

### Limitations (also stated in the store description)

- Readable paths are under the home folder (hidden folders excluded) and /media, /mnt once connected. `/var/log` and the like are not readable.
- Only the YAML syntax has been verified; building and launching the snap are first exercised in steps 5–6 above.

---

### Snap Store: uwdiff (second snap built from this repo)

The snap for [UwDiff](https://github.com/amru195704/UwDiff), UwView's sister tool for comparing two files or folders (command `uvd`). In the store it is a **separate snap named `uwdiff`**, but its definition and workflow live in this repo.

- Definition: `uwdiff/snap/snapcraft.yaml`, `uwdiff/snap/gui/uwdiff.desktop` (UwView's `snap/` is untouched)
- Workflow: `.github/workflows/workflow-snap-uwdiff.yml` (named `snap-uwdiff`; run manually, not triggered by UwView's `v*` tags)
- What you get: the "UwDiff" app and the command `uwdiff.uvd` (`sudo snap alias uwdiff.uvd uvd` to call it as `uvd`)

First release: register `uwdiff` at snapcraft.io (same account); re-export store credentials for both snaps (`--snaps=uwview,uwdiff`) and overwrite `SNAPCRAFT_STORE_CREDENTIALS`; run Actions → **snap-uwdiff**; test with `sudo snap install uwdiff --edge` and `uwdiff.uvd ~/a.log ~/b.log -open`; fill the listing; promote edge to stable. Version bumps work the same way with `uwdiff/snap/snapcraft.yaml`.

Known gaps: no icon yet (add `uwdiff/snap/gui/uwdiff.png`, `icon:` in `snapcraft.yaml` and `Icon=${SNAP}/meta/gui/uwdiff.png` in the `.desktop` once available); UwDiff's release archive unpacks into `UwDiff-<version>/` (UwView unpacks flat), handled in `override-build`; only the YAML syntax has been verified so far.

---

## 日本語

> **本体リポ**（ソース・リリース・ドキュメント）: https://github.com/amru195704/UwView — このリポはパッケージ定義だけです。UwDiff: https://github.com/amru195704/UwDiff

Ubuntu の「ソフトウェア」アプリ（Snap Store）から UwView を入れられるようにする。
GitHub Releases の Linux 版（x86_64・aarch64）をそのまま詰めるだけで、ソースからのビルドはしない。
ビルドと Snap Store への送信は GitHub Actions（Ubuntu の amd64・arm64 ランナー）で行うので、Mac だけで進められる。

入るもの: アプリ「UwView」（メニュー・アイコン付き）とコマンド `uwview.uvf`（`sudo snap alias uwview.uvf uvf` で `uvf` に）。

## 初めて出す（1回だけ）

1. **アカウント**: https://snapcraft.io で Ubuntu One アカウントを作り、開発者規約に同意する。
2. **名前の登録**: https://snapcraft.io/register-snap で `uwview` を登録する。
3. **リポジトリ**: GitHub に `amru195704/snap-uwview`（公開）を作る。**`workflow-snap.yml` を `.github/workflows/snap.yml` に移して**から（宣伝部の道具では `.github` の中に書けないため、外に置いてある）、このフォルダの中身を push する。
   ```bash
   mkdir -p .github/workflows && git mv -f workflow-snap.yml .github/workflows/snap.yml 2>/dev/null || mv workflow-snap.yml .github/workflows/snap.yml
   ```
4. **送信用の鍵**: snapcraft の入った Linux（Ubuntu の VM など）で次を実行し、出てきた文字列を GitHub の
   Settings → Secrets and variables → Actions に `SNAPCRAFT_STORE_CREDENTIALS` という名前で入れる。
   ```bash
   snapcraft export-login --snaps=uwview \
     --acls package_access,package_push,package_update,package_release -
   ```
5. **作って送る**: GitHub の Actions → snap → Run workflow。amd64・arm64 の2本ができて、Snap Store の **edge** に出る。
6. **試す**: Ubuntu で `sudo snap install uwview --edge` → メニューから起動・`uwview.uvf ~/some.log ERROR -open`。
7. **ストアの掲載ページ**: snapcraft.io のダッシュボード（My snaps → uwview → Listing）で、スクリーンショット・カテゴリ（Utilities／Development）・連絡先を入れる。
8. **公開**: ダッシュボードの Releases で edge のリビジョンを **stable** に上げる。ここで一般の検索に出る。

## あとで頼むとよいこと（Snapcraft フォーラムの store-requests）

- `uvf` の自動エイリアス（`uwview.uvf` を `uvf` で呼べるように）。
- `removable-media` の自動接続（外付けドライブを、connect なしで読めるように）。

## 版を上げるとき

`snap/snapcraft.yaml` の `version` と、`source` の URL 2本の版数を書き換えて commit → Actions を走らせる → edge で試して stable へ。

## 制限（説明文にも書いた）

- 読めるのはホームフォルダの下（隠しフォルダを除く）と、connect したときの /media・/mnt。`/var/log` などは読めない。
- 確かめたのは YAML の書式だけ。snap の作成と起動は、上の 5・6 で初めて確かめることになる。

---

# Snap Store: uwdiff（同じリポで作る2本目の snap）

UwView の姉妹ツール [UwDiff](https://github.com/amru195704/UwDiff)（2つのファイル・フォルダを比べる。コマンド `uvd`）の snap。ストアでは UwView とは**別の snap（名前 `uwdiff`）**になるが、定義と GitHub Actions はこのリポにまとめた。

- 定義: `uwdiff/snap/snapcraft.yaml`・`uwdiff/snap/gui/uwdiff.desktop`（UwView の `snap/` には触らない）
- ワークフロー: `.github/workflows/workflow-snap-uwdiff.yml`（名前 `snap-uwdiff`・手で走らせる。UwView のタグ `v*` では動かない）
- 入るもの: アプリ「UwDiff」とコマンド `uwdiff.uvd`（`sudo snap alias uwdiff.uvd uvd` で `uvd` に）

## 初めて出す（1回だけ）

1. **名前の登録**: https://snapcraft.io/register-snap で `uwdiff` を登録する（アカウントは UwView と同じ）。
2. **送信用の鍵を両方に効くものにする**: 今の鍵は `--snaps=uwview` に限ってあるので、作り直して `SNAPCRAFT_STORE_CREDENTIALS` を上書きする。
   ```bash
   snapcraft export-login --snaps=uwview,uwdiff \
     --acls package_access,package_push,package_update,package_release -
   ```
3. **作って送る**: GitHub の Actions → **snap-uwdiff** → Run workflow。amd64・arm64 の2本ができて、Snap Store の **edge** に出る。
4. **試す**: Ubuntu で `sudo snap install uwdiff --edge` → メニューから起動・`uwdiff.uvd ~/a.log ~/b.log -open`。
5. **ストアの掲載ページ**: My snaps → uwdiff → Listing で、スクリーンショット・カテゴリ（Utilities／Development）・連絡先。
6. **公開**: Releases で edge のリビジョンを **stable** に上げる。

あとで頼むとよいこと（store-requests）: `uvd` の自動エイリアス、`removable-media` の自動接続。

## 版を上げるとき

`uwdiff/snap/snapcraft.yaml` の `version` と、`source` の URL 2本の版数を書き換えて commit → Actions の snap-uwdiff を走らせる → edge で試して stable へ。

## 制限・未確認

- 読めるのはホームフォルダの下と、connect したときの /media・/mnt。
- **アイコンが無い**（UwDiff のリポに画像が無い）。用意できたら `uwdiff/snap/gui/uwdiff.png` に置き、`snapcraft.yaml` に `icon: snap/gui/uwdiff.png`、`.desktop` に `Icon=${SNAP}/meta/gui/uwdiff.png` を足す。
- UwDiff の配布物は `UwDiff-<版>/` の下に入っている（UwView は直下）。`override-build` でどちらの形でも `opt/uwdiff` に置く。
- 確かめたのは YAML の書式だけ。snap の作成と起動は、上の 3・4 で初めて確かめる。
