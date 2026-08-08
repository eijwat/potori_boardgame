# ぽとり POTORI Board Game

**Caught between two, it drops. Capture 10 to win.**
**挟んだ石は、返らずに落ちる。先に10個取れば勝ち。**

[English](#english) | [日本語](#日本語)

A reverse-Othello capture game in a single HTML file. Sandwiched stones don't flip — they drop off the board with a soft *potori*. First to capture ten enemy stones wins.

---

<a name="english"></a>
## English

### How to Play

- **Board & start** — 6×6 board with an Othello-style cross of four stones in the center. Black moves first.
- **Placing** — Players alternate placing one stone on **any empty cell**. Unlike Othello, you are never forced to capture and never blocked from a cell.
- **Sandwich & drop** — If your newly placed stone and another of your stones bracket a straight, unbroken line of enemy stones (orthogonal or diagonal), those stones don't flip — they **drop off the board** into your capture tray. Multiple directions can capture at once.
- **Bait stones are safe** — Captures trigger only for the player who just placed a stone. Placing your own stone *into* an existing sandwich is completely safe. Deliberately offering stones — *suteishi*, the sacrificial stone — is the heart of the game: give one, take three back.
- **Winning** — First to capture **10** enemy stones wins. Because stones leave the board, the endgame opens up and accelerates instead of clogging.
- **If nobody reaches 10** — If the board fills or 60 total moves pass, the higher capture count wins; ties go to White (the second player).

### Why It Works

POTORI inverts Othello's central promise. In Othello, captured stones stay and change sides, so the board monotonically fills and the game is about final territory. In POTORI, captured stones *leave*, so the board breathes — every capture opens new lines through the space it vacates. Three consequences follow:

1. **No stalling.** In self-play across every tested strength level, 100% of games ended by reaching the capture target. The board-full and move-limit rules exist as formal backstops and essentially never fire.
2. **The suteishi economy.** Since placing into a sandwich is safe, the deep game is about *offering* stones on your terms: bait a one-stone capture that forces your opponent's stone onto a square where you take back two or three.
3. **Accelerating endgames.** Games average 25–30 moves. The final stretch plays fast because the emptying board multiplies capture lines for both sides — the race to ten tightens on its own.

### Balance

First-move advantage was measured by automated self-play with randomized openings. At the strongest symmetric setting tested (alpha-beta depth 3, 150 games), Black won 57% — the same band as 二十 NIJŪ (55%), our reference for acceptable balance. Two correction mechanisms were prototyped and rejected:

- **A one-stone komi** (White needs only 9) flipped the game to 65–71% White — a full stone is too coarse in a game where one placement often captures several stones.
- **A "last reply" rule** (White gets one final move after Black reaches 10) overcorrected even further, to 70–85% White, for the same reason.

The plain, symmetric rule set was the best-balanced version — a happy result. The tie-goes-to-White rule remains as a nominal nod to the second player.

### CPU Opponents

Four levels:

| Level | Engine |
|---|---|
| Easy | Greedy capture with 20% wandering |
| Normal | Alpha-beta, depth 2 |
| Strong | Alpha-beta, depth 4 (≤0.4s/move) |
| Strongest | Iterative deepening, 1.5s time budget |

The Strongest level searches depth 5 in the midgame and reaches **depth 8 in the endgame** as the emptying board narrows the move tree — the game's signature mechanic directly feeds the engine's late-game power. In benchmark matches (0.5s budget, randomized openings), Strongest beat Strong 13–7 overall and won its White games 6–4 despite the first-move disadvantage.

### Technical Notes

- Single self-contained HTML file (~29KB), zero external dependencies (Google Fonts only).
- Rules engine validated by a 26-case unit test suite (Node.js) covering captures in all 8 directions, multi-direction captures, suteishi safety, edge runs, win/adjudication conditions, and state cloning.
- Full UI verified by a 22-case jsdom integration suite: boot state, mode selection, live play with captures, tray rendering, language toggle, rules modal, and a complete CPU game to the result screen.
- Japanese/English bilingual UI with browser-language auto-detection and a live toggle.
- Capture animation: stones tumble off the board (*potori*); captured stones fill a ten-slot tray per player.

## Play at Github Pages!!!
[POTPRI](https://eijwat.github.io/potori_boardgame/)

---

<a name="日本語"></a>
## 日本語

### 遊びかた

- **盤とはじまり** — 6×6の盤。中央にオセロ式の初期配置（黒白2個ずつ）、黒が先手です。
- **石を置く** — 交互に、**空いているマスならどこにでも**置けます。オセロと違い「挟める場所」の縛りはありません。
- **はさんで落とす** — 置いた石と自分の石で、相手の石列をまっすぐ挟むと（縦・横・ななめ）、挟まれた石は裏返らずに**ぽとりと盤から落ちて**、あなたの取り石になります。一度に複数方向で取れます。
- **捨て石は安全** — 取りが起きるのは「石を置いた側」だけ。挟まれる位置に自分から置いても取られません。わざと1個挟ませて、相手の着手を誘い、2個3個と取り返す——「捨て石」がこのゲームの駆け引きの核です。
- **勝ち** — 先に相手の石を**10個**取ったら勝ち。石が落ちて盤が空くので、終盤ほど展開が速くなります。
- **決着がつかないとき** — 盤が埋まるか合計60手に達したら終了し、取り石の多い方が勝ち。同数なら後手（白）の勝ちです。

### 設計の話

ぽとりは、オセロの約束事をひっくり返したゲームです。オセロでは取った石は盤に残って色を変えるので、盤は埋まる一方。ぽとりでは取った石は**盤から消える**ので、盤は呼吸します。取るたびに、その跡地から新しい挟みの筋が生まれるのです。

自己対戦実験では、テストしたすべての強さで**100%の対局が10個先取で決着**しました。盤満杯・手数上限のルールは形式上の保険で、実際にはほぼ発動しません。平均手数は25〜30手。終盤は盤が空くほど両者の取り筋が増えるので、10個へのレースは自然と加速します。

### バランス調整の記録

開幕をランダム化した自己対戦（アルファベータ深さ3・150局）で、黒の勝率は**57%**でした。これは前作「二十」の55%（コミ込み）と同じ帯域です。補正案も2つ試作しました——白の目標を9個にする「1個コミ」は白65〜71%へ、黒が10個に達しても白に最後の1手を与える「返し手」は白70〜85%へ、どちらも逆に振れすぎ。1手で複数個取れるゲームでは、1個ぶんの補正すら粗すぎたのです。結果、**素の対称ルールがいちばんバランスが良い**という嬉しい結論になりました。

### CPU

4段階です。「もっと強い」は反復深化（持ち時間1.5秒）で、中盤は5手先、盤が空く終盤は**8手先**まで読みます。石が消えて選択肢が減るというこのゲームの性質が、そのままCPUの終盤力に変わる仕組みです。ベンチマークでは「つよい」（深さ4）に通算13勝7敗、後手番でも勝ち越しました。

### 技術メモ

- 単一HTMLファイル（約29KB）、外部依存なし（Google Fontsのみ）
- ルールエンジンは26項目の単体テスト、UIは22項目のjsdom統合テストで検証済み
- 日本語／英語の二言語UI（ブラウザ言語の自動判定＋切替ボタン）
- 取られた石が「ぽとり」と落ちるアニメーションと、10個の皿（あげはま）が埋まるトレイ表示

## Play at Github Pages!!!
[POTPRI](https://eijwat.github.io/potori_boardgame/)

---

*Series: NEURONS / FlipFall / Shogi-Chess / KOROKORO / 還暦 KANREKI / うたかた UTAKATA / 二十 NIJŪ / ぽとり POTORI*
