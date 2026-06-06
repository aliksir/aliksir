# aliksir

AI コーディングエージェント（Claude Code）を安全・確実に運用するためのツールを作っています。ほとんどが依存ゼロの Node.js か軽量 CLI です。
Tools to run AI coding agents (Claude Code) safely and reliably. Most are zero-dependency Node.js or lightweight CLIs.

## どれから使う？ / Where to start

| やりたいこと | ツール |
|------------|--------|
| 顧客資料を安全に AI に読ませたい | [pii-mask-yoshi](https://github.com/aliksir/pii-mask-yoshi) |
| PDF/Word/Excel を Markdown に変換したい | [markitdown-yoshi](https://github.com/aliksir/markitdown-yoshi) |
| Claude Code をマルチエージェントで安全運用 | [neko-gundan](https://github.com/aliksir/neko-gundan) |
| ブラウザ操作 MCP を安全に使う | [secure-browser-mcp](https://github.com/aliksir/secure-browser-mcp) |
| MCP 通信を監視する | [mcp-yoshi](https://github.com/aliksir/mcp-yoshi) |
| 公開前に顧客名・個人情報を検査する | [neko-not-yoshi](https://github.com/aliksir/neko-not-yoshi) |
| Claude Code 環境を診断する | [neko-harness-doctor](https://github.com/aliksir/neko-harness-doctor) |
| セッションの知識を次回に引き継ぐ | [context-distill](https://github.com/aliksir/context-distill) |
| 会話ログを残す | [conversation-log](https://github.com/aliksir/conversation-log) |
| 固まった Claude Code から会話を救出する | [neko-rescue](https://github.com/aliksir/neko-rescue) |
| npm 供給網攻撃を検知する | [npm-postinstall-attack-scanner](https://github.com/aliksir/npm-postinstall-attack-scanner) |
| Next.js / React の脆弱性を検査する | [nextjs-security-scanner](https://github.com/aliksir/nextjs-security-scanner) |

## 推奨構成プリセット / Recommended Presets

用途に合わせて組み合わせるなら、まずこのプリセットから。
Pick a preset that fits your use case.

| Preset | Tools | Use case |
|--------|-------|----------|
| **Minimal** | [neko-gundan](https://github.com/aliksir/neko-gundan) + [neko-not-yoshi](https://github.com/aliksir/neko-not-yoshi) | AI エージェントの品質ルール + 漏洩検査 |
| **Secure** | Minimal + [mcp-yoshi](https://github.com/aliksir/mcp-yoshi) + [pii-mask-yoshi](https://github.com/aliksir/pii-mask-yoshi) | MCP 通信監視 + PII マスク付き多層防御 |
| **Enterprise** | Secure + [neko-harness-doctor](https://github.com/aliksir/neko-harness-doctor) + [context-distill](https://github.com/aliksir/context-distill) + [skill-validator](https://github.com/aliksir/skill-validator) | 環境診断 + セッション引継 + スキル監査 |
| **Document AI** | [markitdown-yoshi](https://github.com/aliksir/markitdown-yoshi) + [pii-mask-yoshi](https://github.com/aliksir/pii-mask-yoshi) + [mcp-yoshi](https://github.com/aliksir/mcp-yoshi) | PDF/Office 変換 + PII 保護 |
| **Recovery** | [yoshi](https://github.com/aliksir/yoshi) (rescue) + [conversation-log](https://github.com/aliksir/conversation-log) + [context-distill](https://github.com/aliksir/context-distill) | セッション救出 + ログ保存 + 知識引継 |

## PII 防御チェーン / PII Protection Chain

顧客情報・機密データを扱う場合のツール連携。各層は独立して動き、組み合わせで多層防御になる。

```
                          ┌─────────────────────────┐
                          │ Claude Code (Read/Grep)  │
                          └────────┬────────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │  neko-not-yoshi              │
                    │  顧客名・NGワードリスト管理   │
                    │  push 前の漏洩検査にも使用   │
                    └──────┬───────────┬──────────┘
                           │           │
              パターン辞書提供│           │ push 前検査
                           │           │
           ┌───────────────▼───┐   ┌───▼──────────────────┐
           │  pii-mask-yoshi   │   │  git hook / CI       │
           │  safe_read で     │   │  コミット前に         │
           │  PII マスク       │   │  顧客名混入ブロック   │
           │  (24 内蔵パターン) │   └──────────────────────┘
           └───────┬───────────┘
                   │ マスク済みテキスト
                   │
           ┌───────▼───────────┐
           │  mcp-yoshi        │
           │  MCP 通信フィルター │
           │  API key 漏洩 /    │
           │  インジェクション   │
           │  検出              │
           └───────────────────┘
```

## その他のツール / More

- スキル管理 / skill tooling: [skill-validator](https://github.com/aliksir/skill-validator) · [skill-memo](https://github.com/aliksir/skill-memo)
- コミュニティスキルの監査 / community skill audit: [claude-code-skill-security-check](https://github.com/aliksir/claude-code-skill-security-check)
- 教訓→スキル自動化 / lessons to skills: [lesson-skill-loop](https://github.com/aliksir/lesson-skill-loop)
- 付箋メモ / sticky notes: [memo-yoshi](https://github.com/aliksir/memo-yoshi)
- 秘密情報の取り回し / secrets: [secret-store](https://github.com/aliksir/secret-store)
- CLI 非依存の品質ルール / CLI-agnostic quality rules: [other-neko-gundan](https://github.com/aliksir/other-neko-gundan)
