# ORRERY ワークショップ

2026 年 10 月 7 日、大阪大学システム科学ゼミナール用の資料です。全編英語・60 分・最大 136 名を想定しています。[English](README.md)

install → **Your first flight** → [しりとり](play/shiritori.md) の順で、基本操作、子の起動、Mail の往復を確認します。続いて論文ノート、または好きな遊びを試してください。掲載 prompt はすべて **Not yet tested on a fresh install**。所要時間は目安で、実測値ではありません。

## 当日の流れ

| 分 | 内容 |
| --- | --- |
| 0–15 | coding agent・指示・permission・ORRERY の説明 |
| 15–25 | install、CLI のログイン、Your first flight |
| 25–30 | [しりとり](play/shiritori.md)で install の確認 |
| 30–45 | [論文 1 本からノート](play/paper-to-note.md) |
| 45–60 | 自由に遊ぶ・結果を比べる・質問 |

## Install

Mac はターミナル、Windows は WSL2 の Ubuntu 内で実行します。PowerShell に貼らないでください。[上流の手順](https://github.com/gyroid-eth/orrery/blob/master/docs/en/install.md)で前提条件を確認し、Claude Code または Codex の CLI を少なくとも一方、ログイン済みにします。論文ノートを別会社のモデルで確かめるには両方が必要です。WSL では Ubuntu 内の Linux CLI が必要です。

```bash
curl -fsSL https://raw.githubusercontent.com/gyroid-eth/orrery/master/scripts/get.sh | bash
```

上流の installer を取得して実行します。表示される計画を読んで承認し、doctor と Mail の確認結果を確かめてください。ORRERY の install と CLI のアカウント認証は別です。

## Your first flight のあと、しりとり

初回に右側へ出る7項目の **Your first flight** を通します。最初の項目では **NEW AGENT** から利用できるモデルを選び、空の作業フォルダで起動します。その agent を選び、端末の下の入力欄から短い挨拶を送って、画面の案内を続けます。Mail を読んだ項目は `Mark as read` で確認します。閉じている場合は **Settings → Getting started → Your first flight** から開けます。

続いて同じ agent に[しりとりの Prompt](play/shiritori.md)を貼ります。初回ガイドは基本操作を知るため、しりとりの実 Mail 3往復は委任と通信が通るかを確認するためです。論文のときは研究セットが表示した demo vault を作業フォルダにします。

自由に試す時間には **Settings → Getting started → Show help map** で主要部品の注記を見回し、**Full tour** の16段で子とのゲーム、端末の配置、Telemetry の EXIT / RESUME・replay を試せます。Full tour にも同じしりとり Prompt があり、新しいゲームを観測するので、本編の後に通すならもう3往復が必要です。後で試しても構いません。現在の入口は[上流のクイックスタート](https://github.com/gyroid-eth/orrery/blob/master/README.md#クイックスタート)を参照してください。

API credit の購入や外部への公開を要求する遊びはありませんが、CLI の既存アカウントの利用枠は消費します。公開・架空の入力を使ってください。モデルに渡した内容は選択した会社へ送られる場合があります。

## 入れられない場合

[ブラウザのデモ](https://agentstack-demo.pages.dev/)を見るか、隣の人の画面で参加できます。デモを見るだけでは自分の CLI・Mail・委任が動いた証拠にはなりません。

## 遊びと資料

- [しりとり](play/shiritori.md): 本編。子 1 体と Mail で 3 往復（5 分）。
- [人狼](play/werewolf.md): GM＋4 人、1 日、昼の発言 2 回（10–15 分）。
- [論文ノート](play/paper-to-note.md): Claude が書き、Codex が本文と図を確かめる（10–15 分）。
- [2 人のレビュー](play/two-reviewers.md): 独立の指摘を比べ、Mail で確かめ合う（5–10 分）。
- [分担して統合](play/split-the-work.md): ファイルの担当を分け、並行で作り、統合する（10–15 分）。

英語の [agent の説明](guide/what-is-a-coding-agent.md)、[cockpit と Telemetry](guide/cockpit-and-telemetry.md)、[設定と秘密の守り方](guide/settings.md)、[FAQ](guide/faq.md)、[試走記録の書式](tests/README.md)を用意しています。

設定は 2026 年 10 月 3 日の公式 docs を根拠にしています。OS・版・アカウントにより違うため、講演直前にも確認してください。ライセンスは ORRERY Telemetry と同じ [PolyForm Perimeter 1.0.1](LICENSE) です。
