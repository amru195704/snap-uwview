# forum.snapcraft.io 依頼文（uwview: uvf の自動エイリアス＋removable-media の自動接続）

投稿先: https://forum.snapcraft.io/ → カテゴリ **store-requests**
タイトル: `Auto-alias "uvf" and auto-connect removable-media for uwview`

---

Hello,

I'm the publisher of the **uwview** snap (https://snapcraft.io/uwview), a viewer and search tool for large text and log files. It ships a GUI app (`uwview`) and a command-line app (`uwview.uvf`) that share one engine.

I'd like to request two things:

**1. Auto-alias `uvf` → `uwview.uvf`**

`uvf` is the command's upstream name. It's the name used in the documentation, the Homebrew and Scoop packages, and the release archives on GitHub (https://github.com/amru195704/UwView/releases). Users type `uvf PATTERN FILES`, as with grep. Right now they have to run `uwview.uvf` or set `sudo snap alias uwview.uvf uvf` by hand. I'm not aware of any other snap that uses `uvf`.

**2. Auto-connect `removable-media`**

The tool is used on large log files and data dumps. These often sit on external drives (/media, /mnt) because of their size, for example tens to hundreds of GB. The app only reads the files the user names on the command line or opens in the GUI. Without the connection, opening a file on an external drive fails, and the user has to know to run `sudo snap connect uwview:removable-media`.

- Snap: uwview (strict confinement, core24, amd64 / arm64)
- Plugs: home, removable-media
- Upstream: https://github.com/amru195704/UwView
- Website: https://uvp.y42u.net/en/

Thank you for your time.

---

## メモ（宣伝部）
- 投稿はオーナー。審査（投票）は通常1週間ほど。
- 承認されたらストア側で設定が入るので、snapcraft.yaml の変更は不要。新しいリビジョンも不要。
- 承認後は Description の「Run the command as uwview.uvf (or set sudo snap alias …)」と「run sudo snap connect …」の部分を短くする（次の版で更新）。
- removable-media は断られることもある。断られた場合は今の説明文のままでよい。
