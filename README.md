# skills

Claude Code と Codex で使う、外部スキルと MCP サーバーの marketplace です。表示名は `onokatio plugins`、marketplace 識別子は `onokatio-plugins` です。

スキル本体はこのリポジトリにコピーしません。各クライアントがインストール時に参照先から取得し、自身のキャッシュで管理します。

MCP サーバーはカタログに起動設定だけを定義し、実装は公式パッケージから取得します。

| プラグイン | 参照先 |
| --- | --- |
| gh-stack | [github/gh-stack/skills/gh-stack](https://github.com/github/gh-stack/tree/main/skills/gh-stack) |
| stop-ai-slop-jp | [iKora128/stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) |
| claude-real-video | [claude-real-video/skills/claude-real-video-for-agents](https://github.com/HUANGCHIHHUNGLeo/claude-real-video/tree/master/skills/claude-real-video-for-agents) |
| japanese-tech-writing | [k16shikano の Gist](https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d) |
| cognitive-rhythm-writing | [k16shikano の Gist](https://gist.github.com/k16shikano/eb2929f13ed19c97188393d297be8432) |
| bitwarden (MCP) | [bitwarden/mcp-server](https://github.com/bitwarden/mcp-server) |

## Claude Code

```text
/plugin marketplace add onokatio/skills
/plugin install gh-stack@onokatio-plugins
/plugin install stop-ai-slop-jp@onokatio-plugins
/plugin install claude-real-video@onokatio-plugins
/plugin install japanese-tech-writing@onokatio-plugins
/plugin install cognitive-rhythm-writing@onokatio-plugins
/plugin install bitwarden@onokatio-plugins
```

旧識別子 `other` から移行する場合は、`extraKnownMarketplaces.other` を次の `onokatio-plugins` エントリに置き換えます。`enabledPlugins` のキーも `プラグイン名@other` から `プラグイン名@onokatio-plugins` に変更し、新しい識別子でプラグインをインストールしてください。

```json
{
  "onokatio-plugins": {
    "source": {
      "source": "github",
      "repo": "onokatio/skills"
    }
  }
}
```

## Codex

```sh
codex plugin marketplace add onokatio/skills
codex plugin add gh-stack@onokatio-plugins
codex plugin add stop-ai-slop-jp@onokatio-plugins
codex plugin add claude-real-video@onokatio-plugins
codex plugin add japanese-tech-writing@onokatio-plugins
codex plugin add cognitive-rhythm-writing@onokatio-plugins
codex plugin add bitwarden@onokatio-plugins
```

旧識別子 `other` で登録済みの場合も、上記のコマンドで marketplace を追加し、新しい識別子でプラグインをインストールしてください。

## Bitwarden MCP

`bitwarden` はスキルではなく、`npx -y @bitwarden/mcp-server` をローカルの stdio サーバーとして起動するプラグインです。

- Node.js 22 以上と npm/npx が必要です。保管庫の操作には Bitwarden CLI (`bw`) をインストールし、`bw login` でログインしておきます。
- ロック中は MCP の `unlock` ツールから OS のパスワード入力ダイアログで解除できます。セッショントークンなどの認証情報はカタログに含めません。
- 組織管理 API やファイル操作などの追加設定は [公式 README](https://github.com/bitwarden/mcp-server#readme) を参照してください。

## カタログの管理

編集するのは [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) だけです。Codex もこの形式を読み込めるため、別のカタログや生成処理は不要です。

- GitHub / Gist のリポジトリ全体は `source: "url"`、サブディレクトリは `source: "git-subdir"` で参照します。
- 参照先に plugin manifest がなくても、`strict: false` と `skills: ["./"]` でルートの `SKILL.md` をスキルとして読み込みます。
- MCP サーバーは `strict: false` と `mcpServers` で起動コマンドを定義します。
- `interface` と `policy` は Codex 用のメタデータです。Claude Code の検証では未知のフィールドとして警告されますが、読み込み時には無視されます。
- コミットやバージョンを固定していません。更新の取得とキャッシュは各クライアントの更新機能に従います。
- 各プラグインのライセンス、実行に必要なツールや認証の条件は参照先に従います。

形式の根拠: [Claude Code marketplace](https://code.claude.com/docs/en/plugin-marketplaces)、[OpenAI plugin packaging](https://developers.openai.com/plugins/build/plugins)。
