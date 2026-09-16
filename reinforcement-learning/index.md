# 強化学習

- Kaggle PTCG AI Battle Challenge の続き（[ptcg-abc](https://github.com/uents/ptcg-abc) での行動クローニング → PPO の実装、上位解法の再現）を自分の判断で進めるための学習ノート
- 「AI に説明させる」のではなく、教科書と実装を自分で突き合わせて、判断の土台を自分に残すことを目的とする
- 関連イシュー: [tech-notes#7](https://github.com/uents/tech-notes/issues/7)

## 目次

| 章 | タイトル | 主な問い | 目安 (hour) |
|---|---|---|---:|
| 1 | [MDP と TD 学習](01-mdp-and-td.md) | 状態・行動・報酬をどう定式化するか。価値とは何か | 8 |
| 2 | [方策勾配法](02-policy-gradient.md) | 方策を直接学ぶとはどういうことか。なぜ advantage と GAE が要るか | 6 |
| 3 | [PPO](03-ppo.md) | 重要度比とクリップは何を防いでいるか。設定値は何を決めているか | 8 |
| 4 | [オンポリシーとオフポリシー](04-on-policy-and-off-policy.md) | 違いは何か。どちらがどんな条件に向くか | 3 |
| 5 | [自己対戦と評価](05-self-play-and-evaluation.md) | 誰と戦わせ、何で強さを測るか | 4 |
| 6 | [模倣学習](06-imitation-learning.md) | BC はなぜ崩れるか。DAgger と蒸留は何を解くか | 4 |
| 7 | [24位の PPO 実装を読む](07-reading-24th-ppo.md) | 式とコードはどう対応するか | 6 |
| | 合計 | | 39 |

目安は、教材を読む時間とノートを書く時間の合計。
演習を解く時間や、学習を実際に回す時間は含まない。
1日1時間なら6〜8週間、週末にまとめて進めるなら5〜6週の見当。
実際にかかった時間は、各章のメモに残して見直す。

## 教材

| 教材 | 使う場所 | 備考 |
|---|---|---|
| [Sutton & Barto『Reinforcement Learning: An Introduction』第2版](http://incompleteideas.net/book/the-book-2nd.html) | 1〜2章 | 著者サイトで PDF 公開。邦訳は[『強化学習（第2版）』森北出版](https://www.morikita.co.jp/books/mid/082662)。[初版の邦訳](https://www.morikita.co.jp/books/book/1990)でも1章（MDP・TD 学習）の範囲は読める（後述） |
| [OpenAI Spinning Up in Deep RL](https://spinningup.openai.com/en/latest/) | 2〜3章 | 方策勾配から PPO までを式とコードで説明。[Intro to Policy Optimization](https://spinningup.openai.com/en/latest/spinningup/rl_intro3.html)、[PPO](https://spinningup.openai.com/en/latest/algorithms/ppo.html) |
| ["The 37 Implementation Details of Proximal Policy Optimization"](https://iclr-blog-track.github.io/2022/03/25/ppo-implementation-details/) | 3章 | 実装の細部と、その理由（ICLR Blog Track） |
| [CleanRL の `ppo.py`](https://github.com/vwxyzjn/cleanrl/blob/master/cleanrl/ppo.py) | 3章 | 1ファイルの実装。教科書の式との対応を見る |
| [AlphaStar の論文（Nature 2019）](https://www.nature.com/articles/s41586-019-1724-z) | 5章 | リーグと評価 |
| [DAgger の論文（Ross et al. 2011）](https://proceedings.mlr.press/v15/ross11a.html) | 6章 | 模倣学習の分布のずれ。[arXiv 版](https://arxiv.org/abs/1011.0686) |
| [ptcg-population-rl](https://github.com/ymgaq/ptcg-population-rl)（24位の公開コード、Apache-2.0） | 3〜7章 | `src/agents/dragapult/{ppo,transformer,gbdt}/` |
| [ptcg-abc](https://github.com/uents/ptcg-abc) の `docs/research/solutions/survey.md` | 5章 | 上位25チームの横断分析 |
| ptcg-abc の `docs/research/reproduce/24-population-rl.md` | 3〜7章 | 24位のパイプラインを動かした実測 |
| ptcg-abc の `docs/recap/ptcg-devenv-rl-retrospective/` | 4〜6章 | チームの取り組みの振り返り |

### Sutton & Barto の版について

手元にあるのが初版（1998、邦訳2000）なら、1章（MDP と TD 学習）はそのまま初版で読める。
章番号も、3章（MDP）、4章（動的計画法）、5章（モンテカルロ法）、6章（TD 学習）まで第2版と揃っている。

一方、2章（方策勾配法）は初版に対応する章がない（第2版の13章で追加された）。
方策勾配から先は、第2版の PDF か Spinning Up を使う。

第2版で加わったもののうち、このノートに関係するのは次のあたり。

- 5章・7章の重要度サンプリング（オフポリシーの補正。4章で使う）
- 6章の Double Learning（DQN の過大評価の話につながる）
- 13章の方策勾配法（REINFORCE、actor-critic）

## 進め方

1. 各章の「この章で説明できるようになること」を先に読む
2. 教材の該当箇所を読み、自分の言葉で本文を書く
3. 章末の「確認」に答えられるか試す。答えられない項目は、教材に戻る
4. 「手を動かす」の課題で、実装や実測と突き合わせる

## 表記

- 式は必要な範囲で書き、記号の意味を必ず併記する
- 用語は初出で英語も書く（例: 方策 policy）
- 教材の記述と自分の解釈は分けて書く
