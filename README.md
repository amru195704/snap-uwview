# Snap Store: uwview

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
