# 強化学習

- Kaggle PTCG AI Battle Challenge の続き（[ptcg-abc](https://github.com/uents/ptcg-abc) での行動クローニング → PPO の実装、上位解法の再現）を自分の判断で進めるための学習ノート
- 「AI に説明させる」のではなく、教科書と実装を自分で突き合わせて、判断の土台を自分に残すことを目的とする
- 関連イシュー: [tech-notes#7](https://github.com/uents/tech-notes/issues/7)

## 目次

| 章 | タイトル | 主な問い |
|---|---|---|
| 1 | [MDP と TD 学習](01-mdp-and-td.md) | 状態・行動・報酬をどう定式化するか。価値とは何か |
| 2 | [方策勾配法](02-policy-gradient.md) | 方策を直接学ぶとはどういうことか。なぜ advantage と GAE が要るか |
| 3 | [PPO](03-ppo.md) | 重要度比とクリップは何を防いでいるか。設定値は何を決めているか |
| 4 | [オンポリシーとオフポリシー](04-on-policy-and-off-policy.md) | 違いは何か。どちらがどんな条件に向くか |
| 5 | [自己対戦と評価](05-self-play-and-evaluation.md) | 誰と戦わせ、何で強さを測るか |
| 6 | [模倣学習](06-imitation-learning.md) | BC はなぜ崩れるか。DAgger と蒸留は何を解くか |
| 7 | [24位の PPO 実装を読む](07-reading-24th-ppo.md) | 式とコードはどう対応するか |

## 教材

| 教材 | 使う場所 | 備考 |
|---|---|---|
| Sutton & Barto『Reinforcement Learning: An Introduction』第2版 | 1〜2章 | 著者サイトで PDF 公開。邦訳は『強化学習』（森北出版） |
| OpenAI Spinning Up in Deep RL | 2〜3章 | 方策勾配から PPO までを式とコードで説明 |
| "The 37 Implementation Details of Proximal Policy Optimization"（ICLR Blog Track） | 3章 | 実装の細部と、その理由 |
| CleanRL の `ppo.py` | 3章 | 1ファイルの実装。教科書の式との対応を見る |
| AlphaStar の論文（Nature 2019） | 5章 | リーグと評価 |
| DAgger の論文（Ross et al. 2011） | 6章 | 模倣学習の分布のずれ |
| ptcg-population-rl（24位の公開コード、Apache-2.0） | 3〜7章 | `src/agents/dragapult/{ppo,transformer,gbdt}/` |
| ptcg-abc の `docs/research/solutions/survey.md` | 5章 | 上位25チームの横断分析 |
| ptcg-abc の `docs/research/reproduce/24-population-rl.md` | 3〜7章 | 24位のパイプラインを動かした実測 |
| ptcg-abc の `docs/recap/ptcg-devenv-rl-retrospective/` | 4〜6章 | チームの取り組みの振り返り |

## 進め方

1. 各章の「この章で説明できるようになること」を先に読む
2. 教材の該当箇所を読み、自分の言葉で本文を書く
3. 章末の「確認」に答えられるか試す。答えられない項目は、教材に戻る
4. 「手を動かす」の課題で、実装や実測と突き合わせる

## 表記

- 式は必要な範囲で書き、記号の意味を必ず併記する
- 用語は初出で英語も書く（例: 方策 policy）
- 教材の記述と自分の解釈は分けて書く
