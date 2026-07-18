# cargo-gen-templates

## blender
### local
cargo generate --path ./cargo-gen-templates blender --name my-pj
### github
cargo generate --git https://github.com/POPOPOHOGEMOGE/cargo-gen-templates blender --name my-pj

生成時に `use_mcp` を聞かれる。true にすると Claude Code 用の `.mcp.json` と `MCP_SETUP.md`(blender-mcp のセットアップ手順)が含まれる。プロンプトなしで指定する場合:

    cargo generate --git https://github.com/POPOPOHOGEMOGE/cargo-gen-templates blender --name my-pj -d use_mcp=true
