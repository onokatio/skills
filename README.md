# skills

Claude Code と Codex で使う、外部スキルの marketplace です。marketplace 名は既存設定と同じ `other` です。

スキル本体はこのリポジトリにコピーしません。各クライアントがインストール時に参照先から取得し、自身のキャッシュで管理します。

| スキル | 参照先 |
| --- | --- |
| stop-ai-slop-jp | [iKora128/stop-ai-slop-jp](https://github.com/iKora128/stop-ai-slop-jp) |
| claude-real-video | [claude-real-video/skills/claude-real-video-for-agents](https://github.com/HUANGCHIHHUNGLeo/claude-real-video/tree/master/skills/claude-real-video-for-agents) |
| japanese-tech-writing | [k16shikano の Gist](https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d) |
| cognitive-rhythm-writing | [k16shikano の Gist](https://gist.github.com/k16shikano/eb2929f13ed19c97188393d297be8432) |

## Claude Code

```text
/plugin marketplace add onokatio/skills
/plugin install stop-ai-slop-jp@other
/plugin install claude-real-video@other
/plugin install japanese-tech-writing@other
/plugin install cognitive-rhythm-writing@other
```

既存の `extraKnownMarketplaces.other` をインライン設定から移行する場合は、そのエントリを次のように置き換えます。既存の `enabledPlugins` の `スキル名@other` はそのまま使えます。

```json
{
  "other": {
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
codex plugin add stop-ai-slop-jp@other
codex plugin add claude-real-video@other
codex plugin add japanese-tech-writing@other
codex plugin add cognitive-rhythm-writing@other
```

## カタログの管理

編集するのは [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) だけです。Codex もこの形式を読み込めるため、別のカタログや生成処理は不要です。

- GitHub / Gist のリポジトリ全体は `source: "url"`、サブディレクトリは `source: "git-subdir"` で参照します。
- 参照先に plugin manifest がなくても、`strict: false` と `skills: ["./"]` でルートの `SKILL.md` をスキルとして読み込みます。
- `interface` と `policy` は Codex 用のメタデータです。Claude Code の検証では未知のフィールドとして警告されますが、読み込み時には無視されます。
- コミットやバージョンを固定していません。更新の取得とキャッシュは各クライアントの更新機能に従います。
- 各スキルのライセンス、実行に必要なツールや認証の条件は参照先に従います。

形式の根拠: [Claude Code marketplace](https://code.claude.com/docs/en/plugin-marketplaces)、[OpenAI plugin packaging](https://developers.openai.com/plugins/build/plugins)。
