---
layout: default
title: 現代制御編：最適制御・MPCと関連手法の研究文献
---

# 現代制御編：最適制御・MPCと関連手法

[最新版](./) · [論文の前に読む日本語書籍](japanese-foundations.html) · [今回の構成変更前・173件](archive-2026-10-26-before-modern.html) · [ゲーム理論改訂前・157件](archive-2026-10-26-games.html) · [初回保存版・135件](archive-2026-10-25.html)

**自分自身がこれから読むための文献をChatGPTにリストアップさせ、学習候補として公開している資料です。（2026年10月27日改訂）**

> **このリストの作成経緯について**  
> **この文献リストは、私自身がこれから読むために、ChatGPTに論文の探索・候補選定・概要説明・分類・推奨読書順の整理をさせたものです。私自身がこれらの文献を読んで内容を確認し、その知見に基づいて作成したリストではありません。**  
> 個人の学習用に作らせた候補一覧を、同じ分野に関心のある方の参考にもなるよう公開しています。**説明文・評価・研究史上の位置づけ・推奨読書順は主にChatGPTが生成・整理した内容であり、精読済み文献の書評や、私自身による内容の検証・推奨を示すものではありません。** 誤りや不正確な記述が残り得るため、研究で引用・利用する際は原論文の該当箇所を確認してください。

> **選定・構成の方針**：最適制御・MPCと、それに組み合わせる予測モデル・学習・保証・ゲーム理論を中心にする。手法の原理と発展を追い、教育的な説明、汎用的な設計知識、深い理論、後続研究への影響を優先する。同程度なら新しく公開版のあるものを選ぶ。新しさや件数をそろえるための追加はしない。各章の冒頭に総説・チュートリアルを示し、良い共通教材を複数章で使う。一般的なシステム同定・状態推定の独立章は設けない。

**主表の文献：162件／19章。** 冒頭のサーベイ15件・チュートリアル18件と、各章の129件を重複なく計上する。章内にも総説を含むため、129件すべてを原著とは呼ばない。公開講義・ソフトウェア等は別に35件。論文の前提を補う[日本語書籍17冊](japanese-foundations.html)も別枠で掲載している。前版173件から39件を保存版へ移し、28件を追加した。Explicit MPCの10件とゲーム理論の33件は維持している。

このページを**現代制御編**と位置づける。古典制御は今後の別編として予定する。

## 目次

- [手法と研究の流れ：全体マップ](#roadmap)
- [論文の前に読む日本語書籍：17冊と章別の対応〔別ページ〕](japanese-foundations.html)
- [研究の流れを俯瞰するサーベイ・総説](#sec-s)
- [各手法の導出を学ぶチュートリアル・概説](#sec-t)
- [1. 最適制御・MPCの共通理論：安定性・ロバスト化・確率的な拡張](#sec-k) — 有限ホライズンの解を繰り返し使う制御の保証を学ぶ。
- [2. SQP／RTIによるNMPC：直接法・感度計算・構造化ソルバー](#sec-sqp) — 制約付き最適制御をQPの反復で解き、実時間化する。
- [3. iLQR／DDP：前後方計算から制約・初期化・収束理論へ](#sec-ddp) — 軌道近傍の二次近似を、時間構造に沿って解く。
- [4. MPPI・経路積分制御：PI²・Information-Theoretic MPC・ロバスト化](#sec-p) — 候補入力の軌道を多数評価し、コストで重み付けして更新する。
- [5. Explicit MPC：多パラメトリック最適化・制御則の事前計算と近似](#sec-explicit) — 状態ごとの最適制御則を事前に計算し、オンライン計算を減らす。
- [6. スパース最適制御：maximum hands-off・予測入力列・疎なフィードバック](#sec-sparse) — 作動時間・入力要素・制御器の結合を減らす最適化を追う。
- [7. 深層モデルを使うMPC：NNダイナミクス・RNN・残差学習](#sec-nn) — 深層学習で予測モデルを作り、MPCの中で使う研究。
- [8. Gaussian Processに基づく予測制御：GP-MPCと計算近似](#sec-h) — 予測平均と不確かさをMPCへ組み込む。
- [9. Koopman MPC：作用素近似・入力付き予測・閉ループ保証](#sec-e) — 状態の関数を使う線形予測から、MPCと誤差保証へ進む。
- [10. DeePC：軌道データによる予測から非線形・保証へ](#sec-d) — モデル行列を介さず、入出力軌道の組合せで予測制御する。
- [11. 微分可能最適化：解感度からDifferentiable MPCへ](#sec-a) — MPCの解を微分し、モデルやコストを学習する。
- [12. Differentiable Predictive Control（DPC）：ロールアウトによる方策学習](#sec-n) — 予測モデルを微分して、オンラインで使う制御方策を事前学習する。
- [13. MPCと強化学習の統合：価値・モデル・初期解・方策パラメータ](#sec-u) — 学習をMPCのどの部分に使うかを追う。
- [14. 安全フィルタ法：CBF・予測フィルタ・HJ到達可能性](#sec-f) — MPCや学習方策の入力を監視し、安全制約を守る構成を追う。
- [15. Neural Lyapunov法：保証関数の学習・検証と制御への接続](#sec-b) — 学習した制御の安定性を示す関数を構成・検証する。
- [16. Contraction・CCM：軌道間の収束から追従・ロバスト化へ](#sec-c) — 状態差を縮める計量を使い、追従誤差や頑健性を評価する。
- [17. System Level Synthesis（SLS）：閉ループ応答からロバストMPCへ](#sec-g) — 外乱に対する状態・入力の応答を設計する。
- [18. Neural OperatorとPDE制御：予測モデルから方策表現へ](#sec-i) — 分布定数系の予測や制御を、関数間の写像として学ぶ。
- [19. ゲーム理論に基づく制御：min–max・Nash／Stackelberg・Pareto・Potential Games](#sec-game) — 複数主体や最悪外乱との相互作用を最適制御へ組み込む。
- [公開実装・講義・チュートリアル資料](#implementations)
- [選定変更と文献の読み方](#evaluation)

<a id="roadmap"></a>
## 手法と研究の流れ：全体マップ

最初にMPCの共通理論を押さえ、**SQP／RTI、iLQR／DDP、MPPI、Explicit MPC**を独立した章で追う。スパース最適制御も独立させ、予測表現・学習・保証の各方法を組み合わせる関係で読む。制御工学の基本知識を前提に、個別手法に固有の用語だけ補足する。

**分類の階層**：SQPやDDPは数値解法、NN・GP・Koopmanは主に予測表現、スパース性は評価・構造の設計、CBFやCCMは保証や補助制御の道具である。例えば深層モデルMPCをSQPで解き、安全フィルタを組み合わせられる。これらを排他的な競合手法として並べない。Explicit MPCやDPCはオンライン計算の構成も変える。

| 方法族・章 | 核となる考え方と研究の広がり |
|---|---|
| [1. 最適制御・MPCの共通理論：安定性・ロバスト化・確率的な拡張](#sec-k) | 安定性の条件 → 線形Tube → 非線形・確率的な拡張。 |
| [2. SQP／RTIによるNMPC：直接法・感度計算・構造化ソルバー](#sec-sqp) | RTI → 感度・構造化QP → AS-RTI／問題構成の工夫。 |
| [3. iLQR／DDP：前後方計算から制約・初期化・収束理論へ](#sec-ddp) | iLQRの前後方計算 → 入力制約 → 初期化・一般制約・収束理論。 |
| [4. MPPI・経路積分制御：PI²・Information-Theoretic MPC・ロバスト化](#sec-p) | 経路積分・PI² → MPPI／IT-MPC → 追従器を含むRMPPI。 |
| [5. Explicit MPC：多パラメトリック最適化・制御則の事前計算と近似](#sec-explicit) | mpQPの厳密解 → 領域探索 → 近似・NN・コード生成。 |
| [6. スパース最適制御：maximum hands-off・予測入力列・疎なフィードバック](#sec-sparse) | 疎なゲイン／予測入力列 → hands-off → 非凸・ロバスト拡張。 |
| [7. 深層モデルを使うMPC：NNダイナミクス・RNN・残差学習](#sec-nn) | RNNの安定性・観測器 → 誤差補償 → 大きい深層モデルの実時間計算。 |
| [8. Gaussian Processに基づく予測制御：GP-MPCと計算近似](#sec-h) | GP予測 → 多段不確かさ → cautious MPC・計算近似。 |
| [9. Koopman MPC：作用素近似・入力付き予測・閉ループ保証](#sec-e) | EDMD・入力付き予測 → Koopman MPC → 学習表現・誤差と安定性。 |
| [10. DeePC：軌道データによる予測から非線形・保証へ](#sec-d) | 基本補題 → DeePC → ノイズ・非線形・閉ループ保証。 |
| [11. 微分可能最適化：解感度からDifferentiable MPCへ](#sec-a) | argmin微分 → 最適化層 → MPC／NMPCの感度と方策学習。 |
| [12. Differentiable Predictive Control（DPC）：ロールアウトによる方策学習](#sec-n) | 予測ロールアウト → 明示的方策 → 複数対象への適応。 |
| [13. MPCと強化学習の統合：価値・モデル・初期解・方策パラメータ](#sec-u) | MPCのパラメータ学習／潜在モデルと価値学習／NMPC初期化。 |
| [14. 安全フィルタ法：CBF・予測フィルタ・HJ到達可能性](#sec-f) | HJ安全領域／CBF → 予測安全フィルタ → 学習・DeePCとの接続。 |
| [15. Neural Lyapunov法：保証関数の学習・検証と制御への接続](#sec-b) | 関数と制御則の学習 → 出力フィードバック → 訓練と検証の統合。 |
| [16. Contraction・CCM：軌道間の収束から追従・ロバスト化へ](#sec-c) | 収縮解析 → CCM → 追従とロバスト性、計量・制御則の学習。 |
| [17. System Level Synthesis（SLS）：閉ループ応答からロバストMPCへ](#sec-g) | 閉ループ応答の表現 → 局所・分散制約 → ロバストMPC。 |
| [18. Neural OperatorとPDE制御：予測モデルから方策表現へ](#sec-i) | FNO／DeepONet → 物理残差 → PDEの予測・方策表現。 |
| [19. ゲーム理論に基づく制御：min–max・Nash／Stackelberg・Pareto・Potential Games](#sec-game) | min–max／Nash・Stackelberg／Pareto／Potential Games → 混合整数・多数主体。 |

**学習順と研究史を分ける**：入口の総説を先に読み、関心のある枝の原著へ戻ってよい。章内の矢印は原理の拡張や学習上の接続を示す整理であり、全論文の直接の引用関係や厳密な優先権を確認した系譜ではない。プレプリント、オンライン先行公開、巻号の年は分けて記す。

<a id="japanese-books"></a>
**論文の前に基礎を補いたい方へ**：[日本語書籍17冊と、各章へのつながり](japanese-foundations.html)を別ページにまとめた。最適制御・MPC・数値最適化を共通の土台とし、GP・Koopman・深層学習・ゲーム理論などは関心のある枝に応じて補う。すべての本を読む前提ではなく、先に読む範囲も示している。各章の冒頭から対応する書籍へ移れる。

> **書籍の選定について**：掲載する書籍をすべて読んだわけではなく、公開された目次・内容紹介から、同じ分野を扱い基礎を学べる可能性が高いとChatGPTが判断した候補である。私自身の読了・検証に基づく推薦一覧ではない。書籍ページでは、確認できた内容とChatGPTによる読み方の提案を分けている。

<a id="sec-s"></a>
## 研究の流れを俯瞰するサーベイ・総説

MPCを中心に、予測表現・誤差保証・学習・ゲーム理論のどこを深掘りするかを決めるための入口。解法の導出を学ぶ文献は次のチュートリアル節に置く。

**選定方針**：教育的な説明と範囲の広さを最優先にし、同程度なら新しいもの、公開版・著者原稿があるものを優先する。狭い最新論文で良い基礎総説を置き換えず、古い総説の射程は新しい原著で補う。一般的な同定・World Model・汎用RLの広い総説は今回の主表から外した。

**収録文献：15件**

| 分野・選定理由 | 年・論文・入手先 | 論文の紹介：何をして、何が分かったか | カバー範囲・教育的な内容 | 関連する章 | 読む際の注意 |
|---|---|---|---|---|---|
| <a id="ref-a-survey-on-compressed-sensing-approach-to-systems-and-control"></a> **［スパース最適制御・基礎と実装］** Nagahara・Yamamoto。MATLAB例付きで学ぶ。 | 2024〔オンライン2023〕 / **[A Survey on Compressed Sensing Approach to Systems and Control](https://doi.org/10.1007/s00498-023-00366-1)**<br>[公開全文・補助コード](https://link.springer.com/article/10.1007/s00498-023-00366-1) | 圧縮センシングの考え方を、疎なフィードバック、低次元化、maximum hands-off制御へつなぐ。問題設定と計算法を、付属するMATLAB例で確認できる。 | 時間方向と制御器構造のスパース性を比較する教材。MCSS 36:1–20。 | [スパース最適制御](#sec-sparse)の最初の入口。 | 対象は紹介する設定に限られる。L₁緩和の成立条件と、非凸計算の局所性を確認する。 |
| <a id="ref-sparsity-in-control-problems-recent-trends-and-open-challenges"></a> **［スパース制御・近年の俯瞰］** Ballottaほか。基本理論から問題の分岐を追う。 | 2026〔プレプリント〕 / **[Sparsity in Control Problems: Recent Trends and Open Challenges](https://arxiv.org/html/2610.09787v1)**<br>[公開全文](https://arxiv.org/html/2610.09787v1) | 制御のスパース性を、可制御性、作動させる入力の選択、hands-off、センサ・アクチュエータのスケジュールなどから整理。各枝の理論・計算法と残る問題を比較する。 | hands-offの凸緩和・非凸計算を含む広い総説。必要な章を選んで原著へ進むために使う。 | [スパース最適制御](#sec-sparse)と資源制約の周辺研究。 | 2026年10月の公開原稿で、査読刊行済みとはしない。推定・攻撃などの章も含むため、MPCの実装入門とは役割が異なる。 |
| <a id="ref-data-science-and-model-predictive-control-a-survey-of-recent-advances-on-data-dr"></a> **［データ駆動MPC・優先］** 直接法・間接法をまとめて比較できる広さを評価。 | 2024 / **[Data Science and Model Predictive Control: A Survey of Recent Advances on Data-Driven MPC Algorithms](https://doi.org/10.1016/j.jprocont.2024.103327)** <br>[出版社の全文ページ](https://www.sciencedirect.com/science/article/pii/S0959152424001677) | データを使うMPCを、同定モデルを経由する方法と軌道データを直接使う方法などに整理。手法間の関係と、それぞれの長所・限界を比較する。 | モデルを同定してから制御する方法と、軌道データから予測・制御する方法を横断する。特定の二方式だけの比較より全体把握を優先。 | データ駆動MPCの設計方式選定・研究文献の探索。 | モデルを使う直接／間接法を同一条件で評価する際の比較軸が多い。 |
| <a id="ref-an-overview-of-systems-theoretic-guarantees-in-data-driven-model-predictive-cont"></a> **［DeePCの保証・優先］** 雑音なし線形系から、雑音・非線形の問題まで条件を追える。 | 2025 / **[An Overview of Systems-Theoretic Guarantees in Data-Driven Model Predictive Control](https://doi.org/10.1146/annurev-control-030323-024328)** <br>[公開版（arXiv）](https://arxiv.org/abs/2406.04130) | 軌道データから直接予測するMPCについて、安定性・雑音への頑健性・制約充足を保証する研究を整理。線形・雑音なしの場合から、非線形や誤差のある場合へ追う。 | 基本補題に基づく予測制御を対象に、安定性・ロバスト性・制約充足の保証を比較する。導入の後に定理と仮定を確認するための総説。 | データ駆動MPCの保証条件確認、DeePCとモデル同定型MPCの比較研究に利用できる。 | データ励起・ノイズ境界・モデルクラスの仮定がなお強く、大域非線形系の保証と計算効率の両立が課題である。 |
| <a id="ref-koopman-operators-in-robot-learning"></a> **［Koopman・ロボットへの応用］** 理論の概説から多様な実機・コードまでをつなぐ教育的な構成で選定。 | 2026〔公開プレプリントは2024〕 / **[Koopman Operators in Robot Learning](https://doi.org/10.1109/TRO.2026.3654384)**<br>[公開版](https://arxiv.org/abs/2408.04200) | ロボットの非線形な動きを、Koopman作用素で線形に予測する研究を整理。モデルの学び方から制御・推定・計画への使い方まで、具体例とコードで説明する。 | 数学的基礎、入力の扱い、観測関数・データの選び方を解説し、脚・車輪・飛行・水中・ソフトロボットなどを横断。[著者コード](https://github.com/sunnyshi0310/KoopmanRobo/tree/main)と併読できる。 | Koopman予測器の構成、MPC、状態推定、ロボット学習。 | 応用の幅を学ぶ総説。有限次元近似の誤差と閉ループ保証は個別の仮定に依存し、保証を詳しく学ぶには本節の保証総説も読む。 |
| <a id="ref-an-overview-of-koopman-based-control-from-error-bounds-to-closed-loop-guarantees"></a> **［Koopmanの保証・優先］** 予測誤差から閉ループ保証までを一続きに学べる。 | 2026 / **[An Overview of Koopman-Based Control: From Error Bounds to Closed-Loop Guarantees](https://doi.org/10.1016/j.arcontrol.2025.101035)** <br>[公開版（arXiv）](https://arxiv.org/abs/2509.02839) | Koopmanモデルの近似誤差を評価し、それを安定性や制約保証へ結び付ける研究を整理。予測がよいモデルを作ることと、制御を保証することの関係を示す。 | 有限次元・有限データの近似、線形・双線形の制御表現、誤差上界、ロバスト設計を扱う。チュートリアル節の基礎・実装入門から進む。 | Koopmanモデルの学習・MPC設計・保証研究の文献マップとして使える。 | 学習した持ち上げ座標の誤差評価と真の非線形系への保証の橋渡しが難しく、高次元化による計算負荷も残る。 |
| <a id="ref-synthesis-of-model-predictive-control-and-reinforcement-learning-survey-and-clas"></a> **［MPC×RL・優先］** 基礎説明、方式分類、理論、公開ソフトまで広くまとまる。 | 2026 / **[Synthesis of Model Predictive Control and Reinforcement Learning: Survey and Classification](https://doi.org/10.1016/j.arcontrol.2026.101045)** <br>[公開版（arXiv）](https://arxiv.org/abs/2502.02133) | MPCと強化学習を組み合わせる研究を、両者が担う役割から分類。MPCを教師・方策・価値評価などに使う構成と、その利点・課題を整理する。 | MPCとRLの基礎・比較から、MPCを教師・方策・価値関数として使う構成、保証、実装基盤へ進む。価値関数だけに絞った概説より優先。 | MPCとRLの融合アーキテクチャ設計、論文選定や研究課題の整理に有用である。 | 方式によってサンプル効率、オンライン計算量、安全性の仮定が異なり、各比較を同一条件で行うベンチマークが不足する。 |
| <a id="ref-safe-control-with-learned-certificates-a-survey-of-neural-lyapunov-barrier-and-c"></a> **［学習した保証関数・共通の入口］** Dawson・Gao・Fan。Lyapunov・CBF・CCMを比較する。 | 2023〔初稿2022〕 / **[Safe Control With Learned Certificates: A Survey of Neural Lyapunov, Barrier, and Contraction Methods for Robotics and Control](https://doi.org/10.1109/TRO.2022.3232542)**<br>[公開版](https://arxiv.org/abs/2202.11762)・[著者コード](https://github.com/dawsonc/neural_certificate_examples) | 制御則と一緒に、安定性や安全性を示す関数を学ぶ研究を体系化。学習・反例探索・検証がどう組み合わさるかを、例とコードで学べる。 | Lyapunov・Barrier・Contractionの役割、モデルや検証条件の違いを横断する。最新論文を並べる前の共通教材として選定。 | [Neural Lyapunov](#sec-b)、[安全フィルタ](#sec-f)、[CCM](#sec-c)。 | 保証は対象領域とモデル・検証の条件に依存する。学習損失が小さいことと、条件を全領域で証明したことを区別する。 |
| <a id="ref-dual-control-on-exploration-exploitation-in-linear-systems"></a> **［Dual Control・理論の俯瞰］** 探索と制御の両立を、古典から近年の理論まで同じ線形系の枠組みで追うため選定。 | 2026〔プレプリント・2027年掲載予定〕 / **[Dual Control: On Exploration-Exploitation in Linear Systems](https://arxiv.org/abs/2608.20073)**<br>[公開全文](https://arxiv.org/html/2608.20073v1) | 未知の線形システムを制御しながら、制御に必要な情報も集めるDual Controlを解説。自己調整制御・後悔最小化・最悪ケース設計の関係を整理する。 | バンディットとの接点、self-tuning regulator、regretに基づく設計、minimax dual controlを扱う。Annual Review of Control, Robotics, and Autonomous Systemsの2027年掲載予定として公開された原稿。 | 適応制御、能動的なシステム同定、学習中の制御性能の評価。 | 線形系が中心。非線形MPCでの近似・実装はチュートリアル節の2018年Dual Control総説を併読する。将来の掲載情報は確定版で再確認する。 |
| <a id="ref-operator-learning-a-statistical-perspective"></a> **［作用素学習の数理・優先］** 関数間の回帰として定義し、データ量と誤差を理解できる。 | 2026 / **[Operator Learning: A Statistical Perspective](https://doi.org/10.1146/annurev-statistics-042424-070908)** <br>[公開版（arXiv）](https://arxiv.org/abs/2504.03503) | PDEの解などの関数間の変換を、データからどこまで学べるかを統計的に解説。近似能力、学習に必要なデータ、未知の条件での誤差を整理する。 | 関数空間での近似・統計的推定・PDE固有の構造を整理する。チュートリアル節のFNO/DeepONet実践入門と、理論・実装で補完し合う。 | Neural Operatorの理論、PDE制御モデルの学習。 | 無限次元関数のサンプル複雑性と実環境の一般化。 |
| <a id="ref-game-theory-and-control"></a> **［ゲーム理論と制御・基礎の補助］** Marden・Shamma。零和・team・分散制御の基本構造を補う総説として残す。 | 2018 / **[Game Theory and Control](https://doi.org/10.1146/annurev-control-060117-105102)**<br>[出版社ページ・閲覧条件は掲載先参照](https://www.annualreviews.org/content/journals/10.1146/annurev-control-060117-105102) | ゲーム理論と制御の関係を、外乱に対抗する零和ゲーム、情報が分かれたチーム、分散協調の設計から解説。各分野で何をゲームとして扱うかを整理する。 | 零和・team games・利得と学習則を設計する分散制御を横断する。最新アルゴリズムの一覧より、基本構造をつかむ教育的な内容を評価して残す。 | [ゲーム理論に基づく制御](#sec-game)全体、H∞、分散制御。 | 2018年時点の総説。近年のロボット用ゲームソルバーは本節の2025年運転総説と原著で補う。公開全文は今回未確認。 |
| <a id="ref-ergodic-risk-sensitive-control-a-survey"></a> **［risk-sensitive・理論の俯瞰］** Biswas・Borkar。確率制御と零和ゲームの接点を、広いモデル群で確認できるため選定。 | 2023 / **[Ergodic Risk-Sensitive Control—A Survey](https://doi.org/10.1016/j.arcontrol.2023.03.001)**<br>[公開版](https://arxiv.org/abs/2301.00224) | 長期平均のrisk-sensitive制御を、Markov連鎖や拡散過程を通して整理。指数型の評価基準、最適性の条件、H∞・零和ゲームとのつながりを解説する。 | 離散・連続時間、有限・無限状態、Bellman方程式や固有値問題を体系的に扱う。risk-sensitiveを一般のリスク尺度と混同しないための理論的な総説。 | 確率制御、[零和・min–maxとの接続](#sec-game)。 | ergodicな長期評価が中心で、有限ホライズンMPCの実装入門ではない。CVaR・確率制約などの一般的なリスク制御全体を覆うものではない。 |
| <a id="ref-multi-agent-reinforcement-learning-a-selective-overview-of-theories-and-algorith"></a> **［MARL・理論を学ぶ入口］** Zhang・Yang・Başar。協調・競争・混合と、収束保証を同じ枠組みで比べられるため選定。 | 2021〔総説章・初稿2019〕 / **[Multi-Agent Reinforcement Learning: A Selective Overview of Theories and Algorithms](https://arxiv.org/abs/1911.10635)**<br>[公開版・2021年改訂](https://arxiv.org/abs/1911.10635v2) | 複数の学習者がいる強化学習を、協調・競争・両者の混合に分けて解説。Markovゲームなどの枠組みから、分散学習や方策更新の収束・非収束を整理する。 | 理論解析を伴うアルゴリズムを重視した総説章。Markovゲーム、展開型ゲーム、ネットワーク型の協調学習、mean-fieldの近似を含む。 | [ゲーム理論的な学習制御](#sec-game)、[方策合成](#sec-u)。 | 最新の深層MARL実装・ベンチマーク全体の総覧ではない。学習の収束、閉ループ安定性、安全性は別の性質として読む。 |
| <a id="ref-scenario-based-decision-making-using-game-theory-for-interactive-autonomous-driv"></a> **［ゲーム理論的な運動計画・近年の応用］** Lin・Tian。相互作用計画の広がりと問題設定を比較するため選定。 | 2025〔プレプリント〕 / **[Scenario-Based Decision-Making Using Game Theory for Interactive Autonomous Driving: A Survey](https://arxiv.org/abs/2509.05777)** | 他車との相互作用を扱う自動運転のゲーム理論的な意思決定を整理。合流・交差点・ラウンドアバウト・レースなどを通じ、ゲームのモデル化と解法の違いを比較する。 | Nash・Stackelbergなどのモデルを、各運転場面の条件と対応させて調べる応用総説。2018年の制御総説を近年の運動計画側から補う。 | [iLQGames・ALGAMES](#sec-game)、車両MPC、相互作用計画。 | 自動運転に範囲を絞ったプレプリント。ここでの場面別分類は応用の比較用であり、本文の方法族の分類軸とは区別する。 |
| <a id="ref-a-survey-of-recent-trends-in-multiobjective-optimal-control-surrogate-models-fee"></a> **［Pareto・多目的最適制御の全体像］** Peitz・Dellnitz。計算法・MPC・計算量削減をまとめて学ぶ広さを評価。 | 2018 / **[A Survey of Recent Trends in Multiobjective Optimal Control—Surrogate Models, Feedback Control and Objective Reduction](https://doi.org/10.3390/mca23020030)**<br>[公開版](https://www.mdpi.com/2297-8747/23/2/30)・[公開プレプリント](https://www.preprints.org/manuscript/201805.0221) | 複数の競合する評価を持つ最適制御で、折衷解の集合をどう計算し、フィードバックへ使うかを整理。代理モデル、MPC、事前計算とオンライン計算の分担を見渡せる。 | Pareto集合、PDE制御のモデル低次元化、実時間MPC、目的数を減らす方法を横断する。新しい狭い応用論文より、まず全体像を得る教材として選定。 | [Pareto・協調制御と多目的MPC](#game-pareto)、[Explicit MPC](#sec-explicit)。 | 2018年の総説であり最新研究の総覧ではない。2023年の閉ループ解析、2025年の学習・選択法、2026年の情報制約の原著で補う。多目的制御全般をゲームとはみなさない。 |

<a id="sec-t"></a>
## 各手法の導出を学ぶチュートリアル・概説

最初は **Caillauほか（2023）** で数値最適制御の全体を学び、**Aoyamaほか（2026）** でSQPとDDPの違いを確かめる。MPPI、深層モデルMPC、GP、Koopmanなどは関心のある行へ進む。各章の冒頭にも同じ入口へのリンクを置いた。

**読む用途を優先**：新規手法の比較も含む解説論文は、そのことを明記する。専用の広い総説が不足する微分可能最適化・DPCには、[著者・会議の公開チュートリアル](#implementations)も対応させる。講義資料を査読論文として数えない。

**収録文献：18件**

| 分野・選定理由 | 年・論文・入手先 | 論文の紹介：何をして、何が分かったか | カバー範囲・教育的な内容 | 関連する章 | 読む際の注意 |
|---|---|---|---|---|---|
| <a id="ref-an-algorithmic-guide-for-finite-dimensional-optimal-control-problems"></a> **［最適制御の数値解法・最初に読む］** Caillauほか。複数の解法を同じ例で比較する。 | 2023〔公開初稿2022〕 / **[An Algorithmic Guide for Finite-Dimensional Optimal Control Problems](https://doi.org/10.1016/bs.hna.2022.11.006)**<br>[公開版](https://arxiv.org/abs/2212.03157) | 直接法・間接法・Hamilton–Jacobi法などを、Zermelo問題とGoddard問題で比較する解説。式から実装まで追い、各手法の長所と組合せ方を理解できる。 | Handbook of Numerical Analysis 24:559–626。コード付きで、離散化・最適性条件・数値解法の区別を学ぶ。 | [MPCの共通理論](#sec-k)、[SQP／RTI](#sec-sqp)、他の最適制御法への入口。 | MPCの最新ソルバーを網羅する総説ではない。間接法とHJBは本リストの独立章にはせず、ここから参照する。 |
| <a id="ref-second-order-constrained-dynamic-optimization"></a> **［SQPとDDP・制約処理の比較］** Aoyamaほか。両方を共通の数値最適化から読む。 | 2026〔オンライン刊行・初稿2024〕 / **[Second-Order Constrained Dynamic Optimization](https://doi.org/10.1177/02783649251415417)**<br>[公開改訂版](https://arxiv.org/html/2409.11649v4) | 制約付きDDPとSQPを、single／multiple shooting、拡張Lagrangian、内点法などの共通軸で導出・比較。初期軌道や制約誤差が計算へ与える影響を数値実験で調べる。 | NLPの基礎からDDP・SQPまで説明し、新しいPDAL-DDPも含む解説・比較研究。各方式の更新式を見比べられる点を評価。 | [SQP／RTI](#sec-sqp)と[iLQR／DDP](#sec-ddp)をつなぐ最優先の教材。 | 実験での傾向は問題と実装に依存する。一方が常に速い・高精度という普遍的な順位ではない。 |
| <a id="ref-a-tutorial-review-of-neural-network-modeling-approaches-for-model-predictive-con"></a> **［NNモデルをMPCへ入れる手順］** Renほか。モデル構築から閉ループ比較まで。 | 2022 / **[A Tutorial Review of Neural Network Modeling Approaches for Model Predictive Control](https://doi.org/10.1016/j.compchemeng.2022.107956)**<br>[所属機関の書誌・要旨](https://impact.ornl.gov/en/publications/a-tutorial-review-of-neural-network-modeling-approaches-for-model/)・[公開アクセス先](https://www.osti.gov/pages/biblio/1961482) | FFNN・RNN・encoder–decoderなどの時系列モデルを構築し、MPCへ組み込む手順を説明。非線形プロセスの例で、予測精度と閉ループの振る舞いを比較する。 | 2025年のML-MPC総説と併読する、NNモデル設計に重点を置く教材。一般的なシステム同定全体の章は設けない。 | [深層モデルを使うMPC](#sec-nn)の実装前の入口。 | 化学プロセスの例が中心。実機固有の観測・励起・計算負荷は別途設計する。公開版の閲覧条件は掲載先参照。 |
| <a id="ref-tutorial-on-amortized-optimization"></a> **［最適化を学習で近似する共通理論］** Amos。DPCとExplicit MPC近似を深く読む。 | 2023〔初稿2022・公開版2025改訂〕 / **[Tutorial on Amortized Optimization](https://doi.org/10.1561/2200000102)**<br>[公開版](https://arxiv.org/abs/2202.00665)・[著者コード](https://github.com/facebookresearch/amortized-optimization-tutorial) | 繰り返し解く最適化問題の解を、学習した関数で素早く予測する方法を解説。教師付き・教師なしの学習、解の改善、制御・RLなどへの応用を同じ枠組みで整理する。 | Foundations and Trends in Machine Learning 16(5):592–732。解の近似を学習する目的、勾配、精度・速度の関係を掘り下げる。 | [DPC](#sec-n)、[Explicit MPCの近似](#sec-explicit)、学習した初期解。 | DPC専用の総説ではなく、制約充足や閉ループ安定性の一般保証を与えるものでもない。 |
| <a id="ref-safe-physics-informed-machine-learning-for-dynamics-and-control"></a> **［物理情報と安全性・導入］** 学習モデルと制御の保証を広く結び、具体例・教材を備えたACC 2025チュートリアルとして選定。 | 2025 / **[Safe Physics-Informed Machine Learning for Dynamics and Control](https://arxiv.org/abs/2504.12952)**<br>[著者公開PDF](https://mallada.ece.jhu.edu/pubs/2025-ACC-Tutorial-DNBetal.pdf) | 物理法則や制御理論の条件を機械学習に組み込み、安全性・安定性を扱う方法を解説。モデルの学習から制御器の設計・検証までを例とともに見渡せる。 | 構造を持つモデル、Lyapunov関数・CBF、MPC、制約への射影、ロバスト最適化、不確かさ、到達可能性、NN検証を概説。[講習資料・コード](https://github.com/nxt-lab/2025-ACC-Tutorial-session-Safe-PIML)を併用できる。 | 物理情報付きモデル学習、安全な学習制御、保証付きNN設計。 | 各手法の導入をつなぐ教材で、すべてを詳細に導出するものではない。物理情報を加えるだけで安全性が保証されるわけではなく、手法ごとの仮定と検証が必要。 |
| <a id="ref-a-tutorial-review-of-machine-learning-based-model-predictive-control-methods"></a> **［ML-MPC・最初に読む］** モデル作成、安定性、実装上の課題まで広い42ページの解説。 | 2025 / **[A Tutorial Review of Machine Learning-Based Model Predictive Control Methods](https://doi.org/10.1515/revce-2024-0055)** <br>[出版社PDF・閲覧条件は掲載先参照](https://www.degruyterbrill.com/document/doi/10.1515/revce-2024-0055/pdf) | 学習モデル付きMPCを、モデル作成から制御器への組込みまで解説。予測精度だけでなく、安定性・データ不足・安全性を含めた設計上の論点を整理する。 | NNモデル、汎化誤差と閉ループ安定性、データ不足、実装、安全性・セキュリティを扱う。2022年のNNモデル専用入門に代えて、より新しく広い入口とする。 | 学習モデル付きNMPCの設計、モデル誤差に敏感な制約付き制御。 | データ量、外挿、オンライン学習、リアルタイム最適化と閉ループ保証の両立。 |
| <a id="ref-model-predictive-control-via-probabilistic-inference-a-tutorial-and-survey"></a> **［サンプリングMPC・最初に読む］** 導出と方式分類の両方があり、2026年の展開を追える。 | 2026 / **[Model Predictive Control via Probabilistic Inference: A Tutorial and Survey](https://doi.org/10.1016/j.arcontrol.2026.101052)** <br>[公開版（arXiv）](https://arxiv.org/abs/2511.08019) | MPCを、良い入力列に高い確率を与える分布を求める問題として解説。MPPIの更新則を導き、制約・複数の候補・高速化を扱う研究を整理する。 | 確率推論としての定式化、MPPIの更新、事前分布、複数の候補、制約、高速化、理論を扱う。経路積分制御と近年のサンプリングMPCを比較して学べる。 | MPPI、サンプリング最適制御、分布に基づくロボット計画。 | サンプル効率、制約保証、計算時間、モデル不確かさが残る。 |
| <a id="ref-stochastic-model-predictive-control-with-active-uncertainty-learning-a-survey-on"></a> **［Dual MPC・導出の補完］** 古いが、未知パラメータの学習をMPCに含める数式を追える。 | 2018 / **[Stochastic Model Predictive Control with Active Uncertainty Learning: A Survey on Dual Control](https://doi.org/10.1016/j.arcontrol.2017.11.001)** <br>[著者公開PDF](https://static1.squarespace.com/static/5d4488dc0d24df0001719b1f/t/5d59d85ca827b9000184e753/1566169182091/ARC_2018.pdf) | 制御入力が、目標達成と未知モデルを知るための情報収集を同時に担うDual Controlを整理。探索の価値を、確率的MPCへ取り入れる考え方を解説する。 | 制御入力の情報収集効果、確率的予測、探索と制御のトレードオフを説明する。サーベイ節の2026年Dual Control俯瞰は線形系中心のため、別の役割で残す。 | Active Learning、オンラインシステム同定、GP-MPC。 | 探索時の安全性、情報利得評価の計算コスト、実機の励起制約。 |
| <a id="ref-stochastic-model-predictive-control-how-does-it-work"></a> **［確率的MPC・最初に読む］** 2018年でも、定式化から例題までの導入の明快さを評価。 | 2018 / **[Stochastic Model Predictive Control — How Does It Work?](https://doi.org/10.1016/j.compchemeng.2017.10.026)** <br>[著者公開PDF](https://www.mesbahlab.com/s/CACE_2018_1.pdf) | 外乱を確率的に扱うMPCについて、予測のばらつきをコストや制約へ反映する方法を解説。通常のMPCとの違いと、性能・違反リスクの調整方法が分かる。 | 予測分布、確率制約、通常のMPCとの関係、外乱モデルの選択を13ページで説明する。新しい狭い手法の総説より、まずこの導入を読む。 | GP-MPCの確率制約、SMPCの基礎理論。 | 分布仮定や近似共分散、制約違反確率の較正に依存する。 |
| <a id="ref-a-tutorial-on-gaussian-process-learning-based-model-predictive-control"></a> **［GP-MPC・最初に読む］** 多段予測の平均・分散と確率制約を段階的に導出する。 | 2024 / **[A Tutorial on Gaussian Process Learning-based Model Predictive Control](https://arxiv.org/abs/2404.03689)** <br>[公開版（arXiv）](https://arxiv.org/abs/2404.03689) | GPで学んだモデルの平均と分散が、将来予測でどう変わるかを段階的に導出。予測の不確かさを、MPCの確率制約へ組み込む計算を解説する。 | GP回帰から、不確かさの伝播とMPCの制約構築までをつなぐ。GP-MPC章の2025年GP総説で研究全体を確認し、本稿で計算を学ぶ。 | Cautious GP-MPC、Zero-order GP-MPC、L4acadosの事前学習。 | 分布のガウス近似、GP計算量、共分散の較正、オンライン制御。 |
| <a id="ref-advances-in-the-theory-of-control-barrier-functions-addressing-practical-challen"></a> **［CBF・基礎から実装へ］** 実機で問題になる条件を幅広く説明する。 | 2024 / **[Advances in the Theory of Control Barrier Functions: Addressing Practical Challenges in Safe Control Synthesis for Autonomous and Robotic Systems](https://doi.org/10.1016/j.arcontrol.2024.100945)** <br>[公開版（arXiv）](https://arxiv.org/abs/2312.16719) | CBFを実装するときに問題になる入力制限、高相対次数、離散的な入力更新、モデル誤差を整理。基本の安全条件を、実際の制御設計へ広げる方法を解説する。 | 入力制約、高相対次数、サンプル値実装、モデル誤差、保守性などを扱う。入力制限とモデル誤差が安全条件に与える影響を学べる。 | ロボットの安全フィルタ設計、CBFの理論と実装の接続。 | QPの実行不能、誤差による安全集合の破れ、現実のアクチュエータ制約。 |
| <a id="ref-from-theory-to-application-a-practical-introduction-to-neural-operators-in-scien"></a> **［Neural Operator・最初に読む］** 新しい実践解説として、概念と実装をつなぐ。 | 2026 / **[From Theory to Application: A Practical Introduction to Neural Operators in Scientific Computing](https://doi.org/10.3390/math14132421)** <br>[公開全文（出版社）](https://www.mdpi.com/2227-7390/14/13/2421) | FNOやDeepONetでPDEの解を学ぶ仕組みを、理論と実装の両面から解説。入力の表し方、学習、予測の評価など、使い始める際の設計項目を整理する。 | FNO・DeepONet、入力関数の表現、学習・評価と計算上の注意点を整理する。PDE制御の前に、代理モデルの作成と評価を学ぶ入口。 | PDE制御へ進む前のScientific Machine Learning実装。 | 物理整合性、解像度間汎化、長時間予測誤差、学習計算量。 |
| <a id="ref-koopman-operator-theory-fundamentals-control-and-applications"></a> **［Koopman・最初に読む］** 2026年の基礎説明と公開コード付きの学習手順を評価。 | 2026 / **[Koopman Operator Theory: Fundamentals, Control, and Applications](https://arxiv.org/abs/2607.01819)** <br>[公開プレプリント・HTML](https://arxiv.org/html/2607.01819v1) | Koopman作用素の基礎から、データによるモデル近似、入力付き予測、MPCまでを解説。理論上の考え方と、シミュレーション・実装の手順をつなぐ。 | 作用素、EDMD・カーネル法、入力付き予測、Koopman MPCを段階的に説明する。サーベイ節のロボット応用総説・保証総説へ進むための入口。 | Koopman MPCの最初の自作、学習モデルの推定誤差と制御を結ぶ学習。 | 辞書選択、有限データの誤差、真の非線形系に対する閉ループ保証。 |
| <a id="ref-learning-for-feasible-and-safe-control-with-control-barrier-functions-a-tutorial"></a> **［CBFの発展・補完］** 2026年の解説で、QPの実行可能性と学習の関係を深掘りする。 | 2026 / **[Learning for Feasible and Safe Control with Control Barrier Functions: A Tutorial](https://doi.org/10.26599/CAI.2025.9390009)** <br>[公開全文（出版社）](https://www.sciopen.com/article/10.26599/CAI.2025.9390009) | CBFによる安全制御で、制約を満たす入力が見つからなくなる問題を扱う。CBFのパラメータや安全性の情報を学び、実行可能な入力を確保する方法を解説する。 | 高次CBFのパラメータ学習と、実行可能な領域を学ぶ方法を解説する。分野全体の総説の代わりではなく、2024年の広いCBF入門を補う。 | 学習によるCBF設計・実行可能領域拡張、安全制御の自動化。 | 学習誤差と安全保証の整合、入力飽和、多数の安全制約との競合。 |
| <a id="ref-a-crash-course-in-differential-games-and-applications"></a> **［微分ゲーム・導入］** de Zeeuw。open-loopと状態フィードバックの違いを、式と具体例で学べる公開チュートリアルとして選定。 | 2024 / **[A Crash Course in Differential Games and Applications](https://doi.org/10.1007/s10640-024-00844-3)**<br>[出版社公開全文](https://link.springer.com/article/10.1007/s10640-024-00844-3) | 最適制御を複数の意思決定者へ広げる微分ゲームを、環境・経済の例で解説。情報の使い方でNash均衡が変わることや、Stackelbergの時間整合性を説明する。 | open-loop・Markov-perfect Nash、最大原理と動的計画法、汚染・湖の管理、先手と後手の意思決定を扱う。制御の基礎を知る読者が解概念を整理する入口。 | [非零和の動的ゲーム](#sec-game)、最適制御の拡張。 | 例はロボットより環境・経済が中心。最大原理と動的計画法の違いだけで均衡が決まるのではなく、採用する戦略・情報構造を指定して読む。 |
| <a id="ref-coalitional-control-cooperative-game-theory-and-control"></a> **［協調・連合形成の概説］** Fele・Maestre・Camacho。制御構成と協力ゲームの対応を、具体的な設計の文脈で学べるため選定。 | 2017〔著者公開版は2025〕 / **[Coalitional Control: Cooperative Game Theory and Control](https://doi.org/10.1109/MCS.2016.2621465)**<br>[公開版](https://arxiv.org/abs/2501.17582) | 制御器が協調する相手を選び、連合を組み替えるcoalitional controlを概説。通信や協調の利益をどう評価し、協力ゲーム理論を制御構成へ使うかを説明する。 | 分散MPCなどを背景に、連合形成・協調の費用や利益・制御構成の関係を整理する。2025年は公開原稿の登録年で、論文自体の刊行年ではない。 | [協調・連合形成](#sec-game)、分散制御、MPC。 | 古いが連合制御の基本構成を学ぶ教材として残す。Potential Gamesによる利得設計や、共通コストを最小化するチーム最適制御と同一ではない。 |
| <a id="ref-receding-horizon-control-and-dissipativity-optimal-control-games-and-uncertainty"></a> **［MPCからゲーム理論的制御へ・近年の入口］** Hall・Schießl・Grammatico・Faulwasser。最適制御とゲームの差を、閉ループ解析までつないで学ぶ。 | 2026〔プレプリント・CDC 2026掲載予定〕 / **[Receding Horizon Control and Dissipativity—Optimal Control, Games and Uncertainty](https://arxiv.org/abs/2609.36858)**<br>[公開全文](https://arxiv.org/html/2609.36858v1) | 通常のMPC、複数主体のゲーム、確率的不確かさを伴う制御を比較するチュートリアル。目的関数・制約・均衡の違いを整理し、散逸性やturnpikeを通じて閉ループ解析へつなぐ。 | OCPとopen-loopゲーム、分散MPCとreceding-horizon games、安定性、確率的な拡張、GNEの数値解法を横断する。既知のMPCからゲーム理論的制御へ入るため優先する。 | [ゲーム理論に基づく制御](#sec-game)、MPCの閉ループ保証。 | ゲーム理論全般の教科書ではなく、receding-horizon制御と解析に重点がある。2026年10月時点では公開原稿として扱い、会議開催・刊行済みとはしない。 |
| <a id="ref-machine-learning-methods-for-large-population-games-with-applications-in-operati"></a> **［Mean-Field Games・近年の入門］** Dayanıklı・Laurière。多数主体の古典へ戻る前に、モデル・計算法・例を学ぶ教材。 | 2024 / **[Machine Learning Methods for Large Population Games with Applications in Operations Research](https://doi.org/10.1287/educ.2024.0272)**<br>[公開版](https://arxiv.org/abs/2406.10441) | 一主体の確率制御から多数主体のゲームへ進み、Mean-Field Gamesの均衡を学習で求める方法を解説。離散・連続時間の計算法を、交通・電力などの例とともに説明する。 | 有限人数の動的ゲーム、mean-field近似、固定点・RLに基づく方法、SDE・PDEの最適性条件を使う学習法を段階的に説明する。Huangほか（2006）への入口として選定。 | [多数主体のゲーム](#sec-game)、Mean-Field Gamesと学習。 | 近似均衡を扱う多数主体の仮定に注意。集団の総コストを最適化するMean-Field Controlと同じ問題として扱わない。 |

## 文献一覧（手法とその発展ごと）

各章は、入口の教材、核となる考え方、研究の展開、主表の順で読む。主表は原則年代順。各文献を一度だけ計上し、同じ総説を複数章の入口として参照する。紹介文はAbstractの逐語訳ではなく、何に取り組み何を示したかを短い独立した日本語で記す。

<a id="sec-k"></a>
<a id="sec-r"></a>
### 1. 最適制御・MPCの共通理論：安定性・ロバスト化・確率的な拡張

**論文の前に補う日本語書籍**：[非線形最適制御入門](japanese-foundations.html#book-oc)／[モデル予測制御](japanese-foundations.html#book-mpc)／[システム制御のための安定論](japanese-foundations.html#book-stability)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Caillauほか（2023）：最適制御の数値解法の全体像](#ref-an-algorithmic-guide-for-finite-dimensional-optimal-control-problems)。MPCの構成とゲーム・不確かさへの展開は[Hallほか（2026）](#ref-receding-horizon-control-and-dissipativity-optimal-control-games-and-uncertainty)、確率制約は[確率的MPCの2018年チュートリアル](#ref-stochastic-model-predictive-control-how-does-it-work)へ。

**核となる考え方**：最適制御問題の解法を選ぶ前に、制約付きのreceding-horizon実行がいつ実行可能で安定になるかを押さえる。数値解法の枝は次の各章に分け、ここは共通理論とモデル誤差の扱いを基準点にする。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 共通の保証条件 | Mayneほか（2000） | 終端要素、実行可能性、閉ループ安定性の関係を整理する。 |
| 外乱への頑健化 | 線形Tube（2005）→非線形Tube（2021） | 誤差を予測軌道の周囲へ収め、制約を引き締める。増分安定性はCCMと接続する。 |
| 確率評価・学習との接続 | 冒頭の確率的MPC・Dual Control教材、NN／GP／DeePCの各章 | 最悪値、確率制約、未知パラメータの学習は異なる仮定で扱う。 |

理論を知ったうえで、[SQP／RTI](#sec-sqp)・[iLQR／DDP](#sec-ddp)・[MPPI](#sec-p)の解法へ進む。これらはMPCとは別の制御目的ではなく、有限ホライズン最適制御を計算する方法である。

**収録文献：3件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-constrained-model-predictive-control-stability-and-optimality"></a> **［安定性理論・総説］** **［基礎追加］** Mayne・Rawlings・Rao・Scokaert。制約付きMPCの安定性理論の基本総説。 | 2000 / **[Constrained Model Predictive Control: Stability and Optimality](https://doi.org/10.1016/S0005-1098(99)00214-9)** | 制約付きMPCが安定に動き続けるための条件を整理した総説。終端コスト・終端集合・実行可能性が、安定性とどう結び付くかを示す。 | 終端コスト・終端集合・終端制御則などの条件を整理し、MPCの安定性と最適性の論理を体系化。特定実験の高速化倍率を示す文献ではない。 | Tube MPC、学習モデル付きMPC、Koopman MPCの保証を読む共通の土台。 | 十分条件とモデルの仮定を要する。学習誤差・確率的保証・ソルバーの実時間実装は別途扱う。 |
| <a id="ref-robust-model-predictive-control-of-constrained-linear-systems-with-bounded-distu"></a> **［Tube MPC］** 現代のRobust MPCの基本。 | 2005 / **[Robust Model Predictive Control of Constrained Linear Systems with Bounded Disturbances](https://doi.org/10.1016/j.automatica.2004.08.019)** | 外乱で状態が予測からずれても、制約を守って制御するTube MPCを提案。名目軌道と誤差補正を組み合わせ、所定の条件で安定性を示した。 | 名目軌道と誤差に対する固定安定化フィードバックを分離し、制約引き締めによる実行可能性・安定性の保証を示すTube MPCを定式化した。 | ロバストMPC、GP/学習モデルの誤差集合に対する制約保証。 | 外乱集合の既知性・保守性、チューブ計算、非線形系の扱いが課題。 |
| <a id="ref-a-computationally-efficient-robust-model-predictive-control-framework-for-uncert"></a> **［非線形Tube MPC］** Köhlerほか。線形Tubeから非線形・状態依存誤差へ。 | 2021〔オンライン2020〕 / **[A Computationally Efficient Robust Model Predictive Control Framework for Uncertain Nonlinear Systems](https://doi.org/10.1109/TAC.2020.2982585)**<br>[公開拡張版](https://arxiv.org/abs/1910.12081) | 非線形系の状態・入力に依存するモデル誤差を、予測軌道の周囲のチューブで扱う。制約の引き締めと実用的安定性を保証し、名目MPCに近い計算構造を保った。 | 増分Lyapunov関数と追従フィードバックをオフラインで用意し、オンラインではスカラー上界を使う。線形Tube MPCからCCM・学習誤差の扱いへ接続する。 | 誤差上界を持つ非線形MPC、学習モデル付きの頑健設計。 | 増分安定化可能性と既知の誤差上界などが必要。任意の学習誤差に対する保証ではない。 |

**この区分の収録件数：3件。**

<a id="sec-sqp"></a>
### 2. SQP／RTIによるNMPC：直接法・感度計算・構造化ソルバー

**論文の前に補う日本語書籍**：[非線形最適化法](japanese-foundations.html#book-nlp)／[非線形最適制御入門](japanese-foundations.html#book-oc)／[PythonとCasADiで学ぶMPC](japanese-foundations.html#book-casadi)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Caillauほか（2023）](#ref-an-algorithmic-guide-for-finite-dimensional-optimal-control-problems)で直接法の全体を学び、[Aoyamaほか（2026）](#ref-second-order-constrained-dynamic-optimization)でSQPとDDP・内点法などの制約処理を比較する。

**核となる考え方**：SQP（Sequential Quadratic Programming）は非線形最適化をQPの列で解く方法。RTIはその更新をサンプリングに合わせて進める実行方式である。single／multiple shootingやcollocationは離散化・問題構成、SQPや内点法はその問題を解くアルゴリズムなので、分類の階層を分けて読む。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 実時間更新の基礎 | RTI（2005） | 解を毎周期完全に収束させる方式から、準備と短いフィードバック処理へ。 |
| 微分と時間構造の利用 | CasADi（2019）／acados（2022） | 自動微分、数値積分、QP、condensingの役割を結ぶ。ソフトウェアは個別解法の代名詞ではない。 |
| 精度・計算量の調整 | AS-RTI（2024）／multi-phase OCP（2025） | 更新量を準備段階へ配分する枝と、モデル・入力の表現を区間ごとに変える枝。 |

接触最適化や特定の四脚MPCは、汎用解法の主表を膨らませず[再現実装・講義](#implementations)から参照する。内点法の説明も共通教材に含まれるが、SQPとは別のアルゴリズムである。

**収録文献：5件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-a-real-time-iteration-scheme-for-nonlinear-optimization-in-optimal-feedback-cont"></a> **［SQP・RTI］** NMPCの実時間計算の歴史を理解する要石。 | 2005 / **[A Real-Time Iteration Scheme for Nonlinear Optimization in Optimal Feedback Control](https://doi.org/10.1137/S0363012902400713)** | 非線形MPCの最適化を毎周期最後まで解かず、少しずつ更新して制御するRTIを提案。解の追従性を解析し、凧の飛行例で実時間計算を示した。 | SQPの一回の反復をサンプリング周期に合わせて実行するRTIを提案し、実時間のフィードバック最適化を可能にする理論と数値例を提示。制御性能・演算時間の普遍的な改善倍率はない。 | acados RTI、非線形車両MPC、ロボットのオンライン軌道最適化。 | 良い初期解、滑らかな力学、サンプリング間の変化が十分小さいこと等に依存し、単一反復のため厳密収束しない場合がある。 |
| <a id="ref-casadi-a-software-framework-for-nonlinear-optimization-and-optimal-control"></a> **［導出と実装の接続］** Anderssonほか。微分・離散化・NLPをつなぐ。 | 2019〔オンライン2018〕 / **[CasADi: A Software Framework for Nonlinear Optimization and Optimal Control](https://doi.org/10.1007/s12532-018-0139-4)**<br>[著者公開版](https://optimization-online.org/2018/01/6420/) | 最適制御のモデル、微分、数値積分、最適化ソルバーを組み合わせるCasADiを説明。自動微分と疎行列計算を、直接法の実装へつなぐ設計を示した。 | 数学的なモデルから感度・NLPを構成するソフトウェア論文。SQPと内点法は解法、CasADiはモデル化・微分の基盤という役割の違いを学べる。 | 独自NMPCの実装、複数のソルバー・離散化法の比較。 | CasADi自体が単一のSQPアルゴリズムではない。ソルバーの収束保証と実行時間は別途確認する。 |
| <a id="ref-acados-a-modular-open-source-framework-for-fast-embedded-optimal-control"></a> **［構造化SQP・実装］** Verschuerenほか。OCPの時間構造を利用する。 | 2022〔オンライン2021〕 / **[acados—A Modular Open-Source Framework for Fast Embedded Optimal Control](https://doi.org/10.1007/s12532-021-00208-8)**<br>[著者公開PDF](https://publications.syscop.de/Verschueren2021.pdf)・[公式教材](https://docs.acados.org/) | 非線形MPCを組込み機器で高速に解くため、積分器・SQP・QP解法・線形代数を組み合わせるacadosを提示。数式から実時間ソルバーまでの構成をまとめて学べる。 | multiple shooting、感度計算、condensing、構造化QPなどの役割を整理する。ソフトウェア名と個別の数値解法を区別して読む。 | SQP／RTIによる車両・ロボットNMPC、計算構成の設計。 | 性能はモデル・離散化・QP解法・ハードウェアに依存する。最新版の全機能が原論文に含まれるわけではない。 |
| <a id="ref-advanced-step-real-time-iterations-with-four-levels-new-error-bounds-and-fast-im"></a> **［RTIの精度と計算配分］** Frey・Nurkanović・Diehl。準備段階を活用する。 | 2024 / **[Advanced-Step Real-Time Iterations with Four Levels—New Error Bounds and Fast Implementation in acados](https://doi.org/10.1109/LCSYS.2024.3412007)**<br>[公開版](https://arxiv.org/abs/2403.07101) | RTIで入力を素早く返す構成を保ちつつ、予測状態を使う準備段階に追加の反復を行う。四つの計算レベルの局所誤差解析を整え、acadosで実装した。 | どの感度・関数評価を再計算するかで精度と計算量を調整する。RTIからAS-RTIへ進む際の具体的な更新手順を学べる。 | 計算時間の厳しいNMPC、準備・フィードバック処理の分担。 | 局所解析と予測状態の誤差に依存する。大きな状態変化でも一律に精度や安定性が保証されるわけではない。 |
| <a id="ref-multi-phase-optimal-control-problems-for-efficient-nonlinear-model-predictive-co"></a> **［問題構成による高速化］** Freyほか。ホライズン内でモデルと入力表現を変える。 | 2025 / **[Multi-Phase Optimal Control Problems for Efficient Nonlinear Model Predictive Control With acados](https://doi.org/10.1002/oca.3234)**<br>[公開全文](https://onlinelibrary.wiley.com/doi/10.1002/oca.3234)・[原稿](https://arxiv.org/abs/2408.07382) | 予測区間の場所に応じて、モデルの精度や入力の表し方を変えるNMPCを構成。導入例と三つの数値例で、制御性能と計算負荷を調整する方法を示した。 | 多項式入力、closed-loop costing、異なる精度のモデルなどを同じ枠組みで扱う。新しいSQPの更新則ではなく、解く問題を設計する発展。 | 長ホライズンNMPC、実時間計算と予測精度の調整。 | 近似の選び方に性能が依存する。多相化しただけで任意の閉ループ性能を保証するものではない。 |

**この区分の収録件数：5件。**

<a id="sec-ddp"></a>
### 3. iLQR／DDP：前後方計算から制約・初期化・収束理論へ

**論文の前に補う日本語書籍**：[非線形最適制御入門](japanese-foundations.html#book-oc)／[非線形最適化法](japanese-foundations.html#book-nlp)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Aoyamaほか（2026）](#ref-second-order-constrained-dynamic-optimization)を最初に読み、導出後は[Rouletほか（2025）](#ref-on-global-and-local-convergence-of-iterative-linear-quadratic-optimization-algor)で収束理論を深める。

**核となる考え方**：iLQR／DDPは候補軌道の近傍で局所問題を作り、後退計算でフィードバックゲインを求め、前進計算で軌道を更新する。典型的なiLQRでは力学を一次近似し、DDPは力学の二階微分も使う。名称が似た変種では、実際に採用する微分と近似を確認する。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 基本計算からオンラインへ | iLQR（2004）→Tassaほか（2012） | 局所LQ近似と前後方計算を、逐次再計画へ使う。DDPの歴史的起源自体はさらに古い。 |
| 入力制約と初期化 | Control-Limited DDP（2014）→Box-FDDP（2022） | 入力飽和を局所QPで扱い、力学を満たさない状態軌道からの初期化へ広げる。 |
| 一般制約と理論 | ProxDDP（2025）／Rouletほか（2025） | 近接法による制約処理と、Gauss–Newtonを通した収束解析は別の発展軸。 |

[微分可能MPC](#sec-a)や[動的ゲームのiLQGames／Potential iLQR](#sec-game)への接続もある。multiple shooting型DDPもあるため、「DDPは必ず状態を変数に持たない」という固定的な分類にはしない。

**収録文献：6件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-iterative-linear-quadratic-regulator-design-for-nonlinear-biological-movement-sy"></a> **［iLQR／DDP］** **［基礎追加］** Li・Todorov。iLQRの局所近似と前後方計算を学ぶ基礎。 | 2004 / **[Iterative Linear Quadratic Regulator Design for Nonlinear Biological Movement Systems](https://doi.org/10.5220/0001143902220229)** | 非線形な運動の軌道とフィードバックを、局所的なLQR計算の反復で求めるiLQRを提示。身体運動モデルを使って計算法を示した。 | 力学の線形化とコストの二次近似からLQR型の後退計算を行い、前進シミュレーションで改善する。DDPで用いる力学の二階微分との違いを確認できる。 | Tassaのオンライン軌道最適化、Differentiable MPC、ロボットNMPC。 | 局所解と初期軌道に依存する。一般の状態制約や接触モード探索をそのまま処理する方法ではない。 |
| <a id="ref-synthesis-and-stabilization-of-complex-behaviors-through-online-trajectory-optim"></a> **［iLQR／DDP］** DDP/iLQRによる非線形最適制御の実践的な起点。 | 2012 / **[Synthesis and Stabilization of Complex Behaviors through Online Trajectory Optimization](https://roboti.us/lab/papers/TassaIROS12.pdf)** | オンライン軌道最適化だけで、ヒューマノイドの起き上がりや大きな外乱からの回復動作を生成。より単純な運動では実時間計算も示した。 | DDP/iLQG系の計算をオンライン軌道最適化へ適用し、起き上がり・外乱回復などを生成した。当時のヒューマノイド例は実時間より遅く、acrobot・平面遊泳・一脚跳躍などの単純な例で実時間計算を示した。 | MPC、軌道最適化、四脚・多関節ロボットの動作生成。 | 局所解に依存し、接触・摩擦などの非滑らかな遷移と厳しい制約の扱いが難しい。 |
| <a id="ref-control-limited-differential-dynamic-programming"></a> **［iLQR／DDP］** **［基礎追加］** Tassa・Mansard・Todorov。入力制約を含むDDP/iLQRの基本。 | 2014 / **[Control-Limited Differential Dynamic Programming](https://doi.org/10.1109/ICRA.2014.6907001)** | DDPに入力の上下限制約を組み込み、飽和を考慮して軌道を改善する方法を提案。入力を後から切り詰める方法より効率的な計算を示した。 | 後退計算の局所制御問題をbox制約付きQPとして扱い、単なる入力クリッピングと異なる制約を考慮した軌道改善を行う。 | 入力飽和を持つロボット制御、MPCの制約処理、微分可能MPCの背景。 | 扱う入力制約と一般の非線形状態制約は異なる。局所近似と初期解依存は残る。 |
| <a id="ref-a-feasibility-driven-approach-to-control-limited-ddp"></a> **［初期軌道・入力制約］** Mastalliほか。力学を満たさない初期軌道から改善する。 | 2022 / **[A Feasibility-Driven Approach to Control-Limited DDP](https://doi.org/10.1007/s10514-022-10061-w)**<br>[公開全文](https://link.springer.com/article/10.1007/s10514-022-10061-w)・[Crocoddyl](https://github.com/loco-3d/crocoddyl) | 状態軌道も初期値として与えられるようにし、力学の不整合を減らしながら入力上下限を守るBox-FDDPを提案。複雑な脚ロボット動作とMPCへの適用を示した。 | single shootingの初期化制約を緩めるfeasibility-drivenな計算と、入力制約のactive-set処理を組み合わせる。 | 初期化が難しい軌道最適化、脚ロボットのDDP-MPC。 | 一般の障害物・状態制約をすべて厳密に扱う方法ではない。局所解・初期化・数値条件への依存が残る。 |
| <a id="ref-proxddp-proximal-constrained-trajectory-optimization"></a> **［一般制約・warm start］** Jalletほか。近接法とDDPを組み合わせる。 | 2025 / **[ProxDDP: Proximal Constrained Trajectory Optimization](https://doi.org/10.1109/TRO.2025.3554437)**<br>[著者解説・公開原稿](https://simple-robotics.github.io/publications/proxddp-tro-2025/) | DDP系の計算に近接法を組み込み、入力・状態などの制約を持つ軌道最適化を解く。初期解の再利用を含むソルバーを公開し、四脚の全身MPC実機で検証した。 | 制約処理、数値的な安定性、warm startをまとめて学ぶ実装付きの原著。ALIGATORに完全実装がある。 | 制約付きDDP-MPC、全身ロボット制御。 | 非凸問題の大域解を保証するものではない。有限反復時の制約誤差と実機の安全性は区別する。 |
| <a id="ref-on-global-and-local-convergence-of-iterative-linear-quadratic-optimization-algor"></a> **［収束理論を深く読む］** Rouletほか。iLQR/DDPとGauss–Newtonの関係。 | 2025 / **[On Global and Local Convergence of Iterative Linear Quadratic Optimization Algorithms for Discrete Time Nonlinear Control](https://www.jmlr.org/papers/v26/22-1271.html)**<br>[公開PDF・コード](https://www.jmlr.org/papers/v26/22-1271.html) | iLQRとDDPが何に収束し、近傍でどれほど速く改善するかを解析。一般化Gauss–Newtonとして捉え、正則化・力学・コストの性質と収束率の関係を詳しく示した。 | 85ページの理論論文で、導出を理解した後の深掘りに向く。停留点への大域収束と、追加構造の下で最小解へ収束する結果を分けて読む。 | 正則化やline searchの設計、iLQR/DDPの理論検討。 | 題名のglobal convergenceを、任意の非線形制御問題の大域最適解が得られるという意味に取らない。 |

**この区分の収録件数：6件。**

<a id="sec-p"></a>
### 4. MPPI・経路積分制御：PI²・Information-Theoretic MPC・ロバスト化

**論文の前に補う日本語書籍**：[非線形最適制御入門](japanese-foundations.html#book-oc)／[モンテカルロ法と準モンテカルロ法](japanese-foundations.html#book-mc)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[確率推論MPCのチュートリアル・総説（2026）](#ref-model-predictive-control-via-probabilistic-inference-a-tutorial-and-survey)で、経路積分と情報理論の関係をつかむ。

**核となる考え方**：軌道分布と経路コストを使う最適制御の方法族。経路積分による理論的導出、方策改善、オンライン入力列の更新、追従制御器を含むロバスト化を分けて追う。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 基礎と方策改善 | Kappen（2005）／PI²（2010） | HJBの変換と経路コストの重み付けを、サンプリングによる方策改善へ使う。 |
| オンライン計算と導出 | MPPI（2017）／IT-MPC（2018） | 重要度サンプリング・並列ロールアウトと、相対エントロピーを使う制御分布の最適化を結ぶ。 |
| 追従器・頑健性 | RMPPI（2021） | 低位制御器の追従能力も考慮して、サンプリングと名目状態を更新する。 |

サンプリング型MPC全体と、特定の仮定から導かれる経路積分制御は同義ではない。有限サンプルで得る入力、制約充足、理論上の最適分布の関係を分ける。

**収録文献：5件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-path-integrals-and-symmetry-breaking-for-optimal-control-theory"></a> **［経路積分の理論］** **［基礎追加］** Kappen。経路積分制御のHJB変換を学ぶ基礎。 | 2005 / **[Path Integrals and Symmetry Breaking for Optimal Control Theory](https://arxiv.org/abs/physics/0505066)** | 特定の構造を持つ確率最適制御を、軌道の期待値を計算する問題へ変換。難しいHJB方程式を線形化し、サンプリングで解く理論的な道筋を示した。 | 指数変換により非線形HJB方程式を線形化し、拡散過程の期待値・経路積分で価値関数を表す。モンテカルロ評価への理論的接続を与える。 | PI²、MPPI、情報理論に基づくサンプリング最適制御の背景。 | 制御と雑音の作用・コストに構造条件が必要。任意の最適制御問題を無条件に線形化する結果ではない。 |
| <a id="ref-a-generalized-path-integral-control-approach-to-reinforcement-learning"></a> **［PI²・方策改善］** **［基礎追加］** Theodorou・Buchli・Schaal。PI²による方策改善の原典。 | 2010 / **[A Generalized Path Integral Control Approach to Reinforcement Learning](https://jmlr.org/papers/v11/theodorou10a.html)** | 試した軌道の良し悪しで重みを付け、制御方策を更新するPI²を提案。勾配計算を使わずに方策を改善でき、ロボットの運動学習で有効性を示した。 | 経路積分の重み付けを用いるPolicy Improvement with Path Integrals（PI²）を導出し、ロボット運動学習で評価。オンラインMPPIとは更新対象と運用が異なる。 | 経路コストによる指数重み付け、サンプリング型方策探索とMPCの比較。 | 探索分布・方策表現・確率制御の仮定に依存。PI²とMPPIを同一アルゴリズムとして扱わない。 |
| <a id="ref-model-predictive-path-integral-control-from-theory-to-parallel-computation"></a> **［MPPI］** MPPIを研究する際の基盤。 | 2017 / **[Model Predictive Path Integral Control: From Theory to Parallel Computation](https://doi.org/10.2514/1.G001921)** | 多数の入力列を並列に試し、低コストの候補を反映して入力を更新するMPPIを提案。GPU計算を使い、非線形な対象のMPCを高速に実行する方法を示した。 | 経路積分制御をGPU並列計算へ展開し、サンプリングベースの実時間MPCとして実証。演算速度・追従性能の共通倍率は未確認。 | 車両の高速走行、接触を含むロボティクス、GPU搭載プラットフォーム。 | サンプリング数による計算負荷、モデル精度、入力分布設計、厳密な安全保証が課題。 |
| <a id="ref-information-theoretic-model-predictive-control-theory-and-applications-to-autono"></a> **［IT-MPC］** **［基礎追加］** Williams・Drews・Goldfain・Rehg・Theodorou。MPPIの基本論文。上の論文と異なり、情報理論から導出している。2017年プレプリント、2018年T-RO。 | 2018 / **[Information-Theoretic Model Predictive Control: Theory and Applications to Autonomous Driving](https://arxiv.org/abs/1707.02342)** | 情報理論からサンプリング型MPCの更新則を導出。未舗装路を高速走行する車両で試し、別のサンプリング最適化法であるCEMと比較した。 | 情報理論の変分的な関係と相対エントロピーを用いて制御分布の最適化を導出し、重要度サンプリングによるMPCを構成。実車で評価しCEM型MPCと比較する。 | MPPIの重みと分布更新の理解、情報理論に基づくMPC、GPUサンプリング制御。 | サンプル数・提案分布・モデル誤差に依存し、一般のハード制約保証は自動で得られない。雑誌版DOIは10.1109/TRO.2018.2865891。 |
| <a id="ref-robust-model-predictive-path-integral-control-analysis-and-performance-guarantee"></a> **［追従制御とロバスト化］** Gandhiほか。追従器の働きをサンプリングへ反映する。 | 2021 / **[Robust Model Predictive Path Integral Control: Analysis and Performance Guarantees](https://doi.org/10.1109/LRA.2021.3057563)**<br>[公開版](https://arxiv.org/abs/2102.09027) | 名目系と実際の系を一緒に予測し、低位の追従制御器を考慮してMPPIを更新するRMPPIを提案。性能上界を解析し、AutoRally実機で外乱下の走行を検証した。 | free energyの増加上界を、追従誤差・サンプリング誤差などと結ぶ。iLQGやCCMとの組合せを学ぶ接点。 | 追従器とMPPIの共同設計、外乱のある高速走行。 | 解析は構成・誤差境界の仮定に依存し、任意の障害物回避を厳密保証するものではない。 |

**この区分の収録件数：5件。**

<a id="sec-explicit"></a>
### 5. Explicit MPC：多パラメトリック最適化・制御則の事前計算と近似

**論文の前に補う日本語書籍**：[モデル予測制御](japanese-foundations.html#book-mpc)／[非線形最適化法](japanese-foundations.html#book-nlp)／[しっかり学ぶ数理最適化](japanese-foundations.html#book-opt)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Alessio–Bemporad（2009）](#ref-a-survey-on-explicit-model-predictive-control)を研究史の入口にする。学習による近似は[Amos（2023、公開版2025改訂）](#ref-tutorial-on-amortized-optimization)を併読する。

**核となる考え方**：MPCの最適入力を、状態・参照値などのパラメータに対する関数としてオフラインで求め、オンラインではその関数を評価する。線形モデル・二次評価・多面体制約の標準形では、多パラメトリック二次計画（mpQP）を使って区分的アフィン（PWA）制御則を構成する。critical regionは、同じアクティブ集合に対応する最適解の式が成り立つパラメータ領域を指す。

**選定方針（ChatGPTによる学習用の提案）**：基礎から近年の発展までを追える10件に絞る。原理、領域構成、オンライン探索、近似、NN表現と保証、近年の求解・実装という役割を押さえ、先行会議版との重複、個別の圧縮法、特殊な制約・対象系への拡張は外した。非線形・ハイブリッド系などの広がりは、まず収録サーベイで確認する。

**研究の展開**

| 流れ・分岐 | 収録文献の位置づけ | 何が拡張・接続されたか |
|---|---|---|
| 厳密解を作る・使う | Bemporadら（2002）→Tøndelらの領域構成／二分探索木（2003）。近年の求解はArnström・Axehill（2024）、コード生成はSchallerら（2026）。 | PWA構造の理論から、オフラインでの領域計算、オンライン評価、現在のソルバー・実装へ進む。 |
| 近似・学習による制御則の構成 | Bemporad・Filippi（2003）、Hertneckら（2018）、Karg・Lucia（2020）、Castellanoら（2026）。 | 最適性と複雑さのトレードオフ、ロバスト設計と統計的検証、ReLU表現、データ被覆に基づく近似は、それぞれ異なる枝。 |
| 全体像 | Alessio・Bemporad（2009）のサーベイ。 | 初期理論と対象系の拡張を俯瞰し、2010年代以降の発展は残りの原著で補う。 |

[一般のMPC・数値解法](#sec-k)から独立した分類。[微分可能MPC](#sec-a)は最適解の感度を学習に使い、[DPC](#sec-n)は予測ロールアウトを通して方策を訓練する。厳密なExplicit解、ReLUによる表現可能性、学習した方策の近似精度・保証は区別する。以下の枝分けと読書順は学習用の整理であり、全論文の直接の継承関係を意味しない。

**収録文献（厳選10件・原則年代順）**

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足（定量指標など） | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| <a id="ref-the-explicit-linear-quadratic-regulator-for-constrained-systems"></a> **［厳密解・QPの基礎］** Bemporad・Morari・Dua・Pistikopoulos。 | 2002 / **[The Explicit Linear Quadratic Regulator for Constrained Systems](https://doi.org/10.1016/S0005-1098(01)00174-1)** | 制約付き線形MPCの最適入力を、状態ごとに切り替わるアフィンな式として事前計算する方法を示した。制御中のQP求解を関数評価に置き換える基礎論文。 | 多パラメトリック二次計画（mpQP）として最適解の状態依存性を求め、連続な区分的アフィン（PWA）制御則を構成する。 | Explicit MPCの理論上の出発点。オンラインMPCと同じ最適入力を関数評価で得る。 | 設定したモデル・評価・制約・ホライズンに対する解。領域数と保存量が問題規模とともに増える。既存項目の誤ったDOIを訂正。 |
| <a id="ref-an-algorithm-for-multi-parametric-quadratic-programming-and-explicit-mpc-solutio"></a> **［厳密解・領域構成］** **［Explicit MPC追加］** Tøndel・Johansen・Bemporad。 | 2003 / **[An algorithm for multi-parametric quadratic programming and explicit MPC solutions](https://doi.org/10.1016/S0005-1098(02)00250-9)** | Explicit MPCで必要な状態領域と制御則を、隣り合う領域をたどって計算する方法を提案。事前計算を効率化し、実際に制御則を作る手順を示した。 | アクティブ集合と隣接領域の関係を利用し、領域とそのアフィン解を構成する。初期の幾何的mpQPアルゴリズムを学ぶ。 | 基礎理論から実際のオフラインソルバーへ進む入口。 | 退化・数値精度・多面体の幾何計算が重要。オフライン構成の高速化とオンライン評価の高速化を区別する。 |
| <a id="ref-evaluation-of-piecewise-affine-control-via-binary-search-tree"></a> **［厳密解・オンライン探索］** **［Explicit MPC追加］** Tøndel・Johansen・Bemporad。 | 2003 / **[Evaluation of piecewise affine control via binary search tree](https://doi.org/10.1016/S0005-1098(02)00308-4)** | 事前計算した多数の制御則から、現在状態に合うものを二分探索木で探す方法を提案。Explicit MPCを実行するときの領域判定を軽くした。 | 領域所属判定を二分探索木として構成し、オンラインで対応する制御則を探す計算を軽量化する。 | 組込みMPC、領域探索の演算量・実行時間の設計。 | 領域構成自体を速める論文とは役割が異なる。探索木の生成時間と保存量も評価する。 |
| <a id="ref-suboptimal-explicit-receding-horizon-control-via-approximate-multiparametric-qua"></a> **［近似mpQP］** **［Explicit MPC追加］** Bemporad・Filippi。 | 2003 / **[Suboptimal Explicit Receding Horizon Control via Approximate Multiparametric Quadratic Programming](https://cse.lab.imtlucca.it/~bemporad/publications/papers/jota-submpqp.pdf)** | Explicit MPCの最適解を少し近似して、必要な領域数を減らす方法を提案。近似誤差を評価し、制約充足と安定性を保てる条件も示した。 | 最適性条件の緩和を用いて近似度と領域数を調整し、解・最適値の誤差を評価する。条件の下で制約充足と閉ループ安定性を扱う。 | 領域数と最適性のトレードオフを設計する近似Explicit MPC。 | 近似誤差と保証条件の確認が必要。厳密な解の表現圧縮と、制御則自体の近似を区別する。 |
| <a id="ref-a-survey-on-explicit-model-predictive-control"></a> **［全体像・総説］** **［Explicit MPC追加］** Alessio・Bemporad。 | 2009 / **[A Survey on Explicit Model Predictive Control](https://cse.lab.imtlucca.it/~bemporad/publications/papers/nmpc08-survey-explicit.pdf)** | Explicit MPCの仕組みから、解の計算・探索・近似・対象系の拡張までを整理した総説。初期研究の全体像と、各原著の関係をつかめる。 | 厳密解、mpLP/mpQP、近似、ハイブリッド系、min-max制御などを整理する総説。 | 最初の全体把握と、個別原著への索引。 | 2009年以降のNN表現、近年のソルバーやコード生成は別の収録論文で補う。総説であり、独自の共通高速化倍率はない。 |
| <a id="ref-learning-an-approximate-model-predictive-controller-with-guarantees"></a> **［近似・学習と保証］** **［Explicit MPC追加］** Hertneck・Köhler・Trimpe・Allgöwer。 | 2018 / **[Learning an Approximate Model Predictive Controller with Guarantees](https://arxiv.org/abs/1806.04167)** | MPCの入力を学習器で近似するとき、誤差に耐える制御設計と統計的な検証を組み合わせた。安定性と制約充足に、統計的保証を付ける方法を示した。 | 入力誤差を許容するロバストMPCと、Hoeffdingの不等式に基づく学習誤差の検証を組み合わせ、安定性・制約充足の統計的保証を与える。 | NN等によるMPC近似と、学習した方策の閉ループ評価。 | 全状態に対する一様な決定論的認証とは異なる。MPC設計の仮定、許容誤差、検証データと信頼度を確認する。 |
| <a id="ref-efficient-representation-and-approximation-of-model-predictive-control-laws-via-"></a> **［PWAのNN表現・近似］** **［Explicit MPC追加］** Karg・Lucia。 | 2020〔プレプリント2018〕 / **[Efficient Representation and Approximation of Model Predictive Control Laws via Deep Learning](https://arxiv.org/abs/1806.10644)** | 線形MPCの区分的アフィン制御則をReLUのNNで表せることを示した。さらに、深いNNで制御則を近似し、必要な保存量を抑える方法を検討した。 | ReLU NNによるPWA制御則の厳密な表現可能性とネットワーク構造の理論を示し、深層NNによる近似と省メモリ化を評価する。 | Explicit制御則の別表現、MPC近似のネットワーク設計。 | 厳密に表現できるという存在・構成上の結果と、学習したNNの精度・制約保証は別。学習だけで元の最適解が再現されるとは限らない。 |
| <a id="ref-a-high-performant-multi-parametric-quadratic-programming-solver"></a> **［厳密解・mpQP計算］** **［Explicit MPC追加］** Arnström・Axehill。 | 2024 / **[A High-Performant Multi-Parametric Quadratic Programming Solver](https://arxiv.org/abs/2404.05511)** | 有効になる制約の組合せをたどって、Explicit MPCの解を計算する方法を提案。重い幾何計算を避け、既存ソルバーより速い事前計算を示した。 | アクティブ集合の組合せ的隣接性による連結グラフを利用し、重い多面体の幾何計算と探索する組合せを削減する。 | 近年の厳密mpQPソルバーPDAQPを理解する入口。 | 報告される高速化はオフライン解構成のベンチマーク。オンライン制御の実行時間の改善倍率とは区別する。 |
| <a id="ref-data-driven-acceleration-of-mpc-with-guarantees"></a> **［近似・データによる構成］** Castellano・Pan・Mallada。 | 2026 / **[Data-driven Acceleration of MPC with Guarantees](https://proceedings.mlr.press/v331/castellano26a.html)** | 多数のMPC解を事前に保存し、制御中はデータから入力を選ぶ方法を提案。十分なデータ被覆があれば、実行可能性と最適解からのずれを保証できる。 | 最適cost-to-goの上界に対してgreedyなルックアップ則を構成。十分なデータ被覆の下で再帰的実行可能性と最適性ギャップを評価し、論文の実験で通常のMPCに対し100～1000倍の高速化を報告する。 | オフライン解を蓄積してオンライン最適化を置き換える制御。 | 厳密なcritical region列挙によるExplicit MPCとは異なる近似系統。被覆条件・高次元でのデータ量・実験の比較条件を確認する。 |
| <a id="ref-automatic-generation-of-explicit-quadratic-programming-solvers"></a> **［厳密解・コード生成］** **［Explicit MPC追加］** Schaller・Arnström・Bemporad・Boyd。 | 2026〔プレプリント2025〕 / **[Automatic Generation of Explicit Quadratic Programming Solvers](https://web.stanford.edu/~boyd/papers/pdf/cvxpygen_mpqp.pdf)** | CVXPYで書いたQPから、Explicitな解とC/C++コードを自動生成する仕組みを開発。Pythonでの試作から、高速な関数評価による実装までをつないだ。 | CVXPYの問題記述をPDAQPとCVXPYgenに接続し、領域・係数の事前計算からC/C++コードとPythonインターフェースの生成まで統合する。 | 理論から現在のPython試作・Cコード実装へ進む橋渡し。 | 対象となるQPの構造と領域数に実用性が依存する。通常の反復型QPソルバーのコード生成と、Explicit写像のコード生成を区別する。 |

**読む順番（学習用の提案）**

1. **まず読む4本**：Alessio・Bemporad（2009、サーベイ）→Bemporadら（2002、基礎理論）→Tøndelら（2003、領域構成）→Tøndelら（2003、二分探索木）。何が厳密に求まり、どう事前計算して、どう実行するかを押さえる。
2. **近似・学習へ進む3本**：Bemporad・Filippi（2003、近似mpQP）で誤差と複雑さを学び、Karg・Lucia（2020、NN表現）とHertneckら（2018、統計的保証）を読む。表現できること、学習で近似できること、閉ループ保証が得られることを分ける。
3. **最近の発展を追う3本**：厳密解の計算・実装はArnström・Axehill（2024）→Schallerら（2026）。Castellanoら（2026）はデータに基づく近似・保証の別の枝として比較する。

**この区分の収録件数：10件。** 以前の19件から厳選した10件を、今回も維持する。

<a id="sec-sparse"></a>
### 6. スパース最適制御：maximum hands-off・予測入力列・疎なフィードバック

**論文の前に補う日本語書籍**：[スパースモデリング](japanese-foundations.html#book-sparse)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Nagahara–Yamamoto（2024）：基礎とMATLAB例](#ref-a-survey-on-compressed-sensing-approach-to-systems-and-control)から入り、[Ballottaほか（2026、公開原稿）](#ref-sparsity-in-control-problems-recent-trends-and-open-challenges)で近年の広がりを確認する。

**核となる考え方**：最小にしたいのは入力の大きさだけでなく、入力を出す時間、使う入力の数、制御器の結合数などである。時間方向のmaximum hands-off、予測入力列の圧縮、フィードバック構造の疎化を同じ章の枝として扱う。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 空間・結合の疎性 | Linほか（2013） | 閉ループ性能と通信結合数を同時に設計する。時間的な休止とは別。 |
| 予測制御と時間的休止 | Sparse Packetized MPC（2014）／Maximum Hands-Off（2016） | 予測入力列の通信量と、入力が非ゼロとなる時間を減らす並行する枝。 |
| 緩和の限界と不確かさ | Ikeda（2025）／Ganguly–Kashima（2026、原稿） | L₁緩和が一致しない場合の非凸計算と、パラメータ不確かさを含む理論へ広がる。 |

ここでのスパース性は設計する制御の性質。[SQP／RTI](#sec-sqp)でKKT行列やQPの疎構造を利用する高速計算とは区別する。後者を併用することはできる。

**収録文献：5件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-design-of-optimal-sparse-feedback-gains-via-the-alternating-direction-method-of-"></a> **［結合のスパース化］** Lin・Fardad・Jovanović。通信結合と性能を調整する。 | 2013 / **[Design of Optimal Sparse Feedback Gains via the Alternating Direction Method of Multipliers](https://doi.org/10.1109/TAC.2013.2257618)**<br>[公開版](https://arxiv.org/abs/1111.6188) | フィードバックゲインに疎性を促す罰則を加え、閉ループ性能を保ちながら通信結合を減らす。ADMMで疎な構造を選び、その構造の下でゲインを再最適化する。 | 時間方向に入力を休止するhands-offとは別の、制御器構造の疎性。H₂性能と結合数のトレードオフを扱う基準文献。 | 分散フィードバック設計、MPCの補助制御器の構造設計。 | ゲインに関する問題は一般に非凸。ADMMを使うだけで大域最適性は付かない。 |
| <a id="ref-sparse-packetized-predictive-control-for-networked-control-over-erasure-channels"></a> **［MPC・通信量の削減］** Nagahara・Quevedo・Østergaard。送る予測入力列を疎にする。 | 2014〔オンライン2013〕 / **[Sparse Packetized Predictive Control for Networked Control over Erasure Channels](https://doi.org/10.1109/TAC.2013.2294622)**<br>[公開版](https://arxiv.org/abs/1307.8242) | 通信で送る将来入力の列をスパースにし、パケット損失に備えつつデータ量を抑える。連続して失われるパケット数に上限がある場合の安定性条件を示した。 | 有限ホライズンの予測制御と、疎性を促す最適化を直接結ぶ。MPCへスパース性を組み込む最初の読書候補。 | 通信制約下の予測制御、バッファ付き制御入力送信。 | 損失回数の境界と設計条件が必要。疎な入力を送れば一般に安全になるという主張ではない。 |
| <a id="ref-maximum-hands-off-control-a-paradigm-of-control-effort-minimization"></a> **［時間方向のスパース性］** Nagahara・Quevedo・Nešić。入力を出さない時間を最大化する。 | 2016〔オンライン2015・初稿2014〕 / **[Maximum Hands-Off Control: A Paradigm of Control Effort Minimization](https://doi.org/10.1109/TAC.2015.2452831)**<br>[公開版](https://arxiv.org/abs/1408.3025) | 制御目標を達成しながら、入力が非ゼロとなる時間を最小にする。normality条件下でL₀最適化とL₁最適化が一致することを示し、滑らかさやフィードバックへの展開も扱った。 | 入力の大きさを抑えるだけでなく、作動する時間そのものを減らす。L₁緩和が使える条件を理解する基礎。 | アクチュエータ休止、通信・駆動資源を節約する制御。 | L₁最適化との同値性には条件がある。ゼロ入力が実機で安全に休止できる入力かどうかも確認する。 |
| <a id="ref-nonconvex-optimization-problems-for-maximum-hands-off-control"></a> **［L₁緩和の限界と非凸計算］** Ikeda。特異な場合まで扱う。 | 2025〔オンライン・初稿2024〕 / **[Nonconvex Optimization Problems for Maximum Hands-Off Control](https://doi.org/10.1109/TAC.2024.3474061)**<br>[公開初稿](https://arxiv.org/abs/2402.10402) | L₁緩和だけでは最も疎な入力を得られない場合に向け、hands-off問題と同値になる非凸評価のクラスを導く。DC最適化を使う計算法と数値例を示した。 | L₀とL₁の同値条件を外すと何が変わるかを学ぶ。TAC 70(3):1905–1912。 | 特異なhands-off問題、疎性を強く要求する制御。 | 問題の同値性と、数値アルゴリズムが大域最適解に到達する保証は別である。 |
| <a id="ref-robust-maximum-hands-off-optimal-control-existence-maximum-principle-and-l-l-equ"></a> **［不確かさへの展開］** Ganguly・Kashima。パラメータ誤差を含むhands-off制御。 | 2026〔プレプリント〕 / **[Robust Maximum Hands-Off Optimal Control: Existence, Maximum Principle, and L₀–L₁ Equivalence](https://arxiv.org/abs/2601.07256)**<br>[公開全文](https://arxiv.org/html/2601.07256v1) | パラメータが不確かな制約付き線形系で、最も疎な入力を求める問題を扱う。解の存在、最大原理、所定の仮定下でのL₀・L₁最適解の同値性を示した。 | 無限個の不確かさに対応する制約を持つ問題を、ロバスト最適化の計算法へ接続する。新しい理論の枝として読む。 | モデル不確かさを考慮したスパース最適制御。 | 査読刊行版とは区別する。対象は所定の線形系・不確かさ集合で、一般の非線形ロバストMPCの結果ではない。 |

**この区分の収録件数：5件。**

<a id="sec-nn"></a>
### 7. 深層モデルを使うMPC：NNダイナミクス・RNN・残差学習

**論文の前に補う日本語書籍**：[深層学習](japanese-foundations.html#book-dl)／[PythonとCasADiで学ぶMPC](japanese-foundations.html#book-casadi)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Renほか（2022）](#ref-a-tutorial-review-of-neural-network-modeling-approaches-for-model-predictive-con)でモデル構築を学び、[Wuほか（2025）](#ref-a-tutorial-review-of-machine-learning-based-model-predictive-control-methods)で実装と閉ループ保証を俯瞰する。

**核となる考え方**：物理モデル全体または残差をNNで表し、そのモデルで将来を予測しながらオンライン最適化する。独立したシステム同定の一覧ではなく、観測器・予測誤差・制約・計算時間までMPCへつながる研究を選ぶ。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 動的モデルと状態推定 | LSTM-MPC（2021）→GRUへの設計（2024） | RNNのISS・増分ISS、内部状態の観測器、予測長と安定性を結ぶ。単純な新旧モデルの優劣ではない。 |
| 実モデルとのずれ | NNARXのoffset-free MPC（2023）／Deep MPC（2025） | 誤差推定で定常偏差を補う枝と、既知物理モデルに深層残差を学ぶ枝。 |
| 計算への統合 | Real-Time Neural MPC（2023） | 大きいNNの評価・微分を勾配型ソルバーへ組み込み、実機で使う。 |

NNで**予測モデル**を作る本章、MPCの**解を微分**する[微分可能最適化](#sec-a)、制御**方策を直接学ぶ**[DPC](#sec-n)を区別する。[GP](#sec-h)・[Koopman](#sec-e)は予測表現と保証の異なる方法族として残す。

**収録文献：5件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-learning-model-predictive-control-with-long-short-term-memory-networks"></a> **［RNNモデル・観測器・MPC］** Terziほか。モデルの安定性から制御を組む。 | 2021 / **[Learning Model Predictive Control with Long Short-Term Memory Networks](https://doi.org/10.1002/rnc.5519)**<br>[著者公開原稿](https://re.public.polimi.it/bitstream/11311/1169896/5/11311-1169896_Bonassi.pdf) | LSTMを予測モデルに使うMPCで、ネットワークの安定性条件、状態観測器、追従制御を構成。pH反応器の数値例を通して設計手順を示した。 | LSTM内部状態の推定と、ISS・増分ISSを使う解析を結ぶ。学習誤差が小さいことと、制御の保証が付くことの違いを学べる。 | 入出力履歴から学ぶ非線形プロセスMPC。 | モデルに対する安定性と実プラントに対する保証の条件を分ける。任意の学習済みLSTMに使える保証ではない。 |
| <a id="ref-real-time-neural-mpc-deep-learning-model-predictive-control-for-quadrotors-and-a"></a> **［深層モデルの実時間化］** Salzmannほか。大きいNNを勾配型NMPCへ入れる。 | 2023 / **[Real-Time Neural MPC: Deep Learning Model Predictive Control for Quadrotors and Agile Robotic Platforms](https://doi.org/10.1109/LRA.2023.3246839)**<br>[著者公開PDF](https://rpg.ifi.uzh.ch/docs/RAL2023_Salzmann.pdf)・[コード](https://github.com/TUM-AAS/neural-mpc) | 容量の大きいNNダイナミクスをNMPCで使う計算構成を提案。GPUなどを活用し、クアッドロータの高速・障害物近傍の実機飛行で検証した。 | 学習モデルの微分・評価をオンライン最適化にどう組み込むかを学べる。モデルをNNにする研究であり、MPC制御則自体をNNへ置き換える研究とは異なる。 | 機械系の残差モデル、実時間Neural MPC。 | 高速な数値計算・実験性能と、全状態での制約保証・安定性は別。ハードウェアとモデル構成への依存がある。 |
| <a id="ref-robust-offset-free-nonlinear-model-predictive-control-for-systems-learned-by-neu"></a> **［モデル誤差・定常偏差］** Xieほか。学習モデルと実系のずれを扱う。 | 2023 / **[Robust Offset-Free Nonlinear Model Predictive Control for Systems Learned by Neural Nonlinear Autoregressive Exogenous Models](https://doi.org/10.1002/rnc.6883)**<br>[公開版](https://arxiv.org/abs/2210.06801) | NNARXで学んだ非線形モデルを使い、モデル誤差が残っても定常偏差をなくす追従MPCを構成。外乱推定とロバストな制約処理を組み合わせた。 | 入力・出力の履歴から予測するモデルに、観測器・誤差補償・制約の引き締めを接続する。 | 学習モデル付きのoffset-free MPC、プロセス制御。 | 保証には学習モデルの安定性、誤差境界などの条件がある。予測精度だけで実プラントの制約充足は保証できない。 |
| <a id="ref-nonlinear-mpc-design-for-incrementally-iss-systems-with-application-to-gru-netwo"></a> **［終端要素の設計負荷を減らす］** Bonassiほか。予測長から安定性を保証する。 | 2024〔初稿2023〕 / **[Nonlinear MPC Design for Incrementally ISS Systems with Application to GRU Networks](https://doi.org/10.1016/j.automatica.2023.111381)**<br>[公開全文](https://arxiv.org/html/2309.16428v2) | 増分ISSを満たす系に対し、安定性を確保する最小予測長を使ったNMPCを提案。GRUモデルと観測器へ適用し、重い終端要素の計算を避ける構成を示した。 | モデルの収束特性をMPCの設計条件へ変える理論。ネットワーク構造の比較だけで終わらず、閉ループ解析まで追える。 | 安定なRNNモデルを使う出力フィードバックNMPC。 | 増分ISS等の仮定と予測長条件が必要。一般のNNモデルに終端条件が不要になるという結果ではない。 |
| <a id="ref-deep-model-predictive-control-with-stability-guarantees"></a> **［深層残差学習・Tube MPC］** Mishra・Gasparino・Chowdhary。学習中の頑健性を考える。 | 2025〔公開初稿2021〕 / **[Deep Model Predictive Control With Stability Guarantees](https://doi.org/10.1109/TAC.2025.3550072)**<br>[所属機関の書誌・要旨](https://experts.illinois.edu/en/publications/deep-model-predictive-control-with-stability-guarantees/)・[初期原稿](https://arxiv.org/abs/2104.07171) | 未知の状態依存誤差を深層NNで近似し、Tube型NMPCと組み合わせる。所定の誤差・モデル条件の下で制約充足とISSを示し、学習中の振る舞いも設計に含めた。 | 出力層を速く、内部層を遅く更新する構成を使う。TAC 70(8):5460–5467。全モデルをNNで置換する枝とは別に、既知モデルへの補正として読む。 | 既知の物理モデルと深層残差を組み合わせる頑健MPC。 | matchedかつ有界な不確かさ等の仮定を要する。公開初稿と刊行版には著者・記述の差があり、保証は刊行版の条件を確認する。 |

**この区分の収録件数：5件。**

<a id="sec-h"></a>
### 8. Gaussian Processに基づく予測制御：GP-MPCと計算近似

**論文の前に補う日本語書籍**：[ガウス過程と機械学習](japanese-foundations.html#book-gp)／[モデル予測制御](japanese-foundations.html#book-mpc)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[GP-MPCチュートリアル（2024）](#ref-a-tutorial-on-gaussian-process-learning-based-model-predictive-control)で回帰・多段予測・確率制約を順に学び、章内の[GPダイナミクス総説（2025）](#ref-gaussian-processes-for-dynamics-learning-in-model-predictive-control)へ進む。

**核となる考え方**：GP（Gaussian Process）は、未知の関数を確率的にモデル化し、予測平均と分散を求める回帰手法。GP-MPCはそれを力学・残差の予測に用い、不確実性を考慮した制約設計や計算近似へ展開する。

**研究の展開**

| 流れ・分岐 | 収録文献の位置づけ | 何が拡張・接続されたか |
|---|---|---|
| 初期のGP予測制御 | Gaussian Process Model Based Predictive Control（2004） | 非線形予測とモデル不確実性をGPで扱う構成。 |
| 残差学習・確率制約 | Cautious GP-MPC（2020） | 公称モデル＋GP残差、共分散伝播、確率制約を統合。 |
| 応用と計算の展開 | レーシングMPC（2019）／Zero-order GP-MPC（2023） | 実機応用の枝と、共分散関連の感度近似による計算量削減の枝。刊行順だけで一つの続編列にはしない。 |
| 実装基盤への展開 | L4acados（掲載年表記2026） | GP/NNモデルの外部感度を実時間SQPへ接続。GP専用の制御理論ではなく、他の学習モデルにも広がる基盤。 |

確率的MPC全体とGP-MPCは同義ではない。SMPC・GPのTutorial（T）とGP-MPC総説を併読する。

**収録文献**

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足（定量指標など） | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| <a id="ref-gaussian-process-model-based-predictive-control"></a> **［GP予測・不確実性］** GPの確率的予測と分散をMPCへ導入 | 2004 / **[Gaussian Process Model Based Predictive Control](https://mlg.eng.cam.ac.uk/pub/pdf/KocMurRasGir04.pdf)** | GPで非線形モデルを学び、予測の不確かさも入力の計画に使うMPCを提案。pH制御の数値例で、平均予測だけでなく分散を考慮する制御を示した。 | 物理モデルが不十分でもGPによって非線形入出力関係を表し、モデル不確実性を予測に組み込めることを初期の制御例で示した。改善の百分率は今回の確認範囲で取得できていない。 | 化学・プロセス制御の学習型MPC、未知非線形プラントの確率的予測に適用できる。 | GPの標準的な訓練計算はデータ数に対して概ね3乗で増え、長期分布伝播も重い。分布の正規近似が実系で正しいとは限らない。 |
| <a id="ref-learning-based-model-predictive-control-for-autonomous-racing"></a> **［実機応用］** GP-MPCの実車／レース系への橋渡し。 | 2019 / **[Learning-Based Model Predictive Control for Autonomous Racing](https://doi.org/10.1109/LRA.2019.2926677)** | レーシングカーの走行データからモデル誤差を学び、その補正をMPCへ反映。小型実機で、事前のモデルだけに頼る制御から走行性能を改善する方法を示した。 | GPを含むモデル誤差の学習をMPCへ組み込み、小型レーシングカーの実機で走行性能の改善を検証した。モデルの更新と、更新した予測を使う制御計画を組み合わせた研究。 | 自動運転、タイヤ特性の変動を含むGP-MPCの実機適用。 | 学習データ範囲外・極限摩擦条件・確率的保証と実時間制約が課題。 |
| <a id="ref-cautious-model-predictive-control-using-gaussian-process-regression"></a> **［GP予測・不確実性］** GP残差モデル・共分散伝播・確率制約 | 2020 / **[Cautious Model Predictive Control Using Gaussian Process Regression](https://arxiv.org/abs/1705.10702)** | 物理モデルとGPによる誤差補正を組み合わせ、予測の不確かさに応じて制約に余裕を取るMPCを提案。小型車の実験で、性能と安全性の改善を示した。 | 公称力学とGP残差学習を組み合わせ、将来共分散を伝播して確率的制約を引き締めるCautious MPCを提案した。モデル誤差に慎重な挙動を示したが、要旨で汎用的な成功率・改善率は示していない。 | 不確かな摩擦特性を持つ車両、制約付きロボット、確率的な安全余裕設計に応用できる。 | 状態分布のガウス近似、共分散伝播の線形化、GP校正精度を仮定する。共分散まで最適化すると計算量が大きく、真の違反確率保証には追加条件が必要となる。 |
| <a id="ref-zero-order-optimization-for-gaussian-process-based-model-predictive-control"></a> **［共分散感度の近似］** 共分散関連のSQP感度を近似し計算量削減 | 2023 / **[Zero-Order Optimization for Gaussian Process-Based Model Predictive Control](https://arxiv.org/abs/2211.15522)** | GP-MPCで重い共分散関連の微分計算を近似し、GPの評価を並列化。実行可能な近似解を保ちながら計算時間を減らし、状態数が増えたときの負荷を抑えた。 | Zero-order SQPによって、対象の疎QP求解について状態次元n_xに対する理論的複雑度をO(n_x^6)からO(n_x^3)へ低減できると示し、数値例でスケーリングを検証した。 | 実時間GP-MPC、GP残差を含むChance-Constrained NMPC、SQP-RTIの高速化に使える。 | 計算量低減は特定構造のQPの各反復に関する値であり、GP推論・全システム時間の改善倍率を示さない。微分を近似するため通常のNLP局所最適性は一般に失われる。 |
| <a id="ref-gaussian-processes-for-dynamics-learning-in-model-predictive-control"></a> **［GP-MPC総説］** GPを用いた動的モデル学習とMPCの総説 | 2025 / **[Gaussian Processes for Dynamics Learning in Model Predictive Control](https://doi.org/10.1016/j.arcontrol.2025.101034)** <br>[公開版（arXiv）](https://arxiv.org/abs/2502.02310) | GPで学んだモデルをMPCに使う際の、予測誤差・不確かさ・計算負荷を整理した総説。モデル学習から多段予測、制約設計、実時間実装までをつなぐ。 | GPの平均だけを使うMPCから、共分散を用いる確率的MPCと実時間近似までの技術的課題を体系化した。独自の比較実験に基づく一律の改善率はない。 | GP-MPCの研究課題整理、近似推論とロバストMPCの手法選択に適している。 | モデル誤差がGPの予測分散に十分反映されるとは限らず、オンライン更新時の計算負荷と安全性保証の両立が未解決である。 |
| <a id="ref-l4acados-learning-based-models-for-acados-applied-to-gaussian-process-based-pred"></a> **［学習モデル接続基盤］** GPモデルとacadosを接続した実時間GP-MPC | 2026 / **[L4acados: Learning-Based Models for acados, Applied to Gaussian Process-Based Predictive Control](https://arxiv.org/abs/2411.19258)** | PythonのNN・GPモデルを高速なMPCソルバーacadosへ接続するL4acadosを開発。計算効率を評価し、小型車と実車で学習モデル付きMPCを動かした。 | Pythonで得た外部感度をacadosのSQPへ渡すL4acadosを実装し、既存ソフトより高速・良好な計算量スケーリングを報告した。GP-MPCを小型車およびフルスケール車の2種類のハードウェアで実証したが、全体の改善倍率は要旨で定量化されていない。 | GPyTorch・NNモデルを使うリアルタイムNMPC、acadosベースのモデル誤差補償制御に応用できる。 | 外部感度計算・ソルバー連携・GPU/CPU転送に性能が左右される。実機の2例は汎用的な安全保証を示すものではなく、別対象では計算時間と信頼性の評価が必要である。 |

**この区分の収録件数：6件。**

<a id="sec-e"></a>
### 9. Koopman MPC：作用素近似・入力付き予測・閉ループ保証

**論文の前に補う日本語書籍**：[クープマン解析](japanese-foundations.html#book-koopman)／[モデル予測制御](japanese-foundations.html#book-mpc)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Koopmanの理論・制御チュートリアル（2026）](#ref-koopman-operator-theory-fundamentals-control-and-applications)、[誤差境界と閉ループ保証の総説](#ref-an-overview-of-koopman-based-control-from-error-bounds-to-closed-loop-guarantees)を入口にする。

**核となる考え方**：観測関数に作用するKoopman作用素を近似し、非線形系を高次元の線形予測で扱う。一般的な同定の網羅ではなく、入力付き表現、MPCへの組込み、近似誤差と閉ループ保証を追う。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 予測表現の基礎 | EDMD／DMD with Control（2015） | 作用素の有限次元近似と入力の取り込みを学ぶ。DMDや1931年の作用素論の原著は保存版から参照できる。 |
| MPCと学習表現 | Koopman MPC（2018）／深層埋込み（2018） | 線形予測を制約付き最適制御へ使う枝と、観測関数を学ぶ枝。 |
| 近年の保証と計算 | kernel EDMD、安定な埋込み、指数安定性、BoxQP | モデルの表現力、誤差境界、制御の安定性、高速計算を別の論点として確認する。 |

学習で線形表現を得たことだけでは、全状態での厳密な線形化やMPCの安定性は保証されない。[深層モデルMPC](#sec-nn)との共通点・違いを、入力表現と誤差保証から比較する。

**収録文献：8件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-a-data-driven-approximation-of-the-koopman-operator-extending-dynamic-mode-decom"></a> **［EDMD］** **［基礎追加］** Williams・Kevrekidis・Rowley。辞書関数を使うEDMDの原典。 | 2015 / **[A Data-Driven Approximation of the Koopman Operator: Extending Dynamic Mode Decomposition](https://arxiv.org/abs/1408.4408)** | 状態から計算する複数の観測量を使い、非線形系の時間変化をデータから線形近似するEDMDを提案。Koopman作用素の固有値・固有関数などを推定できるようにした。 | 辞書空間上の最小二乗近似としてExtended DMDを構成し、固有値・固有関数・モードをデータから求める。 | Korda–Mezićの持ち上げ予測器、Kernel EDMD、Koopman MPC。 | 有限辞書と有限データの近似である。入力付き予測器・真の制御対象の安定性は追加理論を要する。 |
| <a id="ref-dynamic-mode-decomposition-with-control"></a> **［入力付きDMD］** **［基礎追加］** Proctor・Brunton・Kutz。DMDに入力の影響を明示するDMDcの基本。 | 2016 / **[Dynamic Mode Decomposition with Control](https://arxiv.org/abs/1409.6358)** | 状態変化を、系自身の動きと制御入力の効果に分けて学ぶDMDcを提案。入力の影響で通常のDMDが誤る問題に対処し、制御に使える予測モデルを作った。 | 状態と入力のスナップショットを用いて線形入力状態予測器を同定するDMDcを構成。EDMDの辞書拡張とは別の役割を持つ。 | 入力付きKoopman予測器を理解する準備、データ駆動線形モデルとMPC。 | 十分な励起とモデル表現が必要。DMDcだけで任意の非線形系が大域的に線形化されるわけではない。 |
| <a id="ref-deep-learning-for-universal-linear-embeddings-of-nonlinear-dynamics"></a> **［自律系の表現学習］** 非線形系の埋め込み座標を深層学習 | 2018 / **[Deep Learning for Universal Linear Embeddings of Nonlinear Dynamics](https://doi.org/10.1038/s41467-018-07210-0)** | 非線形な動きを線形に予測しやすい座標へ変換するNNを学習。座標変換を手作業で選ぶ負担を減らし、振動系や流体系で予測できることを示した。 | 学習した座標上で近似線形の時間発展を表せることを数値例で示し、あらかじめ辞書を手作業で選ぶ必要性を減らした。汎用的な予測誤差改善率は問題ごとに異なる。 | Deep Koopman、潜在状態MPC、非線形システムの低次元表現学習に利用できる。 | 有限データとNN構造に依存し、入力付き制御モデル、安定性、可観測性、物理的意味、分布外状態での予測を自動では保証しない。 |
| <a id="ref-linear-predictors-for-nonlinear-dynamical-systems-koopman-operator-meets-model-p"></a> **［入力付き予測・MPC］** 持ち上げ線形モデルで非線形MPCを実現 | 2018 / **[Linear Predictors for Nonlinear Dynamical Systems: Koopman Operator Meets Model Predictive Control](https://arxiv.org/abs/1611.03537)** | 非線形系のデータを観測量の空間へ変換し、線形の予測モデルを構築。そのモデルでMPCを組み、非線形系の制御を線形予測とQPで扱う方法を示した。 | EDMD型の最小二乗同定で線形予測器を構築し、論文の数値例では通常の局所線形化等より良好な予測を報告した。MPCのオンライン最適化は持ち上げ状態の次元に応じた線形QPの構造にできる。 | 高速な非線形予測制御、線形MPCソルバーの再利用、PDEを含むデータ駆動制御に適用できる。 | 持ち上げ辞書と学習範囲によって予測精度が変わり、有限次元近似の誤差が閉ループ安定性・制約違反を引き起こし得る。 |
| <a id="ref-kernel-edmd-for-data-driven-nonlinear-koopman-mpc-with-stability-guarantees"></a> **［MPCの閉ループ保証］** Kernel EDMD予測誤差から実用安定性を解析 | 2025 / **[Kernel EDMD for Data-Driven Nonlinear Koopman MPC with Stability Guarantees](https://doi.org/10.1016/j.ifacol.2025.11.080)** | カーネルEDMDで学んだモデルを使うMPCについて、誤差を見込んだ安定性を解析。所定の条件で、安定化用の終端条件なしでも目標付近へ収束することを示した。 | 状態に比例する点ごとのモデル誤差上界を前提に、終端安定化条件を置かずにKoopman MPCの原点への実用的漸近安定性を証明した。数値シミュレーションでも理論を検証したが、具体的な改善率は公開要旨にはない。 | Kernel Koopman MPC、学習モデルの誤差境界を利用した安定性保証に利用できる。 | 点ごとの誤差上界をデータから実証する必要があり、得られるのは原点付近の実用的漸近安定性である。大域的な厳密安定化や任意のモデル誤差には適用できない。 |
| <a id="ref-koopman-boxqp-solving-large-scale-nmpc-at-khz-rates"></a> **［高速求解］** Koopmanモデル学習よりもオンライン求解技術に重点を置く分岐。 | 2026 / **[Koopman-BoxQP: Solving Large-Scale NMPC at kHz Rates](https://proceedings.mlr.press/v331/wu26a.html)** | 高次元のKoopmanモデルを使う制御問題を、構造を持つbox制約付きQPへ変換。専用の求解法により、約千変数の数値例をkHz級で計算した。 | 持ち上げ変数の消去、BoxQP化、Warm Startと構造化内点法を組み合わせ、1040変数・2080不等式制約の数値例でkHz級の求解を報告した。 | 高次元NMPC、高速線形代数、GPU/CPU最適化、Koopman制御。 | BoxQP構造への依存、学習モデルの予測誤差、一般NLPへの移植性。 |
| <a id="ref-learning-stable-koopman-embeddings-for-identification-and-control"></a> **［モデルの構造・表現学習］** 安定性を構造的に組み込んだKoopman埋め込み | 2026 / **[Learning Stable Koopman Embeddings for Identification and Control](https://doi.org/10.1016/j.automatica.2026.113003)** | 安定性や安定化可能性を、Koopmanモデルの構造に組み込んで学習する方法を提案。予測精度だけでなく、制御に必要な性質を備えたモデルを作れるようにした。 | 安定性の構造を持ち上げモデルのパラメータ化に組み込むことにより、単に予測誤差を最小化するだけの設計と異なる制御向け表現を示した。数値的な誤差削減割合は原著要旨からは確認できない。 | 学習モデルに保証を埋め込むKoopman-MPC、構造保存型システム同定に使える。 | モデルの表現力と構造制約の両立が課題で、学習分布外での真の非線形系の安定性が無条件に保証されるわけではない。 |
| <a id="ref-on-the-exponential-stability-of-koopman-model-predictive-control"></a> **［MPCの閉ループ保証］** Koopman MPCの元の非線形系に関する指数安定性 | 2026 / **[On the Exponential Stability of Koopman Model Predictive Control](https://proceedings.mlr.press/v331/shang26a.html)** | Koopmanモデルを使うLQRとMPCで、元の非線形系がどの条件で指数的に収束するかを証明。振子の数値例でも、通常の局所線形化MPCより良い挙動を示した。 | 持ち上げ写像と1ステップ誤差が原点でLipschitzという条件などの下で、局所指数安定性を証明した。倒立振子の数値例では線形化MPCより収束挙動と累積コストが改善したが、要旨に具体的な割合はない。 | Koopman MPCの終端コスト・終端集合設計と閉ループ安定性評価に利用できる。 | 保証は原点近傍の局所結果であり、持ち上げ写像・誤差のLipschitz条件を実データから検証する難しさが残る。 |

**この区分の収録件数：8件。**

<a id="sec-d"></a>
### 10. DeePC：軌道データによる予測から非線形・保証へ

**論文の前に補う日本語書籍**：[モデル予測制御](japanese-foundations.html#book-mpc)／[しっかり学ぶ数理最適化](japanese-foundations.html#book-opt)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Berberich–Allgöwerの総説（2025）](#ref-an-overview-of-systems-theoretic-guarantees-in-data-driven-model-predictive-cont)と[著者のDeePCチュートリアル](#ref-deepc-lecture)を使う。

**核となる考え方**：DeePC（Data-Enabled Predictive Control）は、入出力の軌道データを組み合わせて予測を表し、そのまま制約付き最適化に使う方法。Willemsの基本補題によるLTI軌道表現を基盤に、ノイズ・追従・非線形系への拡張が進んでいる。

**研究の展開**

| 流れ・分岐 | 収録文献の位置づけ | 何が拡張・接続されたか |
|---|---|---|
| 軌道表現の基礎 | Willemsの基本補題（2005） | 持続励起とLTI軌道のデータ表現を結び付ける。 |
| 制御方式としての構成 | DeePC（2019） | Hankelデータによる軌道表現から制約付き予測制御を構成。 |
| ノイズ・保証・定式化 | Robust Data-Driven MPC（2021）、Offset-free（2022）、Bridging（2023） | 安定性・ロバスト性、定常偏差、直接法と同定経由の間接法の関係へ枝分かれ。 |
| 非線形系への展開 | Linear Tracking Part II（2022）、Local DPC／Select-DPC（2026） | 局所的なデータ更新・選択・逐次QPなどを使って非線形性へ対応する並行する枝。 |

非線形の各論文を単一アルゴリズムの改良列とはしない。Koopmanとの接点は「データから予測制御を構成する」点で、予測表現自体は別の方法。

**収録文献**

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足（定量指標など） | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| <a id="ref-a-note-on-persistency-of-excitation"></a> **［軌道表現の基礎］** 持続励起データからLTI軌道を表現する基本補題 | 2005 / **[A Note on Persistency of Excitation](https://doi.org/10.1016/j.sysconle.2004.09.003)** | 十分に励起された一つの入出力データから、線形系の他の軌道も表せる条件を証明。モデル行列を同定せずに予測するDeePCの理論的な土台となった。 | 入力が必要次数で持続励起されれば、長さLのすべての許容軌道が一つのHankelデータ行列の列空間で表現できることを示した。主要成果は実験改善率ではなくランク条件による厳密な表現性である。 | DeePC、データ駆動状態推定、線形システムの行動的同定の基礎になる。 | ノイズなしLTI系と十分な持続励起が本質的な前提であり、非線形系・ノイズ下ではそのままの等価性は成立しない。LTI系への入力上下限制約自体は基本補題と矛盾しない。 |
| <a id="ref-data-enabled-predictive-control-in-the-shallows-of-the-deepc"></a> **［DeePC・保証］** Hankelデータからモデルを明示せずMPCを構築 | 2019 / **[Data-Enabled Predictive Control: In the Shallows of the DeePC](https://arxiv.org/abs/1811.05890)** | 入出力データの組合せから将来軌道を予測し、そのまま制御入力を最適化するDeePCを提案。雑音のない線形系では、条件の下で通常のMPCと等価になる。 | 有限の入出力データから予測軌道を最適化し、ノイズなし決定論的LTI系では適切な条件下で従来MPCと等価な入力を得ることを証明した。ノイズ系では正則化による実験的改善を示したが数値倍率は未確認。 | モデルを明示的に同定しないMPC、未知プラントのオンライン制御に利用できる。 | ノイズなしLTIという等価性の条件は強く、ノイズ・非線形・データ量増加により正則化・次元削減・安定性解析が必要になる。 |
| <a id="ref-data-driven-model-predictive-control-with-stability-and-robustness-guarantees"></a> **［DeePC・保証］** 直接データ駆動MPCの安定性・ロバスト性 | 2021 / **[Data-Driven Model Predictive Control with Stability and Robustness Guarantees](https://doi.org/10.1109/TAC.2020.3000182)** | 雑音を含む入出力データから制御するMPCを設計。データに誤差があっても、条件の下で状態が目標付近へ収束することを理論的に示した。 | 測定ノイズなどを考慮したMPC設計で、明示的な同定モデルに頼らずに閉ループ実用安定性を保証する十分条件を提示した。性能改善率より、ノイズ上界と設計条件の明確化が主要成果である。 | 制御保証を重視するDeePC系、未知線形プラントの実装に有用である。 | 小さなノイズ、十分な励起、特定の重みやホライズンなどの条件を要する。一般の非線形・時変系への厳密な保証はそのまま適用できない。 |
| <a id="ref-linear-tracking-mpc-for-nonlinear-systems-part-ii-the-data-driven-case"></a> **［非線形・局所データ］** 局所データ更新による非線形システムの追従制御 | 2022 / **[Linear Tracking MPC for Nonlinear Systems—Part II: The Data-Driven Case](https://arxiv.org/abs/2105.08567)** | 新しい入出力データで局所的な予測を更新し、未知の非線形系を線形MPCで追従制御。入力制約を守り、到達可能な最適平衡点付近へ収束する条件を示した。 | 逐次更新するデータ駆動線形MPCにより、一定の局所条件下で非線形系の実用的追従保証を与え、数値例で追従を示した。定量的な追従誤差の改善率は要旨では確認できない。 | 局所線形化モデルを明示同定しないNonlinear Data-Driven MPCに適用できる。 | 動作点に近い局所データ、十分な励起、局所モデル誤差の小ささに依存する。大域非線形性や急激なモード遷移に対応するには追加工夫が必要である。 |
| <a id="ref-offset-free-data-driven-predictive-control"></a> **［定常偏差］** 従来MPCとDeePCを制御目的の観点で結ぶ教材。 | 2022 / **[Offset-free Data-driven Predictive Control](https://doi.org/10.1109/CDC51059.2022.9992453)** | MPC・サブスペース予測制御・DeePCで、目標からの定常的なずれをなくす設計を比較。データ駆動制御にもオフセット除去を組み込む考え方を解説した。 | 積分作用・増分入力やデータ駆動法の正則化を関連づけ、オフセットフリー追従を説明。論文の特定実験改善率は未確認。 | 精密サーボの定常誤差対策、DeePCへの積分構造導入。 | 観測ノイズ、ランク・励起条件、外乱推定に関する仮定。 |
| <a id="ref-bridging-direct-and-indirect-data-driven-control-formulations-via-regularization"></a> **［直接法と間接法］** 直接法と間接法の関係を正則化の観点から整理 | 2023 / **[Bridging Direct and Indirect Data-Driven Control Formulations via Regularizations and Relaxations](https://doi.org/10.1109/TAC.2022.3148374)** | データから直接制御する方法と、モデルを同定してから制御する方法の関係を解析。正則化を入れた直接法を、間接法の緩和として理解できることを示した。 | 両形式が特定の正則化と制約緩和の下で対応することを解析し、従来異なると見られていた設計問題の接点を明らかにした。主要成果は定理による関係の解明で、実験上の一律の改善率ではない。 | DeePCの正則化設計、間接同定型MPCと直接法の比較に利用できる。 | 対応関係は仮定したデータ・同定・正則化に依存する。非線形、強雑音、オンライン計算時間を含む総合性能差は別途評価が必要である。 |
| <a id="ref-taming-non-linearity-with-local-data-driven-predictive-control"></a> **［非線形・局所データ］** Select-DPCと比較して読むと局所データの意味が分かる。 | 2026 / **[Taming Non-linearity with Local Data-driven Predictive Control](https://doi.org/10.1016/j.ifacsc.2026.100396)** | 現在の状態付近のデータを使い、局所的な予測とQP求解を繰り返す制御法を提案。CartPoleで、データを増やすとモデルベースMPCに近づく挙動を示した。 | Sequential Data-driven QPという構成で局所モデルと最適化を更新し、倒立振子Swing-upで既存のデータ駆動・モデルベース手法と比較。数値改善率は未確認。 | 非線形DeePC、SQP型データ駆動最適制御。 | 局所線形性とデータ被覆、反復収束、実時間QP計算負荷。 |
| <a id="ref-choose-wisely-data-driven-predictive-control-for-nonlinear-systems-using-online-"></a> **［非線形・局所データ］** 局所的に有用なデータをオンライン選択するSelect-DPC | 2026 / **[Choose Wisely: Data-driven Predictive Control for Nonlinear Systems Using Online Data Selection](https://proceedings.mlr.press/v331/naf26a.html)** | 現在の運転状態に役立つデータだけを毎時刻選び、非線形系を制御する方法を提案。着陸・ロボットアーム・CartPoleの数値例で、比較したDeePCを上回った。 | 現在の運転点に関連する軌道データをオンライン選択するSelect-DPCが、3種類の評価系で両比較法を上回る制御性能を示した。誤差削減率・成功率の具体値は公開要旨に記載されていない。 | 非線形系の局所データ駆動制御、履歴データが大量にある装置へのオンラインMPCに向く。 | 選択データの密度・多様性に性能が左右され、局所データが足りない領域で精度が落ちる。一般の非線形系について広域の安定性・制約保証はなお課題である。 |

**この区分の収録件数：8件。**

<a id="sec-a"></a>
### 11. 微分可能最適化：解感度からDifferentiable MPCへ

**論文の前に補う日本語書籍**：[非線形最適化法](japanese-foundations.html#book-nlp)／[非線形最適制御入門](japanese-foundations.html#book-oc)／[深層学習](japanese-foundations.html#book-dl)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[IJCAI 2022 Differentiable Optimizationチュートリアル](#ref-diffopt-tutorial)で最適化を層として使う導出を学ぶ。制御との統合の全体像は[ML-MPCチュートリアル](#ref-a-tutorial-review-of-machine-learning-based-model-predictive-control-methods)を併読する。

**核となる考え方**：モデルやコストなどのパラメータを変えたとき、最適化問題の解がどう変わるかを微分する。最適性条件の感度を使う方法と反復計算を展開して微分する方法を区別し、MPCの学習へ接続する。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 解の微分の基礎 | argmin／argmaxの微分（2016） | 最適性条件から解の感度を求め、二段階最適化へ利用する。 |
| 制約付き最適化を層にする | OptNet（2017）／Differentiable Convex Optimization Layers（2019） | QPや凸問題をNNの計算の一部に置く。MPC以外も含む共通の道具。 |
| 最適制御・MPCへ | Differentiable MPC（2018）→NMPC感度（2025）／未知系の方策更新（2026） | 局所LQ計算・制約付きNMPCの感度から、実系での性能調整へ広がる。 |

[DPC](#sec-n)は主に予測ロールアウトを通して方策を学習する。最適化の解を微分する本章と、制御方策の直接学習は同じ計算ではない。

**収録文献：6件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-on-differentiating-parameterized-argmin-and-argmax-problems-with-application-to-"></a> **［MPCの解感度］** **［基礎追加］** Gouldら。最適解の感度と二段階最適化の導入となる技術報告。 | 2016 / **[On Differentiating Parameterized Argmin and Argmax Problems with Application to Bi-level Optimization](https://arxiv.org/abs/1607.05447)** | 最適化問題の答えが、問題のパラメータ変更でどう動くかを微分する方法を整理。最適化の解を通して、外側の学習を行うための基礎を示す。 | 制約なし・制約付きargmin/argmaxの微分を整理し、最適性条件から解のパラメータ感度を求める計算を説明。 | OptNet、微分可能MPC、逆最適制御の数理的準備。 | 局所解の正則性などを要する。非滑らかな解の切替えや解の非一意性を無条件に扱えるわけではない。 |
| <a id="ref-optnet-differentiable-optimization-as-a-layer-in-neural-networks"></a> **［汎用の最適化層・解感度］** QPのKKT感度を利用した微分可能な最適化層 | 2017 / **[OptNet: Differentiable Optimization as a Layer in Neural Networks](https://arxiv.org/abs/1703.00443)** | QPをNNの一層として組み込み、その最適解を通して学習できるOptNetを提案。制約や変数間の関係を学べることを、数独などの例で示した。 | QPのKKT条件から厳密な局所解感度を計算する仕組みを実装し、制約を備えた学習可能層として機能することを示した。逆伝播の追加計算はQPの因数分解を再利用でき、著者らは「求解に対しごく小さい追加コスト」と報告している。 | 制約付き行動生成、学習可能なQP制御、逆最適化、制約付き機械学習に利用できる。 | QPの解の一意性、正則性および微分可能な有効制約集合を暗黙に仮定する。非凸NLPでは局所解・特異なKKT行列への対応が必要であり、改善率の一律な数値は示せない。 |
| <a id="ref-differentiable-mpc-for-end-to-end-planning-and-control"></a> **［MPCの解感度］** 時系列最適制御の解感度を学習へ逆伝播 | 2018 / **[Differentiable MPC for End-to-end Planning and Control](https://arxiv.org/abs/1810.13400)** | MPCの最適解を微分して、コストとモデルを教師データから学習。振子・CartPoleでは、比較した通常のNNより少ないデータで制御を学べた。 | Box-DDP固定点のKKT条件から制御系列の感度を求め、コスト重みとダイナミクスを逆伝播で学習した。汎用NNより少ない教師データで学習でき、専門家がモデルクラス外の条件でも通常のシステム同定より優れる事例を示したが、具体的な改善率は要旨から確認できない。 | 逆最適制御、コスト学習、模倣学習、学習可能な非線形MPC層へ応用できる。 | 局所凸近似の固定点が得られることに依存し、非凸問題や有効制約集合の切替えで勾配が不安定になり得る。閉ループ安定性・安全性を自動で保証するものではない。 |
| <a id="ref-differentiable-convex-optimization-layers"></a> **［MPCの解感度］** 凸最適化層を汎用的に構築する枠組み | 2019 / **[Differentiable Convex Optimization Layers](https://papers.neurips.cc/paper_files/paper/2019/hash/9ce3c52fc54362e22053399d3181c638-Abstract.html)** | 凸最適化問題を、学習に組み込める微分可能な層へ変換する仕組みを提案。CVXPYで記述した問題を通して、NNのパラメータを学習できるようにした。 | Disciplined Parametrized Programmingに従う凸問題を自動的に微分可能層へ変換し、Forwardの最適解とBackwardの感度を統合した。複数の学習実験で利用可能性を示しているが、汎用的な精度向上率は課題依存である。 | CVXPYを使った微分可能MPC、最適化を含む計算グラフ、パラメータ同定へ適用できる。 | 凸性とDPPに基づく定式化が必要で、一般の非凸NMPCは直接扱えない。大きなバッチや長ホライズンでは求解と感度計算のメモリが制約になる。 |
| <a id="ref-differentiable-nonlinear-model-predictive-control"></a> **［MPCの解感度］** 制約付きNMPCの解感度とacadosへの実装 | 2025 / **[Differentiable Nonlinear Model Predictive Control](https://arxiv.org/abs/2505.01353)** | 制約付きNMPCの解を微分する計算をacadosに実装。モデルやコストの学習に必要な感度を求め、比較した微分可能ソルバーより高速な計算を示した。 | acadosに前向き・随伴解感度を実装し、原著要旨でmpc.pytorchおよびcvxpygenに対し3倍超の高速化を報告した。これは論文で設定したベンチマークにおける計算時間の比較である。 | 学習可能なNMPC、最適制御方策の勾配学習、モデル・コスト同定に利用できる。 | 最適性条件の正則性が必要であり、有効制約の切替えや局所最適解の非一意性は感度の信頼性を損なう。3倍超という数値は任意の問題・ハードウェアへ一般化できない。 |
| <a id="ref-policy-optimization-for-unknown-systems-using-differentiable-mpc"></a> **［MPCの解感度］** モデル誤差下で微分可能MPCを使って方策最適化 | 2026 / **[Policy Optimization for Unknown Systems using Differentiable MPC](https://proceedings.mlr.press/v331/zuliani26a.html)** | モデルから計算する勾配と、試行から推定する勾配を組み合わせてMPCを調整。モデルが不正確でも学習でき、クアッドコプターの数値例で初期の学習を速めた。 | モデルから得る微分勾配とゼロ次推定を組み合わせ、モデルフリーのみの更新より初期の学習進行が速いことを示した。論文は一定の仮定下の収束解析も提示するが、改善率の数値は要旨では公開していない。 | モデル誤差を含むMPCの評価関数調整、閉ループ性能直接最適化、実機適応に適用可能である。 | ゼロ次勾配推定には試行回数を要し、未知実機への探索入力は安全性上の問題がある。収束保証は対象モデルと雑音に課された条件に依存する。 |

**この区分の収録件数：6件。**

<a id="sec-n"></a>
### 12. Differentiable Predictive Control（DPC）：ロールアウトによる方策学習

**論文の前に補う日本語書籍**：[深層学習](japanese-foundations.html#book-dl)／[PythonとCasADiで学ぶMPC](japanese-foundations.html#book-casadi)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Amos（2023、公開版2025改訂）](#ref-tutorial-on-amortized-optimization)で解を学習する共通原理を学び、[著者のNeuromancerチュートリアル](#ref-dpc-tutorial)でDPCの実装へ進む。前者はDPCを含む解の近似学習に関連する共通教材、後者はDPCの実装教材として読む。

**核となる考え方**：DPC（Differentiable Predictive Control）は、微分可能な予測モデルのロールアウトを通して制御損失を逆伝播し、明示的なNN方策をオフラインで学習する方法。MPCの最適解そのものを微分する[微分可能最適化](#sec-a)とは区別する。

**研究の展開**

| 流れ・分岐 | 収録文献の位置づけ | 何が拡張・接続されたか |
|---|---|---|
| 基本構成 | DPC（2022） | 学習した予測モデルと方策を組み合わせ、制約を考慮した制御損失で方策を訓練。 |
| モデル群への拡張 | Function Encoder DPC（2026） | 系の集合を表現するFE-NODEと組み合わせ、追加訓練なしの適応へ展開。 |
| 作用素モデルとの接続 | PDE-DPC（2025、Neural Operator章） | TI-DeepONetを微分可能な予測器に使い、PDEの方策学習へ広げる。 |

[Explicit MPCの独立節](#sec-explicit)との関係は、オンライン最適化の代替という構成上の接点。DPCがパラメトリックQPの厳密解法から直接導かれた、という研究史は主張しない。

**収録文献**

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足（定量指標など） | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| <a id="ref-differentiable-predictive-control-deep-learning-alternative-to-explicit-model-pr"></a> **［DPC・明示的方策学習］** **［基礎追加］** Drgoňaら。予測ロールアウトを使うDPCの基本論文。 | 2022 / **[Differentiable Predictive Control: Deep Learning Alternative to Explicit Model Predictive Control for Unknown Nonlinear Systems](https://doi.org/10.1016/j.jprocont.2022.06.001)** | 学習した予測モデルで先の動きを計算し、制御則そのものをNNとして事前学習するDPCを提案。非線形系で、制御中の最適化をNNの評価に置き換えた。 | 微分可能なダイナミクスモデルを通して有限ホライズン制御損失を逆伝播し、方策をオフライン最適化する。オンラインでは学習した方策を評価する。 | オンライン最適化を減らす制御、PDE-DPC、Zero-Shot Function Encoder DPCの入口。 | 制約の損失への組込みだけで一般の安全性が保証されるわけではない。モデル誤差、訓練分布、事前学習コストを考慮する必要がある。 |
| <a id="ref-zero-shot-function-encoder-based-differentiable-predictive-control"></a> **［DPC・明示的方策学習］** MPC解の微分と混同しない：DPCは予測ロールアウトで方策学習。 | 2026 / **[Zero-Shot Function Encoder-Based Differentiable Predictive Control](https://proceedings.mlr.press/v331/iqbal26a.html)** | 性質の異なる複数のシステムで、予測モデルと制御方策を事前学習。新しい対象でも再学習せずに適応できることを、複数の非線形系の数値例で示した。 | Function Encoder型Neural ODEとDifferentiable Predictive Controlを組み合わせ、オフライン学習した明示的方策によるゼロショット適応を数値実験で検証。改善率の詳細は未転記。 | 類似装置間の制御方策の転移、オンライン最適化なしの高速制御。 | 事前学習分布外のモデルへの保証、安全性、学習モデル誤差、転移限界が課題。 |

**この区分の収録件数：2件。**

<a id="sec-u"></a>
### 13. MPCと強化学習の統合：価値・モデル・初期解・方策パラメータ

**論文の前に補う日本語書籍**：[強化学習](japanese-foundations.html#book-rl)／[モデル予測制御](japanese-foundations.html#book-mpc)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Reiterほか（2026）：MPCとRLの統合の総説](#ref-synthesis-of-model-predictive-control-and-reinforcement-learning-survey-and-clas)を入口に、必要な計算法は[Amosのチュートリアル](#ref-tutorial-on-amortized-optimization)で補う。

**核となる考え方**：RLをMPCと比較するだけでなく、評価関数・モデル・終端価値・初期軌道を学んで予測制御へ使う方法を扱う。汎用RLのアルゴリズム集は設けず、MPCとの接続が明確な基準文献へ絞る。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| MPC自身を学習する | Gros–Zanon（2020） | MPCをパラメータ付き方策・価値として使い、実系の性能から調整する。 |
| モデルと価値を計画用に学習する | TD-MPC（2022）→TD-MPC2（2024） | 短い潜在状態での計画と、長期の価値予測を組み合わせる。 |
| 既存NMPCを学習で補助する | AC4MPC（2026） | actorを初期化、criticを長期評価に使い、前周期の候補と比較する。 |

TD-MPC系の潜在計画を、物理制約を厳密に満たすNMPCと同じ保証で扱わない。MPCの解を微分する更新は[微分可能最適化](#sec-a)、明示的方策のオフライン学習は[DPC](#sec-n)へ。

**収録文献：4件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-data-driven-economic-nmpc-using-reinforcement-learning"></a> **［MPCを方策として学習］** Gros・Zanon。モデル誤差とコスト調整の関係。 | 2020 / **[Data-Driven Economic NMPC Using Reinforcement Learning](https://doi.org/10.1109/TAC.2019.2913768)**<br>[公開版](https://arxiv.org/abs/1904.04152) | MPCをパラメータ付きの方策・価値関数として使い、実系の性能から調整する枠組みを示す。モデルが完全でなくても、コスト等の適切な表現で方策を補正できる条件を論じた。 | モデルを単に高精度化する学習と、閉ループの性能を良くする学習を区別する基準文献。 | MPCの評価関数調整、経済NMPCとRLの統合。 | 適切なパラメータ表現や最適化に条件がある。誤った任意のモデルでも学習が必ず成功するという意味ではない。 |
| <a id="ref-temporal-difference-learning-for-model-predictive-control"></a> **［モデルベースRL］** **［基礎追加］** Hansenら。TD-MPC2の直接の前身となるTD-MPC。 | 2022 / **[Temporal Difference Learning for Model Predictive Control](https://proceedings.mlr.press/v162/hansen22a.html)** | 短い将来は学習モデルで計画し、その先は学習した価値で評価するTD-MPCを提案。両者を一緒に学び、状態・画像を使う連続制御でデータ効率を改善した。 | タスク指向の潜在ダイナミクスと終端価値をTD学習し、潜在空間の短ホライズン軌道最適化で入力を決める。 | TD-MPC2、World Model型MPC、学習終端コストの理解。 | 潜在モデルの誤差と価値外挿が計画に影響する。形式的な制約・安定性保証は別途必要。 |
| <a id="ref-td-mpc2-scalable-robust-world-models-for-continuous-control"></a> **［モデルベースRL］** 潜在モデルと価値予測による計画の代表例。 | 2024 / **[TD-MPC2: Scalable, Robust World Models for Continuous Control](https://arxiv.org/abs/2310.16828)** | TD-MPCを改良し、多様な連続制御課題を同じハイパーパラメータで学習。複数タスクを一つのモデルで扱い、モデルとデータを増やしたときの性能向上も示した。 | 潜在状態の短期予測と価値推定を使うサンプリング計画を統合し、大規模・多タスクで高性能なモデルベースRLを実証。代表的な点数の正確な引用は未再確認。 | ロボットのWorld Model型MPC、データ効率の高い連続制御。 | 学習データの範囲、計画の安全保証、実機分布シフト、訓練コストが課題。 |
| <a id="ref-ac4mpc-actor-critic-reinforcement-learning-for-guiding-model-predictive-control"></a> **［actor・criticでNMPCを補助］** Reiterほか。初期解と終端評価を学習から得る。 | 2026〔オンライン2025・初稿2024〕 / **[AC4MPC: Actor-Critic Reinforcement Learning for Guiding Model Predictive Control](https://doi.org/10.1109/TCST.2025.3620521)**<br>[公開刊行版](https://research.tudelft.nl/files/264646528/AC4MPC_Actor-Critic_Reinforcement_Learning_for_Guiding_Model_Predictive_Control.pdf)・[著者コード](https://github.com/lmcggg/AC4MPC) | actorの軌道でNMPCを初期化し、criticで長期性能を評価する。前周期の解をずらした候補も併用し、学習方策に対する性能関係を解析して追越し例などで検証した。 | 学習した方策を置き換えるだけでなく、初期化・終端価値・候補評価に役割を分けて使う。TCST 34(1):395–410。 | 学習方策を活用する短ホライズンNMPC、非凸問題の初期化。 | 性能境界にはcritic誤差等が入る。大域最適なNMPC解を前提にしない結果と、無条件の安全・安定保証を混同しない。 |

**この区分の収録件数：4件。**

<a id="sec-f"></a>
### 14. 安全フィルタ法：CBF・予測フィルタ・HJ到達可能性

**論文の前に補う日本語書籍**：[システム制御のための安定論](japanese-foundations.html#book-stability)／[非線形最適化法](japanese-foundations.html#book-nlp)／[非線形最適制御入門](japanese-foundations.html#book-oc)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[CBFの実用課題のチュートリアル](#ref-advances-in-the-theory-of-control-barrier-functions-addressing-practical-challen)と[学習とCBFのチュートリアル](#ref-learning-for-feasible-and-safe-control-with-control-barrier-functions-a-tutorial)を使う。共通の保証関数は[Dawsonほか（2023）](#ref-safe-control-with-learned-certificates-a-survey-of-neural-lyapunov-barrier-and-c)で補う。

**核となる考え方**：基準となるMPCや学習方策が出した入力を必要に応じて修正し、安全集合を保つ。CBFの一段先の条件、予測・バックアップを使う安全フィルタ、HJ到達可能性による安全領域を同じ章の枝として比較する。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 安全領域の計算 | HJ到達可能性（2005） | 外乱・対抗入力を含む到達可能性から、危険を避けられる状態を求める。 |
| 入力を制約で修正 | CBF-QP（2017） | 安全集合の不変条件を入力への制約として課す。 |
| 予測・学習との接続 | 予測安全フィルタ（2018）→統一的な整理（2025）、DeePC・latent CBFとの接続 | MPC型の先読み、学習した制約・モデル、バックアップの仮定を確認する。 |

安全フィルタを通すことで、元のMPCの最適性や追従性能がそのまま残るとは限らない。安全性の保証条件と、基準制御器との相互作用を分けて読む。

**収録文献：6件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-a-time-dependent-hamilton-jacobi-formulation-of-reachable-sets-for-continuous-dy"></a> **［HJ・到達可能性］** **［基礎追加］** Mitchell・Bayen・Tomlin。HJ到達可能性に基づく安全解析の基礎。 | 2005 / **[A Time-Dependent Hamilton–Jacobi Formulation of Reachable Sets for Continuous Dynamic Games](https://www.cs.ubc.ca/~mitchell/Papers/publishedIEEEtac05.pdf)** | 最悪の外乱が加わると危険状態へ到達し得る範囲を、HJI方程式で計算する方法を提示。車両の衝突回避例で計算結果を検証した。 | 後方到達可能集合を時間依存HJI方程式の粘性解の零劣位集合として表現し、数値解法と衝突回避例を示す。 | Reachability型安全フィルタ、Deep QP Safety Filter、CBFとの比較。 | 格子に基づく計算は次元の増加に厳しい。数値離散化誤差とモデル妥当性も評価が必要。 |
| <a id="ref-control-barrier-function-based-quadratic-programs-for-safety-critical-systems"></a> **［CBF］** CBFの安全条件をQP制約へ組み込む | 2017 / **[Control Barrier Function Based Quadratic Programs for Safety Critical Systems](https://doi.org/10.1109/TAC.2016.2638961)** | 目標へ向かう条件と、安全集合から出ない条件を一つのQPに組み込む制御法を提示。条件の下で安全を保ちながら、制御目的を実現する仕組みを示した。 | CBFをQP制約に組み込むことで、適切な可行性・正則性条件下で安全集合の前方不変性を保証できる枠組みを与えた。制御誤差や実行速度の改善割合は主要成果ではない。 | 衝突回避、安全フィルタ、入力制限を持つ安全重要制御へ利用できる。 | CBF条件の可行性が入力飽和や複数障害物で失われる可能性がある。モデル誤差、測定誤差、高次相対次数への対応が必要である。 |
| <a id="ref-a-predictive-safety-filter-for-learning-based-control-of-constrained-nonlinear-d"></a> **［予測安全フィルタ］** 学習器の入力を予測軌道による安全制約で修正 | 2021 / **[A Predictive Safety Filter for Learning-Based Control of Constrained Nonlinear Dynamical Systems](https://doi.org/10.1016/j.automatica.2021.109597)** | 学習方策の入力で安全に動き続けられるかを、将来予測で確認するフィルタを提案。危険な入力だけを修正し、安全な計画を維持する方法を示した。 | 有限ホライズンの安全計画で、元の学習方策からの入力偏差を小さくしながら安全制約を維持する仕組みを示した。実験の成功率や改善倍率は公開要旨に含まれていない。 | 学習済みRL方策に後付けする安全層、モデル予測型の安全入力修正に適用できる。 | 安全なバックアップ集合・終端条件とモデル妥当性を必要とする。計算時間や未知外乱が大きい場合、予測フィルタ自体の可行性が課題になる。 |
| <a id="ref-the-safety-filter-a-unified-view-of-safety-critical-control-in-autonomous-system"></a> **［安全フィルタ総説］** CBF/HJ/MPC安全フィルタを比較・整理 | 2024 / **[The Safety Filter: A Unified View of Safety-Critical Control in Autonomous Systems](https://doi.org/10.1146/annurev-control-071723-102940)** <br>[公開版（arXiv）](https://arxiv.org/abs/2309.05837) | CBF・到達可能性解析・予測型の安全フィルタを、入力を監視・修正する共通の枠組みで整理。各方式の仕組み、必要な仮定、保証の違いを比較できる。 | 安全集合、入力修正、到達可能性、予測時間という観点で異なる安全フィルタの前提と保証を整理した。比較実験に基づく一つの改善率を報告する論文ではない。 | 制御対象の危険状態・モデル精度・計算予算に応じたSafety Filter設計に使える。 | 統一的な概念整理にとどまり、安全が成立するためのモデル妥当性・集合の非空性・リアルタイム性は個別方式で検証が必要である。 |
| <a id="ref-two-component-controller-design-to-safeguard-data-driven-predictive-control"></a> **［保護フィードバック］** 保証がない学習予測器を制御に導入する実践的な例。 | 2025 / **[Two-Component Controller Design to Safeguard Data-Driven Predictive Control](https://arxiv.org/abs/2505.19131)** | データ駆動の予測制御に、モデル不要の保護フィードバックを追加。データが少なく予測が不確かでも、出力制約を守る制御を支える構成を示した。 | 学習モデルが不十分なときに安全側フィードバックで介入する構成を提案し、設定値移行の数値例で出力拘束の扱いを実演。数値改善率は未確認。 | モデル学習の初期段階の安全化、DeePCと既知フィードバックの併用。 | 相対次数等の仮定、高ゲイン制御の騒音増幅、モデルベース予測器との切替え。 |
| <a id="ref-how-to-train-your-latent-control-barrier-function-smooth-safety-filtering-under-"></a> **［CBF］** 潜在状態上で滑らかなCBF安全フィルタを学習 | 2026 / **[How to Train Your Latent Control Barrier Function: Smooth Safety Filtering Under Hard-to-Model Constraints](https://proceedings.mlr.press/v331/nakamura26a.html)** | 画像などから学ぶ潜在空間で、入力を滑らかに修正できる安全フィルタを提案。ロボット操作の実験で、方策を切り替える比較法より成功率を改善した。 | 滑らかな潜在CBFを学習することで、従来のスイッチング型Safety Filterと比べて、対象実験の成功率を約2倍にしたと報告している。 | 画像観測からのロボット操作、安全な潜在状態フィルタ、スムーズな制御介入に利用できる。 | 成功率2倍は論文中の比較課題に依存し、画像の分布変化や潜在状態の不完全性の下で一般的な安全性が保証されるわけではない。 |

**この区分の収録件数：6件。**

<a id="sec-b"></a>
### 15. Neural Lyapunov法：保証関数の学習・検証と制御への接続

**論文の前に補う日本語書籍**：[システム制御のための安定論](japanese-foundations.html#book-stability)／[深層学習](japanese-foundations.html#book-dl)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Dawsonほか（2023、コード付き）](#ref-safe-control-with-learned-certificates-a-survey-of-neural-lyapunov-barrier-and-c)で、学習・検証・制御の関係をつかむ。MPCへの接続は、終端コスト・終端集合などに利用できる条件を別途確認する。

**核となる考え方**：制御則と、安定性を示すLyapunov関数をNNで構成し、学習と条件の検証を結ぶ。MPCに利用する場合は、終端関数・局所補助制御器などの要件と学習した証明条件が一致するかを確認する。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 学習と反例探索 | Neural Lyapunov Control（2019） | 関数と制御則の学習、検証、反例を戻す更新を組み合わせる。 |
| 観測の拡張 | 状態・出力フィードバック（2024） | 状態をすべて観測する設定から、観測器を含む構成へ。 |
| 検証を訓練に組み込む | Certified Training with Branch-and-Bound（2026） | 検証の計算を学習へ統合し、条件を満たすモデルの探索を改善する。 |

新しい検証法を使うことと、実プラントのモデル誤差・観測誤差まで保証することは別。MPC全体の安定性へ接続する際は[共通理論](#sec-k)の条件も参照する。

**収録文献：3件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-neural-lyapunov-control"></a> **［SMT検証］** 制御器とLyapunov関数の同時学習・反例探索 | 2019 / **[Neural Lyapunov Control](https://papers.neurips.cc/paper/8587-neural-lyapunov-control)** | NNの制御則とLyapunov関数を一緒に学び、安定条件を破る状態を探索して修正。検証した領域内で、安定性を保証できる制御則を作る方法を示した。 | ニューラル制御則とLyapunov候補を同時に更新し、反例が見つかる領域を修正して検証可能な安定領域を構築した。小規模な非線形系で方法を示したが、認証領域の倍率は今回の確認範囲では取得できなかった。 | ニューラル制御の安定領域推定、強化学習方策の証明付き設計に利用できる。 | 形式検証の計算負担が状態次元とNNサイズで増す。数値探索で反例が見つからないだけでは安定性証明にならず、モデル誤差も別途扱う必要がある。 |
| <a id="ref-lyapunov-stable-neural-control-for-state-and-output-feedback-a-novel-formulation"></a> **［NN制御・安定性検証］** 状態・出力フィードバックの保証付き学習 | 2024 / **[Lyapunov-stable Neural Control for State and Output Feedback: A Novel Formulation](https://proceedings.mlr.press/v235/yang24f.html)** | NN制御器と観測器を学び、状態を直接使えない場合も含めて安定性を検証。GPUを使う検証法と新しい条件式で、保証できる領域を広げた。 | 従来より大きな認証可能安定領域を持つ定式化を提案し、GPU上でBranch-and-Boundによる事後検証を実行した。著者らは出力フィードバックのNN制御器・観測器の安定性を形式的に示したが、要旨中に領域拡大倍率はない。 | センサだけから制御するNN出力フィードバック系、観測器と制御器の一体学習に応用できる。 | 認証は指定した領域・近似モデルに限定され、GPU利用でも高次元系の大域的な認証は難しい。定量的な改善倍率は本表では確認できていない。 |
| <a id="ref-certified-training-with-branch-and-bound-for-lyapunov-stable-neural-control"></a> **［NN制御・安定性検証］** 検証境界を損失関数に組み込む認証型学習 | 2026 / **[Certified Training with Branch-and-Bound for Lyapunov-stable Neural Control](https://proceedings.mlr.press/v331/shi26a.html)** | 安定性の検証に使う計算を学習中にも取り入れ、検証しやすいNN制御器を作る方法を提案。比較法より広い安定領域と、速い検証を示した。 | 最大規模の実験として挙げた2Dクアッドローター出力フィードバック系で、従来比11倍超の検証高速化と164倍の認証可能ROA（引込み領域）を報告した。 | 形式検証を含むNN制御器の訓練、広い安定領域を必要とする制御設計に応用できる。 | 11倍超・164倍は論文中の特定の2D系での比較であり、他の次元・ネットワークでも達成されるとは限らない。分枝分割の組合せ的な計算負担は残る。 |

**この区分の収録件数：3件。**

<a id="sec-c"></a>
### 16. Contraction・CCM：軌道間の収束から追従・ロバスト化へ

**論文の前に補う日本語書籍**：[システム制御のための安定論](japanese-foundations.html#book-stability)／[LMIによるシステム制御](japanese-foundations.html#book-lmi)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：章内の[収縮理論チュートリアル（2021）](#ref-contraction-theory-for-nonlinear-stability-analysis-and-learning-based-control-a)を最初に読む。CCMは、制御によって状態差を縮めることを示す状態依存の計量。

**核となる考え方**：Contractionは、異なる軌道間の状態差が縮むことを微小変位の力学から評価する。CCMは制御によってその収縮を実現する計量で、予測した名目軌道への追従やTube MPCの誤差上界とつながる。

**研究の展開**

| 流れ・分岐 | 文献・段階 | 何が広がったか |
|---|---|---|
| 収縮解析の基礎 | Lohmiller–Slotine（1998） | 平衡点だけでなく軌道間の収束を、状態依存の距離で調べる。 |
| 制御の設計へ | CCM（2017） | 計量の条件から参照軌道へのフィードバックを構成する。 |
| 学習と理論の整理 | C3M（2021）／チュートリアル総説（2021） | 計量・追従則を学ぶ枝と、頑健性や学習への接続を体系化する教材を併読する。 |

[非線形Tube MPC](#sec-k)や[RMPPI](#sec-p)の追従・誤差評価と関連するが、CCMそのものが任意の制約付き最適制御問題を解くわけではない。

**収録文献：4件**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 関連する制御・応用 | 条件・限界 |
|---|---|---|---|---|---|
| <a id="ref-on-contraction-analysis-for-non-linear-systems"></a> **［収縮解析］** **［基礎追加］** Lohmiller・Slotine。CCMより前に読む収縮解析の基礎。 | 1998 / **[On Contraction Analysis for Non-linear Systems](https://doi.org/10.1016/S0005-1098(98)00019-3)** | 非線形系の軌道どうしが近づく性質を、微小な状態差の動きから調べる収縮解析を体系化。平衡点一つに限らず、軌道間の収束を示す条件を与えた。 | 変分系と状態・時間依存の計量を使い、軌道間の指数収縮を評価する条件を示す。平衡点周りのLyapunov解析との関係を理解できる。 | CCM、非線形追従制御、収縮に基づく推定・学習モデル解析。 | これはまず解析の枠組みであり、制御器合成にはCCM等の追加構成が必要。計量の探索も別問題。 |
| <a id="ref-control-contraction-metrics-convex-and-intrinsic-criteria-for-nonlinear-feedback"></a> **［CCM・制御器合成］** 非線形系の微分収縮性に基づくフィードバック合成 | 2017 / **[Control Contraction Metrics: Convex and Intrinsic Criteria for Nonlinear Feedback Design](https://arxiv.org/abs/1503.03144)** | 非線形系を任意の実現可能な参照軌道へ追従させるためのCCM条件を導出。状態差の収縮を示す計量から、制御則を構成する方法を示した。 | 状態依存の正定値Metricが満たす凸条件から、参照軌道間の指数収縮を保証する制御則を導出した。実験の改善率ではなく、収縮率を示す指数境界が主要成果である。 | 非線形追従制御、ロバスト制御、軌道依存安定性の証明に使える。 | Metricの探索が難しく、高次元系では条件評価が重い。原論文の定理の一部には後続の技術的条件の補足があるため訂正論文との併読が必要である。 |
| <a id="ref-contraction-theory-for-nonlinear-stability-analysis-and-learning-based-control-a"></a> **［収縮解析］** 収縮理論の安定性・ロバスト性・学習制御への総説 | 2021 / **[Contraction Theory for Nonlinear Stability Analysis and Learning-based Control: A Tutorial Overview](https://doi.org/10.1016/j.arcontrol.2021.10.001)** <br>[公開版（arXiv）](https://arxiv.org/abs/2110.00675) | 収縮理論による非線形系の安定性・追従・外乱への頑健性を整理した総説。学習したモデルや制御則に、どのように保証を付けるかまで解説する。 | 収縮率、入力外乱に対する誤差上界、制御・推定の設計条件を統一的に説明している。新規ベンチマークや性能改善率を報告する実験論文ではない。 | CCM研究の理論整理、神経収縮計量やロバスト追従の先行研究探索に有用である。 | 総説自体は新しい制御器の実機性能を検証していない。扱う理論保証は正定値計量などの存在・検証可能性に依存する。 |
| <a id="ref-learning-certified-control-using-contraction-metric"></a> **［CCM・制御器合成］** Contraction Metricと制御方策の同時学習（C3M） | 2021 / **[Learning Certified Control Using Contraction Metric](https://proceedings.mlr.press/v155/sun21b.html)** | 参照軌道へ追従する制御則とCCMをNNで同時に学習。外乱下の追従誤差を評価し、ロボットの数値例で比較法より小さい誤差と速い実行を示した。 | 制御器と収縮MetricをNNで同時学習し、対象実験ではSOS・RL・MPC系比較手法より小さい追従誤差と速い実行を報告した。改善倍率は要旨で明記されていない。 | 非線形軌道追従、ロボットの有界外乱下での追従誤差保証に適用できる。 | 保証は制御アフィン系などのモデル仮定とMetric条件の検証に依存する。高次元での全領域検証、モデルバイアスへの耐性はなお課題である。 |

**この区分の収録件数：4件。**

<a id="sec-g"></a>
### 17. System Level Synthesis（SLS）：閉ループ応答からロバストMPCへ

**論文の前に補う日本語書籍**：[LMIによるシステム制御](japanese-foundations.html#book-lmi)／[マルチエージェントシステムの制御](japanese-foundations.html#book-mas)／[モデル予測制御](japanese-foundations.html#book-mpc)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：章内の[SLS総説（2019）](#ref-system-level-synthesis)で、応答表現と制約の扱いを学ぶ。

**核となる考え方**：SLS（System Level Synthesis）は、制御器の係数ではなく、外乱から状態・入力への閉ループ応答を設計変数として制御則を構成する方法。応答上に局所性・分散性・ロバスト性の条件を課せる。

**研究の展開**

| 流れ・分岐 | 収録文献の位置づけ | 何が拡張・接続されたか |
|---|---|---|
| 基本枠組みと体系化 | System Level Approach（2016年プレプリント）、System Level Synthesis（2019） | 応答パラメータ化とアフィンな整合条件を基礎に、局所・情報制約を設計へ組み込む。 |
| ロバストMPCへの展開 | Polytopic Robust MPC（2024） | 有限ホライズンの応答で、モデル不確かさと拘束を扱う。 |
| 数値計算の展開 | Riccati Recursions（2024） | SLS型最適化の構造を利用した計算へ展開。 |
| 統計的な誤差モデルとの接続 | Conformalized SLS（2026） | データによる誤差集合の較正を応答・到達集合の設計に組み合わせる。 |

DGKF（補助節X）とはロバスト制御における接点があるが、この節は閉ループ応答のパラメータ化という手法の流れに沿って構成する。

**収録文献**

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足（定量指標など） | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| <a id="ref-a-system-level-approach-to-controller-synthesis"></a> **［SLS・応答設計］** 制御器ではなく閉ループ応答を直接設計する枠組み | 2016 / **[A System Level Approach to Controller Synthesis](https://arxiv.org/abs/1610.04815)** | 制御器の係数ではなく、外乱に対する状態・入力の応答を直接設計するSLSを提案。局所性や情報の制約を含む制御設計を、凸最適化で扱う道筋を示した。 | 外乱から状態・入力への応答をアフィン等式で表し、内部安定性を満たす制御器を応答上の凸制約で扱えることを示した。主要成果は数値的な改善率ではなくパラメータ化の等価性である。 | 分散制御、通信制約付き制御、大規模ロバスト設計に適用できる。 | 理論の基本形は線形系を前提とする。応答の有限ホライズン近似や複雑な通信制約では問題サイズと実装負荷が増大する。 |
| <a id="ref-system-level-synthesis"></a> **［SLS・応答設計］** SLSの安定化、局所性、分散制御、ロバスト性を体系化 | 2019 / **[System Level Synthesis](https://arxiv.org/abs/1904.01634)** | SLSの理論と設計例をまとめ、分散制御とロバスト制御を共通の枠組みで整理。近傍だけで動く制御器や、モデル誤差に耐える制御器の設計を解説する。 | 閉ループ応答の自由度を直接制約することで、局所性・情報制約を表現する統一的設計枠組みをまとめた。総説として数値的な性能向上を新たに一律で実証したわけではない。 | ネットワーク化制御、車両隊列、分散MPC、ロバスト設計の学習基盤となる。 | 応答の近似次数と系統規模が計算負荷に影響し、強非線形・ハイブリッド力学には直接適用できない。 |
| <a id="ref-fast-system-level-synthesis-robust-model-predictive-control-using-riccati-recurs"></a> **［専用数値解法］** Riccati再帰によりSLS-MPCのQP構造を高速処理 | 2024 / **[Fast System Level Synthesis: Robust Model Predictive Control using Riccati Recursions](https://doi.org/10.1016/j.ifacol.2024.09.027)** | SLS型ロバストMPCの構造を利用し、Riccati再帰で計算する方法を提案。名目軌道と誤差補正を同時に設計する際の、求解負荷を軽くした。 | 問題構造を利用した再帰計算で、汎用的な密行列QPより効率的にロバストMPCを解く方法を提示した。論文の具体的な計算時間比は今回の確認範囲で未検証。 | 高周期ロバストMPC、組込み向けSLSソルバーの設計に利用できる。 | 高速化はブロック構造などの特定の定式化に依存し、一般の非凸NLPにそのまま拡張できるわけではない。 |
| <a id="ref-robust-model-predictive-control-with-polytopic-model-uncertainty-through-system-"></a> **［ロバストMPC］** 多面体モデル不確かさを含むSLSロバストMPC | 2024 / **[Robust Model Predictive Control with Polytopic Model Uncertainty through System Level Synthesis](https://doi.org/10.1016/j.automatica.2023.111431)** | モデル行列の不確かさと外乱を持つ系に対し、SLSでロバストMPCを設計。不確かな状態変化を応答で表し、制約を守るための余裕を計算する方法を示した。 | 閉ループ外乱応答を用いた制約の引き締めによって、不確かなモデルに対して制約充足を設計できることを示した。定量的な比較倍率は公開情報から検証できなかった。 | モデル誤差が集合で評価できる多変数・分散系のロバストMPCに利用できる。 | 不確かさ集合を正しく指定する必要があり、ポリトープ頂点や予測ホライズンが増えると計算負荷と保守性が増す。 |
| <a id="ref-safety-beyond-the-training-data-robust-out-of-distribution-mpc-via-conformalized"></a> **［統計的誤差集合］** Conformal Predictionによる誤差境界とSLSの融合 | 2026 / **[Safety Beyond the Training Data: Robust Out-of-Distribution MPC via Conformalized System Level Synthesis](https://proceedings.mlr.press/v331/srinivasan26a.html)** | 学習時と異なる条件でのモデル誤差をデータから見積もり、SLS-MPCの安全余裕へ反映。車両とクアッドコプターの数値例で、安全性と信頼性を改善した。 | 重み付きConformal Predictionからモデル誤差集合を学び、SLSによる到達集合と制約引き締めへ反映した。対象の4D・12D非線形系では、固定誤差境界・ロバスト化なしの比較法より安全性・信頼性が改善したが、要旨に改善率はない。 | 学習済み動力学を使うロバストNMPC、データ密度に応じた不確実性境界の設計に利用できる。 | 被覆保証は較正データ、分布シフト仮定、誤差モデルの妥当性に依存する。いかなる未知環境でも無条件に安全となるわけではない。 |

**この区分の収録件数：5件。**

<a id="sec-i"></a>
### 18. Neural OperatorとPDE制御：予測モデルから方策表現へ

**論文の前に補う日本語書籍**：[偏微分方程式の計算数理](japanese-foundations.html#book-pde)／[深層学習](japanese-foundations.html#book-dl)。補う範囲と先に読む箇所はリンク先に記す。

**入口（総説・チュートリアル）**：[Neural Operator実践入門（2026）](#ref-from-theory-to-application-a-practical-introduction-to-neural-operators-in-scien)でFNO／DeepONetを学び、[統計理論の総説](#ref-operator-learning-a-statistical-perspective)で予測誤差の考え方を補う。PDEモデルを使う制御と、方策自体を作用素にする枝を区別する。

**核となる考え方**：Neural Operatorは、PDEの解作用素など、関数から関数への写像をNNで学習する方法。FNO・DeepONetを基盤に、物理残差の利用、DPCとの結合、制御方策自体の作用素表現へ広がる。

**研究の展開**

| 流れ・分岐 | 収録文献の位置づけ | 何が拡張・接続されたか |
|---|---|---|
| 予測作用素の構成 | FNO／DeepONet（2021） | Fourier空間の層、Branch/Trunk構造など、作用素を学習する並行するモデル構造。 |
| 物理法則との統合 | PINO（2021年プレプリント） | 予測作用素の学習へPDE残差を組み合わせる枝。 |
| 予測器と方策学習の接続 | TI-DeepONet＋PDE-DPC（2025） | 学習した時間発展モデルを通してDPCの方策を訓練する。 |
| 方策自体の作用素化 | CINOC（2026） | 状態場から制御関数への写像を作用素方策として学習し、センサ・アクチュエータ構成の変化へ展開。 |

PINO→PDE-DPC→CINOCを同一アーキテクチャの改良列とはしない。作用素学習の共通原理から、物理制約・予測器・方策表現へ分岐する。

**収録文献**

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足（定量指標など） | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| <a id="ref-fourier-neural-operator-for-parametric-partial-differential-equations"></a> **［予測作用素］** Fourier空間でPDEの作用素を学習 | 2021 / **[Fourier Neural Operator for Parametric Partial Differential Equations](https://arxiv.org/abs/2010.08895)** | 初期条件や係数などの分布からPDEの解を直接予測するFNOを提案。流体などの数値例で、高速な予測と学習時より高い解像度への適用を示した。 | Fourier Neural Operatorは固定解像度の比較で既存の学習法より高精度と報告され、乱流予測のゼロショット高解像度化を実証した。特定のベンチマークでは従来PDEソルバーより最大約10^3倍高速だった。 | 流体・熱・拡散PDEの高速代理モデル、シミュレーション高速化、分布定数系のMPCに適用できる。 | 最大10^3倍は特定実装と解像度での推論比較であり、学習時間は含まない。分布外パラメータ、長時間積分、保存則や誤差保証は別途検討が必要である。 |
| <a id="ref-learning-nonlinear-operators-via-deeponet-based-on-the-universal-approximation-t"></a> **［予測作用素］** 入力関数から出力関数の非線形作用素を学習 | 2021 / **[Learning Nonlinear Operators via DeepONet Based on the Universal Approximation Theorem of Operators](https://doi.org/10.1038/s42256-021-00302-5)** | 入力された関数と、解を求めたい位置を別々のNNで処理するDeepONetを提案。条件の異なる微分方程式の解を、一つの学習器で予測する方法を示した。 | DeepONetが関数空間間の非線形写像を学習できることを複数のPDE・作用素ベンチマークで示した。原著の多様な課題を横断する単一の改善率はない。 | 熱・流体・構造解析の代理モデル、逆問題、PDE制御器の学習に応用できる。 | 測定点配置と訓練用関数の分布に強く依存する。高周波現象、未観測条件、長期安定性は別途保証が必要である。 |
| <a id="ref-physics-informed-neural-operator-for-learning-partial-differential-equations"></a> **［物理残差を使う予測作用素］** 物理法則の残差をNeural Operator学習に統合 | 2021 / **[Physics-Informed Neural Operator for Learning Partial Differential Equations](https://arxiv.org/abs/2111.03794)** | PDEの解を学ぶNNに、データだけでなく支配方程式の条件も与えるPINOを提案。粗い学習データからでも、細かな解像度で精度を保つ予測を示した。 | Physics-Informed Neural Operatorは、粗い教師データだけで学ぶFNOに物理制約を加え、訓練解像度より細かなゼロショット予測でも精度の劣化を抑えた。Kolmogorov流など、通常のPINNで最適化が難しい設定への適用も示した。 | 物理制約付き代理モデル、限られた測定データを用いるPDE同定と予測制御に使える。 | 物理残差を高解像度で評価する学習コストが高く、PDE自体が未知・誤差を持つ場合は制約が逆効果になり得る。制御系の安定性は直接示していない。 |
| <a id="ref-learning-to-control-pdes-with-differentiable-predictive-control-and-time-integra"></a> **［予測器を通したDPC方策学習］** PDEモデルの微分可能予測に基づいて方策学習 | 2025 / **[Learning to Control PDEs with Differentiable Predictive Control and Time-Integrated Neural Operators](https://arxiv.org/abs/2511.08992)** | PDEの動きを近似するNNモデルで、制御方策そのものを事前学習。熱伝導などの数値例で、制御中の最適化を省き、入力生成を大幅に速めた。 | TI-DeepONetを通じたDifferentiable Predictive Controlでオフラインに方策を学習し、推論時の制御入力生成をNMPCベンチマークより約10^4〜10^5倍高速化したと著者らは報告した。制御対象と比較実装を固定した実験値である。 | 分布定数系の高速な明示的NN制御、熱・拡散・流体系でのオンライン最適化の代替に利用できる。 | オンライン速度は極めて速いがオフライン学習費用を含まず、予測器のモデル誤差と安全制約の一般的保証は残る。数値倍率は論文の3種のPDE設定に依存する。 |
| <a id="ref-cinoc-cardinality-invariant-neural-operator-policies-for-scalable-pde-control"></a> **［作用素方策］** センサ・アクチュエータ数に依らない作用素方策 | 2026 / **[CINOC: Cardinality-Invariant Neural Operator Policies for Scalable PDE Control](https://proceedings.mlr.press/v306/zanotta26a.html)** | 状態の分布から制御の分布を出すNN方策を学び、センサやロボットの数が変わっても使う方法を提案。小さな群での学習を、大きな群へ再学習なしで転用した。 | Neural Operatorで状態場から制御場への方策を学習し、小規模群で訓練した同じパラメータを大規模群へ再学習なしで転移させた。部分的なエージェント故障に耐性がある例も示したが、群規模の具体倍率は公開要旨にない。 | センサ・アクチュエータの配置が変化するPDE制御、マルチエージェント協調制御に適用できる。 | ゼロショット転移は論文で扱った作用素・環境・群規模での実証であり、任意の密度や物理相互作用への適用を保証しない。訓練時の微分可能PDEソルバーも必要である。 |

**この区分の収録件数：5件。**

<a id="sec-game"></a>
### 19. ゲーム理論に基づく制御：min–max・Nash／Stackelberg・Pareto・Potential Games

**論文の前に補う日本語書籍**：[ゲーム理論](japanese-foundations.html#book-game)／[最適化アルゴリズム〔12章：多目的〕](japanese-foundations.html#book-multiopt)／[しっかり学ぶ数理最適化](japanese-foundations.html#book-opt)。min–max・分散制御の基礎と、枝ごとの選び方は[書籍ページの章別対応](japanese-foundations.html#chapter-map)を参照する。

**入口（総説・チュートリアル）**：[Hallほか（2026）](#ref-receding-horizon-control-and-dissipativity-optimal-control-games-and-uncertainty)でMPCとの接続、[de Zeeuw（2024）](#ref-a-crash-course-in-differential-games-and-applications)で解概念を学ぶ。枝ごとの総説・チュートリアルと読む順も章内に示す。

**核となる考え方**：制御入力を決める主体が複数あり、一人の最適な行動が相手の選択にも依存する問題を扱う。相手は実在の制御器・ロボットの場合もあれば、最悪外乱や最悪分布を選ぶ仮想的な相手の場合もある。

**この章の選定方針**：前版で補強したmin–max・Nash／Stackelberg・Pareto・Potential Gamesなどの33件を維持する。複数主体の有限ホライズン計画、ロバスト最適制御、単一OCPへの帰着を中心に読む。協調・混合整数・多数主体への分岐も残し、各均衡概念と情報構造の違いを確認する。

**まず読む入口**：MPCの知識から入るなら **Receding Horizon Control and Dissipativity（2026、公開プレプリント）**、Nash・Stackelbergと情報構造を整理するなら **A Crash Course in Differential Games and Applications（2024）**、多数主体なら **Machine Learning Methods for Large Population Games（2024）** を使う。いずれも[チュートリアル節](#sec-t)に収録した。**Game Theory and Control（2018）** は、零和・team・分散制御の基本構造を補う総説として残す。Paretoの全体像には **Peitz–Dellnitz（2018、サーベイ節）**、MPCから入る原著には **Eichfelderほか（2023）** を使う。後者は総説ではないが、定義と閉ループ解析を追える入口として読む。方法の枝分けと以下の読む順は、ChatGPTによる学習用の整理である。

**用語の補足**：Nash均衡は、相手の戦略を固定したときに誰も一方的な変更で得をしない状態。零和では両者の損得が正反対になり、min–maxの鞍点を考える。Stackelbergは先手が後手の反応を考慮する構造。Generalized Nash Equilibrium（GNE）は、衝突回避などで各主体の実行可能集合が相手の選択にも依存する均衡。Pareto最適は、誰かの評価を悪化させずには他の評価も改善できない効率的な解を指す。Nash均衡は一方的変更への応答、Pareto最適は入力・戦略の組をまとめて変えたときの効率を問うため、同じ条件ではない。どの主体が何を観測し、何を戦略として選ぶかも問題設定の一部になる。

**研究の展開**

| 流れ・分岐 | 収録文献の位置づけ | 何が拡張・接続されたか |
|---|---|---|
| 零和・min–maxと確率制御との接続 | Jacobson（1973）、RARLの安定性（2020）、モデルフリーな頑健方策最適化（2021）、Yang（2021）、Wasserstein罰則・minimax regret（2023）、零和LQの学習（2025）、未知系の確率的H∞（2026） | 外乱入力を選ぶ相手と、分布を選ぶ相手を区別する。最悪コスト・外乱抑制・理想制御器からの最悪性能差は異なる評価基準であり、枝ごとの保証を確認する。 |
| 非零和・Nash／Stackelbergとfeedback計算 | Starr–Ho（1969）、iLQGames（2020）、ALGAMES（2022）、近似GFNE（2023）、feedback Stackelberg（2024）、KLGame（2024）、分散NPG（2025）、SLS（2026） | 近年の原著から、共有制約、状態に応答する均衡、先手・後手、学習方策の取り込み、局所情報での学習、部分観測へ広がる。これらは並行する枝で、一本道の置換列ではない。 |
| 動的Potential Games・単一OCPへの帰着と分散化 | Potential iLQR（2021）／Distributed Potential iLQR（2023）／重み付き・制約付きPotential Games（2025） | 単一の最適制御問題への帰着を基礎に、近傍ごとの分散計算と、非対称な相互作用・共有制約への拡張へ分かれる。 |
| 混合整数Potential Games・離散判断を含む制御 | Fabiani–Grammatico（2020）／Mengほか（2024、公開プレプリント） | 車線・方向指示器などの整数・二値変数と、速度・加速度などの連続変数を併せて扱う。道路グラフ上の経路・判断へ広げる応用も追う。時間を離散化することとは別の分類。 |
| Pareto・協調ゲームと多目的最適制御 | 協調微分ゲームの条件（2010）／分散Nash bargaining（2020）／未知系の方策反復（2024）／情報制約下の性能保証（2026）。多目的MPC（2009、2023）・学習と選好選択（2025）へ接続。 | 複数評価を保って効率的な解を求め、合意・選好によって一つを選ぶ。Nash均衡やPotential Gamesの別名ではなく、効率性を扱う枝として整理する。 |
| 協調・利得設計と均衡の効率 | Potential Games（1996）、協調制御への接続（2009）、Price of Anarchyの評価・設計（2024） | 個体の利得を全体目的へ合わせ、均衡に落ち着いたときの性能を評価・改善する。連合形成の基本構成はFeleほか（2017、チュートリアル節）で補う。 |
| 多数主体・Mean-Field Gamesと学習 | Huangほか（2006）／大規模ゲームの学習入門（2024、チュートリアル節） | 個々の相手ではなく集団分布との整合から近似均衡を構成する。近年の入門で計算法をつかみ、古典で仮定と理論を確認する。 |

**近年の文献から追う順序**

| 枝 | 最初に読む原著・教材 | 必要に応じて戻る基礎 |
|---|---|---|
| 零和・学習とロバスト化 | モデルフリー学習はWuほか（2025）、連続時間確率系のH∞はSun–Jia（2026）。学習中の安定性はZhangほか（2020）→Zhangほか（2021）を併読する。 | risk-sensitive総説（2023、サーベイ節）からJacobson（1973）へ。 |
| 最悪分布・minimax regret | Yang（2021）→Kim–Yang（2023）で分布集合と距離罰則を比較し、Al Tahaほか（2023）でregret基準へ進む。 | 最悪の期待コストと、先読みできる理想制御器との性能差を区別する。 |
| feedback Nash／Stackelberg | Laineほか（2023）→Liほか（2024）。学習した参照方策との接続はKLGame（2024）。 | iLQGames（2020）で反復LQ近似、Starr–Ho（1969）で非零和の解概念を確認する。 |
| 動的Potential Games | Bhattほか（2025）を入口に、Potential iLQR（2021）の構成とDistributed Potential iLQR（2023）の分散化を併読。 | Monderer–Shapley（1996）。 |
| 混合整数のゲーム理論的制御 | Fabiani–Grammatico（2020）→Mengほか（2024）。 | MLD・混合整数MPCの知識と、Potential Gamesの条件を確認する。 |
| Pareto・協調と折衷解の選択 | [Paretoの見取り図](#game-pareto)から、協調LQならZhaoほか（2024）、多目的MPCならEichfelderほか（2023）へ。交渉はCamisaほか（2020）、学習はNairほか（2025）へ分かれる。 | Engwerda（2010）とBemporad–Muñoz de la Peña（2009）を、それぞれの理論の基準点として読む。 |
| 協調・利得設計 | Chandanほか（2024）。 | Mardenほか（2009）、Monderer–Shapley（1996）。 |
| 多数主体 | Dayanıklı–Laurière（2024、チュートリアル節）。 | Huangほか（2006）。 |

上表は**学習上の順序**であり、各論文が直前の手法を直接拡張したという意味ではない。連合形成、探索信号を使うNash均衡探索、静的なStackelberg学習などの古い入口は[改訂前の保存版](archive-2026-10-26-games.html#sec-game)からも参照できる。

<a id="game-minmax"></a>
**min–max系列の見取り図**：今回の追加5件を含め、以下の軸で読む。これは学習用の整理で、各論文を同じアルゴリズムの置換列にするものではない。

| 相手が選ぶもの・評価基準 | 文献の流れ | 学ぶ論点 |
|---|---|---|
| 外乱入力／外乱抑制・最悪コスト | 対抗的RLの安定性（2020）→モデルフリーな頑健方策最適化（2021）→零和LQの学習（2025）。連続時間確率系はSun–Jia（2026）。 | 制御器と外乱側をどう更新し、学習中の安定性・頑健性と収束を両立するか。 |
| 外乱の確率分布／最悪期待コスト | Yang（2021）／Kim–Yang（2023） | 分布球の制約で不確かさを限定する設計と、分布を変える量にWasserstein罰則を課すゲームの違い。 |
| 外乱の確率分布／最悪期待regret | Al Taha・Yan・Bitar（2023） | 未来の外乱を知る非因果制御器との差を小さくする。コスト自体の最悪化とは別の設計基準。 |
| 確率的な評価からの接続 | Jacobson（1973）／risk-sensitive総説（2023、サーベイ節） | 指数型の確率評価と零和・H∞の接続には条件がある。risk-sensitive全般をmin–maxと同一視しない。 |

<a id="sec-x"></a>
H₂/H∞の状態空間理論を補う場合は[DGKF（1989）の原典](https://doi.org/10.1109/9.29425)、安全性との接点は[HJ到達可能性・安全フィルタ](#sec-f)を参照する。DGKFは主表外の補助参照とし、今回の独立した章にはしない。最悪の分布を**制約の評価だけ**に使う分布ロバストMPCと、最悪コストを最小化する零和ゲームは区別し、この追加では後者とminimax regretを中心に選んだ。

<a id="game-pareto"></a>
**Pareto系列の見取り図：効率的な解を求め、使う解を選ぶ**

パレート解は通常一つに決まらない。実現する入力・戦略の集合をPareto set、対応する評価値の集合をPareto frontと呼ぶ。ここでは、各主体のコストを調整する協調ゲームを中心に、同じ効率性の考え方を使う多目的MPCを接続先として収録する。

| 流れ・分岐 | 文献と読む順 | 核となる論点 |
|---|---|---|
| 協調微分ゲーム：モデル既知からデータ・情報制約へ | Zhaoほか（2024）から入り、Engwerda（2010）でパレート最適性の条件を確認。Roy–Tallapragada（2026）は情報制約による計算の難しさと性能保証へ進む別の展開。 | 複数主体の入力を協調して決める。重み付き最適制御への帰着、未知モデルでの方策反復、使える観測の制約を区別する。 |
| 多目的MPC：解の選択と閉ループ解析 | Eichfelderほか（2023）を入口に、Bemporad–Muñoz de la Peña（2009）へ戻る。Nairほか（2025）は反復タスクの学習、Herrmann-Wicklmayr–Flaßkamp（2025初出・2026改訂）は選好の反映へ分かれる。 | 単一の制御対象でも複数目的を持てる。各時刻の効率性、閉ループ安定性、反復間の非悪化、収束先のパレート最適性は別々に確認する。 |
| 効率的な解から合意する解へ：Nash bargaining | Camisaほか（2020） | 交渉不成立時の基準からの改善を使い、効率的な候補の中から交渉解を選ぶ。Nash均衡計算と混同しない。 |

**読み方の注意**：パレート最適性だけでは公平性や全員の参加意欲は決まらず、閉ループ安定性も自動では付かない。正の重みを付けた総コストの**大域最適解**はパレート解になるが、非凸問題では重みを変えるだけで全パレート面を得られるとは限らない。Potential Gamesの共通ポテンシャルも、各主体のコストの総和とは限らず、その均衡が元の各コストについてパレート最適になる保証は別に必要である。上の分岐は学習用の整理で、全論文が直前の論文を直接継承したとする引用系譜ではない。

<a id="game-potential"></a>
**動的Potential Gamesで計算をまとめる枝**：Negar Mehrらの **Potential iLQR（Kavuncu・Yaraneri・Mehr、2021）** は、この節で読むべき計算上の重要な接続である。各主体が自分の入力だけを変えたときのコスト差を、一つのポテンシャルの差で表せる条件を使い、そのポテンシャルを評価関数とする**単一の最適制御問題（OCP）**を解く。力学や入力が一主体分になるわけではなく、複数主体の状態・入力をまとめた一つのOCPになる。

この基礎から、**Distributed Potential iLQR（Williams・Chen・Mehr、2023）** は分散計算へ進む。予測軌道に基づいて近傍の相互作用を選び、各主体が自身と近傍の小さなOCPを解くことで、集中計算の規模を抑える。集中問題の厳密解をそのまま保証する分解という意味ではなく、近傍選択と局所的な計画を用いる方法として読む。

一方、**Bhatt・Jia・Mehr（2025）** は重み付き・制約付きの構成を扱い、非対称な相互作用や共有制約を含む設定へ広げる。条件を満たすゲームの構造を利用する話であり、任意のNash均衡計算を、元のゲームと同じ均衡を保ったままPotential Gameへ変換できるという主張ではない。また、ポテンシャルの最適解から均衡を得る理論と、局所ソルバーがその解に到達する保証は分けて読む。

<a id="game-mixed-integer"></a>
**混合整数計画法によるゲーム理論的制御**：Fabiani・Grammatico（2020）では、走行車線・方向指示器の離散判断と速度・加速度の連続変数を持つ車両の問題を、generalized mixed-integer potential gameとして扱う。整数変数を含む各主体の問題を更新し、共有する安全制約の下で近似均衡を探す。Mengほか（2024）は、道路をwaypoint graphで表す方法と更新順の工夫を通じ、異なる道路構造への適用を示す。一般に整数化すれば均衡が必ず存在するわけではなく、ポテンシャル構造、実行可能性、近似誤差と組合せ計算の負荷を確認する。

**混同しやすい点**

- **risk-sensitiveは接続する別の理論**：出発点は確率的なコストの評価であり、ここでは代表的な指数型基準を扱う。特定の条件で、分布の変更への相対エントロピー罰則を持つ零和問題やH∞制御に結び付く。CVaR・確率制約・任意のmin–maxと同一視しない。主題を深める場合は確率制御の文献群として読む。
- **協調と協力ゲーム理論は同義ではない**：共通コストを最小化するteam問題、非協力ゲームの利得設計で協調させるPotential Games、連合の価値・利益配分を扱う協力ゲームは区別する。通常の合意制御や分散最適化すべてをゲーム理論に含めない。
- **open-loopとfeedbackの均衡は異なる**：入力列を決めるゲームをMPCとして繰り返すことと、状態への応答方策そのものを選ぶfeedback Nash均衡は同じではない。局所的な一階条件を解いたことも、厳密な均衡・大域最適性・閉ループ安定性の証明とは分けて読む。

**収録文献（33件・原則年代順）**

| 読む価値・研究上の役割 | 年・論文（原文リンク・入手先） | 論文の紹介：何をして、何が分かったか | 手法・成果の補足 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| <a id="ref-nonzero-sum-differential-games"></a> **［非零和・動的Nash］** Starr・Ho。相手と目的が一致しない最適制御への拡張。 | 1969 / **[Nonzero-Sum Differential Games](https://doi.org/10.1007/BF00929443)** | 異なるコストを持つ複数の参加者が、同じ動的システムを操作する問題を解析。Nash・minimax・交渉による解が異なることを、LQ系と追跡回避例で示した。 | LQゲームで複数の解概念を行列微分方程式に結び付ける。零和の鞍点をそのまま非零和へ持ち込めない理由を学べる。 | 相互作用するロボット、複数主体の動的最適化。 | 情報構造と解概念の指定が必要。すべてのゲームで均衡が存在・一意になるわけではない。出版社全文は購読等が必要な場合がある。 |
| <a id="ref-optimal-stochastic-linear-systems-with-exponential-performance-criteria-and-thei"></a> **［接続：risk-sensitive］** Jacobson。確率制御から微分ゲームへの橋渡し。 | 1973 / **[Optimal Stochastic Linear Systems with Exponential Performance Criteria and Their Relation to Deterministic Differential Games](https://doi.org/10.1109/TAC.1973.1100265)** | ランダムな二次コストを指数関数で評価する線形確率制御を解く。大きな損失への感度を制御則に反映し、決定論的な微分ゲームとの関係を示した。 | 指数型評価基準と、それに対応するRiccati型の計算を扱う。risk-sensitive制御と対抗的な外乱設計がつながる基礎。 | 確率的な性能ばらつきを嫌う制御、ロバスト設計との比較。 | risk-sensitiveは確率的な評価基準であり、常にゲームそのものではない。零和ゲームとの関係にはモデル・評価基準の条件がある。 |
| <a id="ref-potential-games"></a> **［協調・Potential Gamesの基礎］** Monderer・Shapley。利己的な改善を共通関数に対応させる。 | 1996 / **[Potential Games](https://doi.org/10.1006/game.1996.0044)**<br>[大学公開PDF](https://people.csail.mit.edu/jrennie/trg/papers/monderer-potential-96.pdf) | 各参加者の一方的な改善が、一つのポテンシャル関数の改善と対応するゲームを整理。均衡や改善過程を、その共通関数から調べられる構造を明確にした。 | exact・weighted・ordinalなどのポテンシャルを区別する。後の協調制御では、全体の目的に対応する利得を設計する土台になる。 | 分散的な資源配分、被覆制御、利得設計。 | 元論文は静的ゲームの理論。ポテンシャルが存在しても、任意の学習則の収束や全体の大域最適性は保証されない。 |
| <a id="ref-large-population-stochastic-dynamic-games-closed-loop-mckean-vlasov-systems-and-"></a> **［多数主体・Mean-Field Games］** Huang・Malhamé・Caines。多数の相手を集団分布で扱う。 | 2006 / **[Large Population Stochastic Dynamic Games: Closed-Loop McKean–Vlasov Systems and the Nash Certainty Equivalence Principle](https://doi.org/10.4310/CIS.2006.v6.n3.a5)**<br>[出版社公開PDF](https://www.ims.cuhk.edu.hk/~cis/2006.3/05.pdf) | 多数の相手の行動を個別に追う代わりに、集団の分布と代表的な個体の制御を整合させる。分散制御則が、大きな有限集団で近似Nash均衡になる条件を示した。 | 個体の最適応答と集団挙動の自己整合性を用いるNash certainty equivalenceの構成。Mean-Field Gamesの制御側の基礎文献。 | 大規模な群・電力・交通などの多数主体モデル。 | 多数・弱い結合などの仮定に依存する。全体利益を最適化するMean-Field Controlとは異なり、少数の強い相互作用をそのまま近似できるとは限らない。 |
| <a id="ref-cooperative-control-and-potential-games"></a> **［協調・利得設計］** Marden・Arslan・Shamma。Potential Gamesを協調制御に使う。 | 2009 / **[Cooperative Control and Potential Games](https://doi.org/10.1109/TSMCB.2009.2017273)**<br>[著者所属機関の公開版](https://authors.library.caltech.edu/records/xq9ws-8wp90) | 合意形成やセンサ被覆を、各主体が自分の利得を改善するゲームとして構成。ポテンシャルと学習則を使い、局所的な意思決定から協調を生む設計を示した。 | potential gameとweakly acyclic gameを協調制御へ接続し、扱える問題を広げる。全体の目的から各主体の利得を設計する読み方が重要。 | 分散協調、被覆、センサ配置。 | 協調目的でも、数学的な定式化は非協力ゲームである場合がある。利得構造・情報・更新則により収束と得られる均衡の質が変わる。 |
| <a id="ref-multiobjective-model-predictive-control"></a> **［Pareto・多目的MPCの基礎］** Bemporad・Muñoz de la Peña。状態や時刻に応じて折衷解を選ぶ。 | 2009 / **[Multiobjective Model Predictive Control](https://doi.org/10.1016/j.automatica.2009.09.032)**<br>[著者公開PDF](https://cse.lab.imtlucca.it/~bemporad/publications/papers/automatica-mompc.pdf) | 複数の制御目標に対するパレート解から、その時点で使う入力を選ぶMPCを提案。選択を変えても閉ループを安定に保つ条件と、制御則を事前計算する構成を示した。 | 状態・時刻に依存する選択基準と多パラメトリック最適化を結ぶ。凸区分アフィンコストなどを扱い、シミュレーションで検証する。 | 複数仕様を切り替えるMPC、[Explicit MPC](#sec-explicit)との接続。 | 多目的最適制御からの接続であり、複数の戦略主体を持つゲームとは限らない。任意に選好を変更しても安定になるという保証ではない。 |
| <a id="ref-necessary-and-sufficient-conditions-for-pareto-optimal-solutions-of-cooperative-"></a> **［Pareto・協調微分ゲームの基礎］** Engwerda。効率的な協調戦略が存在する条件を学ぶ。 | 2010 / **[Necessary and Sufficient Conditions for Pareto Optimal Solutions of Cooperative Differential Games](https://doi.org/10.1137/080726227)**<br>[所属機関の書誌・要旨](https://research.tilburguniversity.edu/en/publications/necessary-and-sufficient-conditions-for-pareto-optimal-solutions-/) | 各参加者のコストを同時に考える協調微分ゲームで、パレート最適性の必要条件・十分条件を導く。不定符号のLQゲームを解析し、スカラー系では効率的な解を列挙する計算法を示した。 | 協調する入力の組を、各主体の評価を保って比較する理論の基準点。新しい方策反復・情報制約の論文から必要に応じて戻る。 | 協調LQ制御、複数主体間の性能調整。 | 一般の高次元非線形系で全パレート解を簡単に計算できるという結果ではない。公平な解の選択は別問題。公開全文は今回未確認。 |
| <a id="ref-efficient-iterative-linear-quadratic-approximations-for-nonlinear-multi-player-g"></a> **［動的Nash・数値解法］** Fridovich-Keilほか。iLQRからiLQGamesへの接続。 | 2020 / **[Efficient Iterative Linear-Quadratic Approximations for Nonlinear Multi-Player General-Sum Differential Games](https://doi.org/10.1109/ICRA40945.2020.9197129)**<br>[公開版](https://arxiv.org/abs/1909.04694) | 非線形な多人数ゲームを、線形力学・二次コストのゲームで繰り返し近似する解法を提案。交差点シミュレーションと実機の衝突回避で、実時間の相互作用計画を示した。 | 各主体のコストを保ったままLQゲームを反復する。相手の予測軌道を固定する単一主体MPCとの違いを、既知のiLQRと比較して学べる。 | 相互作用するロボット、車両のゲーム理論的MPC。 | 局所近似と初期化に依存する。非線形元問題の厳密なfeedback Nash均衡や大域収束を一般に保証する解法ではない。 |
| <a id="ref-multi-vehicle-automated-driving-as-a-generalized-mixed-integer-potential-game"></a> **［混合整数Potential Games・制御への応用］** Fabiani・Grammatico。離散判断と連続入力を併せて扱う入口。 | 2020 / **[Multi-Vehicle Automated Driving as a Generalized Mixed-Integer Potential Game](https://doi.org/10.1109/TITS.2019.2901505)**<br>[大学公開PDF](https://pure.tudelft.nl/ws/portalfiles/portal/71119721/MVAD_final_version.pdf) | 車線や方向指示器の離散判断と、速度・加速度の連続変数を持つ車両間のゲームを定式化。安全制約の下で近似均衡を求め、予測に基づく分散制御へ組み込んだ。 | Mixed-Logical-Dynamicalモデルとgeneralized mixed-integer potential gameを用いる。best responseに基づく更新で近似均衡を計算し、open-loop・closed-loopの制御方式をシミュレーションで比較する。 | 離散的な行動選択を伴う車両MPC、混合整数の多主体制御。 | 連続入力だけのゲームとは計算が異なる。共有制約、整数変数、近似均衡の許容誤差とモデル上の安全条件を確認する。CartPoleそのものの実験ではない。 |
| <a id="ref-on-the-stability-and-convergence-of-robust-adversarial-reinforcement-learning-a-"></a> **［零和・対抗的RLの安定性］** Zhang・Hu・Başar。交互に学習させるだけで安定になるかを調べる。 | 2020 / **[On the Stability and Convergence of Robust Adversarial Reinforcement Learning: A Case Study on Linear Quadratic Systems](https://papers.nips.cc/paper_files/paper/2020/hash/fb2e203234df6dee15934e448ee88971-Abstract.html)**<br>[公開PDF](https://proceedings.neurips.cc/paper_files/paper/2020/file/fb2e203234df6dee15934e448ee88971-Paper.pdf) | 制御側と外乱側を交互に学習させるRARLを、LQ系で解析。単純な交互更新が系を不安定にする例を示し、頑健に安定化する初期方策の下で安定性を保つ収束法を構成した。 | 制御入力を選ぶ主体と外乱入力を選ぶ主体の零和ゲーム。double-loopの方策更新を解析し、学習中のロバスト安定性と最適コストへの収束を結び付ける。 | 対抗的な外乱によるロバスト方策学習、RARLの理論。 | LQ設定と初期方策の頑健な安定化条件が重要。任意のニューラル方策や通常の交互勾配更新に、そのまま安定性・収束保証が付くわけではない。 |
| <a id="ref-a-distributed-optimization-algorithm-for-nash-bargaining-in-multi-agent-systems"></a> **［Pareto・Nash bargainingと分散計算］** Camisaほか。効率的な解から交渉で一つを選ぶ。 | 2020 / **[A Distributed Optimization Algorithm for Nash Bargaining in Multi-Agent Systems](https://doi.org/10.1016/j.ifacol.2020.12.402)**<br>[大学公開PDF](https://cris.unibo.it/retrieve/e1dcb338-0c4f-7715-e053-1705fe0a6cc9/nash_bargaining-ifac.pdf) | 局所制約・共有制約を持つネットワークで、各主体が合意するNash交渉解を分散計算する。交渉を重み付き目的の最適化へ変換し、重みの推定と分散更新を組み合わせた。 | IFAC-PapersOnLine 53(2):2684–2689。特定の設定で理論解析し、数値例で確認。制御系の最適定常点を決める問題も含む。 | 分散制御の運転点決定、複数主体の利益調整。 | Nash交渉解は非協力ゲームのNash均衡と別の解概念。交渉不成立時の基準や仮定が選択を左右する。一般の動的MPCの安定性まで保証した論文ではない。 |
| <a id="ref-potential-ilqr-a-potential-minimizing-controller-for-planning-multi-agent-intera"></a> **［動的Potential Games・単一OCPへの帰着］** Kavuncu・Yaraneri・Mehr。今回の追加でまず読む文献。 | 2021 / **[Potential iLQR: A Potential-Minimizing Controller for Planning Multi-Agent Interactive Trajectories](https://doi.org/10.15607/RSS.2021.XVII.084)**<br>[公開版](https://arxiv.org/abs/2107.04926)・[RSS公式ページ](https://www.roboticsproceedings.org/rss17/p084.html) | 多主体の動的ゲームがポテンシャル構造を持つ条件を利用し、均衡計算を一つの最適制御問題にまとめる。iLQRで解く計画法を構成し、シミュレーションと2機のクアッドロータで示した。 | 各主体の一方的なコスト変化を共通のポテンシャルで表せる構造に着目し、連立した最適制御問題を単一OCPへ帰着する。一般のLQゲームを反復するiLQGamesとは異なる計算の枝。 | ポテンシャル構造を持つ相互作用計画、既存iLQRの多主体計画への利用。 | 任意の非零和ゲームを同じ均衡のまま変換できる方法ではない。コスト・力学と戦略の条件が必要。iLQRの数値解から、一般の大域均衡や厳密なfeedback Nash均衡まで保証されたとは解釈しない。 |
| <a id="ref-wasserstein-distributionally-robust-stochastic-control-a-data-driven-approach"></a> **［min–max・分布ロバスト制御］** Yang。外乱の値から、外乱の確率分布へ相手を広げる。 | 2021 / **[Wasserstein Distributionally Robust Stochastic Control: A Data-Driven Approach](https://doi.org/10.1109/TAC.2020.3030884)**<br>[公開版](https://arxiv.org/abs/1812.09808) | 観測データの近くにある外乱分布のうち、最も不利なものを相手に制御を設計。Wasserstein距離で分布のずれを定め、動的計画法による求解と性能評価を示した。 | 制御入力を選ぶ側と分布を選ぶ側のmin–max問題。Kantorovich双対性を使って扱いやすい問題へ変換し、価値・方策反復を構成する。 | 外乱分布を信頼し切れない確率制御、データを用いる頑健設計。 | 分布集合の半径や段階ごとの不確かさの設定に性能・保守性が依存する。指数型risk-sensitiveやCVaR制御と同じ定式化ではない。 |
| <a id="ref-derivative-free-policy-optimization-for-linear-risk-sensitive-and-robust-control"></a> **［零和・モデルフリー方策最適化］** Zhang・Zhang・Hu・Başar。軌道データからH∞型・risk-sensitive制御を学ぶ。 | 2021 / **[Derivative-Free Policy Optimization for Linear Risk-Sensitive and Robust Control Design: Implicit Regularization and Sample Complexity](https://papers.neurips.cc/paper_files/paper/2021/hash/1714726c817af50457d810aae9d27a2e-Abstract.html)**<br>[公開版](https://arxiv.org/abs/2101.01041) | 線形のrisk-sensitive制御と外乱抑制問題を、軌道サンプルだけで方策を更新して解く方法を提案。大域収束と必要データ数を解析し、更新中に頑健性を保つ条件を示した。 | 有限ホライズンLEQGとLQ disturbance attenuationの二設定を扱い、零和LQゲームのモデルフリー学習へ接続する。implicit regularizationとサンプル数の理論を学ぶ文献。 | 未知モデルでのロバスト制御器学習、零和LQゲームの方策最適化。 | 線形・有限ホライズンなどの条件を要する。指数型確率評価と外乱抑制は対応する設定を区別し、すべてのrisk-sensitive制御をH∞問題と同一視しない。 |
| <a id="ref-algames-a-fast-augmented-lagrangian-solver-for-constrained-dynamic-games"></a> **［制約付き動的Nash・ALGAMES］** Le Cleac’h・Schwager・Manchester。衝突回避などの共有制約を解く。 | 2022〔オンライン公開・原稿は2021〕 / **[ALGAMES: A Fast Augmented Lagrangian Solver for Constrained Dynamic Games](https://doi.org/10.1007/s10514-021-10024-7)**<br>[公開版](https://arxiv.org/abs/2104.08452) | 複数車両の軌道を、各車両の目的と衝突回避制約を保って計算するソルバーを提案。拡張Lagrangian法で制約を扱い、合流などのMPCシミュレーションで高速な求解を示した。 | 一階条件の根を準Newton法で求め、共有制約付きのgeneralized Nash問題を扱う。RSS 2020版を拡張した雑誌論文を1件として収録。 | 相互作用を考慮した車両MPC、多主体軌道最適化。 | 主にopen-loopの局所的な均衡条件を扱う。毎周期再計画してもfeedback Nash均衡と同じではない。相手の目的・モデルの推定と初期化に依存する。 |
| <a id="ref-distributed-potential-ilqr-scalable-game-theoretic-trajectory-planning-for-multi"></a> **［動的Potential Games・分散化］** Williams・Chen・Mehr。Potential iLQRを近傍ごとの小さな計算へ分ける発展。 | 2023 / **[Distributed Potential iLQR: Scalable Game-Theoretic Trajectory Planning for Multi-Agent Interactions](https://doi.org/10.1109/ICRA48891.2023.10161176)**<br>[公開版](https://arxiv.org/abs/2303.04842)・[著者コード](https://github.com/labicon/dp-ilqr) | Potential iLQRの集中計算を、相互作用する近傍ごとの局所問題へ分ける計画法を提案。主体数を増やしたシミュレーションと5機のドローン実験で、計算効率と実時間動作を示した。 | 予測軌道上の接近からinteraction graphを作り、各主体が自身と近傍を含む小さなOCPをiLQRで解く。動的Potential Gamesの構造と分散MPCの考え方を接続する。 | 多数のロボットの相互作用計画、分散的なreceding-horizon制御。 | ポテンシャル構造と力学の分離などの仮定が必要。近傍の選び方・局所近似を含むため、元の全体ゲームの厳密なNash均衡や集中最適解と常に一致する保証とは区別する。 |
| <a id="ref-the-computation-of-approximate-generalized-feedback-nash-equilibria"></a> **［制約付きfeedback Nash・近年の入口］** Laine・Fridovich-Keil・Chiu・Tomlin。open-loopの再計画と異なる解概念を学ぶ。 | 2023〔初稿2021〕 / **[The Computation of Approximate Generalized Feedback Nash Equilibria](https://doi.org/10.1137/21M142530X)**<br>[公開版](https://arxiv.org/abs/2101.02900) | 状態・入力制約を持つ動的ゲームで、局所的なfeedback Nash均衡の条件を整理。計算しやすい近似条件と反復LQゲームの解法を構成し、自動運転の例で示した。 | GFNEの必要・十分条件と、必要条件を近似したGeneralized Feedback Quasi-Nash Equilibria（GFQNE）を区別する。等式・不等式制約付きLQゲームの数値計算を詳しく扱う。 | 共有制約を持つ相互作用計画、feedback均衡の数値解法。 | 近似条件を満たすGFQNEと、元の問題の厳密なGFNEは区別する。局所条件・入れ子の微分計算・初期化に依存し、一般の大域均衡を保証しない。 |
| <a id="ref-distributional-robustness-in-minimax-linear-quadratic-control-with-wasserstein-d"></a> **［min–max・Wasserstein罰則とRiccati］** Kim・Yang。最悪の分布を選ぶ相手を、LQ構造で解析する。 | 2023〔初稿2021〕 / **[Distributional Robustness in Minimax Linear Quadratic Control with Wasserstein Distance](https://doi.org/10.1137/22M1494105)**<br>[公開版](https://arxiv.org/abs/2102.12715) | 外乱の分布を悪化させる相手にWasserstein距離の罰則を課すmin–max LQ制御を提案。Riccati方程式から制御側と相手側の方策を導き、安定性と分布ずれへの性能を解析した。 | 有限ホライズンから無限ホライズン平均コストへ広げ、罰則係数と性能上界を扱う。H∞制御との理論的な接続を示し、Yang（2021）の分布集合による設計と比較できる。 | 外乱分布が不確かな線形制御、確率制御とH∞の接続。 | Wasserstein罰則を用いるゲームと、分布球内の最悪期待コストを直接最小化する問題を区別する。罰則係数・可解性・安定化の条件に依存する。 |
| <a id="ref-a-distributionally-robust-approach-to-regret-optimal-control-using-the-wasserste"></a> **［minimax regret・性能差の頑健設計］** Al Taha・Yan・Bitar。最悪コストに加え、先読みできる理想制御器との性能差を見る。 | 2023 / **[A Distributionally Robust Approach to Regret Optimal Control Using the Wasserstein Distance](https://doi.org/10.1109/CDC49753.2023.10384311)**<br>[著者公開PDF](https://bitar.engineering.cornell.edu/papers/MROC2023.pdf)・[公開原稿](https://arxiv.org/abs/2304.06783) | 将来の外乱をすべて知る理想的な制御器との性能差を、最悪の外乱分布に対して小さくする制御を提案。Wasserstein分布球を用い、minimax設計を半正定値計画へ変換した。 | 離散時間線形系・二次コストと、strictly causalな線形外乱フィードバックを扱う。最悪期待コストの設計と最悪期待regretの設計を、数値例で比較する。 | 予測できない外乱下の性能保証、分布ロバスト制御とregret設計の比較。 | 比較対象は未来の外乱を知る非因果制御器で、実装する制御器とは異なる。ここでのregretはオンライン学習の累積regretと同一ではない。線形構造と分布集合の条件を要する。 |
| <a id="ref-relaxed-dissipativity-assumptions-and-a-simplified-algorithm-for-multiobjective-"></a> **［Pareto・多目的MPCの閉ループ保証］** Eichfelder・Grüne・Krügel・Schießl。安定性に必要な仮定を緩める。 | 2023〔オンライン公開2022〕 / **[Relaxed Dissipativity Assumptions and a Simplified Algorithm for Multiobjective MPC](https://doi.org/10.1007/s10589-022-00398-4)**<br>[公開全文](https://link.springer.com/article/10.1007/s10589-022-00398-4) | 複数の競合するコストを持つ非線形MPCで、各時刻に効率的な解を選ぶ方法を解析。一つの目的に厳密散逸性と適合する終端コストを要求する構成へ仮定を緩め、安定性と各評価の性能を調べた。 | Computational Optimization and Applications 86:1081–1116。解の選択則による違いも数値比較する。MPCの知識から読む理論の入口。 | 経済性と安定化を併せて扱うMPC、複数評価の性能解析。 | 終端条件、効率解の存在・選択、初期状態などの仮定がある。毎時刻パレート解を選ぶだけで閉ループ全体もパレート最適になるとは限らない。 |
| <a id="ref-methodologies-for-quantifying-and-optimizing-the-price-of-anarchy"></a> **［協調・利得設計・近年の入口］** Chandan・Paccagnan・Marden。均衡での全体性能を評価し、改善する設計。 | 2024 / **[Methodologies for Quantifying and Optimizing the Price of Anarchy](https://rahul-chandan.github.io/publications/23-0363_MS_full.pdf)**<br>[刊行版DOI](https://doi.org/10.1109/TAC.2024.3401787) | 各主体が個別に判断した結果、全体の性能がどこまで悪くなるかをPrice of Anarchyで評価。その限界を計算し、最悪の均衡性能を改善する利得設計へつなげる枠組みを示した。 | generalized smoothnessと最適化により、混雑ゲーム・welfare最大化などのクラスで性能比を特徴付け、局所目的関数の設計を行う。2009年の協調制御と利得設計を近年の性能保証へつなぐ。 | 分散資源配分、個体の利得と全体目的を揃える設計。 | 主に均衡の効率を扱う。実プラントの過渡応答・閉ループ安定性や、任意のゲームの最良利得設計を直接保証する論文ではない。 |
| <a id="ref-blending-data-driven-priors-in-dynamic-games"></a> **［学習と動的ゲームの接続・KLGame］** Lidard・Huほか。データから得た行動方策をゲーム計算に取り込む。 | 2024 / **[Blending Data-Driven Priors in Dynamic Games](https://doi.org/10.15607/RSS.2024.XX.020)**<br>[公開版](https://arxiv.org/abs/2402.14174)・[著者解説・コード](https://kl-games.github.io/) | 学習した行動方策とゲーム理論的な計画を、KL正則化によって組み合わせるKLGameを提案。複数の行動候補を持つ近似feedback Nash方策を計算し、運転場面で検証した。 | 参照方策からのずれとタスクコストを両立する確率的ゲーム。LQ・Gaussianの解析から一般の非線形・多峰性の参照方策へ広げ、シミュレーションと実機・実データで評価する。 | 人の行動予測を使う相互作用計画、学習方策とモデルベース計画の融合。 | KL正則化を加えたゲームの均衡を扱う。参照方策の品質と正則化の強さに依存し、元の非正則化ゲームや現実の人の行動と均衡が一致する保証ではない。 |
| <a id="ref-the-computation-of-approximate-feedback-stackelberg-equilibria-in-multiplayer-no"></a> **［feedback Stackelberg・近年の入口］** Li・Sojoudi・Tomlin・Fridovich-Keil。先手・後手の応答と共有制約を扱う。 | 2024〔公開原稿は2025年に訂正〕 / **[The Computation of Approximate Feedback Stackelberg Equilibria in Multiplayer Nonlinear Constrained Dynamic Games](https://doi.org/10.1137/24M1634709)**<br>[公開版](https://arxiv.org/abs/2401.15745) | 先手と後手の関係を持つ非線形・制約付き動的ゲームで、局所的なfeedback Stackelberg均衡を近似計算する方法を提案。入れ子の最適化条件と反復LQ近似を使い、収束を解析した。 | KKT条件と二階十分条件を整理し、制約付きLQゲームを主双対内点法で解く構成から非線形ゲームへ進む。近年の動的なStackelberg計算を原著から追う入口。 | 階層的なロボット計画、先手の行動への応答を考慮する制御。 | 連続状態・入力と局所的な近似均衡が対象。戦略の順序と情報構造を指定する必要があり、任意のゲームの大域解を保証しない。2025年改訂は新しい別論文として数えない。 |
| <a id="ref-a-universal-multi-vehicle-cooperative-decision-making-approach-in-structured-roa"></a> **［混合整数Potential Games・道路構造への展開］** Meng・Huang・Ma。2020年の入口と併読する近年の応用。 | 2024〔プレプリント〕 / **[A Universal Multi-Vehicle Cooperative Decision-Making Approach in Structured Roads by Mixed-Integer Potential Game](https://arxiv.org/abs/2409.16190)** | 複数車両の判断を道路上の経路探索として表し、混合整数Potential Gameで計算する方式を提案。更新する車両の順序も工夫し、複数の都市交通シナリオで計算効率を評価した。 | waypoint graph上でMILPとして表した問題からMIPGを構成し、二つのGauss–Seidel更新を比較する。高速道路の車線選択から、異なる道路構造へ広げる一例として読む。 | 離散的な経路・行動選択を含む多車両計画。 | 実験は設定した道路グラフ・シナリオに依存する。一般の整数ゲームの大域解法や、全体最適とNash均衡の一致を主張するものとして読まない。掲載版は今回未確認。 |
| <a id="ref-policy-iteration-based-cooperative-linear-quadratic-differential-games-with-unkn"></a> **［Pareto・未知モデルの協調LQゲーム］** Zhaoほか。モデルからRiccatiを解く構成をデータへ広げる。 | 2024 / **[Policy Iteration Based Cooperative Linear Quadratic Differential Games with Unknown Dynamics](https://doi.org/10.1016/j.jfranklin.2024.107301)**<br>[出版社の要旨・書誌](https://www.sciencedirect.com/science/article/pii/S0016003224007221) | ダイナミクスが一部または全部未知の協調LQゲームで、状態・入力データを使う方策反復を提案。重み付きRiccati方程式に対応するパレート戦略を求め、数値例で動作を示した。 | Journal of the Franklin Institute 361(18):107301。既知モデルの協調最適制御から、データに基づく計算への展開として読む。 | モデルが十分に分からない多主体の協調制御。 | データ量・励起、初期方策などの条件は本文で確認する。重みを定めた解と全パレート集合の探索は別。公開全文は今回未確認で、紹介は出版社の要旨・節概要に基づく。 |
| <a id="ref-strategic-decision-making-in-multi-agent-domains-a-weighted-constrained-potentia"></a> **［重み付き・制約付きPotential Games］** Bhatt・Jia・Mehr。単一OCPへの帰着を、非対称な相互作用と共有制約へ広げる。 | 2025〔初稿2023〕 / **[Strategic Decision-Making in Multi-Agent Domains: A Weighted Constrained Potential Dynamic Game Approach](https://doi.org/10.1109/TRO.2025.3552325)**<br>[公開版・2025年改訂](https://arxiv.org/abs/2308.05876) | 重み付きポテンシャルを持つ制約付き動的ゲームの条件を整理し、open-loop GNEを単一の制約付き最適制御問題から求める。複数主体の計画と、2機のドローンによる共同運搬で検証した。 | 対称な結合コストに限られていた構成を広げ、weighted constrained potential dynamic gamesを扱う。定理の前提の下で単一OCPの解がopen-loop GNEになることを示す。[先行する制約付き計画の公開原稿](https://arxiv.org/abs/2206.08963)は関連資料として参照する。 | 衝突回避・共同運搬など、共有制約を持つ多主体の軌道計画。 | ポテンシャルの存在条件と最適性の条件を確認する。単一OCPは依然として非凸になり得る。open-loop均衡のreceding-horizon実行とfeedback Nash均衡は異なる。 |
| <a id="ref-learning-zero-sum-linear-quadratic-games-with-improved-sample-complexity-and-las"></a> **［零和LQ・学習の近年の入口］** Wu・Barakat・Fatkhullin・He。必要データ数と最後の更新値の収束を改善する。 | 2025〔初稿2023〕 / **[Learning Zero-Sum Linear Quadratic Games with Improved Sample Complexity and Last-Iterate Convergence](https://doi.org/10.1137/23M1626104)**<br>[公開版・2025年改訂](https://arxiv.org/abs/2309.04272) | 有限ホライズンの零和LQゲームで、モデルを使わず均衡を学ぶ入れ子型の更新法を提案。必要サンプル数の評価を改善し、更新値そのものが均衡へ近づく条件を示した。 | deterministicな設定でのlast-iterate線形収束と、モデルフリー設定のzeroth-order推定を解析する。2019年の基礎的な方策最適化論文は改訂前の保存版から参照できる。 | ロバスト制御と対抗的な方策学習の理論。 | 有限ホライズン・LQ構造とアルゴリズムの仮定に依存する。一般の非線形な対抗学習へ収束率やサンプル数をそのまま持ち込めない。 |
| <a id="ref-scalable-natural-policy-gradient-for-general-sum-linear-quadratic-games-with-kno"></a> **［非零和LQ・分散的な方策学習］** Shibl・Suttle・Gupta。全員の情報を持たず、近傍情報で学ぶ。 | 2025 / **[Scalable Natural Policy Gradient for General-Sum Linear Quadratic Games with Known Parameters](https://proceedings.mlr.press/v283/shibl25a.html)**<br>[公開PDF](https://raw.githubusercontent.com/mlresearch/v283/main/assets/shibl25a/shibl25a.pdf) | 複数主体の一般和LQゲームで、近傍の状態・行動情報だけを使う自然方策勾配法を提案。扱う近傍の広さに応じて、Nash均衡の近くへ収束する条件を示した。 | 既知パラメータ・有限ホライズン・確率的力学の設定。力学またはコストの結合グラフを利用し、局所情報での計算と近似誤差の関係を解析する。 | 大規模な非協力的制御、分散的な方策最適化。 | モデルパラメータは既知。近傍を限定した場合は一般に厳密な均衡ではなく近傍への収束であり、未知モデル・任意の非線形ゲームの保証ではない。 |
| <a id="ref-multi-objective-learning-model-predictive-control"></a> **［接続：Pareto・Learning MPC］** Nair・Vallon・Borrelli。繰り返すタスクで各評価の悪化を防ぐ。 | 2025〔SysDO 2024・原稿初出2024〕 / **[Multi-Objective Learning Model Predictive Control](https://doi.org/10.1007/978-3-031-83191-1_17)**<br>[公開版](https://arxiv.org/abs/2405.11698)・[著者コード](https://github.com/shn66/Multi-Objective-LMPC) | 反復タスクの軌道データからMPCの終端集合・終端コストを更新し、複数の評価が前回より悪くならない制御を構成。線形系と凸コストの設定で、実行可能性・安定性と条件付きのパレート最適性を示した。 | 学習は同じタスクを繰り返すLMPCの意味。公開版では、追加仮定と反復軌道が有限回で固定される条件の下で、収束先のパレート最適性を証明する。 | 繰り返し運転、時間・エネルギーなど複数の消費量の改善。 | 反復の収束自体を無条件に保証したものではない。各回の非悪化と、全目的が必ず厳密に改善することを区別する。複数主体ゲームであることは必須ではない。 |
| <a id="ref-individual-minima-informed-multi-objective-model-predictive-control-for-fixed-po"></a> **［接続：Pareto・選好を変更するMPC］** Herrmann-Wicklmayr・Flaßkamp。折衷解の選択と安定性を両立する。 | 2025〔プレプリント初出・2026年4月改訂〕 / **[Individual Minima-Informed Multi-Objective Model Predictive Control for Fixed Point Stabilization](https://arxiv.org/abs/2510.23454)**<br>[公開全文・改訂版](https://arxiv.org/html/2510.23454v2) | 各目的を単独で最適化した基準点を使い、指定した選好をパレート面上の解へ対応させるMPCを提案。選択法6種を整理し、制限を緩めたコスト減少条件の下で閉ループ安定性を保つ構成を示した。 | 目的数によらず二段階の最適化で選択を実装する枠組み。室内環境制御の数値例で、運転中に選好を変更する効果を確認する。 | 評価の優先度が変わる多目的MPC、選択則の設計。 | 査読刊行版として扱わない。終端要素と減少条件が必要で、任意の選好変更・任意の非凸問題に対する安定性や大域最適性の一般保証ではない。 |
| <a id="ref-a-system-level-approach-to-generalized-feedback-nash-equilibrium-seeking-in-part"></a> **［GNE・SLS・部分観測］** Neto・Mulas・Corona。閉ループ応答を使って均衡と安定性を扱う。 | 2026〔オンライン先行公開〕 / **[A System-Level Approach to Generalized Feedback Nash Equilibrium Seeking in Partially Observed Games](https://doi.org/10.1109/TCYB.2026.3711940)**<br>[公開版・2026年改訂](https://arxiv.org/abs/2503.24159) | 部分観測の線形確率系で、複数の制御器のfeedback Nash均衡を探す方法を提案。SLSで安定性や通信制約を組み込み、分散電力網のシミュレーションで示した。 | 閉ループ応答を均衡探索の変数にし、単調作用素を使って収束条件を与える。既存のSLサーベイ節と、制約付き動的ゲームを結ぶ近年の展開。 | 部分観測下の非協力的な分散制御、電力系。 | 線形モデルと選んだ方策表現・作用素の条件に依存する。任意の非線形ゲームの全feedback均衡を列挙する方法ではない。 |
| <a id="ref-robust-policy-iteration-for-the-continuous-time-stochastic-h-control-problem-wit"></a> **［H∞・未知モデルの確率系］** Sun・Jia。連続時間の零和微分ゲームをデータから解く。 | 2026〔オンライン公開2025・初稿2024〕 / **[Robust Policy Iteration for the Continuous-Time Stochastic H∞ Control Problem with Unknown Dynamics](https://doi.org/10.1016/j.matcom.2025.09.009)**<br>[公開版](https://arxiv.org/abs/2402.04721) | 未知の連続時間線形確率系で、H∞制御を零和微分ゲームとして学ぶ方策反復を提案。一度集めた状態・入力データを利用し、反復誤差が小さいと鞍点近傍へ収束する条件を示した。 | 確率的なgame algebraic Riccati equationをデータから解き、内側・外側の反復の頑健性と収束を解析する。二つのシミュレーション例を報告する。 | モデル未知の連続時間確率的H∞制御、対抗的外乱下の学習。 | データのランク条件と、反復誤差・初期条件などの仮定を確認する。有限データや誤差がある場合の鞍点近傍への収束と、厳密な最適方策への収束を区別する。 |
| <a id="ref-guaranteed-cost-structured-control-in-infinite-horizon-linear-quadratic-cooperat"></a> **［Pareto・情報制約と性能保証への展開］** Roy・Tallapragada。観測が限られる協調ゲームの計算を扱う。 | 2026〔公開プレプリント・CDC 2026採択、開催前〕 / **[Guaranteed Cost Structured Control in Infinite-Horizon Linear-Quadratic Cooperative Differential Games](https://arxiv.org/abs/2605.13103)**<br>[公開原稿・2026年8月改訂](https://arxiv.org/abs/2605.13103v3) | 出力フィードバックの構造制約があると、協調LQゲームのパレート制御も計算が難しくなる点を扱う。重み付き総コストを指定上限以下に抑える制御へ設計目標を広げ、凸LMIによる条件と数値例を示した。 | パレート解が存在すれば提案するguaranteed cost structured controlのクラスに含まれることを示し、性能差を評価。マイクログリッドの追従同期例を含む。 | 局所観測の協調制御、分散された情報の下での性能設計。 | 得られた制御器が必ず厳密なパレート最適解になるわけではない。情報構造を満たした性能保証の枝として読む。2026年10月時点で会議刊行済みとはしない。 |

**この区分の収録件数：33件。** 関連する総説5件・チュートリアル4件は冒頭の対応する節に収録し、重複計上しない。[SLS](#sec-g)・[方策合成](#sec-u)・[微分可能最適化](#sec-a)とも接続する。

<a id="implementations"></a>
## 公開実装・講義・チュートリアル資料

**以下の35件は主表の文献件数に含めない。** 再現実装の入口として収録する。対応する章から参照して使う。講義・Notebook・ソフトウェアを査読論文と区別する。

| 種別 | 教材・コード | どの論文と併読するか・用途 |
|---|---|---|
| ゲーム理論的な計画の講義 | [Game Theory tutorial（ICRA 2021、ETH公開）](https://ethz.ch/content/dam/ethz/special-interest/mavt/dynamic-systems-n-control/idsc-dam/Research_Frazzoli/workshops/ICRA21_tutorial_gametheoryplanning.pdf) | 微分ゲームとロボットの計画を結ぶ講義スライド。論文ではなく、動的ゲームの定式化・解概念を学ぶ補助教材。 |
| iLQGames著者コード | [ilqgames](https://github.com/HJReachability/ilqgames) | Fridovich-Keilほか（2020）の反復LQゲーム。既存のiLQR実装と比較する入口。 |
| 制約付き動的ゲームの著者コード | [ALGAMES.jl](https://github.com/RoboticExplorationLab/Algames.jl) | ALGAMESのJulia実装。共有制約を含む軌道最適化とMPCを読む入口。 |
| Distributed Potential iLQR著者コード | [dp-ilqr](https://github.com/labicon/dp-ilqr) | Williams・Chen・Mehr（2023）の分散的な相互作用計画。Potential iLQRから、近傍ごとの局所問題へ計算を分ける実装を読む。 |
| <a id="ref-deepc-lecture"></a> DeePCを体系的に学ぶ講義 | [A Tutorial on Data-Enabled Predictive Control (Dörfler, ICCAS 2024)](https://speakerdeck.com/floriandoerfler/a-tutorial-on-data-enabled-predictive-control-deepc) | Willemsの基本補題からDeePCの定式化、ノイズ・安定性・応用まで著者の講義で追える。論文ではなく約3時間の講義資料。 |
| Differentiable Learning実装 | [Scientific Machine Learning for Modeling, Optimization, and Control (L4DC 2026)](https://github.com/SciML-L4DC2026/SciML-L4DC2026) | 微分可能なモデル・最適化・制御をつなぐ公開チュートリアルとコード。論文ではなく教材リポジトリ。 |
| 四脚MPCの再現実装 | [Quadruped-PyMPC](https://github.com/iit-DLSLab/Quadruped-PyMPC) | Python・acados・MuJoCo/JAX等を使った四脚MPCの教材・実装例。 |
| 四脚Convex MPC実装 | [Cheetah-Software](https://github.com/mit-biomimetics/Cheetah-Software) | MIT Cheetahの制御ソフトウェア。凸MPCの実装を読む用途。 |
| ロボット最適制御 | [OCS2](https://github.com/leggedrobotics/ocs2) | SLQ系・MPCなどのC++最適制御フレームワーク。 |
| MPC+MuJoCo | [MuJoCo MPC](https://github.com/google-deepmind/mujoco_mpc) | モデルベース最適制御の実践的な実装。 |
| 四脚ROS制御 | [legged_control](https://github.com/qiayuanl/legged_control) | 旧ROS1環境の脚ロボット制御参考実装。環境依存・更新状況に注意。 |
| OCS2＋MuJoCo | [ocs2_mujoco](https://github.com/Quangvuisme/ocs2_mujoco) | OCS2とMuJoCoを接続する参考実装。 |
| ロボット軌道最適化 | [Underactuated Robotics](https://underactuated.mit.edu) | 軌道最適化、LQR、接触などを解説するオンライン教材。 |
| Differentiable MPC著者コード | [Differentiable MPC (locuslab)](https://github.com/locuslab/differentiable-mpc) | 2018年のAmosらの手法を追うコード。 |
| Differentiable MPC著者コード | [mpc.pytorch](https://github.com/locuslab/mpc.pytorch) | 微分可能なMPC層のPyTorch実装。 |
| Differentiable Convex Optimization | [CVXPYlayers](https://github.com/cvxpy/cvxpylayers) | 凸最適化レイヤーをPyTorch等へ組み込む。 |
| acados微分可能MPC | [leap-c](https://github.com/leap-c/leap-c) | acadosの感度をPyTorchへ流す学習制御の公開実装。 |
| GP-MPC実装 | [Zero-Order GP-MPC](https://gitlab.ethz.ch/ics/zero-order-gp-mpc) | LahrらのZero-order GP-MPCの実装。 |
| GP-MPC実装 | [L4acados](https://github.com/IntelligentControlSystems/l4acados) | 学習モデルをacadosへ接続するコード。 |
| Koopman基礎 | [KoopmanMPC](https://github.com/MilanKorda/KoopmanMPC) | Korda & Mezić論文の補助コード。 |
| DeePCデータ選択 | [Select-DPC](https://github.com/naefjo/choose-wisely-paper) | DeePC比較・Rocket landing等のNotebook。 |
| Neural Lyapunov検証 | [CT-BaB](https://github.com/shizhouxing/CT-BaB) | 検証を学習へ組み込むNeural Lyapunovの実装。 |
| Neural Lyapunov検証 | [Neural Network Lyapunov](https://github.com/stanfordasl/neural-network-lyapunov) | MIP等によるNN安定性解析の実装。 |
| SLS数値実装 | [SLSpy](https://github.com/shih-hao-tseng/SLSpy) | SLSのPython実装。 |
| Neural Operator | [NeuralOperator library](https://github.com/neuraloperator/neuraloperator) | FNO系の作用素学習実装。 |
| PDE制御 | [CINOC](https://github.com/SOLARIS-JHU/CINOC) | CINOCの著者実装。 |
| Koopman 2026 tutorial | [Koopman tutorial code](https://github.com/KOT-tutorial/CDC26) | Koopman理論・EDMD・MPCの段階的ハンズオン。 |
| <a id="ref-diffopt-tutorial"></a> 微分可能最適化の会議チュートリアル | [IJCAI 2022: Differentiable Optimization](https://guaguakai.github.io/IJCAI22-differentiable-optimization/) | Amos・Perrault・Wangの講義。最適化層の基礎、応用、計算上の課題を学ぶ。論文ではなくスライド教材。 |
| <a id="ref-dpc-tutorial"></a> DPCの著者チュートリアル | [Neuromancer：Learning to Controlの例](https://github.com/pnnl/neuromancer#tutorials-on-methods-for-modeling-optimization-and-control) | 線形・非線形系の予測ロールアウトから方策を学ぶNotebook。DPCとモデル学習を自分で試す教材。 |
| SQP／RTIの実装教材 | [acadosの公式例・解説](https://docs.acados.org/) | acados原論文、AS-RTI、多相OCPと併読する。 |
| 最適制御のモデル化・微分 | [CasADiの公式解説](https://web.casadi.org/docs/) | モデル、感度、single／multiple shooting、collocationの構成を読む。 |
| Box-FDDPとロボット最適制御 | [Crocoddyl](https://github.com/loco-3d/crocoddyl) | DDP・feasibility-drivenな更新とロボットモデルを結ぶ。 |
| 制約付きDDP・ProxDDP | [ALIGATOR](https://simple-robotics.github.io/aligator/) | ProxDDPのアルゴリズムとwarm startを実装で確認する。 |
| 深層モデルを使うNMPC | [Real-Time Neural MPC](https://github.com/TUM-AAS/neural-mpc) | Salzmannほか（2023）のモデル学習・微分・NMPCへの統合。 |
| 解を予測する学習の教材 | [Amortized Optimization Tutorialのコード](https://github.com/facebookresearch/amortized-optimization-tutorial) | Amos（2023）の数値例・図を再現する。 |

<a id="evaluation"></a>
## 選定変更と文献の読み方

**今回の追加（2026年10月27日）**：[論文の前に読む日本語書籍17冊](japanese-foundations.html)を追加し、19章すべてに書籍への導線を設けた。公開目次等で確認した内容と、ChatGPTが提案する読み方を分けて記載している。論文の主表162件と既存の保存版は変更していない。

**前回の構成変更（2026年10月26日）**：前版の173件を[構成変更前の保存版](archive-2026-10-26-before-modern.html)として残し、主表を162件へ再編した。保存版へ移した39件と、追加した28件は別々に数えている。追加は各章の21件と、総説・チュートリアル7件。

| 主表から外した範囲 | 件数 | 再編した理由・参照先 |
|---|---:|---|
| 一般的な同定・状態推定 | 4 | 独立章を削除。新しいNN章は、予測モデルを実際にMPCへ使う研究に絞る。 |
| ICL・メタ同定 | 4 | 今回の中心となるMPCの数値解法・モデル・保証から離れるため、独立章を外す。 |
| 汎用RL・模倣学習の基礎 | 5 | MPCとの統合に直接つながる4件へ章を組み替える。 |
| 接触・四脚の個別応用 | 2 | 汎用の解法を学ぶ主表から、再現実装を参照する構成へ。元の論文は保存版で読める。 |
| 旧補助節のPID・H₂/H∞・書誌未確定のL₁適応制御 | 3 | 独立した補助節を外す。DGKFはmin–maxの補助リンクとして残す。PID等の範囲は今後の古典制御編で改めて検討する。 |
| 同定・汎用学習などの広い周辺総説 | 11 | MPCに近い総説と、章の入口になる教材を優先する。 |
| 細分化された保証・解析・個別応用など | 10 | Neural Lyapunov・CCM・Koopman・安全フィルタ・微分可能最適化の重複と細分化を抑え、基準文献と汎用的な展開を残す。 |
| **計** | **39** | 外した文献の題名・紹介・リンクは173件の保存版に残す。 |

**入口の教材について**：19章すべてに総説・チュートリアルへの導線を用意した。微分可能最適化では会議チュートリアル、DPCでは共通理論の長編チュートリアルと著者Notebookを組み合わせる。各章に専用の新しい総説が存在すると主張するものではなく、教材の範囲と種類を明記する。

**年次と確認範囲**：新規採用分は出版社、公式会議・誌面、著者・所属機関の公開原稿を参照し、書誌、要旨、教材の構成と必要な本文箇所を確認した。例えばacadosはオンライン2021年と巻号2022年、Maximum Hands-Offはオンライン2015年と巻号2016年、AC4MPCはオンライン2025年と巻号2026年を区別する。2026年のスパース制御総説とロバストhands-offは公開原稿として扱う。既存文献まで含めた全証明・全実験の再検証ではない。

**論文紹介と解釈**：Abstractを逐語訳せず、問題・方法・主な成果を独立した短い日本語で記す。研究の分岐、読み順、教材としての選定はChatGPTによる整理。特定論文の主張と、複数論文をつないだ学習上の見取り図を分けて読む。

**保証と数値結果**：モデルの安定性、実系の閉ループ安定性、制約充足、最適化の停留点への収束、大域最適性は別の性質。推論時間だけの高速化と、最適化・学習・検証を含む全体時間も比較対象が異なる。本文の仮定、対象領域、有限反復の誤差を原典で確認する。

---

**資料の性格**：私自身がこれから読むためにChatGPTへ作成・整理を依頼した学習候補一覧であり、私自身が全件を読んで作成したレビューではありません。[構成変更前の173件](archive-2026-10-26-before-modern.html)、[ゲーム理論改訂前の157件](archive-2026-10-26-games.html)、[初回保存版の135件](archive-2026-10-25.html)も引き続き参照できます。
