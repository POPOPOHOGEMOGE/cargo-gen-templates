# Blender MCP セットアップメモ

このプロジェクトには `.mcp.json` が含まれており、Claude Code から Blender を直接操作できます
(テンプレート生成時に `use_mcp = true` を選んだ場合)。

## 前提条件

1. [uv](https://docs.astral.sh/uv/) がインストール済みであること
2. `blender-mcp` がインストール済みであること(下記参照。**マシンごとに初回1回だけ**)
3. Blender が起動しており、MCP アドオンのサーバーが `127.0.0.1:9876` で待ち受けていること

接続先ホスト/ポートは `.mcp.json` の `env`(`BLENDER_HOST` / `BLENDER_PORT`)で変更できます。

## blender-mcp のインストール(未インストールの環境で1回だけ)

`.mcp.json` はユーザーグローバルにインストールされた `blender-mcp` コマンドを呼び出す前提です。未インストールの環境では以下を実行してください:

```powershell
uv tool install --from "git+https://projects.blender.org/lab/blender_mcp.git@v1.0.3#subdirectory=mcp" blender-mcp
```

- 実行ファイルは uv の bin ディレクトリ(例: `C:\Users\<ユーザー名>\.local\bin\blender-mcp.exe`)に置かれます
- 依存パッケージは uv 管理の隔離された venv に閉じるので、他の Python 環境を汚しません
- 更新したいときはタグ(例: `@v1.0.3`)を新しいものに差し替えて `uv tool install --force ...` を再実行

## uv tool install を使いたくない場合の代替(uvx 直接起動)

インストール不要で使いたい場合は、`.mcp.json` の `command` / `args` を以下に書き換えると uvx が都度キャッシュから起動します:

```json
"command": "cmd",
"args": [
  "/c", "uvx",
  "--from", "git+https://projects.blender.org/lab/blender_mcp.git@v1.0.3#subdirectory=mcp",
  "blender-mcp"
]
```

注意点:

- **初回起動はパッケージ取得で20〜30秒かかり、Claude Code の接続タイムアウト(30秒)に引っかかることがある**。失敗したら `/mcp` から再接続すれば2回目以降はキャッシュで数秒で起動する
- タグ(`@v1.0.3`)は必ず付けること。付けないと起動のたびに上流の更新チェックが走り、上流にコミットが入ったタイミングで再インストール(≒タイムアウト)が再発する

## トラブルシューティング

| 症状 | 確認すること |
| --- | --- |
| 接続タイムアウト | Blender 側サーバーが待受中か: `netstat -ano \| Select-String ":9876"` で `LISTENING` が出るか |
| `blender-mcp` が見つからない | `uv tool install` 済みか。uv の bin ディレクトリが PATH に入っているか(`uv tool update-shell`) |
| 初回だけタイムアウト | uvx 直接起動方式の初回取得。`/mcp` で再接続 |
