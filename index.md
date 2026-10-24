---
layout: default
title: 制御工学・ロボティクス・学習ベース制御 研究文献総覧
---

# 制御工学・ロボティクス・学習ベース制御　研究文献総覧

**自分自身がこれから読むための論文・総説・教材をChatGPTにリストアップさせ、学習候補として公開している資料です。（2026年10月24日・分類改訂版）**

> **このリストの作成経緯について**  
> **この文献リストは、私自身がこれから読むために、ChatGPTに論文の探索・候補選定・概要説明・分類・推奨読書順の整理をさせたものです。私自身がこれらの文献を読んで内容を確認し、その知見に基づいて作成したリストではありません。**  
> 個人の学習用に作らせた候補一覧を、同じ分野に関心のある方の参考にもなるよう公開しています。**説明文・評価・研究史上の位置づけ・推奨読書順は主にChatGPTが生成・整理した内容であり、精読済み文献の書評や、私自身による内容の検証・推奨を示すものではありません。** 誤りや不正確な記述が残り得るため、研究で引用・利用する際は原論文の該当箇所を確認してください。

> **編集方針**：従来の107件の選定は維持し、研究上の役割に合わせて分類と読書順を改訂しました。既存の説明と定量指標を引き継ぎつつ、確認できた誤リンク・説明の一部を訂正し、既存分野を理解するための基礎文献20件を追加しています。**全件の本文・定理・実験値を精査した資料ではありません。** 各研究の成果は個々の検証条件に依存し、異なる手法の改善率を単純比較できません。書誌未確定の既存項目は「要照合」として残しています。

**文献数：127件（既存107件＋基礎追加20件、主表の各文献を1回ずつ計上）／関連する公開教材・ソフトウェア：24件。** 「［基礎追加］」は今回補った文献です。年は原則として刊行年とし、プレプリント公開年と異なる場合は注記しています。既存項目には年次基準が未統一のものも残っています。

## 目次

- [まず読む順番：全体ロードマップ](#roadmap)
- [K. MPCの安定性・数値解法：SQP・RTI・iLQR／DDP](#sec-k)
- [P. サンプリング最適制御：Path Integral・MPPI・Information-Theoretic MPC](#sec-p)
- [L. 制御用システム同定・NNモデル・時系列微分の基礎](#sec-l)
- [X. 古典的な設計基盤：PID自動調整・ロバスト制御・適応制御](#sec-x)
- [A. Differentiable MPC・微分可能最適化層](#sec-a)
- [N. 明示的制御則・MPC近似・Differentiable Predictive Control（DPC）](#sec-n)
- [D. DeePC・軌道データに基づく予測制御](#sec-d)
- [E. Koopman：作用素近似・入力付き予測・MPC・保証](#sec-e)
- [H. GPモデル・不確実性を使うMPC・学習モデルの実時間実装](#sec-h)
- [B. Neural Lyapunov・安定性証明・形式的検証](#sec-b)
- [C. Contraction理論・CCM制御器合成・収縮性の形式検証](#sec-c)
- [F. 安全制御：CBF・予測安全フィルタ・HJ到達可能性](#sec-f)
- [G. SLS・閉ループ応答設計・分散／ロバストMPC](#sec-g)
- [R. ロボット応用：接触計画・四脚MPC・状態推定](#sec-r)
- [U. 学習方策：モデルフリーRL・モデルベースRL・模倣学習](#sec-u)
- [I. Neural Operator・PDE予測モデル・PDE制御方策](#sec-i)
- [J. In-Context Learning：システム同定・力学予測・World Model適応](#sec-j)
- [S. 俯瞰用のサーベイ・総説](#sec-s)
- [T. 導入・導出を学ぶチュートリアル／概説](#sec-t)
- [公開実装・講義・チュートリアル資料](#implementations)
- [文献評価・定量指標の読み方](#evaluation)

<a id="roadmap"></a>
## まず読む順番：全体ロードマップ

**共通の俯瞰を読んだ後は、目的に応じて下表の枝を選びます。行を上から順番に通読する必要はありません。** 各行の「→」はその枝の中での推奨学習順であり、歴史的な直接の継承関係や全員に必須の前提を意味しません。「／」「比較」は並行する方法・別の論点を表します。年代順の一覧と、理解しやすい学習順は区別します。

共通の俯瞰は[A Decade of Learning-Based Control](https://arxiv.org/abs/2610.29187)または[Learning-Based MPC](https://doi.org/10.1146/annurev-control-090419-075625)から選びます。個別分野では、その分野のTutorialを先に読み、原著へ戻る進め方も有効です。

### 分類の読み方

| 分類で確認するもの | 例 | 読むときの区別 |
|---|---|---|
| 制御入力をどう決めるか | オンラインMPC、明示的方策、模倣方策、安全フィルタ | オンライン求解・方策の評価・既存入力の修正は異なる役割。 |
| 何を学ぶか | 力学モデル、持ち上げ写像、コスト、価値、制御方策、証明関数 | GP/Koopmanモデルの学習と、制御器・コストの学習を区別する。 |
| 何を保証するか | 安定性、軌道追従、安全集合不変性、確率被覆 | 一つの保証から別の保証が自動で得られるとは限らない。 |
| 文献を何に使うか | 原著、総説、チュートリアル | S/Tは読む用途の索引。技術分類との重なりを許容する。 |

各文献には主な役割を一つ割り当て、必要な関連節を参照します。応用や保証の重複を排除するために論文を削ることはせず、同じ節に置く場合も第一列のタグで枝を区別します。

| 読む枝 | 基礎・導入からの推奨順 | 到達点・並行する枝との関係 |
|---|---|---|
| **MPCの保証・数値解法（K）** | [MayneらのMPC安定性](https://doi.org/10.1016/S0005-1098(99)00214-9)。解法は[Li–TodorovのiLQR](https://doi.org/10.5220/0001143902220229)→Tassaのオンライン軌道最適化→[Control-Limited DDP](https://doi.org/10.1109/ICRA.2014.6907001)／[RTI](https://doi.org/10.1137/S0363012902400713) | 安定性の設計条件と求解アルゴリズムを区別する。iLQR/DDPとSQP/RTIは並行する選択肢。 |
| **MPPI・情報理論型MPC（P）** | [MPPI](https://doi.org/10.2514/1.G001921)→[Information-Theoretic MPC](https://arxiv.org/abs/1707.02342)→[Path Integral総説](https://doi.org/10.1016/j.arcontrol.2023.100931)。導出の基礎は[Kappen](https://arxiv.org/abs/physics/0505066)、方策改善の比較は[PI²](https://jmlr.org/papers/v11/theodorou10a.html) | 重要度サンプリング、経路コストの重み、制御分布の更新を理解する。PI²とオンラインMPCの運用を区別する。 |
| **モデル同定・NN予測（L）** | N4SID／[Narendra–Parthasarathy](https://doi.org/10.1109/72.80202)→[NNモデリングTutorial](https://www.sciencedirect.com/science/article/pii/S0098135422002927)。[BPTT](https://www.werbos.com/Neural/BTT.pdf)を併読 | 線形・NNモデルを同定し、時間展開した予測誤差を微分する。用途に応じGP、Koopman、ICLへ分岐。 |
| **最適解の微分・学習（A）** | [argmin微分](https://arxiv.org/abs/1607.05447)→[OptNet](https://arxiv.org/abs/1703.00443)→[Differentiable MPC](https://arxiv.org/abs/1810.13400)→[Differentiable NMPC](https://arxiv.org/abs/2505.01353) | MPCを方策として使い、モデル・コスト等を外側で学ぶ。CVXPYlayersは凸最適化層、DIPPは予測・計画一体学習の枝。 |
| **明示的方策・DPC（N）** | [Explicit MPC](https://doi.org/10.1016/S0005-1098(02)00174-9)と[DPC](https://doi.org/10.1016/j.jprocont.2022.06.001)を比較。DPC→[Zero-Shot Function Encoder DPC](https://proceedings.mlr.press/v331/iqbal26a.html) | 事前計算された厳密解、データによる近似、予測を通した方策学習を区別する。Aの最適解微分とは異なる。 |
| **DeePC・直接データ駆動予測（D）** | [Willems](https://doi.org/10.1016/j.sysconle.2004.09.003)→[DeePC](https://arxiv.org/abs/1811.05890)。保証は[Berberich](https://doi.org/10.1109/TAC.2020.3000182)、非線形局所法はLinear Tracking Part II／Local DPC／Select-DPCへ分岐 | 軌道表現、正則化、ノイズ下保証、局所データの利用を学ぶ。Koopman MPCは比較する別方式。 |
| **Koopman予測・MPC（E）** | [Koopman Tutorial](https://arxiv.org/abs/2607.01819)を併読し、DMD→[EDMD](https://arxiv.org/abs/1408.4408)／[DMDc](https://arxiv.org/abs/1409.6358)→[Korda–Mezić](https://arxiv.org/abs/1611.03537) | 自律系・入力付き系、辞書・NN埋め込み、モデル安定性・閉ループ保証を区別。Lusch、安定性論文、BoxQP、到達集合はそれぞれの枝へ。 |
| **GP-MPC・不確実性（H）** | Stochastic MPC Tutorial→[GP-MPC Tutorial](https://arxiv.org/abs/2404.03689)→[Cautious GP-MPC](https://arxiv.org/abs/1705.10702)→[Zero-order GP-MPC](https://arxiv.org/abs/2211.15522) | 確率予測・共分散伝播・確率制約を学ぶ。レーシングは応用、L4acadosはNN/GPモデル接続の実装基盤。 |
| **安定化の学習・検証（B）** | [Neural Lyapunov](https://papers.neurips.cc/paper/8587-neural-lyapunov-control)→[MIPによる検証](https://roboticsconference.org/2021/program/papers/063/index.html)。離散時間／出力フィードバック／CT-BaB／遅延結合系へ分岐 | 平衡点の安定性と引込み領域を学ぶ。SMT・MIP・境界伝播による検証の違いを比較する。 |
| **軌道収束・CCM（C）** | [収縮解析](https://doi.org/10.1016/S0005-1098(98)00019-3)→Contraction総説。制御器合成は[CCM](https://arxiv.org/abs/1503.03144)→[C3M](https://proceedings.mlr.press/v155/sun21b.html) | 参照軌道への追従と微分収縮を理解。離散時間Metricの形式検証は解析・認証の別枝。 |
| **安全フィルタ・到達可能性（F）** | [統一総説](https://doi.org/10.1146/annurev-control-071723-102940)で俯瞰。CBF-QP／Predictive Safety Filter／[HJ到達可能性](https://www.cs.ubc.ca/~mitchell/Papers/publishedIEEEtac05.pdf)の各枝へ | 前方不変性、予測による安全継続、最悪外乱に対する到達可能性を比較。B/Cの安定性・追従と同一視しない。 |
| **SLS・分散／ロバストMPC（G）** | [SLS原論文](https://arxiv.org/abs/1610.04815)→[SLS体系化](https://arxiv.org/abs/1904.01634)→Polytopic Robust MPC／Riccati解法／Conformalized SLS | 応答設計を共通基盤に、不確かさの扱い・計算・統計的較正へ分岐する。 |
| **ロボットへの実装（R）** | Kの基本→[MIT Cheetah 3 Convex MPC](https://doi.org/10.1109/IROS.2018.8594448)。接触計画はPosa、視覚慣性推定はForsterを必要に応じて併読 | 接触力計画・接触モード計画・状態推定を分けて実装する。MPPI、RLは別方式との比較として参照する。 |
| **RL・模倣方策（U）** | [TD学習](https://link.springer.com/article/10.1007/BF00115009)。確率的方策は[TRPO](https://proceedings.mlr.press/v37/schulman15.html)→PPO、決定論的方策はDPG、モデルベースは[TD-MPC](https://proceedings.mlr.press/v162/hansen22a.html)→TD-MPC2。Diffusion Policyは模倣学習の枝 | 価値・方策・モデルの学習対象と、報酬／実演データの違いを整理する。 |
| **PDE・作用素学習（I）** | FNO／DeepONetで予測作用素を学ぶ。物理残差の利用はPINO、制御はTI-DeepONet＋DPC／CINOCの枝へ | 予測器と方策を区別する。CINOCは微分可能PDEソルバーを通した作用素方策の学習。 |
| **ICLによる同定・適応（J）** | Class Models→確率的ICL System ID。線形系の表現力解析／World Modelの環境適応を別枝で読む | 文脈を使う未知系の予測と、閉ループ制御の保証を区別する。 |

### 目的から選ぶショートカット

| 優先したい能力 | 選ぶ枝 | 最初の再現実装・比較条件 |
|---|---|---|
| **オンライン求解を速くする** | KのiLQR/DDPまたはSQP/RTI。構造が合えばEのBoxQP。事前計算・近似ならNへ | 同一のモデル・コスト・制約・ホライズンで求解時間と最適性残差を比較する。モデル近似の効果とソルバーの効果を分離する。 |
| **MPCのモデルを改善する** | Lの同定→HのGP／EのKoopman／JのICLから目的に応じ選ぶ | 同一対象で多段予測誤差と閉ループ性能を別々に評価する。 |
| **MPCのコスト・パラメータを学ぶ** | Aの解感度→模倣学習または閉ループ方策最適化 | 二重積分系・CartPoleで有限差分と感度を照合し、その後パラメータ学習を試す。 |
| **オンライン最適化を省く方策を作る** | NのExplicit MPC・MPC近似・DPCを比較 | 方策評価時間だけでなく、オフライン計算、近似誤差、制約違反、分布外性能も測る。 |
| **直接データ駆動制御を理解する** | Dの基本補題→DeePC→目的別の保証・追従・局所データ法 | まずLTIで既知モデルMPCとの対応を確認。Eとの比較は、別方式間の比較としてデータ量・観測条件を明示する。 |
| **安全性を付加する** | FのCBF／予測フィルタ／HJから選ぶ。追従・安定性も必要ならB/Cを併用 | 安全集合・入力制約・モデル誤差・介入量・可行性を評価し、成功率だけで保証を判断しない。 |
| **四脚MPCを学ぶ** | K→RのDi Carlo→Cheetah-Software。必要に応じPosa・状態推定・Quadruped-PyMPCを併読 | 重心・単一剛体モデル、接地スケジュール、摩擦制約、低位制御を区別する。他方式との比較ではこれらの条件も揃える。 |
| **サンプリング最適制御を学ぶ** | PのMPPI→IT-MPC→理論の基礎・総説 | 同一対象・コストでサンプル数、分布、温度、ホライズンと計算時間の関係を測る。 |
| **学習方策とMPCを比較する** | UのRL各枝／Diffusion PolicyとK/P/Nを比較 | 既知モデルの有無、訓練データ・試行回数、観測、計画時間を明記する。 |

## 文献一覧（主な技術分野ごと・各節内は原則年代順）

第一列のタグで同じ節内の枝を区別します。列は、**読む価値・研究上の役割／年・論文／対象・問題設定・実験／手法と主な成果・定量指標／期待される応用／残された課題・限界**です。関連分野の参照と、主表の文献件数は別に扱います。

<a id="sec-k"></a>
### K. MPCの安定性・数値解法：SQP・RTI・iLQR／DDP

**役割：オンラインで有限ホライズン最適制御を解くための基礎。** 安定性の設計条件、SQP型解法、動的計画法に基づく局所解法を分けて読む。RTIとiLQR/DDPは並行する解法であり、前者が後者の必須前提ではない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［安定性理論・総説］** **［基礎追加］** Mayne・Rawlings・Rao・Scokaert。制約付きMPCの安定性理論の基本総説。 | 2000 / **[Constrained Model Predictive Control: Stability and Optimality](https://doi.org/10.1016/S0005-1098(99)00214-9)** | 線形・非線形系の有限ホライズン制御で、実行可能性と閉ループ安定性をどう保証するか。 | 終端コスト・終端集合・終端制御則などの条件を整理し、MPCの安定性と最適性の論理を体系化。特定実験の高速化倍率を示す文献ではない。 | Tube MPC、学習モデル付きMPC、Koopman MPCの保証を読む共通の土台。 | 十分条件とモデルの仮定を要する。学習誤差・確率的保証・ソルバーの実時間実装は別途扱う。 |
| **［iLQR／DDP］** **［基礎追加］** Li・Todorov。iLQRの局所近似と前後方計算を学ぶ基礎。 | 2004 / **[Iterative Linear Quadratic Regulator Design for Nonlinear Biological Movement Systems](https://doi.org/10.5220/0001143902220229)** | 非線形な身体運動系で、局所的に最適な軌道とフィードバックを反復計算する。 | 力学の線形化とコストの二次近似からLQR型の後退計算を行い、前進シミュレーションで改善する。DDPで用いる力学の二階微分との違いを確認できる。 | Tassaのオンライン軌道最適化、Differentiable MPC、ロボットNMPC。 | 局所解と初期軌道に依存する。一般の状態制約や接触モード探索をそのまま処理する方法ではない。 |
| **［SQP・RTI］** NMPCの実時間計算の歴史を理解する要石。 | 2005 / **[A Real-Time Iteration Scheme for Nonlinear Optimization in Optimal Feedback Control](https://doi.org/10.1137/S0363012902400713)** | 非線形MPCで各サンプリング時刻にNLPを完全に収束させる時間がない問題を考える。 | SQPの一回の反復をサンプリング周期に合わせて実行するRTIを提案し、実時間のフィードバック最適化を可能にする理論と数値例を提示。制御性能・演算時間の普遍的な改善倍率はない。 | acados RTI、非線形車両MPC、ロボットのオンライン軌道最適化。 | 良い初期解、滑らかな力学、サンプリング間の変化が十分小さいこと等に依存し、単一反復のため厳密収束しない場合がある。 |
| **［iLQR／DDP］** DDP/iLQRによる非線形最適制御の実践的な起点。 | 2012 / **[Synthesis and Stabilization of Complex Behaviors through Online Trajectory Optimization](https://homes.cs.washington.edu/~todorov/papers/TassaIROS12.pdf)** | 複雑な多自由度非線形系で短いホライズンの軌道を逐次最適化し、閉ループで振る舞いを生成する問題。 | 微分動的計画法／iLQG系の二次近似と前後方掃引を実時間のフィードバック計画へ適用し、複雑な動作をオンラインで生成・安定化できることを示した。全対象で共通する追従誤差・速度改善率は未確認。 | MPC、軌道最適化、四脚・多関節ロボットの動作生成。 | 局所解に依存し、接触・摩擦などの非滑らかな遷移と厳しい制約の扱いが難しい。 |
| **［iLQR／DDP］** **［基礎追加］** Tassa・Mansard・Todorov。入力制約を含むDDP/iLQRの基本。 | 2014 / **[Control-Limited Differential Dynamic Programming](https://doi.org/10.1109/ICRA.2014.6907001)** | アクチュエータの入力上下限制約を持つ非線形最適制御。 | 後退計算の局所制御問題をbox制約付きQPとして扱い、単なる入力クリッピングと異なる制約を考慮した軌道改善を行う。 | 入力飽和を持つロボット制御、MPCの制約処理、微分可能MPCの背景。 | 扱う入力制約と一般の非線形状態制約は異なる。局所近似と初期解依存は残る。 |

**推奨順と理由**：MPCの保証はMayneら（2000）から読む。局所最適制御の計算はLi–Todorov（2004）→Tassaら（2012）→Control-Limited DDP（2014）。SQP型の実時間計算はDiehlらのRTI（2005）へ分岐する。両解法の比較では力学・コスト・制約・停止条件を揃える。

**この区分の収録件数：5件。**

<a id="sec-p"></a>
### P. サンプリング最適制御：Path Integral・MPPI・Information-Theoretic MPC

**役割：軌道分布・経路コストの重み付けに基づく制御。** 経路積分の理論、PI²による方策改善、オンラインMPPI、情報理論によるIT-MPCの導出を区別する。PI²は関連する基礎だが、MPPIと同一の運用・更新対象ではない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［経路積分の理論］** **［基礎追加］** Kappen。経路積分制御のHJB変換を学ぶ基礎。 | 2005 / **[Path Integrals and Symmetry Breaking for Optimal Control Theory](https://arxiv.org/abs/physics/0505066)** | 特定の雑音・制御コスト構造を持つ非線形確率最適制御。 | 指数変換により非線形HJB方程式を線形化し、拡散過程の期待値・経路積分で価値関数を表す。モンテカルロ評価への理論的接続を与える。 | PI²、MPPI、情報理論に基づくサンプリング最適制御の背景。 | 制御と雑音の作用・コストに構造条件が必要。任意の最適制御問題を無条件に線形化する結果ではない。 |
| **［PI²・方策改善］** **［基礎追加］** Theodorou・Buchli・Schaal。PI²による方策改善の原典。 | 2010 / **[A Generalized Path Integral Control Approach to Reinforcement Learning](https://jmlr.org/papers/v11/theodorou10a.html)** | パラメータ化した制御方策を、確率的な軌道ロールアウトで改善する。 | 経路積分の重み付けを用いるPolicy Improvement with Path Integrals（PI²）を導出し、ロボット運動学習で評価。オンラインMPPIとは更新対象と運用が異なる。 | 経路コストによる指数重み付け、サンプリング型方策探索とMPCの比較。 | 探索分布・方策表現・確率制御の仮定に依存。PI²とMPPIを同一アルゴリズムとして扱わない。 |
| **［MPPI］** MPPIを研究する際の基盤。 | 2017 / **[Model Predictive Path Integral Control: From Theory to Parallel Computation](https://doi.org/10.2514/1.G001921)** | 確率的な軌道サンプリングで非線形・非凸の有限時間最適制御を近似する問題。 | 経路積分制御をGPU並列計算へ展開し、サンプリングベースの実時間MPCとして実証。演算速度・追従性能の共通倍率は未確認。 | 車両の高速走行、接触を含むロボティクス、GPU搭載プラットフォーム。 | サンプリング数による計算負荷、モデル精度、入力分布設計、厳密な安全保証が課題。 |
| **［IT-MPC］** **［基礎追加］** Williams・Drews・Goldfain・Rehg・Theodorou。IT-MPCの基本論文。2017年プレプリント、2018年T-RO。 | 2018 / **[Information-Theoretic Model Predictive Control: Theory and Applications to Autonomous Driving](https://arxiv.org/abs/1707.02342)** | 非線形車両の高速な未舗装路走行に対するサンプリング型MPC。 | 情報理論の変分的な関係と相対エントロピーを用いて制御分布の最適化を導出し、重要度サンプリングによるMPCを構成。実車で評価しCEM型MPCと比較する。 | MPPIの重みと分布更新の理解、情報理論に基づくMPC、GPUサンプリング制御。 | サンプル数・提案分布・モデル誤差に依存し、一般のハード制約保証は自動で得られない。雑誌版DOIは10.1109/TRO.2018.2865891。 |

**推奨順と理由**：実装を先に掴むならWilliamsらのMPPI（2017）→Information-Theoretic MPC（2018、プレプリント2017）→2024年Path Integral総説。導出を遡るならKappen（2005）を読む。TheodorouらのPI²（2010）は方策改善の枝として比較する。2026年の確率推論型MPC総説はS節にある。

**この区分の収録件数：4件。**

<a id="sec-l"></a>
### L. 制御用システム同定・NNモデル・時系列微分の基礎

**役割：制御で使う予測モデルの作成と学習計算。** N4SIDは線形状態空間モデルの同定、Narendra–ParthasarathyはNNによる非線形同定・制御、Werbosは時間方向の微分計算を扱う。三者は異なる役割を持つ基礎である。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［NN同定・制御］** **［基礎追加］** Narendra・Parthasarathy。NNによる非線形同定と制御の古典的基盤。 | 1990 / **[Identification and Control of Dynamical Systems Using Neural Networks](https://doi.org/10.1109/72.80202)** | 未知の非線形離散時間系の入出力関係をNNで学習し、制御器構成に利用する。 | 動的システムの同定モデルと制御構成を整理し、NNによる近似と学習を制御問題に結び付ける。現在の深層状態空間モデルとは区別して読む。 | NN-MPC、学習残差モデル、制御用システム同定の歴史的背景。 | ネットワーク近似や訓練性能だけでは閉ループ安定性を保証しない。現代の大規模深層学習の実装教材ではない。 |
| **［時系列の微分計算］** **［基礎追加］** Werbos。動的モデルの時系列展開に勾配を流す計算の基礎。 | 1990 / **[Backpropagation Through Time: What It Does and How to Do It](https://www.werbos.com/Neural/BTT.pdf)** | 再帰的な計算・時系列モデル・システム同定・制御におけるパラメータ勾配。 | 時間方向に展開した計算へ連鎖律を適用するBPTTの基本式と用途を説明。予測ロールアウトを通した方策学習の計算原理を理解できる。 | NNダイナミクス同定、DPC、微分可能シミュレーションによる方策最適化。 | 長期展開では勾配消失・爆発、メモリ負荷、モデル誤差が問題となる。最適化問題のargminを微分する方法とは異なる。 |
| **［線形同定］** 間接データ駆動制御の歴史的基盤。 | 1994 / **[N4SID: Subspace Algorithms for the Identification of Combined Deterministic-Stochastic Systems](https://doi.org/10.1016/0005-1098(94)90030-2)** | 雑音を含む入出力系列からMIMO状態空間モデルを効率よく推定する。 | 過去・未来のデータ行列と射影・SVDから状態空間表現を復元するN4SID系の枠組みを提示。実験系別の誤差改善率は未確認。 | MIMOシステム同定、サブスペース予測器、DeePCとの比較。 | 線形性、モデル次数選択、励起条件、欠損・時変・非線形データへの対応が課題。 |

**推奨順と理由**：線形モデル同定はN4SIDから、NNモデルはNarendra–ParthasarathyとT節の2022年NNモデリングTutorialから読む。BPTTはNNの時系列学習およびN節のDPCの計算原理として併読する。深層同定・Control-Oriented System Identificationの総説はS節にある。

**この区分の収録件数：3件。**

<a id="sec-x"></a>
### X. 古典的な設計基盤：PID自動調整・ロバスト制御・適応制御

**役割：現在の学習制御と比較する従来の設計原理。** PID自動調整、H₂/H∞合成、Tube MPC、L1適応制御は独立した方法族である。刊行年代だけで一つの系譜とはみなさない。旧X節にあった近年の派生研究は、それぞれの技術分野へ移動した。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［PID自動調整］** 先に提示した「古典的制御寄り10本」の一つ。 | 1984 / **[Automatic Tuning of Simple Regulators with Specifications on Phase and Amplitude Margins](https://doi.org/10.1016/0005-1098(84)90013-5)** | モデルを明示同定せずにPID制御器を自動調整し、ゲイン余裕・位相余裕に対応した性能を得る。 | リレー実験から限界振動の周波数と振幅を推定するPID自動調整を提案し、実用的な制御器の自動設計に道を開いた。改善率はシステムごとに異なる。 | 産業PID・ゲイン自動調整・低次元フィードバック制御のベースライン。 | 非線形リレー実験の許容、強い非線形性・遅延、周波数応答を1点で代表する限界。 |
| **［H₂／H∞合成］** DGKFの原典。 | 1989 / **[State-Space Solutions to Standard H2 and H-infinity Control Problems](https://doi.org/10.1109/9.29425)** | 標準H2/H∞問題を状態空間・リカッチ方程式で解く。 | 可解条件と中央解をリカッチ方程式で表し、混合感度などのロバスト制御の数理的基盤を確立。主成果は可解性・構成法の定理。 | H∞混合感度、ロバストコントローラ、周波数整形。 | 標準問題の可解性条件、重み選択、コントローラ次数、非線形・飽和の扱いが課題。 |
| **［Tube MPC］** 現代のRobust MPCの基本。 | 2005 / **[Robust Model Predictive Control of Constrained Linear Systems with Bounded Disturbances](https://doi.org/10.1016/j.automatica.2005.02.005)** | 有界外乱下の制約付き線形系を予測制御する。 | 名目軌道と誤差に対する固定安定化フィードバックを分離し、制約引き締めによる実行可能性・安定性の保証を示すTube MPCを定式化した。 | ロバストMPC、GP/学習モデルの誤差集合に対する制約保証。 | 外乱集合の既知性・保守性、チューブ計算、非線形系の扱いが課題。 |
| **［適応制御・書誌要照合］** 会話で当初挙げた古典よりの選択肢。 | 2007 / **[L1 Adaptive Control: Theory and Applications](https://ieeexplore.ieee.org/search/searchresult.jsp?queryText=L1%20adaptive%20control%20Hovakimyan%20Cao%202007)** | モデル不確かさを有するシステムで高速適応とロバスト性を両立する。 | 適応推定と制御入力の帯域分離・フィルタ設計を特徴とするL1適応制御を体系化。論文題名・原典リンクは再確認が必要。 | 航空・ロボット・機械装置の適応フィードバック。 | 外乱とモデルクラスに関する仮定、入力フィルタによる性能制約、実機遅延。 |

**推奨順と理由**：固定構造・少数パラメータの調整はÅström–Hägglund、周波数領域のロバスト合成はDGKF、有界外乱下のMPCはMayneらのTube MPC、適応系はL1の文献へ分岐する。Tube MPCの前提となるMPC安定性はK節のMayneら（2000）で確認する。

**この区分の収録件数：4件。**

<a id="sec-a"></a>
### A. Differentiable MPC・微分可能最適化層

**役割：最適化問題の解のパラメータ感度を外側の学習へ渡す。** 汎用のargmin微分・凸最適化層、MPC/NMPCの解感度、予測・計画一体学習の応用を分けて読む。微分可能化自体は一般のMPC求解を高速化する保証ではない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［MPCの解感度］** **［基礎追加］** Gouldら。最適解の感度と二段階最適化の導入となる技術報告。 | 2016 / **[On Differentiating Parameterized Argmin and Argmax Problems with Application to Bi-level Optimization](https://arxiv.org/abs/1607.05447)** | 下位の最適化問題の解が、上位の損失関数へ入るパラメータ学習。 | 制約なし・制約付きargmin/argmaxの微分を整理し、最適性条件から解のパラメータ感度を求める計算を説明。 | OptNet、微分可能MPC、逆最適制御の数理的準備。 | 局所解の正則性などを要する。非滑らかな解の切替えや解の非一意性を無条件に扱えるわけではない。 |
| **［汎用の最適化層・解感度］** QPのKKT感度を利用した微分可能な最適化層 | 2017 / **[OptNet: Differentiable Optimization as a Layer in Neural Networks](https://arxiv.org/abs/1703.00443)** | 画像分類・構造化予測などにQP層を組み込む実験と、GPUでのバッチQP求解を評価した。 | QPのKKT条件から厳密な局所解感度を計算する仕組みを実装し、制約を備えた学習可能層として機能することを示した。逆伝播の追加計算はQPの因数分解を再利用でき、著者らは「求解に対しごく小さい追加コスト」と報告している。 | 制約付き行動生成、学習可能なQP制御、逆最適化、制約付き機械学習に利用できる。 | QPの解の一意性、正則性および微分可能な有効制約集合を暗黙に仮定する。非凸NLPでは局所解・特異なKKT行列への対応が必要であり、改善率の一律な数値は示せない。 |
| **［MPCの解感度］** 時系列最適制御の解感度を学習へ逆伝播 | 2018 / **[Differentiable MPC for End-to-end Planning and Control](https://arxiv.org/abs/1810.13400)** | 振子とCartPoleの模倣学習において、専門家の操作をMPC方策で再現する実験を行った。 | Box-DDP固定点のKKT条件から制御系列の感度を求め、コスト重みとダイナミクスを逆伝播で学習した。汎用NNより少ない教師データで学習でき、専門家がモデルクラス外の条件でも通常のシステム同定より優れる事例を示したが、具体的な改善率は要旨から確認できない。 | 逆最適制御、コスト学習、模倣学習、学習可能な非線形MPC層へ応用できる。 | 局所凸近似の固定点が得られることに依存し、非凸問題や有効制約集合の切替えで勾配が不安定になり得る。閉ループ安定性・安全性を自動で保証するものではない。 |
| **［MPCの解感度］** 凸最適化層を汎用的に構築する枠組み | 2019 / **[Differentiable Convex Optimization Layers](https://papers.neurips.cc/paper_files/paper/2019/hash/9ce3c52fc54362e22053399d3181c638-Abstract.html)** | 画像処理等の学習課題で、パラメータ付き凸最適化問題をネットワークの内部に挿入して評価した。 | Disciplined Parametrized Programmingに従う凸問題を自動的に微分可能層へ変換し、Forwardの最適解とBackwardの感度を統合した。複数の学習実験で利用可能性を示しているが、汎用的な精度向上率は課題依存である。 | CVXPYを使った微分可能MPC、最適化を含む計算グラフ、パラメータ同定へ適用できる。 | 凸性とDPPに基づく定式化が必要で、一般の非凸NMPCは直接扱えない。大きなバッチや長ホライズンでは求解と感度計算のメモリが制約になる。 |
| **［応用・予測計画の共同学習］** Differentiable MPCの応用例として紹介された研究。 | 2023 / **[DIPP: Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving](https://arxiv.org/abs/2207.10422)** | 自動運転で周辺車両の将来軌道の予測と自車の軌道最適化を一体化する問題。 | NN予測器→微分可能プランナ→走行軌道損失へ勾配を流し、予測と計画を別々に学習する方式より良好な軌道模倣・計画結果を報告。具体的な改善倍率は未確認。 | 予測・計画のEnd-to-end学習、自動運転のDifferentiable Optimization。 | 予測環境の分布シフト、非凸な最適化と衝突制約、模倣性能と閉ループ安全性の差が課題。 |
| **［MPCの解感度］** 制約付きNMPCの解感度とacadosへの実装 | 2025 / **[Differentiable Nonlinear Model Predictive Control](https://arxiv.org/abs/2505.01353)** | 一般の制約付き最適制御問題で、SQP・内点法に基づく感度計算を既存の微分可能ソルバーと比較した。 | acadosに前向き・随伴解感度を実装し、原著要旨でmpc.pytorchおよびcvxpygenに対し3倍超の高速化を報告した。これは論文で設定したベンチマークにおける計算時間の比較である。 | 学習可能なNMPC、最適制御方策の勾配学習、モデル・コスト同定に利用できる。 | 最適性条件の正則性が必要であり、有効制約の切替えや局所最適解の非一意性は感度の信頼性を損なう。3倍超という数値は任意の問題・ハードウェアへ一般化できない。 |
| **［MPCの解感度］** モデル誤差下で微分可能MPCを使って方策最適化 | 2026 / **[Policy Optimization for Unknown Systems using Differentiable MPC](https://proceedings.mlr.press/v331/zuliani26a.html)** | 状態が12次元の非線形クアッドコプター系で、未知・不正確なモデルに対するMPC方策学習を評価した。 | モデルから得る微分勾配とゼロ次推定を組み合わせ、モデルフリーのみの更新より初期の学習進行が速いことを示した。論文は一定の仮定下の収束解析も提示するが、改善率の数値は要旨では公開していない。 | モデル誤差を含むMPCの評価関数調整、閉ループ性能直接最適化、実機適応に適用可能である。 | ゼロ次勾配推定には試行回数を要し、未知実機への探索入力は安全性上の問題がある。収束保証は対象モデルと雑音に課された条件に依存する。 |

**推奨順と理由**：Gouldらでargminの感度→OptNetでQPのKKT微分→AmosらでMPCの局所最適解感度→Freyらで制約付きNMPC実装を学ぶ。CVXPYlayersは汎用凸最適化層の並行する枝、DIPPは予測・計画の共同学習の応用、Zulianiはモデル誤差下の方策最適化として読む。

**この区分の収録件数：7件。**

<a id="sec-n"></a>
### N. 明示的制御則・MPC近似・Differentiable Predictive Control（DPC）

**役割：オンラインでの最適化を事前計算や学習した方策で置き換える。** Explicit MPCの厳密な区分的アフィン解、データによるMPC解の近似、予測ロールアウトを通して方策を学ぶDPCは異なる枝である。A節のDifferentiable MPCは最適化問題の解を微分するが、DPCは主に学習時の閉ループ予測を微分する。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［厳密な事前計算］** MPCを明示的方策で置き換える研究の歴史的基盤。 | 2002 / **[The Explicit Linear Quadratic Regulator for Constrained Systems](https://doi.org/10.1016/S0005-1098(02)00174-9)** | 線形制約付きMPCの最適入力を状態の区分的アフィン関数としてオフラインで計算する。 | パラメトリックQPの解をExplicit MPCとして表現する枠組みを示し、オンラインQP求解を表引きに近い評価へ置換した。 | マイクロコントローラ向けMPC、MPC方策の学習代替との比較。 | 状態次元・制約数が増えると領域数が爆発し、メモリと探索コストが課題。 |
| **［DPC・明示的方策学習］** **［基礎追加］** Drgoňaら。予測ロールアウトを使うDPCの基本論文。 | 2022 / **[Differentiable Predictive Control: Deep Learning Alternative to Explicit Model Predictive Control for Unknown Nonlinear Systems](https://doi.org/10.1016/j.jprocont.2022.06.001)** | 未知非線形系の学習モデルを使い、制約を考慮する明示的NN制御方策を訓練する。 | 微分可能なダイナミクスモデルを通して有限ホライズン制御損失を逆伝播し、方策をオフライン最適化する。オンラインでは学習した方策を評価する。 | オンライン最適化を減らす制御、PDE-DPC、Zero-Shot Function Encoder DPCの入口。 | 制約の損失への組込みだけで一般の安全性が保証されるわけではない。モデル誤差、訓練分布、事前学習コストを考慮する必要がある。 |
| **［データによるMPC近似］** 古典的Explicit MPCと新しいデータ駆動方策高速化を比較する。 | 2026 / **[Data-driven Acceleration of MPC with Guarantees](https://proceedings.mlr.press/v331/castellano26a.html)** | 多数のオフラインMPC解からオンラインで制御入力を高速に得る。 | データ被覆の条件下で実行可能性と最適性ギャップの保証を与え、前回説明では対象課題で通常のオンラインMPCより約100～1000倍高速だったと報告。 | Explicit MPCの近似、MPC蒸留、組込み制御器の高速化。 | 高次元空間で必要なデータ数、分布外状態、被覆保証、制約切替えが課題。 |
| **［DPC・明示的方策学習］** MPC解の微分と混同しない：DPCは予測ロールアウトで方策学習。 | 2026 / **[Zero-Shot Function Encoder-Based Differentiable Predictive Control](https://proceedings.mlr.press/v331/iqbal26a.html)** | パラメータの異なる非線形システム群で、追加訓練なしに適応する制御器を作る。 | Function Encoder型Neural ODEとDifferentiable Predictive Controlを組み合わせ、オフライン学習した明示的方策によるゼロショット適応を数値実験で検証。改善率の詳細は未転記。 | 類似装置間の制御方策の転移、オンライン最適化なしの高速制御。 | 事前学習分布外のモデルへの保証、安全性、学習モデル誤差、転移限界が課題。 |

**推奨順と理由**：厳密な事前計算はExplicit MPC（2002）、ロールアウトによる方策学習はDPC（2022）→Zero-Shot Function Encoder DPC（2026）。Data-driven Accelerationは近似・被覆保証の枝として読む。PDEでのDPC応用はI節。最適解の感度を学びたい場合はA節へ進む。

**この区分の収録件数：4件。**

<a id="sec-d"></a>
### D. DeePC・軌道データに基づく予測制御

**役割：軌道データの表現を予測制御問題に直接使う。** ノイズなしLTIの基本構成から、ノイズ・正則化・追従・非線形系の局所データ利用へ分岐する。Koopman MPCは別の予測モデル構築法であり、DeePCの後継ではない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［軌道表現の基礎］** 持続励起データからLTI軌道を表現する基本補題 | 2005 / **[A Note on Persistency of Excitation](https://doi.org/10.1016/j.sysconle.2004.09.003)** | LTI系の入出力列からHankel行列を作り、持続励起と軌道表現の関係を理論的に解析した。 | 入力が必要次数で持続励起されれば、長さLのすべての許容軌道が一つのHankelデータ行列の列空間で表現できることを示した。主要成果は実験改善率ではなくランク条件による厳密な表現性である。 | DeePC、データ駆動状態推定、線形システムの行動的同定の基礎になる。 | ノイズなしLTI系と十分な持続励起が本質的な前提であり、非線形系・ノイズ下ではそのままの等価性は成立しない。LTI系への入力上下限制約自体は基本補題と矛盾しない。 |
| **［DeePC・保証］** Hankelデータからモデルを明示せずMPCを構築 | 2019 / **[Data-Enabled Predictive Control: In the Shallows of the DeePC](https://arxiv.org/abs/1811.05890)** | 未知LTI系の軌道追従・入力制約付き制御と、非線形・確率系の数値シミュレーションを扱った。 | 有限の入出力データから予測軌道を最適化し、ノイズなし決定論的LTI系では適切な条件下で従来MPCと等価な入力を得ることを証明した。ノイズ系では正則化による実験的改善を示したが数値倍率は未確認。 | モデルを明示的に同定しないMPC、未知プラントのオンライン制御に利用できる。 | ノイズなしLTIという等価性の条件は強く、ノイズ・非線形・データ量増加により正則化・次元削減・安定性解析が必要になる。 |
| **［DeePC・保証］** 直接データ駆動MPCの安定性・ロバスト性 | 2021 / **[Data-Driven Model Predictive Control with Stability and Robustness Guarantees](https://doi.org/10.1109/TAC.2020.3000182)** | ノイズのある未知LTIシステムの追従制御に対し、データ駆動MPCのロバスト安定性を解析した。 | 測定ノイズなどを考慮したMPC設計で、明示的な同定モデルに頼らずに閉ループ実用安定性を保証する十分条件を提示した。性能改善率より、ノイズ上界と設計条件の明確化が主要成果である。 | 制御保証を重視するDeePC系、未知線形プラントの実装に有用である。 | 小さなノイズ、十分な励起、特定の重みやホライズンなどの条件を要する。一般の非線形・時変系への厳密な保証はそのまま適用できない。 |
| **［非線形・局所データ］** 局所データ更新による非線形システムの追従制御 | 2022 / **[Linear Tracking MPC for Nonlinear Systems—Part II: The Data-Driven Case](https://arxiv.org/abs/2105.08567)** | 未知の非線形システムを、オンラインの入出力データで局所近似しながら参照追従する問題を扱った。 | 逐次更新するデータ駆動線形MPCにより、一定の局所条件下で非線形系の実用的追従保証を与え、数値例で追従を示した。定量的な追従誤差の改善率は要旨では確認できない。 | 局所線形化モデルを明示同定しないNonlinear Data-Driven MPCに適用できる。 | 動作点に近い局所データ、十分な励起、局所モデル誤差の小ささに依存する。大域非線形性や急激なモード遷移に対応するには追加工夫が必要である。 |
| **［定常偏差］** 従来MPCとDeePCを制御目的の観点で結ぶ教材。 | 2022 / **[Offset-free Data-driven Predictive Control](https://doi.org/10.1109/CDC51059.2022.9992453)** | MPC、サブスペース予測制御、DeePCを定常偏差除去という課題から比較。 | 積分作用・増分入力やデータ駆動法の正則化を関連づけ、オフセットフリー追従を説明。論文の特定実験改善率は未確認。 | 精密サーボの定常誤差対策、DeePCへの積分構造導入。 | 観測ノイズ、ランク・励起条件、外乱推定に関する仮定。 |
| **［直接法と間接法］** 直接法と間接法の関係を正則化の観点から整理 | 2023 / **[Bridging Direct and Indirect Data-Driven Control Formulations via Regularizations and Relaxations](https://doi.org/10.1109/TAC.2022.3148374)** | 直接データ駆動制御とモデル同定を経由する間接法を、正則化・緩和を通して比較した。 | 両形式が特定の正則化と制約緩和の下で対応することを解析し、従来異なると見られていた設計問題の接点を明らかにした。主要成果は定理による関係の解明で、実験上の一律の改善率ではない。 | DeePCの正則化設計、間接同定型MPCと直接法の比較に利用できる。 | 対応関係は仮定したデータ・同定・正則化に依存する。非線形、強雑音、オンライン計算時間を含む総合性能差は別途評価が必要である。 |
| **［非線形・局所データ］** Select-DPCと比較して読むと局所データの意味が分かる。 | 2026 / **[Taming Non-linearity with Local Data-driven Predictive Control](https://doi.org/10.1016/j.ifacsc.2026.100396)** | 非線形系で明示的な状態空間同定なしにローカルなデータ表現を作り、反復的にQPを解く。 | Sequential Data-driven QPという構成で局所モデルと最適化を更新し、倒立振子Swing-upで既存のデータ駆動・モデルベース手法と比較。数値改善率は未確認。 | 非線形DeePC、SQP型データ駆動最適制御。 | 局所線形性とデータ被覆、反復収束、実時間QP計算負荷。 |
| **［非線形・局所データ］** 局所的に有用なデータをオンライン選択するSelect-DPC | 2026 / **[Choose Wisely: Data-driven Predictive Control for Nonlinear Systems Using Online Data Selection](https://proceedings.mlr.press/v331/naf26a.html)** | ロケット着陸、ロボットアーム、CartPoleの振り上げという3種類の非線形シミュレータでDeePCとTime-Windowed DeePCを比較した。 | 現在の運転点に関連する軌道データをオンライン選択するSelect-DPCが、3種類の評価系で両比較法を上回る制御性能を示した。誤差削減率・成功率の具体値は公開要旨に記載されていない。 | 非線形系の局所データ駆動制御、履歴データが大量にある装置へのオンラインMPCに向く。 | 選択データの密度・多様性に性能が左右され、局所データが足りない領域で精度が落ちる。一般の非線形系について広域の安定性・制約保証はなお課題である。 |

**推奨順と理由**：Willems→CoulsonのDeePCを基礎に、保証はBerberich（2021）、定常偏差はOffset-free（2022）、直接法と同定経由の間接法の関係はBridging（2023）へ分岐する。非線形への局所的対応はLinear Tracking Part II、Local DPC、Select-DPCを比較する。これらを同一アルゴリズムの単純な改良列とはしない。

**この区分の収録件数：8件。**

<a id="sec-e"></a>
### E. Koopman：作用素近似・入力付き予測・MPC・保証

**役割：観測関数の線形発展を近似し、予測・制御へ使う。** 理論の原点、DMD/EDMD、入力付きモデル、表現学習、閉ループ保証、高速QP、到達集合を区別する。自律系の埋め込み学習と入力付き制御用予測器、モデル自体の安定性と真の制御対象の閉ループ安定性は別問題である。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［作用素理論の原点］** **［基礎追加］** B. O. Koopman。非線形状態の代わりに観測関数の線形作用素を扱う理論的原点。 | 1931 / **[Hamiltonian Systems and Transformation in Hilbert Space](https://doi.org/10.1073/pnas.17.5.315)** | Hamilton系を関数空間上の変換として表現する。 | 観測関数に作用する線形作用素を通して力学を研究する基礎を与える。DMD・EDMD・入力付きMPCのアルゴリズムを提案した論文ではない。 | Koopman理論の歴史と、有限次元データ近似との違いの理解。 | Hamilton系という原設定と現代の一般化を区別する必要がある。MPC実装の最初の教材として必須ではない。 |
| **［DMD］** **［基礎追加］** Schmid。スナップショットから動的モードを求めるDMDの基本論文。 | 2010 / **[Dynamic Mode Decomposition of Numerical and Experimental Data](https://doi.org/10.1017/S0022112010001217)** | 数値流体・実験流れ場の時系列データから支配的な動的構造を抽出する。 | データ間の線形発展を近似して固有値・モードを評価するDMDを提示。Koopman作用素近似への接続と、状態空間全体の厳密線形化は区別する。 | EDMD、DMDc、流体の低次元予測と制御用モデルの準備。 | 観測量とデータ範囲に依存し、非線形系全体の線形モデルや入力応答を自動では同定しない。 |
| **［EDMD］** **［基礎追加］** Williams・Kevrekidis・Rowley。辞書関数を使うEDMDの原典。 | 2015 / **[A Data-Driven Approximation of the Koopman Operator: Extending Dynamic Mode Decomposition](https://arxiv.org/abs/1408.4408)** | 非線形自律系のスナップショット対と観測関数の辞書からKoopman作用素を近似する。 | 辞書空間上の最小二乗近似としてExtended DMDを構成し、固有値・固有関数・モードをデータから求める。 | Korda–Mezićの持ち上げ予測器、Kernel EDMD、Koopman MPC。 | 有限辞書と有限データの近似である。入力付き予測器・真の制御対象の安定性は追加理論を要する。 |
| **［入力付きDMD］** **［基礎追加］** Proctor・Brunton・Kutz。DMDに入力の影響を明示するDMDcの基本。 | 2016 / **[Dynamic Mode Decomposition with Control](https://arxiv.org/abs/1409.6358)** | 入力を受ける高次元系で、自然な状態発展と操作入力の効果をデータから分離する。 | 状態と入力のスナップショットを用いて線形入力状態予測器を同定するDMDcを構成。EDMDの辞書拡張とは別の役割を持つ。 | 入力付きKoopman予測器を理解する準備、データ駆動線形モデルとMPC。 | 十分な励起とモデル表現が必要。DMDcだけで任意の非線形系が大域的に線形化されるわけではない。 |
| **［自律系の表現学習］** 非線形系の埋め込み座標を深層学習 | 2018 / **[Deep Learning for Universal Linear Embeddings of Nonlinear Dynamics](https://doi.org/10.1038/s41467-018-07210-0)** | 非線形振動や流体系のデータで、線形化しやすい潜在座標をオートエンコーダに学習させた。 | 学習した座標上で近似線形の時間発展を表せることを数値例で示し、あらかじめ辞書を手作業で選ぶ必要性を減らした。汎用的な予測誤差改善率は問題ごとに異なる。 | Deep Koopman、潜在状態MPC、非線形システムの低次元表現学習に利用できる。 | 有限データとNN構造に依存し、入力付き制御モデル、安定性、可観測性、物理的意味、分布外状態での予測を自動では保証しない。 |
| **［入力付き予測・MPC］** 持ち上げ線形モデルで非線形MPCを実現 | 2018 / **[Linear Predictors for Nonlinear Dynamical Systems: Koopman Operator Meets Model Predictive Control](https://arxiv.org/abs/1611.03537)** | 非線形振動系や高次元PDEなどに対し、データから持ち上げ線形予測器を構築しMPCに用いた。 | EDMD型の最小二乗同定で線形予測器を構築し、論文の数値例では通常の局所線形化等より良好な予測を報告した。MPCのオンライン最適化は持ち上げ状態の次元に応じた線形QPの構造にできる。 | 高速な非線形予測制御、線形MPCソルバーの再利用、PDEを含むデータ駆動制御に適用できる。 | 持ち上げ辞書と学習範囲によって予測精度が変わり、有限次元近似の誤差が閉ループ安定性・制約違反を引き起こし得る。 |
| **［MPCの閉ループ保証］** Kernel EDMD予測誤差から実用安定性を解析 | 2025 / **[Kernel EDMD for Data-Driven Nonlinear Koopman MPC with Stability Guarantees](https://doi.org/10.1016/j.ifacol.2025.11.080)** | Kernel EDMDで非線形系の予測モデルを同定し、終端安定化制約を課さないMPCの数値シミュレーションを検証した。 | 状態に比例する点ごとのモデル誤差上界を前提に、終端安定化条件を置かずにKoopman MPCの原点への実用的漸近安定性を証明した。数値シミュレーションでも理論を検証したが、具体的な改善率は公開要旨にはない。 | Kernel Koopman MPC、学習モデルの誤差境界を利用した安定性保証に利用できる。 | 点ごとの誤差上界をデータから実証する必要があり、得られるのは原点付近の実用的漸近安定性である。大域的な厳密安定化や任意のモデル誤差には適用できない。 |
| **［高速求解］** Koopmanモデル学習よりもオンライン求解技術に重点を置く分岐。 | 2026 / **[Koopman-BoxQP: Solving Large-Scale NMPC at kHz Rates](https://proceedings.mlr.press/v331/wu26a.html)** | 高次元Koopman予測モデルに由来する構造化NMPCを、低遅延で解く。 | 持ち上げ変数の消去、BoxQP化、Warm Startと構造化内点法を組み合わせ、前回説明では1040変数・2080不等式制約の実験でkHz級の解法を報告。 | 高次元NMPC、高速線形代数、GPU/CPU最適化、Koopman制御。 | BoxQP構造への依存、学習モデルの予測誤差、一般NLPへの移植性。 |
| **［到達集合・統計的被覆］** Koopmanと統計的ロバスト性の接点。 | 2026 / **[Scalable Data-Driven Reachability Analysis and Control via Koopman Operators with Conformal Coverage Guarantees](https://proceedings.mlr.press/v331/nath26a.html)** | 学習したKoopmanモデルを用いて到達可能集合を予測し、近似誤差をデータで補正する。 | Conformal Predictionの誤差境界で到達可能集合を膨張させ、Hopper・Swimmer・クアッドコプター等で評価したと前回説明。被覆率の具体値は未転記。 | 学習ベースの安全なReachability、Koopman MPCのロバスト化。 | 較正データの交換可能性・分布シフトと有限データの制約、到達集合の保守性。 |
| **［モデルの構造・表現学習］** 安定性を構造的に組み込んだKoopman埋め込み | 2026 / **[Learning Stable Koopman Embeddings for Identification and Control](https://doi.org/10.1016/j.automatica.2026.113003)** | 同定データから安定性・安定化可能性を持つKoopman埋め込みを学習し、制御課題を評価した。 | 安定性の構造を持ち上げモデルのパラメータ化に組み込むことにより、単に予測誤差を最小化するだけの設計と異なる制御向け表現を示した。数値的な誤差削減割合は原著要旨からは確認できない。 | 学習モデルに保証を埋め込むKoopman-MPC、構造保存型システム同定に使える。 | モデルの表現力と構造制約の両立が課題で、学習分布外での真の非線形系の安定性が無条件に保証されるわけではない。 |
| **［MPCの閉ループ保証］** Koopman MPCの元の非線形系に関する指数安定性 | 2026 / **[On the Exponential Stability of Koopman Model Predictive Control](https://proceedings.mlr.press/v331/shang26a.html)** | 倒立振子に対してKoopman LQR/MPCとTaylor線形化型MPCを比較し、元の非線形系の安定性を解析した。 | 持ち上げ写像と1ステップ誤差が原点でLipschitzという条件などの下で、局所指数安定性を証明した。倒立振子の数値例では線形化MPCより収束挙動と累積コストが改善したが、要旨に具体的な割合はない。 | Koopman MPCの終端コスト・終端集合設計と閉ループ安定性評価に利用できる。 | 保証は原点近傍の局所結果であり、持ち上げ写像・誤差のLipschitz条件を実データから検証する難しさが残る。 |

**推奨順と理由**：最初はT節のKoopman Tutorialを併読し、DMD→EDMDとDMDcの役割を確認してKorda–Mezićの入力付きMPCへ進む。Koopman（1931）は歴史的参照。Luschの自律系埋め込みは表現学習の枝で、入力付きモデルの学習を別途要する。Kernel EDMD・Stable Embeddings・Shangの安定性を保証対象で比較し、BoxQPは数値解法、Conformal Reachabilityは誤差集合と到達可能性の枝として読む。

**この区分の収録件数：11件。**

<a id="sec-h"></a>
### H. GPモデル・不確実性を使うMPC・学習モデルの実時間実装

**役割：学習した予測モデルと不確実性をMPCへ導入する。** GP-MPCは確率的MPCの一構成であり、確率的MPC全体と同義ではない。L4acadosはGP専用制御方式ではなく、NN等のPythonモデルと感度をacadosへ接続する汎用的な実装基盤である。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［GP予測・不確実性］** GPの確率的予測と分散をMPCへ導入 | 2004 / **[Gaussian Process Model Based Predictive Control](https://mlg.eng.cam.ac.uk/pub/pdf/KocMurRasGir04.pdf)** | 非線形プロセスのGP予測モデルを利用し、将来出力の平均と不確実性をMPCに利用した。 | 物理モデルが不十分でもGPによって非線形入出力関係を表し、モデル不確実性を予測に組み込めることを初期の制御例で示した。改善の百分率は今回の確認範囲で取得できていない。 | 化学・プロセス制御の学習型MPC、未知非線形プラントの確率的予測に適用できる。 | GPの標準的な訓練計算はデータ数に対して概ね3乗で増え、長期分布伝播も重い。分布の正規近似が実系で正しいとは限らない。 |
| **［実機応用］** GP-MPCの実車／レース系への橋渡し。 | 2019 / **[Learning-Based Model Predictive Control for Autonomous Racing](https://arxiv.org/abs/1905.05122)** | 小型レーシングカーを対象に公称力学モデルと学習したモデル誤差を組み合わせ、限界走行でのMPC追従を改善。 | GPを含む学習ベースのモデル補正とMPCを実機レースへ適用し、データ学習と高性能走行を接続した。数値改善率は元回答で確認していない。 | 自動運転、タイヤ特性の変動を含むGP-MPCの実機適用。 | 学習データ範囲外・極限摩擦条件・確率的保証と実時間制約が課題。 |
| **［GP予測・不確実性］** GP残差モデル・共分散伝播・確率制約 | 2020 / **[Cautious Model Predictive Control Using Gaussian Process Regression](https://arxiv.org/abs/1705.10702)** | 小型自動運転レーシングカー等を念頭に、GP残差・状態共分散・確率制約を統合したMPCを評価した。 | 公称力学とGP残差学習を組み合わせ、将来共分散を伝播して確率的制約を引き締めるCautious MPCを提案した。モデル誤差に慎重な挙動を示したが、要旨で汎用的な成功率・改善率は示していない。 | 不確かな摩擦特性を持つ車両、制約付きロボット、確率的な安全余裕設計に応用できる。 | 状態分布のガウス近似、共分散伝播の線形化、GP校正精度を仮定する。共分散まで最適化すると計算量が大きく、真の違反確率保証には追加条件が必要となる。 |
| **［共分散感度の近似］** 共分散関連のSQP感度を近似し計算量削減 | 2023 / **[Zero-Order Optimization for Gaussian Process-Based Model Predictive Control](https://arxiv.org/abs/2211.15522)** | GP共分散を最適化変数に含める従来の確率的MPCと、共分散感度の一部を近似するSQPを比較した。 | Zero-order SQPによって、対象の疎QP求解について状態次元n_xに対する理論的複雑度をO(n_x^6)からO(n_x^3)へ低減できると示し、数値例でスケーリングを検証した。 | 実時間GP-MPC、GP残差を含むChance-Constrained NMPC、SQP-RTIの高速化に使える。 | 計算量低減は特定構造のQPの各反復に関する値であり、GP推論・全システム時間の改善倍率を示さない。微分を近似するため通常のNLP局所最適性は一般に失われる。 |
| **［GP-MPC総説］** GPを用いた動的モデル学習とMPCの総説 | 2025 / **[Gaussian Processes for Dynamics Learning in Model Predictive Control](https://doi.org/10.1016/j.arcontrol.2025.101034)** | GP-MPCの予測平均、共分散、オンライン学習、ロバスト制約、計算効率などを既存研究から整理した総説である。 | GPの平均だけを使うMPCから、共分散を用いる確率的MPCと実時間近似までの技術的課題を体系化した。独自の比較実験に基づく一律の改善率はない。 | GP-MPCの研究課題整理、近似推論とロバストMPCの手法選択に適している。 | モデル誤差がGPの予測分散に十分反映されるとは限らず、オンライン更新時の計算負荷と安全性保証の両立が未解決である。 |
| **［学習モデル接続基盤］** GPモデルとacadosを接続した実時間GP-MPC | 2026 / **[L4acados: Learning-Based Models for acados, Applied to Gaussian Process-Based Predictive Control](https://arxiv.org/abs/2411.19258)** | ニューラルネットワークのモデルをacadosに埋め込むベンチマークと、小型レーシングカー・実車ISOレーンチェンジという2件の実機応用を扱った。 | Pythonで得た外部感度をacadosのSQPへ渡すL4acadosを実装し、既存ソフトより高速・良好な計算量スケーリングを報告した。GP-MPCを小型車およびフルスケール車の2種類のハードウェアで実証したが、全体の改善倍率は要旨で定量化されていない。 | GPyTorch・NNモデルを使うリアルタイムNMPC、acadosベースのモデル誤差補償制御に応用できる。 | 外部感度計算・ソルバー連携・GPU/CPU転送に性能が左右される。実機の2例は汎用的な安全保証を示すものではなく、別対象では計算時間と信頼性の評価が必要である。 |

**推奨順と理由**：T節のStochastic MPCとGP-MPC Tutorial→Cautious GP-MPC→Zero-order近似を基本線とする。2004年のGP-MPCは歴史的な出発点。Kabzanはレース車両への応用、L4acadosは外部モデル・ヤコビアンを実時間SQPへ接続する実装の枝として読む。

**この区分の収録件数：6件。**

<a id="sec-b"></a>
### B. Neural Lyapunov・安定性証明・形式的検証

**役割：制御器や証明関数を学習し、安定性条件を検証する。** SMT、MIP、Branch-and-Boundなどの検証手段を区別する。反例探索が数値サンプリングか形式的な判定かは重要であり、反例が見つからないという事実だけでは一般に証明にならない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［SMT検証］** 制御器とLyapunov関数の同時学習・反例探索 | 2019 / **[Neural Lyapunov Control](https://papers.neurips.cc/paper/8587-neural-lyapunov-control)** | 学習した安定化制御器とLyapunov関数について、学習器とSMTソルバーdRealによる反例探索・形式検証を組み合わせた。 | ニューラル制御則とLyapunov候補を同時に更新し、反例が見つかる領域を修正して検証可能な安定領域を構築した。小規模な非線形系で方法を示したが、認証領域の倍率は今回の確認範囲では取得できなかった。 | ニューラル制御の安定領域推定、強化学習方策の証明付き設計に利用できる。 | 形式検証の計算負担が状態次元とNNサイズで増す。数値探索で反例が見つからないだけでは安定性証明にならず、モデル誤差も別途扱う必要がある。 |
| **［MIP検証］** MIPを用いたNN制御器の安定性認証 | 2021 / **[Lyapunov-stable Neural-network Control](https://roboticsconference.org/2021/program/papers/063/index.html)** | 倒立振子、平面2次元・空間3次元のクアッドコプターを対象にLQRと比較した。 | NN制御器とLyapunov関数をMIPで検証し、対象とした3種類のロボット系で安定領域の内側近似を計算した。選定した条件下ではLQRを上回る制御性能を報告しているが、誤差低減率は要旨からは特定できない。 | 非線形ロボットの証明付き安定化、学習方策とLQRの性能比較に向く。 | MIPによる完全性を狙う検証はNN規模や状態次元の増加で高コストになる。保証は指定した力学・状態領域・モデル精度に限定される。 |
| **［NN制御・安定性検証］** 離散時間系の学習と反例誘導型検証 | 2023 / **[Neural Lyapunov Control for Discrete-Time Systems](https://proceedings.neurips.cc/paper_files/paper/2023/file/08bf1773e94763b6cc366ee7c6582f27-Paper-Conference.pdf)** | 離散時間非線形系で、Lyapunov関数と制御器の学習・反例に基づく検証を扱う。 | 連続時間を前提とする議論を離散時間の状態遷移に合わせ、Lyapunov減少条件に関する検証・学習の仕組みを示した。原著の具体的な改善率は今回の確認範囲では取得できていない。 | サンプル値制御、デジタル制御器、離散時間NN方策の安定性検証に役立つ。 | 離散時間の差分不等式を広い状態集合で保証するには高コストな検証が必要であり、外乱・モデル誤差への拡張が残る。 |
| **［NN制御・安定性検証］** 状態・出力フィードバックの保証付き学習 | 2024 / **[Lyapunov-stable Neural Control for State and Output Feedback: A Novel Formulation](https://proceedings.mlr.press/v235/yang24f.html)** | NN状態フィードバックだけでなく、NN観測器を含む出力フィードバック系を対象にした。 | 従来より大きな認証可能安定領域を持つ定式化を提案し、GPU上でBranch-and-Boundによる事後検証を実行した。著者らは出力フィードバックのNN制御器・観測器の安定性を形式的に示したが、要旨中に領域拡大倍率はない。 | センサだけから制御するNN出力フィードバック系、観測器と制御器の一体学習に応用できる。 | 認証は指定した領域・近似モデルに限定され、GPU利用でも高次元系の大域的な認証は難しい。定量的な改善倍率は本表では確認できていない。 |
| **［遅延結合系の証明］** Neural Lyapunovの分散・時間遅れ方向への分岐。 | 2026 / **[Neural Vector Lyapunov–Razumikhin Certificates for Delayed Interconnected Systems](https://proceedings.mlr.press/v306/zhou26au.html)** | 複数サブシステムが遅延を伴って相互結合する系の安定性検証。 | Vector Lyapunov関数とRazumikhin型遅延条件をNN証明関数で扱い、車両隊列・マルチエージェント・電力系などで解析可能性を示したと前回説明。個別結果の再照合が必要。 | 通信遅延を持つ分散制御、ネットワーク化システム。 | 結合条件の保守性、遅延範囲の仮定、スケーラビリティ。 |
| **［NN制御・安定性検証］** 検証境界を損失関数に組み込む認証型学習 | 2026 / **[Certified Training with Branch-and-Bound for Lyapunov-stable Neural Control](https://proceedings.mlr.press/v331/shi26a.html)** | 状態・出力フィードバックを含むNN制御器の認証可能な安定領域を、従来のCEGIS方式と比較した。 | 最大規模の実験として挙げた2Dクアッドローター出力フィードバック系で、従来比11倍超の検証高速化と164倍の認証可能ROA（引込み領域）を報告した。 | 形式検証を含むNN制御器の訓練、広い安定領域を必要とする制御設計に応用できる。 | 11倍超・164倍は論文中の特定の2D系での比較であり、他の次元・ネットワークでも達成されるとは限らない。分枝分割の組合せ的な計算負担は残る。 |

**推奨順と理由**：Changの学習器とSMT検証器→DaiのMIP検証を比較する。どちらも保証を目指す枠組みであり、「Changは探索だけ、Daiで初めて証明」とは解釈しない。離散時間、出力フィードバック、CT-BaBへ分岐する。遅延結合系のVector Lyapunov–Razumikhinは追加のモデル・証明条件を持つ別枝である。

**この区分の収録件数：6件。**

<a id="sec-c"></a>
### C. Contraction理論・CCM制御器合成・収縮性の形式検証

**役割：軌道間の収束・追従を解析し、必要に応じて制御器を合成する。** 収縮解析、Control Contraction Metricによる制御器合成、与えられた閉ループ系のMetric認証を区別する。すべてのNeural Contraction Metric論文が制御器も同時に設計するわけではない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［収縮解析］** **［基礎追加］** Lohmiller・Slotine。CCMより前に読む収縮解析の基礎。 | 1998 / **[On Contraction Analysis for Non-linear Systems](https://doi.org/10.1016/S0005-1098(98)00019-3)** | 非線形力学における微小変位と軌道間距離の収束を解析する。 | 変分系と状態・時間依存の計量を使い、軌道間の指数収縮を評価する条件を示す。平衡点周りのLyapunov解析との関係を理解できる。 | CCM、非線形追従制御、収縮に基づく推定・学習モデル解析。 | これはまず解析の枠組みであり、制御器合成にはCCM等の追加構成が必要。計量の探索も別問題。 |
| **［CCM・制御器合成］** 非線形系の微分収縮性に基づくフィードバック合成 | 2017 / **[Control Contraction Metrics: Convex and Intrinsic Criteria for Nonlinear Feedback Design](https://arxiv.org/abs/1503.03144)** | 連続時間の非線形制御アフィン系について、収縮計量を使ったフィードバック安定化を理論解析した。 | 状態依存の正定値Metricが満たす凸条件から、参照軌道間の指数収縮を保証する制御則を導出した。実験の改善率ではなく、収縮率を示す指数境界が主要成果である。 | 非線形追従制御、ロバスト制御、軌道依存安定性の証明に使える。 | Metricの探索が難しく、高次元系では条件評価が重い。原論文の定理の一部には後続の技術的条件の補足があるため訂正論文との併読が必要である。 |
| **［収縮解析］** 収縮理論の安定性・ロバスト性・学習制御への総説 | 2021 / **[Contraction Theory for Nonlinear Stability Analysis and Learning-based Control: A Tutorial Overview](https://doi.org/10.1016/j.arcontrol.2021.10.001)** | Contraction理論による非線形安定性・ロバスト制御・推定・学習応用の研究を横断的に整理した総説である。 | 収縮率、入力外乱に対する誤差上界、制御・推定の設計条件を統一的に説明している。新規ベンチマークや性能改善率を報告する実験論文ではない。 | CCM研究の理論整理、神経収縮計量やロバスト追従の先行研究探索に有用である。 | 総説自体は新しい制御器の実機性能を検証していない。扱う理論保証は正定値計量などの存在・検証可能性に依存する。 |
| **［CCM・制御器合成］** Contraction Metricと制御方策の同時学習（C3M） | 2021 / **[Learning Certified Control Using Contraction Metric](https://proceedings.mlr.press/v155/sun21b.html)** | 学習済み非線形力学を含むロボットモデルで、SOS、RL、MPCとの比較を行った。 | 制御器と収縮MetricをNNで同時学習し、対象実験ではSOS・RL・MPC系比較手法より小さい追従誤差と速い実行を報告した。改善倍率は要旨で明記されていない。 | 非線形軌道追従、ロボットの有界外乱下での追従誤差保証に適用できる。 | 保証は制御アフィン系などのモデル仮定とMetric条件の検証に依存する。高次元での全領域検証、モデルバイアスへの耐性はなお課題である。 |
| **［離散時間Metricの検証］** 離散時間の学習Metricを形式的に検証 | 2025 / **[Neural Contraction Metrics with Formal Guarantees for Discrete-Time Nonlinear Dynamical Systems](https://proceedings.mlr.press/v283/li25e.html)** | Van der Pol系、多項式系、二機電力系、倒立振子でNN Contraction Metricを構築・検証した。 | Van der Polでは正規化検証領域の100%（一定Metric比較33.7%）を109秒で認証し、二機電力系では100%（一定Metric31.3%）を62秒で認証した。倒立振子では88.6%を認証したが、検証に5.8時間を要した。 | 連続微分できないNNを含む離散時間系の収縮性証明、安定領域の拡大に利用できる。 | 対象例によって検証時間が62秒から5.8時間へ大きく変動するため、高次元化・領域拡大に対する計算負荷が重い。証明は設定した検証領域の内部に限られる。 |
| **［収縮性とRL］** Contraction保証とRLによる性能改善の統合 | 2026 / **[Contraction-Aware Reinforcement Learning for Nonlinear Control with Statistical Robustness](https://doi.org/10.1109/TRO.2026.3727138)** | シミュレーションと実ロボットの経路追従で、近似ダイナミクスを使ったCCM制御とRLの融合を比較した。 | 収縮性を利用した方策学習CARLを提案し、比較方式より追従性能・モデル誤差への頑健性が良好と報告した。改善百分率は公開要旨で確認できない。 | モデル誤差が残るロボットの追従制御や安定性を意識したRLへ利用できる。 | 理論保証はCCMを構成する近似モデルと仮定に依存し、報酬最適化自体が任意の未知外乱に対する安全保証を与えるわけではない。 |

**推奨順と理由**：Lohmiller–Slotine（1998）→2021年のContraction総説で解析を学び、制御器合成はManchester–SlotineのCCM→C3Mへ進む。Liら（2025）は離散時間系のMetric学習・形式検証、CARLはRLとの統合という並列の枝として読む。

**この区分の収録件数：6件。**

<a id="sec-f"></a>
### F. 安全制御：CBF・予測安全フィルタ・HJ到達可能性

**役割：制約違反や危険集合への侵入を防ぐ。** CBFによる前方不変性、予測とバックアップによる安全フィルタ、HJ到達可能性、高ゲイン保護フィードバックは並行する構成である。安定性・追従保証とは目的が異なり、必要ならB/C節と組み合わせる。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［HJ・到達可能性］** **［基礎追加］** Mitchell・Bayen・Tomlin。HJ到達可能性に基づく安全解析の基礎。 | 2005 / **[A Time-Dependent Hamilton–Jacobi Formulation of Reachable Sets for Continuous Dynamic Games](https://www.cs.ubc.ca/~mitchell/Papers/publishedIEEEtac05.pdf)** | 入力と最悪外乱を持つ非線形系で、危険集合へ到達し得る状態を求める。 | 後方到達可能集合を時間依存HJI方程式の粘性解の零劣位集合として表現し、数値解法と衝突回避例を示す。 | Reachability型安全フィルタ、Deep QP Safety Filter、CBFとの比較。 | 格子に基づく計算は次元の増加に厳しい。数値離散化誤差とモデル妥当性も評価が必要。 |
| **［CBF］** CBFの安全条件をQP制約へ組み込む | 2017 / **[Control Barrier Function Based Quadratic Programs for Safety Critical Systems](https://doi.org/10.1109/TAC.2016.2638961)** | ロボット・制約付き非線形系で、既存の目標追従制御入力を安全条件に従って修正するQPを扱った。 | CBFをQP制約に組み込むことで、適切な可行性・正則性条件下で安全集合の前方不変性を保証できる枠組みを与えた。制御誤差や実行速度の改善割合は主要成果ではない。 | 衝突回避、安全フィルタ、入力制限を持つ安全重要制御へ利用できる。 | CBF条件の可行性が入力飽和や複数障害物で失われる可能性がある。モデル誤差、測定誤差、高次相対次数への対応が必要である。 |
| **［予測安全フィルタ］** 学習器の入力を予測軌道による安全制約で修正 | 2021 / **[A Predictive Safety Filter for Learning-Based Control of Constrained Nonlinear Dynamical Systems](https://doi.org/10.1016/j.automatica.2021.109597)** | 非線形システムで学習方策が提案する制御入力を、予測制約と終端安全性に照らして修正した。 | 有限ホライズンの安全計画で、元の学習方策からの入力偏差を小さくしながら安全制約を維持する仕組みを示した。実験の成功率や改善倍率は公開要旨に含まれていない。 | 学習済みRL方策に後付けする安全層、モデル予測型の安全入力修正に適用できる。 | 安全なバックアップ集合・終端条件とモデル妥当性を必要とする。計算時間や未知外乱が大きい場合、予測フィルタ自体の可行性が課題になる。 |
| **［安全フィルタ総説］** CBF/HJ/MPC安全フィルタを比較・整理 | 2024 / **[The Safety Filter: A Unified View of Safety-Critical Control in Autonomous Systems](https://doi.org/10.1146/annurev-control-071723-102940)** | CBF、Reachability、予測型安全フィルタを統一的に比較した総説であり、独自の新規実験は目的としていない。 | 安全集合、入力修正、到達可能性、予測時間という観点で異なる安全フィルタの前提と保証を整理した。比較実験に基づく一つの改善率を報告する論文ではない。 | 制御対象の危険状態・モデル精度・計算予算に応じたSafety Filter設計に使える。 | 統一的な概念整理にとどまり、安全が成立するためのモデル妥当性・集合の非空性・リアルタイム性は個別方式で検証が必要である。 |
| **［保護フィードバック］** 保証がない学習予測器を制御に導入する実践的な例。 | 2025 / **[Two-Component Controller Design to Safeguard Data-Driven Predictive Control](https://arxiv.org/abs/2505.19131)** | 出力制約を守る必要がある系で、DeePC/EDMD-MPCとモデルフリー高ゲインフィードバックを組み合わせる。 | 学習モデルが不十分なときに安全側フィードバックで介入する構成を提案し、設定値移行の数値例で出力拘束の扱いを実演。数値改善率は未確認。 | モデル学習の初期段階の安全化、DeePCと既知フィードバックの併用。 | 相対次数等の仮定、高ゲイン制御の騒音増幅、モデルベース予測器との切替え。 |
| **［HJ・到達可能性］** HJ到達可能性とモデルフリー価値学習の融合 | 2026 / **[Deep QP Safety Filter: Model-free Learning for Reachability-based Safety Filter](https://proceedings.mlr.press/v331/kim26c.html)** | ハイブリッドシステムを含む複数の動的システムとRLタスクで、学習型HJ価値関数に基づく安全QPを評価した。 | モデル知識なしで価値関数とその導関数を学習し、比較方式より学習収束前の失敗を減らしつつ最終報酬を高める傾向を示した。要旨では削減率や報酬値の具体値を公表していない。 | 未知ダイナミクスの安全学習、モデルフリーRLに付加するQP型安全フィルタに適用できる。 | 価値関数の近似誤差や分布外状態での安全性保証は非自明である。理論の厳密な収束は指定された極限条件に依存し、有限データ実機での無条件保証ではない。 |
| **［CBF］** 潜在状態上で滑らかなCBF安全フィルタを学習 | 2026 / **[How to Train Your Latent Control Barrier Function: Smooth Safety Filtering Under Hard-to-Model Constraints](https://proceedings.mlr.press/v331/nakamura26a.html)** | シミュレーションと画像ベースの実機マニピュレーションで、通常方策と安全方策を切り替える方式と比較した。 | 滑らかな潜在CBFを学習することで、従来のスイッチング型Safety Filterと比べて、対象実験の成功率を約2倍にしたと報告している。 | 画像観測からのロボット操作、安全な潜在状態フィルタ、スムーズな制御介入に利用できる。 | 成功率2倍は論文中の比較課題に依存し、画像の分布変化や潜在状態の不完全性の下で一般的な安全性が保証されるわけではない。 |

**推奨順と理由**：統一総説で共通構造を見た後、CBFの枝はAmes→2024年理論総説→学習CBF/Latent CBF、予測の枝はWabersich–Zeilinger、到達可能性の枝はMitchell–Bayen–Tomlin→Deep QPへ分岐する。Two-Component Controllerは高ゲイン保護則の枝であり、CBF方式と同一ではない。

**この区分の収録件数：7件。**

<a id="sec-g"></a>
### G. SLS・閉ループ応答設計・分散／ロバストMPC

**役割：外乱から状態・入力への閉ループ応答を直接設計する。** 分散・局所性の制約、ロバストMPCへの展開、専用数値解法、統計的な誤差集合の推定は同じ枠組みの異なる論点である。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［SLS・応答設計］** 制御器ではなく閉ループ応答を直接設計する枠組み | 2016 / **[A System Level Approach to Controller Synthesis](https://arxiv.org/abs/1610.04815)** | 線形システムの閉ループ応答を直接最適化変数とする制御器合成を理論的に定式化した。 | 外乱から状態・入力への応答をアフィン等式で表し、内部安定性を満たす制御器を応答上の凸制約で扱えることを示した。主要成果は数値的な改善率ではなくパラメータ化の等価性である。 | 分散制御、通信制約付き制御、大規模ロバスト設計に適用できる。 | 理論の基本形は線形系を前提とする。応答の有限ホライズン近似や複雑な通信制約では問題サイズと実装負荷が増大する。 |
| **［SLS・応答設計］** SLSの安定化、局所性、分散制御、ロバスト性を体系化 | 2019 / **[System Level Synthesis](https://arxiv.org/abs/1904.01634)** | SLSによる集中・分散制御や局所制約を、理論と複数の設計例を通して体系化した。 | 閉ループ応答の自由度を直接制約することで、局所性・情報制約を表現する統一的設計枠組みをまとめた。総説として数値的な性能向上を新たに一律で実証したわけではない。 | ネットワーク化制御、車両隊列、分散MPC、ロバスト設計の学習基盤となる。 | 応答の近似次数と系統規模が計算負荷に影響し、強非線形・ハイブリッド力学には直接適用できない。 |
| **［専用数値解法］** Riccati再帰によりSLS-MPCのQP構造を高速処理 | 2024 / **[Fast System Level Synthesis: Robust Model Predictive Control using Riccati Recursions](https://doi.org/10.1016/j.ifacol.2024.09.027)** | 有限ホライズンのSLS型ロバストMPCについて、Riccati再帰を使う数値最適化アルゴリズムを扱った。 | 問題構造を利用した再帰計算で、汎用的な密行列QPより効率的にロバストMPCを解く方法を提示した。論文の具体的な計算時間比は今回の確認範囲で未検証。 | 高周期ロバストMPC、組込み向けSLSソルバーの設計に利用できる。 | 高速化はブロック構造などの特定の定式化に依存し、一般の非凸NLPにそのまま拡張できるわけではない。 |
| **［ロバストMPC］** 多面体モデル不確かさを含むSLSロバストMPC | 2024 / **[Robust Model Predictive Control with Polytopic Model Uncertainty through System Level Synthesis](https://doi.org/10.1016/j.automatica.2023.111431)** | 行列A,Bがポリトープ状に変化する不確かさと加法外乱を持つ線形制約系で、SLSロバストMPCを評価した。 | 閉ループ外乱応答を用いた制約の引き締めによって、不確かなモデルに対して制約充足を設計できることを示した。定量的な比較倍率は公開情報から検証できなかった。 | モデル誤差が集合で評価できる多変数・分散系のロバストMPCに利用できる。 | 不確かさ集合を正しく指定する必要があり、ポリトープ頂点や予測ホライズンが増えると計算負荷と保守性が増す。 |
| **［統計的誤差集合］** Conformal Predictionによる誤差境界とSLSの融合 | 2026 / **[Safety Beyond the Training Data: Robust Out-of-Distribution MPC via Conformalized System Level Synthesis](https://proceedings.mlr.press/v331/srinivasan26a.html)** | 状態4次元の車両モデルと12次元のクアッドコプターで、学習分布外のモデル誤差を考慮したSLS-MPCを評価した。 | 重み付きConformal Predictionからモデル誤差集合を学び、SLSによる到達集合と制約引き締めへ反映した。対象の4D・12D非線形系では、固定誤差境界・ロバスト化なしの比較法より安全性・信頼性が改善したが、要旨に改善率はない。 | 学習済み動力学を使うロバストNMPC、データ密度に応じた不確実性境界の設計に利用できる。 | 被覆保証は較正データ、分布シフト仮定、誤差モデルの妥当性に依存する。いかなる未知環境でも無条件に安全となるわけではない。 |

**推奨順と理由**：System Level Approach→System Level Synthesisで応答のパラメータ化を学ぶ。以後はPolytopic Robust MPC（不確かさ）、Riccati Recursions（数値計算）、Conformalized SLS（統計的較正）の枝に分ける。Conformalの被覆率と決定論的な全外乱保証を区別する。

**この区分の収録件数：5件。**

<a id="sec-r"></a>
### R. ロボット応用：接触計画・四脚MPC・状態推定

**役割：最適制御・推定をロボットへ適用するための応用別入口。** Posaの接触モードを含む軌道計画、Di Carloの接触力MPC、Forsterの視覚慣性推定は別の構成要素であり、一本の後継系列ではない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［導入・導出］** Contact-implicit最適化の原典。 | 2014 / **[A Direct Method for Trajectory Optimization of Rigid Bodies Through Contact](https://doi.org/10.1177/0278364913506757)** | 接触位置や接触時刻が事前に不明な剛体系の軌道最適化。 | 相補性条件を含む直接法（contact-implicit trajectory optimization）により、接触モードをあらかじめ指定せずに接触を含む動作軌道を求める構成を示した。全タスクの最適値改善率は未確認。 | 脚ロボット、押し操作、接触計画、最適制御と接触力学の統合。 | 非凸性・相補性制約の退化・局所解依存・大規模NLPの計算量が課題。 |
| **［導入・導出］** 幾何学的推定と因子グラフ最適化の代表論文。 | 2017 / **[On-Manifold Preintegration for Real-Time Visual–Inertial Odometry](https://arxiv.org/abs/1512.02363)** | 高速IMU計測をキーフレーム間の制約へ集約し、姿勢をSO(3)上で推定する視覚慣性オドメトリ。 | 多量のIMU計測を再積分せずバイアス更新に対応するPreintegrationを定式化し、実時間のVIO最適化で効果を示した。推定誤差の統一改善率は未確認。 | ロボット状態推定、VIO/SLAM、MPCへ供給する状態の高精度化。 | 観測可能性、センサ較正、時刻同期、強いバイアス変化、低テクスチャ環境に影響を受ける。 |
| **［導入・導出］** 四脚凸MPCの代表的な実機研究。 | 2018 / **[Dynamic Locomotion in the MIT Cheetah 3 Through Convex Model-Predictive Control](https://doi.org/10.1109/IROS.2018.8594448)** | 四脚ロボットの胴体運動と接触力を、接地制約・摩擦制約の下でオンライン計画する。 | 線形化した重心運動モデルと凸QPを用い、動的な四脚歩行・旋回・外乱下動作をMIT Cheetah 3で実証した。論文内の課題依存の数値を抜き出した共通改善率は未確認。 | Quadruped MPC、Contact-force optimization、MuJoCoでの四脚制御教材。 | 重心モデルは脚の慣性や接触切替えを簡略化する。歩容生成・状態推定・低位追従器の品質に依存する。 |

**推奨順と理由**：四脚MPCはK節のMPC/QPの基本を確認してDi Carloを読む。接触時刻・モード自体を最適化したい場合はPosaへ、状態をセンサから推定する部分はForsterへ分岐する。MPPIはP節、RL・模倣学習はU節で独立に学ぶ。

**この区分の収録件数：3件。**

<a id="sec-u"></a>
### U. 学習方策：モデルフリーRL・モデルベースRL・模倣学習

**役割：報酬または実演データから制御方策を学ぶ。** TD価値学習、決定論的／確率的方策勾配、潜在モデルに基づく計画、拡散モデルによる模倣学習を分ける。PPO→TD-MPC2→Diffusion Policyという直列の発展関係は置かない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［モデルベースRL］** **［基礎追加］** Sutton。TD学習とブートストラップ型価値予測の基礎。 | 1988 / **[Learning to Predict by the Methods of Temporal Differences](https://link.springer.com/article/10.1007/BF00115009)** | 系列の最終結果を待たず、隣接時刻の予測の差から予測器を更新する。 | Temporal Difference学習を定式化し、特定条件での収束・最適性を解析。価値推定を報酬と次状態の推定値から更新する考え方を学ぶ。 | Actor-Critic、学習終端価値、TD-MPCの基礎。 | 基本的な予測学習の理論であり、非線形NN・off-policy学習・制約付き制御の一般保証ではない。 |
| **［モデルフリーRL・方策勾配］** **［基礎追加］** Silverら。連続入力Actor-Criticの基礎となる決定論的方策勾配。 | 2014 / **[Deterministic Policy Gradient Algorithms](https://proceedings.mlr.press/v32/silver14.html)** | 連続行動空間で決定論的な目標方策を、探索用方策が収集したデータから学ぶ。 | 行動価値の入力勾配を用いる方策勾配を導出し、off-policy Actor-Criticを構成。PPOの確率的方策更新とは並行する枝。 | DDPGなどの連続制御RL、価値勾配と最適制御の接続。 | 価値近似誤差、探索、分布外の行動評価が課題。安定性や状態制約は自動で保証されない。 |
| **［モデルフリーRL・方策勾配］** **［基礎追加］** Schulmanら。PPOの背景となる方策更新幅の制御。 | 2015 / **[Trust Region Policy Optimization](https://proceedings.mlr.press/v37/schulman15.html)** | NN方策を更新するときに、方策分布の変化を抑えつつ期待報酬を改善する。 | 理論的な改善境界とKL制約を背景に実用的な信頼領域型方策最適化を構成。PPOのclippingと厳密なKL制約の違いを理解する基礎。 | PPO、ロボットの確率的方策最適化。 | 実用アルゴリズムには理論からの近似がある。報酬改善の議論は実機の安全性・閉ループ安定性保証とは異なる。 |
| **［モデルフリーRL・方策勾配］** モデルベース制御と対比するRLの基本論文。 | 2017 / **[Proximal Policy Optimization Algorithms](https://arxiv.org/abs/1707.06347)** | モデルを明示しない方策最適化で、更新幅が大き過ぎることによる学習不安定を抑える。 | Clipped surrogate objective等で方策更新を制限し、連続・離散行動ベンチマークで有効性を示した。対象環境全体に共通する改善倍率は未確認。 | ロボットのモデルフリーRL、MPCとRLの比較ベースライン。 | オンラインの実機探索では試行コストが大きく、制約充足と安定性はそのままでは保証しない。 |
| **［モデルベースRL］** **［基礎追加］** Hansenら。TD-MPC2の直接の前身となるTD-MPC。 | 2022 / **[Temporal Difference Learning for Model Predictive Control](https://proceedings.mlr.press/v162/hansen22a.html)** | 状態・画像ベースの連続制御で、短期計画と長期価値評価を共同学習する。 | タスク指向の潜在ダイナミクスと終端価値をTD学習し、潜在空間の短ホライズン軌道最適化で入力を決める。 | TD-MPC2、World Model型MPC、学習終端コストの理解。 | 潜在モデルの誤差と価値外挿が計画に影響する。形式的な制約・安定性保証は別途必要。 |
| **［模倣学習・生成方策］** 最適化ベース制御と対比する生成方策の代表論文。 | 2023 / **[Diffusion Policy: Visuomotor Policy Learning via Action Diffusion](https://arxiv.org/abs/2303.04137)** | 画像・ロボット状態から多峰性の行動系列を生成する模倣学習。 | Diffusionによって行動系列の分布を学習し、複数のロボット操作タスクで優れた模倣性能を実証。成功率などの具体値は原論文で再確認が必要。 | 視覚ベースのマニピュレーション、MPC以外の軌道生成方策との比較。 | 推論計算量、データ分布外動作、形式的な安全性・安定性保証は課題。 |
| **［モデルベースRL］** MPCとRLの最新系列を理解するための代表例。 | 2024 / **[TD-MPC2: Scalable, Robust World Models for Continuous Control](https://arxiv.org/abs/2310.16828)** | 複数の連続制御タスクで、潜在World ModelとMPCを共同利用して性能・汎化を評価。 | 潜在状態の短期予測と価値推定を使うサンプリング計画を統合し、大規模・多タスクで高性能なモデルベースRLを実証。代表的な点数の正確な引用は未再確認。 | ロボットのWorld Model型MPC、データ効率の高い連続制御。 | 学習データの範囲、計画の安全保証、実機分布シフト、訓練コストが課題。 |

**推奨順と理由**：価値推定はSutton（1988）から読む。確率的方策更新はTRPO→PPO、決定論的Actor-CriticはSilverらのDPGへ分岐。潜在モデルと終端価値による計画はTD-MPC（2022）→TD-MPC2（2024）。Diffusion Policyは実演データに基づく模倣学習の別枝であり、RLの必須後継ではない。

**この区分の収録件数：7件。**

<a id="sec-i"></a>
### I. Neural Operator・PDE予測モデル・PDE制御方策

**役割：分布定数系の予測または制御を関数間の写像として学ぶ。** FNO/DeepONet/PINOの予測作用素と、CINOCの状態場から制御場への作用素方策は学習対象が異なる。PDE-DPCは予測モデルを介して方策を学ぶ構成で、A節の最適解感度型MPCとは区別する。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［予測作用素］** Fourier空間でPDEの作用素を学習 | 2021 / **[Fourier Neural Operator for Parametric Partial Differential Equations](https://arxiv.org/abs/2010.08895)** | Burgers方程式、Darcy流、Navier–Stokes方程式の数値解を学習し、高解像度の予測を比較した。 | Fourier Neural Operatorは固定解像度の比較で既存の学習法より高精度と報告され、乱流予測のゼロショット高解像度化を実証した。特定のベンチマークでは従来PDEソルバーより最大約10^3倍高速だった。 | 流体・熱・拡散PDEの高速代理モデル、シミュレーション高速化、分布定数系のMPCに適用できる。 | 最大10^3倍は特定実装と解像度での推論比較であり、学習時間は含まない。分布外パラメータ、長時間積分、保存則や誤差保証は別途検討が必要である。 |
| **［予測作用素］** 入力関数から出力関数の非線形作用素を学習 | 2021 / **[Learning Nonlinear Operators via DeepONet Based on the Universal Approximation Theorem of Operators](https://doi.org/10.1038/s42256-021-00302-5)** | 多様な入力関数からPDE解を予測する例について、BranchネットとTrunkネットによる作用素近似を評価した。 | DeepONetが関数空間間の非線形写像を学習できることを複数のPDE・作用素ベンチマークで示した。原著の多様な課題を横断する単一の改善率はない。 | 熱・流体・構造解析の代理モデル、逆問題、PDE制御器の学習に応用できる。 | 測定点配置と訓練用関数の分布に強く依存する。高周波現象、未観測条件、長期安定性は別途保証が必要である。 |
| **［物理残差を使う予測作用素］** 物理法則の残差をNeural Operator学習に統合 | 2021 / **[Physics-Informed Neural Operator for Learning Partial Differential Equations](https://arxiv.org/abs/2111.03794)** | 粗い空間解像度のデータと、より細かい解像度のPDE残差を組み合わせ、流体等を予測した。 | Physics-Informed Neural Operatorは、粗い教師データだけで学ぶFNOに物理制約を加え、訓練解像度より細かなゼロショット予測でも精度の劣化を抑えた。Kolmogorov流など、通常のPINNで最適化が難しい設定への適用も示した。 | 物理制約付き代理モデル、限られた測定データを用いるPDE同定と予測制御に使える。 | 物理残差を高解像度で評価する学習コストが高く、PDE自体が未知・誤差を持つ場合は制約が逆効果になり得る。制御系の安定性は直接示していない。 |
| **［予測器を通したDPC方策学習］** PDEモデルの微分可能予測に基づいて方策学習 | 2025 / **[Learning to Control PDEs with Differentiable Predictive Control and Time-Integrated Neural Operators](https://arxiv.org/abs/2511.08992)** | 熱伝導、非粘性Burgers、反応拡散の3種のPDEで追従・制約・曲率低減を学習制御器で試した。 | TI-DeepONetを通じたDifferentiable Predictive Controlでオフラインに方策を学習し、推論時の制御入力生成をNMPCベンチマークより約10^4〜10^5倍高速化したと著者らは報告した。制御対象と比較実装を固定した実験値である。 | 分布定数系の高速な明示的NN制御、熱・拡散・流体系でのオンライン最適化の代替に利用できる。 | オンライン速度は極めて速いがオフライン学習費用を含まず、予測器のモデル誤差と安全制約の一般的保証は残る。数値倍率は論文の3種のPDE設定に依存する。 |
| **［作用素方策］** センサ・アクチュエータ数に依らない作用素方策 | 2026 / **[CINOC: Cardinality-Invariant Neural Operator Policies for Scalable PDE Control](https://proceedings.mlr.press/v306/zanotta26a.html)** | 線形・非線形・カオス・乱流PDEの追従、安定化、密度輸送で、センサ数・エージェント数を変えて評価した。 | Neural Operatorで状態場から制御場への方策を学習し、小規模群で訓練した同じパラメータを大規模群へ再学習なしで転移させた。部分的なエージェント故障に耐性がある例も示したが、群規模の具体倍率は公開要旨にない。 | センサ・アクチュエータの配置が変化するPDE制御、マルチエージェント協調制御に適用できる。 | ゼロショット転移は論文で扱った作用素・環境・群規模での実証であり、任意の密度や物理相互作用への適用を保証しない。訓練時の微分可能PDEソルバーも必要である。 |

**推奨順と理由**：FNOとDeepONetは予測モデル構造の比較、PINOは物理残差を利用する追加の枝。制御方策はTI-DeepONetを予測器として使うPDE-DPCと、微分可能PDEソルバーで作用素方策を訓練するCINOCに分かれる。PINO→PDE-DPC→CINOCという必須の直列関係は置かない。

**この区分の収録件数：5件。**

<a id="sec-j"></a>
### J. In-Context Learning：システム同定・力学予測・World Model適応

**役割：事前学習したモデルを、未知対象の履歴を文脈として使って適応させる。** 入出力同定、線形系に関するTransformerの表現力、World Modelの環境認識・学習は並行する研究課題である。予測への適応が制御系の安全性・安定性を自動で保証するわけではない。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［入出力のメタ同定］** 系集合の事前学習を未知システムの予測に活用 | 2023 / **[From System Models to Class Models: An In-Context Learning Paradigm](https://arxiv.org/abs/2308.13380)** | 異なるパラメータを持つ多数の合成動的システムで、1ステップ予測と多段シミュレーションを評価した。 | Transformerを合成システム群で事前学習し、新しい系の入出力履歴だけからモデルの再学習なしに予測可能であることを示した。既存同定法に対する普遍的な誤差改善率は要旨では確認できない。 | 対象ごとに再同定を行わないメタ同定、オンライン予測器、少量データでの適応に利用できる。 | 事前学習時のシステム分布・Context長によって性能が変動し、真に未知な物理構造や分布外対象への安定した汎化は未解決である。 |
| **［表現力の理論］** ICL同定におけるTransformerの表現力と深さを理論解析 | 2025 / **[In-Context Learning of Linear Dynamical Systems with Transformers: Approximation Bounds and Depth-Separation](https://proceedings.neurips.cc/paper_files/paper/2025/file/f7ae4fe91d96f50abc2211f09b6a7e49-Paper-Conference.pdf)** | 線形状態空間モデルの集合を対象に、Transformerの層数とIn-Context同定能力を理論解析した。 | 適切な構成のTransformerによって線形力学の同定手続きを近似できる条件を示し、ネットワーク深さによって表現可能な処理が変わることを理論化した。主要成果は近似境界・深さ分離の定理であり、実機改善率ではない。 | メタシステム同定の理論設計、Transformer予測器の容量・深さ選択に活用できる。 | 主な理論対象は線形系であり、非線形・未観測状態・長期閉ループ安定性へ直接保証を拡張できない。 |
| **［World Model適応］** 文脈長・環境多様性が未知系への適応に与える影響 | 2026 / **[Context and Diversity Matter: The Emergence of In-Context Learning in World Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/a46b9dcef23e9160f46d800a49a931d0-Abstract-Conference.html)** | 多様な環境のWorld Modelに対し、Contextの長さと訓練環境の多様性を変えて適応能力を評価した。 | 長い観測履歴と多様な学習環境が、未知環境のダイナミクスを文脈内で推定する能力の獲得に重要であると示した。比較改善率は確認した要旨からは取得できていない。 | 汎用World Model、未知動特性への文脈内適応、モデルベースRLの事前学習に有用である。 | 環境の同定と新しい力学則を学習する能力は異なり、訓練分布から大きく外れる場合の性能は不確実である。制御系の保証は扱わない。 |
| **［入出力のメタ同定］** 確率予測・長い履歴・ベンチマーク評価 | 2026 / **[In-Context Learning for System Identification: Probabilistic Formulation, Long-Sequence Handling, and Benchmark Validation](https://arxiv.org/abs/2610.13411)** | 標準的な非線形同定ベンチマークと、分布外条件を含む新規システムの予測を評価した。 | 確率的Transformerメタ同定、長系列入力、少量実測データへの合成データ補完を統合し、比較法に競争力のある予測精度を示した。特定の分布シフトでは入力列のみの適応でも良好な汎化を報告しているが、定量誤差比は要旨にない。 | モデル同定の初期化、データ不足の装置、未知系への即応型予測器に応用できる。 | 2026年10月公開のプレプリントであり、事前学習分布への依存、長系列計算負荷、不確実性の校正、閉ループ制御への保証が残る。 |

**推奨順と理由**：Class Models→確率的ICL System Identificationは入出力予測の基本線。Coleらの線形系理論は表現力解析、Context and DiversityはWorld Modelの適応機構という枝として比較する。ニューラル作用素とICLを一つの「汎用モデル」開発段階には並べない。

**この区分の収録件数：4件。**

<a id="sec-s"></a>
### S. 俯瞰用のサーベイ・総説

**役割：各分野の問題設定・方法・保証条件を俯瞰する。** 研究テーマとは別に文献の種類・読む用途でまとめた索引である。個別分野の節に置いた総説との重なりは許容し、相互参照して利用する。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［俯瞰・総説］** 最初に読む総説の有力候補。 | 2020 / **[Learning-Based Model Predictive Control: Toward Safe Learning in Control](https://doi.org/10.1146/annurev-control-090419-075625)** | 学習したダイナミクス、MPCパラメータの学習、安全な探索の3視点から学習ベースMPCを整理。 | GP・MPC・安全フィルタ等を統一的に分類し、モデル誤差・安全保証と学習効率の緊張関係を明確化した。総説のため共通ベンチマーク値なし。 | 学習ベースMPC全体の地図、個別原著へ進む導入。 | モデル品質、システムの可変性、安全なデータ収集の両立。 |
| **［俯瞰・総説］** 安全制御全般の全体像。 | 2022 / **[Safe Learning in Robotics: From Learning-Based Control to Safe Reinforcement Learning](https://doi.org/10.1146/annurev-control-042920-020211)** | 学習中・学習後のロボットの安全性を、MPC、CBF、Reachability、RLの観点で整理。 | 安全探索と学習済み方策の認証の違いを明確化する総説。主成果は方法間の比較で独自改善倍率なし。 | ロボットの学習制御、安全制約の設計。 | モデル誤差の扱い、実機試行中の事故回避、環境変化への保証。 |
| **［俯瞰・総説］** ロボットRL分野の全体地図。 | 2023 / **[Model-Based Reinforcement Learning: A Survey](https://doi.org/10.1561/2200000086)** | World Modelの学習、計画、価値推定、潜在表現、不確実性処理を広く分類。 | モデルベースRLを一つの設計空間として体系化する長編総説。多くの先行研究の比較を含むが独自の一律な改善倍率はない。 | TD-MPC2、Dreamer、モデル学習とMPCの接続。 | モデル誤差の累積、オフラインデータの偏り、オンライン探索の安全性。 |
| **［俯瞰・総説］** 理論基盤として特に重要。 | 2023 / **[Statistical Learning Theory for Control: A Finite-Sample Perspective](https://arxiv.org/abs/2209.05423)** | 未知線形系の有限データから得る同定誤差とLQR学習性能を、統計理論として整理。 | サンプル複雑性、同定誤差上界、性能下界などの結果を系統立てて解説。改善倍率というより、必要データ数と誤差の評価が成果。 | 学習制御の性能保証、システム同定の実験計画。 | 非線形系、部分観測、現実の雑音・探索制約への一般化に研究余地。 |
| **［俯瞰・総説］** リカッチ方程式と方策勾配の関係を見る。 | 2023 / **[Toward a Theoretical Foundation of Policy Optimization for Learning Control Policies](https://doi.org/10.1146/annurev-control-042920-020021)** | LQR/LQGなどを題材に、方策勾配・Actor更新の最適化地形と収束を整理。 | 非凸な制御方策最適化でも特定条件では勾配法の大域収束を証明できる結果を解説。数値実験の改善率が主眼ではない。 | 制御工学からRLの理論へ進む導入。 | 非線形・部分観測・制約付き実システムでは大域保証が難しい。 |
| **［俯瞰・総説］** 「安全」の意味を数学的に区別する教材。 | 2024 / **[A Survey of Constraint Formulations in Safe Reinforcement Learning](https://doi.org/10.24963/ijcai.2024/913)** | 期待累積コスト・確率制約・時刻ごとの状態入力制約など、異なる安全性の定式化を比較。 | 各方式がどの事象にどの種の保証を与えるかを整理。研究間の成功率をそのまま比較できない理由が分かる。 | Constrained MDP、CBF付きRL、学習方策の仕様選定。 | 実装上の制約違反率と理論保証の差、分布シフト、高次元観測。 |
| **［俯瞰・総説］** DeePC以外も含めた大局観。 | 2024 / **[Data Science and Model Predictive Control: A Survey of Recent Advances on Data-Driven MPC Algorithms](https://doi.org/10.1016/j.jprocont.2024.103327)** | DeePC、同定ベースMPC、学習モデル、適応・強化学習などの方式を分野横断で比較。 | データ取得・構造化・学習モデル・最適化・閉ループ評価という関係を明確化する。独自実験の単一改善率はない。 | データ駆動MPCの設計方式選定・研究文献の探索。 | モデルを使う直接／間接法を同一条件で評価する際の比較軸が多い。 |
| **［俯瞰・総説］** 研究と現場運用のギャップを理解できる。 | 2024 / **[Machine Learning for Industrial Sensing and Control: A Survey and Practical Perspective](https://doi.org/10.1016/j.conengprac.2024.105841)** | 産業プロセスのSoft Sensor、ハイブリッドモデル、プロセス最適化、RL等の実用事例を比較。 | 技術の適用例と産業上の要件、解釈性と物理知識を組み込む意義を整理。独自の制御成功率改善を一律には述べない。 | 装置開発への学習制御の移行、産業での技術選定。 | データ品質、説明可能性、実時間性、安全性、保守運用。 |
| **［俯瞰・総説］** 最適化とフィードバックを横断する最重要の概念論文。 | 2024 / **[Optimization Algorithms as Robust Feedback Controllers](https://doi.org/10.1016/j.arcontrol.2024.100941)** | 数値最適化アルゴリズムを動的系として見なし、物理プラントと並列・閉ループ接続して解析する。 | 時間変動最適化、Online Optimization、フィードバックによる最適解追従・ロバスト性を制御理論の言葉で統一する総説。独自の速度改善率は目的としない。 | 最適化ベース制御、Online Gradient、SQP/RTI設計の概念整理。 | 反復速度・サンプリング時間・プラントの安定性を同時に扱うと仮定が複雑になる。 |
| **［俯瞰・総説］** 依頼者が引用したTutorial Review。 | 2025 / **[A Tutorial Review of Machine Learning-Based Model Predictive Control Methods](https://doi.org/10.1515/revce-2024-0055)** | 学習モデルの汎化誤差とMPC閉ループ安定性の関係を解析し、非線形化学プロセスの例で解説。 | モデルの構築と保証を統合したチュートリアルで、データ不足、次元の呪い、モデル誤差、計算効率・安全性まで体系化。独自の改善倍率は要照合。 | 学習モデル付きNMPCの設計、モデル誤差に敏感な制約付き制御。 | データ量、外挿、オンライン学習、リアルタイム最適化と閉ループ保証の両立。 |
| **［俯瞰・総説］** DeePC等の安定性・ロバスト性・制約保証を整理 | 2025 / **[An Overview of Systems-Theoretic Guarantees in Data-Driven Model Predictive Control](https://doi.org/10.1146/annurev-control-030323-024328)** | DeePCを含むデータ駆動MPCの安定性、ロバスト性、制約充足の研究を体系化した総説である。 | 未知LTIに対する厳密な理論からノイズや非線形系への拡張までを整理し、適用可能な保証条件を比較した。総説固有の追従誤差改善率は存在しない。 | データ駆動MPCの保証条件確認、DeePCとモデル同定型MPCの比較研究に利用できる。 | データ励起・ノイズ境界・モデルクラスの仮定がなお強く、大域非線形系の保証と計算効率の両立が課題である。 |
| **［俯瞰・総説］** システム同定の観点で機械学習を学ぶ基礎。 | 2025 / **[Deep Networks for System Identification: A Survey](https://doi.org/10.1016/j.automatica.2024.111907)** | RNN、状態空間型NN、学習構造、正則化、ノイズ下同定などを整理。 | 深層学習による同定を従来型のシステム同定と接続し、ネットワーク構造・学習手順と識別可能性の問題を概観。総説につき改善率なし。 | ニューラル状態空間モデル、制御用モデル学習。 | 同定の再現性、長期予測、物理整合性、閉ループ性能保証が不足。 |
| **［俯瞰・総説］** 非線形データ駆動制御の比較に使う。 | 2025 / **[Efficient Data-Driven Predictive Control of Nonlinear Systems: A Review and Perspectives](https://doi.org/10.1016/j.dche.2025.100219)** | 非線形系に対するKoopman MPCとDeePCなど、データからの凸予測制御方式を比較。 | 制御性能、凸最適化への落とし込み、計算時間、データ要件の観点から研究動向を整理。新規の一律な性能倍率はない。 | Koopman対DeePCの位置づけ、非線形データ駆動制御手法の選定。 | 非線形系での誤差上界、実行可能性、データ量とオンライン計算負荷。 |
| **［俯瞰・総説］** ロボティクスの将来像を把握する補助。 | 2026 / **[A Survey of World Models for Physical AI with Uncertainty Representation and Control](https://doi.org/10.1007/s44163-026-02122-1)** | 潜在状態、時系列予測、物理事前知識、不確実性、観測形式、制御器結合でWorld Modelを分類。 | Physical AIでのWorld Modelの設計要素を横断整理。独自の制御性能値の提示ではない。 | 自律ロボット、モデルベースRL、汎用的な環境予測器。 | 現実の分布シフト・長期予測・構造保存・安全保証が未解決。 |
| **［俯瞰・総説］** Koopman予測誤差から閉ループ保証に至る総説 | 2026 / **[An Overview of Koopman-Based Control: From Error Bounds to Closed-Loop Guarantees](https://doi.org/10.1016/j.arcontrol.2025.101035)** | Koopman予測モデルの誤差上界とMPCの閉ループ安定性・制約保証に関する研究を概観した総説である。 | 有限データ・有限次元近似の誤差解析から閉ループ保証へつながる研究を整理した。新たな制御ベンチマークの数値改善を報告するものではない。 | Koopmanモデルの学習・MPC設計・保証研究の文献マップとして使える。 | 学習した持ち上げ座標の誤差評価と真の非線形系への保証の橋渡しが難しく、高次元化による計算負荷も残る。 |
| **［俯瞰・総説］** 研究の大局観を得るために優先。 | 2026 / **[Control-Oriented System Identification: Classical, Learning, and Physics-Informed Approaches](https://arxiv.org/abs/2512.06315)** | 古典同定、データ駆動・NN同定、物理構造を保存するモデル学習を、閉ループ制御に必要なモデル品質という視点から比較。 | 予測誤差、安定性・散逸性・保存則などを「制御に適したモデル設計」という軸で体系化した総説。独自の改善倍率を報告するものではない。 | システム同定手法の選択、GP/Koopman/Neural Dynamicsの位置づけ整理。 | 開ループ予測精度と制御品質の乖離、物理制約の表現、有限データと閉ループ保証の両立。 |
| **［俯瞰・総説］** 古典的SQP型MPCと確率推論型MPCを比較できる。 | 2026 / **[Model Predictive Control via Probabilistic Inference: A Tutorial and Survey](https://doi.org/10.1016/j.arcontrol.2026.101052)** | 有限ホライズン最適制御をBoltzmann型の最適制御分布に関する推論問題として表す。 | MPPIを含むSampling MPCを事前分布・変分推論・多峰性・制約・理論解析という軸で整理し、代表的なサンプル更新を導出。総説につき共通の改善倍率はない。 | MPPI、サンプリング最適制御、分布に基づくロボット計画。 | サンプル効率、制約保証、計算時間、モデル不確かさが残る。 |
| **［俯瞰・総説］** 安定性・ロバスト性保証付きNeural Controlの総説 | 2026 / **[Neural Networks in the Loop: Learning with Stability and Robustness Guarantees](https://doi.org/10.1146/annurev-control-022624-032459)** | ニューラルネットワークを含む閉ループ制御の安定性とロバスト保証を既存研究から整理した総説である。 | Lyapunov、Contraction、検証付き学習などの制御保証の枠組みと必要条件を整理した。改善倍率・実験成功率を新規に報告する文献ではない。 | 保証付きニューラル制御器の研究計画、安定性証明手法の選定に使える。 | 大規模NNや高次元非線形系の証明可能領域は狭くなりやすく、モデル誤差・安全性・計算負荷の統一的な保証が難しい。 |
| **［俯瞰・総説］** FNO/DeepONetの背景の数理を補う。 | 2026 / **[Operator Learning: A Statistical Perspective](https://doi.org/10.1146/annurev-statistics-042424-070908)** | 関数から関数への作用素学習を統計回帰問題として整理し、関数空間での近似・汎化を扱う。 | PDE解写像の学習誤差・統計的難しさを体系化する総説。特定制御器の追従改善率ではない。 | Neural Operatorの理論、PDE制御モデルの学習。 | 無限次元関数のサンプル複雑性と実環境の一般化。 |
| **［俯瞰・総説］** MPCをTeacher・Policy・Criticに用いる研究の分類 | 2026 / **[Synthesis of Model Predictive Control and Reinforcement Learning: Survey and Classification](https://doi.org/10.1016/j.arcontrol.2026.101045)** | 既存のMPC・RL融合手法を分類する総説であり、独自のベンチマークによる制御性能向上は対象としない。 | MPCを教師、方策、価値関数等に利用する構成を整理し、手法間の前提と学習・計算上の特徴を比較した。特定手法の成功率を新たに測定した論文ではない。 | MPCとRLの融合アーキテクチャ設計、論文選定や研究課題の整理に有用である。 | 方式によってサンプル効率、オンライン計算量、安全性の仮定が異なり、各比較を同一条件で行うベンチマークが不足する。 |

**推奨順と理由**：まずLearning-Based MPCまたはA Decade（T節）で全体像を把握し、目的に応じて同定、データ駆動MPC、Koopman、RL、安全制御、Neural Operatorの総説を選ぶ。最適化を力学として解析する総説と、最適制御を確率推論として導出する総説は別の見方であり、前者→後者の必須前提関係はない。

**この区分の収録件数：20件。**

<a id="sec-t"></a>
### T. 導入・導出を学ぶチュートリアル／概説

**役割：導出や例題を追って各分野へ入るための読書ガイド。** S節との重なりは許容する。「導入として読む用途」と「新規手法か総説かという文献種別」は別であり、研究論文は主な技術分野に配置する。

| 読む価値・研究上の役割 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| **［導入・導出］** 実験入力設計と制御の両立を学ぶ。 | 2018 / **[Stochastic Model Predictive Control with Active Uncertainty Learning: A Survey on Dual Control](https://doi.org/10.1016/j.arcontrol.2017.11.001)** | 制御行為が状態を変えるだけでなく未知パラメータに関する情報を得るDual Controlを扱う。 | 不確実性学習と探索をMPCへ導入する方式を整理し、探索と制御のトレードオフを解説。独自倍率はない。 | Active Learning、オンラインシステム同定、GP-MPC。 | 探索時の安全性、情報利得評価の計算コスト、実機の励起制約。 |
| **［導入・導出］** 確率的MPCへ進む前の導入。 | 2018 / **[Stochastic Model Predictive Control — How Does It Work?](https://doi.org/10.1016/j.compchemeng.2017.10.026)** | 確率的外乱を含む制約付きMPCで、リスクと性能を両立する設計を説明する。 | Chance Constraint、予測分布、名目MPCとの違いを例題で解説。チュートリアルに共通した改善倍率はない。 | GP-MPCの確率制約、SMPCの基礎理論。 | 分布仮定や近似共分散、制約違反確率の較正に依存する。 |
| **［導入・導出］** ニューラルネットワークの知識を補うための実装志向教材。 | 2022 / **[A Tutorial Review of Neural Network Modeling Approaches for Model Predictive Control](https://www.sciencedirect.com/science/article/pii/S0098135422002927)** | FNN、RNN、LSTM/GRU、Encoder–Decoder等の時系列モデルを構築し、非線形化学プロセスのMPCで比較。 | 問題設定、訓練・パラメータ推定、性能検証、閉ループ実装を順に解説。異なるモデル構造による予測精度・閉ループ挙動の比較が含まれる。 | NNによるシステム同定、学習モデルをCasADi/acadosへ組み込む前段。 | モデル分布外の予測精度、モデル次数・アーキテクチャの選択、制御ループ内の計算負荷。 |
| **［導入・導出］** GP-MPCの入門から高度な論文への導入に適する。 | 2024 / **[A Tutorial on Gaussian Process Learning-based Model Predictive Control](https://arxiv.org/abs/2404.03689)** | GP回帰の平均・分散の導出、確率的な多段予測、MPCでの共分散伝播とChance Constraint。 | GP-MPCの確率近似と制約構築を一続きの数式で説明。一般の性能倍率は報告対象ではない。 | Cautious GP-MPC、Zero-order GP-MPC、L4acadosの事前学習。 | 分布のガウス近似、GP計算量、共分散の較正、オンライン制御。 |
| **［導入・導出］** 基本CBF原論文の次に読む。 | 2024 / **[Advances in the Theory of Control Barrier Functions: Addressing Practical Challenges in Safe Control Synthesis for Autonomous and Robotic Systems](https://doi.org/10.1016/j.arcontrol.2024.100945)** | CBFの基本原理から入力制約、高相対次数、サンプル値制御、モデル誤差、保守性までを整理。 | CBF-QPの実装・解析で出会う問題と対策の理論を体系化。新規の統一改善率はない。 | ロボットの安全フィルタ設計、CBFの理論と実装の接続。 | QPの実行不能、誤差による安全集合の破れ、現実のアクチュエータ制約。 |
| **［導入・導出］** MPPI研究の歴史とバリエーションを掴む。 | 2024 / **[Recent Advances in Path Integral Control for Trajectory Optimization: An Overview in Theoretical and Algorithmic Perspectives](https://doi.org/10.1016/j.arcontrol.2023.100931)** | Path Integral Control、MPPI、CEMなどの軌道最適化を理論とアルゴリズムから比較。 | サンプリング分布・ノイズ重み付け・最適制御の関係を整理し、実装教材も紹介。独自の一律性能倍率はない。 | MPPI、GPUサンプリングMPC、iLQRとの比較。 | 試行軌道数・確率分布の設計、ハード制約・安全性、長いホライズン。 |
| **［導入・導出］** MPCとRLを価値関数から理解する補助。 | 2025 / **[A View on Learning Robust Goal-Conditioned Value Functions: Interplay Between RL and MPC](https://arxiv.org/abs/2502.06996)** | MPCの有限時間価値関数とRLの長期価値関数を共通の概念で比較する。 | 短いMPCホライズンへ学習した終端価値を組み込む構造と、頑健なGoal-conditioned価値学習の論点を整理。新規の統一改善倍率はない。 | RL終端コストを使うMPC、短ホライズン予測制御。 | 価値関数の外挿誤差、制約保証、MPCとRLの誤差相互作用。 |
| **［導入・導出］** データ駆動研究への入口。 | 2025 / **[Tutorial: Data Driven and Learning Enabled Control](https://doi.org/10.1109/CDC57313.2025.11312267)** | 同定後に制御する間接法とデータから制御器を直接構築する直接法を比較する。 | データ駆動制御と学習ベース制御の共通構造を導入し、モデルの役割を整理。定量的な独自実験が主目的ではない。 | DeePC、Koopman、モデル同定ベース制御の統一的理解。 | 説明範囲が広いため、個別手法の詳細な数値再現には原論文が必要。 |
| **［導入・導出］** 総合ロードマップの第一候補。 | 2026 / **[A Decade of Learning-Based Control: From Theoretic Foundations to Practical Deployment](https://arxiv.org/abs/2610.29187)** | 2016～2025年の学習ベース制御の10年間を、安定性・安全性保証、モデル学習、直接データ駆動方策・RL等の側面から俯瞰。 | 方法族ごとの必要な事前知識、学習対象、誤差の扱い、制御保証の対応を比較し、研究史をまとめる。182本のCDC招待セッション論文に関する集計にも言及。 | 今回の全研究系譜を俯瞰する最初の教材。 | 大規模な総説で個々のアルゴリズムの実装詳細は十分でない。掲載時点の新規研究・最新性能評価は別途追う必要。 |
| **［導入・導出］** Operator Learning統計総説と補完関係。 | 2026 / **[From Theory to Application: A Practical Introduction to Neural Operators in Scientific Computing](https://doi.org/10.3390/math14132421)** | FNO、DeepONetを中心にNeural Operatorの構築・学習・PDE代理予測を実践的に概説。 | 理論とコード実装で生じる設計・計算上の問題を整理。PDE制御の性能改善を独自に一律評価する文献ではない。 | PDE制御へ進む前のScientific Machine Learning実装。 | 物理整合性、解像度間汎化、長時間予測誤差、学習計算量。 |
| **［導入・導出］** 2026年時点の理論と実装をつなぐ最優先チュートリアル。 | 2026 / **[Koopman Operator Theory: Fundamentals, Control, and Applications](https://arxiv.org/abs/2607.01819)** | Koopman作用素と可観測関数、EDMD/Kernel EDMD、入力付き系への拡張、Koopman MPCまで段階的に説明。 | 有限データによるモデル近似誤差、EDMDとKoopman MPCの導出、実際のシミュレーションと公式GitHubのハンズオンを収録。チュートリアルなので一律の性能向上値はない。 | Koopman MPCの最初の自作、学習モデルの推定誤差と制御を結ぶ学習。 | 辞書選択、有限データの誤差、真の非線形系に対する閉ループ保証。 |
| **［導入・導出］** 2024年のCBF実装課題チュートリアルの続きとして読む。 | 2026 / **[Learning for Feasible and Safe Control with Control Barrier Functions: A Tutorial](https://doi.org/10.26599/CAI.2025.9390009)** | 安全制約を満たすQPが空集合になる問題や高次CBFの設計で、学習をどこに使うか説明。 | CBF-QPの実行可能性・安全性と学習の関係を事例を交えて整理。改善率の統一値はない。 | 学習によるCBF設計・実行可能領域拡張、安全制御の自動化。 | 学習誤差と安全保証の整合、入力飽和、多数の安全制約との競合。 |

**推奨順と理由**：GPはStochastic MPC→GP-MPC Tutorial、KoopmanはKoopman Tutorial→EDMD/入力付き予測→Korda、NN-MPCはNNモデリング→ML-MPC Tutorialへ進む。その先はモデルを改善するならL/H/E、最適解を微分するならA、明示的方策を学ぶならNへ分岐する。安全性・収縮性のチュートリアルも各分野の原著と併読する。

**この区分の収録件数：12件。**

<a id="implementations"></a>
## 公開実装・講義・チュートリアル資料

**以下の24件は文献件数に含めない。** 再現実装の入口として収録する。対応する方法族を確認し、すべてを同じアルゴリズムの実装とはみなさない。

| 種別 | 教材・コード | どの論文と併読するか・用途 |
|---|---|---|
| DeePCを体系的に学ぶ講義 | [A Tutorial on Data-Enabled Predictive Control (Dörfler, ICCAS 2024)](https://speakerdeck.com/floriandoerfler/a-tutorial-on-data-enabled-predictive-control-deepc) | Willemsの基本補題からDeePCの定式化、ノイズ・安定性・応用まで著者の講義で追える。論文ではなく約3時間の講義資料。 |
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
| World Model ICL | [MazeWorld](https://github.com/airs-cuhk/airsoul/tree/main/projects/MazeWorld) | World ModelのICL現象を再現する著者コード。 |

<a id="evaluation"></a>
## 文献評価・定量指標の読み方

**1. 実験結果と理論保証を分ける。** 「11倍高速化」は特定のベンチマークの処理時間比だが、「局所指数安定性」は仮定の下での数学的性質であり、数値的な優劣を比較するものではない。

**2. 倍率には評価対象がある。** オンラインMPCの求解時間、NN推論だけの時間、オフライン学習を含む総時間、形式検証の認証時間は別指標。PDE-DPCの推論倍率などを、開発総工数や実時間性の普遍的な倍率と解釈しない。

**3. 原著と総説、読む用途を区別する。** 総説やチュートリアルの成果は体系化・導出・教材であり、新規実験の改善率が存在するとは限らない。S/Tは俯瞰・導入の索引で、技術分野の節との重なりを許容する。

**4. 保証対象と前提を確認する。** モデル自体の安定性、実プラントの閉ループ安定性、軌道追従、安全集合の不変性、確率的被覆は異なる性質。GP分散、Conformal Prediction、Lyapunov/CCM認証、SLSの外乱集合にはそれぞれ固有の仮定がある。

**5. 名前ではなくオンライン／オフラインの構造を確認する。** AのDifferentiable MPCは最適化問題の解の感度を学習へ渡す。NのDPCは主に微分可能な予測ロールアウトを通して明示的方策を訓練する。IのNeural Operatorも、予測器を学ぶ場合と方策を学ぶ場合で役割が異なる。

**6. 同じ節・読書順と、直接の研究系譜を区別する。** 「→」は推奨学習順であり、直接の後継・理論的包含を意味しない。RTIとiLQR、DeePCとKoopman MPC、CBFと予測安全フィルタ、PPOとTD-MPC、RLとDiffusion Policyなどは、共通する問題を持っていても異なる方法である。

**7. 書誌・確認範囲について。** 既存107件の選定と記述を引き継ぎ、分類と推奨順を修正した。今回の基礎追加20件は、出版社・著者・所属機関・arXiv・公式会議録などの情報を参照して補った。MIT Cheetah 3のリンクをDOIに、2017年MPPIのリンクを掲載題名と一致するDOIに訂正した。初期MPPIの関連プレプリントは[Model Predictive Path Integral Control using Covariance Variable Importance Sampling](https://arxiv.org/abs/1509.01149)で、別の追加文献として重複計上していない。Information-Theoretic MPCは2017年のプレプリントと2018年の雑誌版を1件として扱う。既存のL1適応制御など書誌要照合の項目は残っており、全リンク・刊行年・定量値の網羅的な再確認は完了していない。

---

**資料の性格**：私自身がこれから読むためにChatGPTへ作成・整理を依頼した学習候補一覧であり、私自身が全件を読んで作成したレビューではありません。既存107件を保持し、基礎文献20件を追加しています。文献を引用したり数値を研究資料に再利用したりする際は、原典の該当図表・定理を確認してください。
