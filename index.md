---
layout: default
title: 制御工学・ロボティクス・学習ベース制御 研究文献総覧
---

# 制御工学・ロボティクス・学習ベース制御　研究文献総覧

**ChatGPTを利用して個人の学習・研究用に調査・整理した文献リストを、参考資料として公開しています。（2026年10月24日版）**

> **このリストの作成経緯について**  
> この文献リストは、**私自身の学習・研究のためにChatGPTを利用して論文を探し、各手法の概要・研究の発展・成果・課題・読む順番などを整理させたもの**です。私がすべての論文を独力で収集・精読し、各記述を検証して作成したものではなく、**これから読む論文と概要をLLMを利用してリストアップしたもの**になります。同じ分野に関心のある方にも参考にしていただければと思い、公開しています。  
> **説明文や数値、評価、推奨読書順にはChatGPTが生成・整理した内容が含まれます。** 誤りや不正確な記述が残っている可能性があるため、論文の内容や実験結果を引用・利用する際は、必ずリンク先の原論文で確認してください。

> **編集方針**：本ファイルは「この会話で紹介した文献と、その際に説明した実験・成果・研究課題」を転記・再整理した文献台帳です。54件の詳細評価Excelを基礎資料として使い、その前後のチャットに登場した原著・総説・チュートリアルを追加しています。全件の本文を今回読み直したわけではありません。定量値は論文や従前の説明で確認できたものだけを記載し、未確認の数値を推測して補っていません。**各研究の成果は「個々の論文の検証条件で報告されたもの」であり、異なる手法の改善率を単純比較できません。** 書誌・リンクの一部については「要照合」を付記しています。

**文献数：107件（同一URLの重複除外）／関連する公開教材・ソフトウェア：24件。** 出版年はオンライン先行公開・会議採録・雑誌掲載の基準が混在する場合があります。

## 目次

- [まず読む順番：全体ロードマップ](#まず読む順番全体ロードマップ)
- [R. ロボティクス・制御の出発点：最適化、接触、推定、RL](#sec-r)
- [X. その他の原著研究：古典的基盤と2026年の派生研究](#sec-x)
- [A. Differentiable MPC・最適化層](#sec-a)
- [B. Neural Lyapunov・形式的検証](#sec-b)
- [C. Control Contraction Metrics](#sec-c)
- [D. DeePC・データ駆動予測制御](#sec-d)
- [E. Koopman MPC・安定性保証](#sec-e)
- [F. Safety Filter・CBF](#sec-f)
- [G. SLS・分散／ロバスト制御](#sec-g)
- [H. GP-MPC・確率的モデル予測制御](#sec-h)
- [I. Neural Operator・PDE制御](#sec-i)
- [J. In-Context Dynamics Learning](#sec-j)
- [S. 分野横断のサーベイ・総説](#sec-s)
- [T. 方法を学ぶチュートリアル論文](#sec-t)
- [公開実装・講義・チュートリアル資料](#公開実装講義チュートリアル資料)
- [文献評価・定量指標の読み方](#文献評価定量指標の読み方)

## まず読む順番：全体ロードマップ

**最初から全部を順読するのではなく、①Surveyで全体像→②Tutorialで導出と実装→③原著を発表年代順→④2025〜2026年の後継研究、という四段階に分ける**。研究系譜間に厳密な前提関係があるわけではなく、以下は学習効率を重視した推奨順である。

| 段階 | まず読むもの（順序） | 何が理解できるか・読み終えたときの到達点 |
|---|---|---|
| 0：俯瞰 | [A Decade of Learning-Based Control](https://arxiv.org/abs/2610.29187)（2026）→ [Learning-Based MPC](https://doi.org/10.1146/annurev-control-090419-075625)（2020）→ [Control-Oriented System Identification](https://arxiv.org/abs/2512.06315)（2026） | モデルを学習する／直接制御則を学ぶ／安定性や安全性を保証する、という研究の分類。各分野の論文の「解決した問題」を位置づけられる。 |
| 1：最適化と学習 | [OptNet](https://arxiv.org/abs/1703.00443)（2017）→ [Differentiable MPC](https://arxiv.org/abs/1810.13400)（2018）→ [Differentiable Nonlinear MPC](https://arxiv.org/abs/2505.01353)（2025）→ [Policy Optimization using Differentiable MPC](https://proceedings.mlr.press/v331/zuliani26a.html)（2026） | KKT感度、SQP・内点法の感度、評価関数学習・閉ループ方策最適化。[CVXPYlayers](https://papers.neurips.cc/paper_files/paper/2019/hash/9ce3c52fc54362e22053399d3181c638-Abstract.html)（2019）と[DIPP](https://arxiv.org/abs/2207.10422)（2023）は枝として読む。 |
| 2：GP-MPC | [Stochastic MPC — How Does It Work?](https://doi.org/10.1016/j.compchemeng.2017.10.026)（2018）→ [GP-MPC Tutorial](https://arxiv.org/abs/2404.03689)（2024）→ [Cautious GP-MPC](https://arxiv.org/abs/1705.10702)（2020）→ [Zero-order GP-MPC](https://arxiv.org/abs/2211.15522)（2023）→ [L4acados](https://arxiv.org/abs/2411.19258)（2026） | 多段確率予測、共分散伝播、Chance Constraint、SQP感度近似、acados実装。[2004年のGP-MPC](https://mlg.eng.cam.ac.uk/pub/pdf/KocMurRasGir04.pdf)は歴史的な出発点として随時参照。 |
| 3：DeePC | [Willemsの基本補題](https://doi.org/10.1016/j.sysconle.2004.09.003)（2005）→ [DeePC](https://arxiv.org/abs/1811.05890)（2019）→ [Robust Data-driven MPC](https://doi.org/10.1109/TAC.2020.3000182)（2021）→ [Nonlinear Data-driven Tracking](https://arxiv.org/abs/2105.08567)（2022）→ [Local DPC](https://doi.org/10.1016/j.ifacsc.2026.100396)／[Select-DPC](https://proceedings.mlr.press/v331/naf26a.html)（2026） | Hankel行列による軌道表現、正則化、閉ループ保証、非線形性への局所データ選択。[Dörflerの講義](https://speakerdeck.com/floriandoerfler/a-tutorial-on-data-enabled-predictive-control-deepc)を併読。 |
| 4：Koopman | [Korda & Mezić](https://arxiv.org/abs/1611.03537)（2018）→ [Deep Linear Embeddings](https://doi.org/10.1038/s41467-018-07210-0)（2018）→ [Koopman Theory Tutorial](https://arxiv.org/abs/2607.01819)（2026）→ [Stable Koopman Embeddings](https://doi.org/10.1016/j.automatica.2026.113003)（2026）→ [Exponential Stability Koopman MPC](https://proceedings.mlr.press/v331/shang26a.html)（2026） | 学習する持ち上げ写像、予測誤差、元の非線形系の安定性。数値最適化に関心があれば[Koopman-BoxQP](https://proceedings.mlr.press/v331/wu26a.html)へ枝分かれ。 |
| 5：保証された学習制御 | [Neural Lyapunov](https://papers.neurips.cc/paper/8587-neural-lyapunov-control)（2019）→ [Lyapunov-stable NN Control](https://roboticsconference.org/2021/program/papers/063/index.html)（2021）→ [CT-BaB](https://proceedings.mlr.press/v331/shi26a.html)（2026）／ [CCM](https://arxiv.org/abs/1503.03144)（2017）→ [C3M](https://proceedings.mlr.press/v155/sun21b.html)（2021）→ [Neural CCM](https://proceedings.mlr.press/v283/li25e.html)（2025） | Lyapunov減少条件とContraction条件の違い、学習した関数の形式的検証。モデル依存と数値的認証範囲を区別する。 |
| 6：安全フィルタ | [CBF-QP](https://doi.org/10.1109/TAC.2016.2638961)（2017）→ [Predictive Safety Filter](https://doi.org/10.1016/j.automatica.2021.109597)（2021）→ [Advances in CBF Theory](https://doi.org/10.1016/j.arcontrol.2024.100945)（2024）→ [Learning for Feasible and Safe CBF](https://doi.org/10.26599/CAI.2025.9390009)（2026）→ [Latent CBF](https://proceedings.mlr.press/v331/nakamura26a.html)（2026） | 前方不変性、QP実行可能性、安全と追従の両立、学習ベース安全関数の限界。 |
| 7：分散・ロバスト | [SLSの原論文](https://arxiv.org/abs/1610.04815)（2016）→ [SLS総説](https://arxiv.org/abs/1904.01634)（2019）→ [Robust SLS MPC](https://doi.org/10.1016/j.automatica.2023.111431)（2024）→ [Conformalized SLS](https://proceedings.mlr.press/v331/srinivasan26a.html)（2026） | 閉ループ応答での設計、外乱集合、統計的なモデル誤差境界。 |
| 8：RLと最適制御 | [iLQG/DDP](https://homes.cs.washington.edu/~todorov/papers/TassaIROS12.pdf)（2012）→ [MPPI](https://arxiv.org/abs/1509.01149)（2017）→ [MPPI Tutorial](https://doi.org/10.1016/j.arcontrol.2023.100931)（2024）／ [PPO](https://arxiv.org/abs/1707.06347)（2017）→ [Model-Based RL Survey](https://doi.org/10.1561/2200000086)（2023）→ [TD-MPC2](https://arxiv.org/abs/2310.16828)（2024） | 勾配型・サンプリング型・価値関数学習型の制御方策を比較できる。 |
| 9：将来の汎用モデル | [FNO](https://arxiv.org/abs/2010.08895)／[DeepONet](https://doi.org/10.1038/s42256-021-00302-5)（2021）→ [Physics-Informed Neural Operator](https://arxiv.org/abs/2111.03794)（2021）→ [PDE-DPC](https://arxiv.org/abs/2511.08992)（2025）→ [CINOC](https://proceedings.mlr.press/v306/zanotta26a.html)（2026）／ [Class Models](https://arxiv.org/abs/2308.13380)（2023）→ [ICL Linear Dynamics](https://proceedings.neurips.cc/paper_files/paper/2025/file/f7ae4fe91d96f50abc2211f09b6a7e49-Paper-Conference.pdf)（2025）→ [ICL System ID](https://arxiv.org/abs/2610.13411)（2026） | PDE代理モデルから方策学習への接続、事前学習したモデルの未知対象への文脈内適応。 |

### 目的から選ぶショートカット

| 優先したい能力 | おすすめの順番 | 最初の再現実装 |
|---|---|---|
| **実時間NMPCを強化する** | [RTI](https://doi.org/10.1137/S0363012902400713) → [Differentiable MPC](https://arxiv.org/abs/1810.13400) → [Differentiable NMPC](https://arxiv.org/abs/2505.01353) → [GP-MPC](https://arxiv.org/abs/1705.10702)／[Koopman-BoxQP](https://proceedings.mlr.press/v331/wu26a.html) | 二重積分系→CartPoleのMPC、最適解のパラメータ感度と求解時間を測定する。 |
| **新しいデータ駆動制御を理解する** | [Willems](https://doi.org/10.1016/j.sysconle.2004.09.003) → [DeePC](https://arxiv.org/abs/1811.05890) → [Berberich](https://doi.org/10.1109/TAC.2020.3000182) → [Select-DPC](https://proceedings.mlr.press/v331/naf26a.html) → [Koopman安定性](https://proceedings.mlr.press/v331/shang26a.html) | 同じ非線形倒立振子でDeePCとKoopman MPCを比較し、データ選択の効果を検証する。 |
| **保証付き学習制御を研究する** | [CBF](https://doi.org/10.1109/TAC.2016.2638961) → [Predictive Safety Filter](https://doi.org/10.1016/j.automatica.2021.109597) → [Neural Lyapunov](https://papers.neurips.cc/paper/8587-neural-lyapunov-control) → [CCM](https://arxiv.org/abs/1503.03144) → [検証付きNN](https://proceedings.mlr.press/v331/shi26a.html) | NN制御器にCBF-QPを追加し、制約違反率と入力修正量を測定する。 |
| **ロボット制御全般を研究する** | [RTI](https://doi.org/10.1137/S0363012902400713)／[iLQR](https://homes.cs.washington.edu/~todorov/papers/TassaIROS12.pdf) → [Quadruped Convex MPC](https://arxiv.org/abs/1803.05580) → [MPPI](https://arxiv.org/abs/1509.01149) → [TD-MPC2](https://arxiv.org/abs/2310.16828) | MuJoCoで四脚の簡易重心MPCとMPPIの追従誤差・計算時間を比較する。 |
| **次世代の汎化型モデルを研究する** | [Neural Operator Tutorial](https://doi.org/10.3390/math14132421) → [FNO](https://arxiv.org/abs/2010.08895) → [CINOC](https://proceedings.mlr.press/v306/zanotta26a.html) ／ [Class Models](https://arxiv.org/abs/2308.13380) → [World Model ICL](https://proceedings.iclr.cc/paper_files/paper/2026/hash/a46b9dcef23e9160f46d800a49a931d0-Abstract-Conference.html) | 物理パラメータが異なる系に対する予測誤差と転移性能を評価する。 |

## 文献一覧（研究系譜ごと・原則年代順）

表の欄は左から、**読む価値・研究史上の位置**（この論文を読む意義と他の研究とのつながり）、**年・論文**、**対象・問題設定・実験**、**手法と主な成果・定量指標**、**期待される応用**、**残された課題・限界**の順に配置した。

<a id="sec-r"></a>
### R. ロボティクス・制御の出発点：最適化、接触、推定、RL

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| NMPCの実時間計算の歴史を理解する要石。 | 2005 / **[A Real-Time Iteration Scheme for Nonlinear Optimization in Optimal Feedback Control](https://doi.org/10.1137/S0363012902400713)** | 非線形MPCで各サンプリング時刻にNLPを完全に収束させる時間がない問題を考える。 | SQPの一回の反復をサンプリング周期に合わせて実行するRTIを提案し、実時間のフィードバック最適化を可能にする理論と数値例を提示。制御性能・演算時間の普遍的な改善倍率はない。 | acados RTI、非線形車両MPC、ロボットのオンライン軌道最適化。 | 良い初期解、滑らかな力学、サンプリング間の変化が十分小さいこと等に依存し、単一反復のため厳密収束しない場合がある。 |
| DDP/iLQRによる非線形最適制御の実践的な起点。 | 2012 / **[Synthesis and Stabilization of Complex Behaviors through Online Trajectory Optimization](https://homes.cs.washington.edu/~todorov/papers/TassaIROS12.pdf)** | 複雑な多自由度非線形系で短いホライズンの軌道を逐次最適化し、閉ループで振る舞いを生成する問題。 | 微分動的計画法／iLQG系の二次近似と前後方掃引を実時間のフィードバック計画へ適用し、複雑な動作をオンラインで生成・安定化できることを示した。全対象で共通する追従誤差・速度改善率は未確認。 | MPC、軌道最適化、四脚・多関節ロボットの動作生成。 | 局所解に依存し、接触・摩擦などの非滑らかな遷移と厳しい制約の扱いが難しい。 |
| Contact-implicit最適化の原典。 | 2014 / **[A Direct Method for Trajectory Optimization of Rigid Bodies Through Contact](https://doi.org/10.1177/0278364913506757)** | 接触位置や接触時刻が事前に不明な剛体系の軌道最適化。 | 相補性条件を含む直接法（contact-implicit trajectory optimization）により、接触モードをあらかじめ指定せずに接触を含む動作軌道を求める構成を示した。全タスクの最適値改善率は未確認。 | 脚ロボット、押し操作、接触計画、最適制御と接触力学の統合。 | 非凸性・相補性制約の退化・局所解依存・大規模NLPの計算量が課題。 |
| MPPIを研究する際の基盤。 | 2017 / **[Model Predictive Path Integral Control: From Theory to Parallel Computation](https://arxiv.org/abs/1509.01149)** | 確率的な軌道サンプリングで非線形・非凸の有限時間最適制御を近似する問題。 | 経路積分制御をGPU並列計算へ展開し、サンプリングベースの実時間MPCとして実証。演算速度・追従性能の共通倍率は未確認。 | 車両の高速走行、接触を含むロボティクス、GPU搭載プラットフォーム。 | サンプリング数による計算負荷、モデル精度、入力分布設計、厳密な安全保証が課題。 |
| 幾何学的推定と因子グラフ最適化の代表論文。 | 2017 / **[On-Manifold Preintegration for Real-Time Visual–Inertial Odometry](https://arxiv.org/abs/1512.02363)** | 高速IMU計測をキーフレーム間の制約へ集約し、姿勢をSO(3)上で推定する視覚慣性オドメトリ。 | 多量のIMU計測を再積分せずバイアス更新に対応するPreintegrationを定式化し、実時間のVIO最適化で効果を示した。推定誤差の統一改善率は未確認。 | ロボット状態推定、VIO/SLAM、MPCへ供給する状態の高精度化。 | 観測可能性、センサ較正、時刻同期、強いバイアス変化、低テクスチャ環境に影響を受ける。 |
| モデルベース制御と対比するRLの基本論文。 | 2017 / **[Proximal Policy Optimization Algorithms](https://arxiv.org/abs/1707.06347)** | モデルを明示しない方策最適化で、更新幅が大き過ぎることによる学習不安定を抑える。 | Clipped surrogate objective等で方策更新を制限し、連続・離散行動ベンチマークで有効性を示した。対象環境全体に共通する改善倍率は未確認。 | ロボットのモデルフリーRL、MPCとRLの比較ベースライン。 | オンラインの実機探索では試行コストが大きく、制約充足と安定性はそのままでは保証しない。 |
| 四脚凸MPCの代表的な実機研究。 | 2018 / **[Dynamic Locomotion in the MIT Cheetah 3 Through Convex Model-Predictive Control](https://arxiv.org/abs/1803.05580)** | 四脚ロボットの胴体運動と接触力を、接地制約・摩擦制約の下でオンライン計画する。 | 線形化した重心運動モデルと凸QPを用い、動的な四脚歩行・旋回・外乱下動作をMIT Cheetah 3で実証した。論文内の課題依存の数値を抜き出した共通改善率は未確認。 | Quadruped MPC、Contact-force optimization、MuJoCoでの四脚制御教材。 | 重心モデルは脚の慣性や接触切替えを簡略化する。歩容生成・状態推定・低位追従器の品質に依存する。 |
| 最適化ベース制御と対比する生成方策の代表論文。 | 2023 / **[Diffusion Policy: Visuomotor Policy Learning via Action Diffusion](https://arxiv.org/abs/2303.04137)** | 画像・ロボット状態から多峰性の行動系列を生成する模倣学習。 | Diffusionによって行動系列の分布を学習し、複数のロボット操作タスクで優れた模倣性能を実証。成功率などの具体値は原論文で再確認が必要。 | 視覚ベースのマニピュレーション、MPC以外の軌道生成方策との比較。 | 推論計算量、データ分布外動作、形式的な安全性・安定性保証は課題。 |
| MPCとRLの最新系列を理解するための代表例。 | 2024 / **[TD-MPC2: Scalable, Robust World Models for Continuous Control](https://arxiv.org/abs/2310.16828)** | 複数の連続制御タスクで、潜在World ModelとMPCを共同利用して性能・汎化を評価。 | 潜在状態の短期予測と価値推定を使うサンプリング計画を統合し、大規模・多タスクで高性能なモデルベースRLを実証。代表的な点数の正確な引用は未再確認。 | ロボットのWorld Model型MPC、データ効率の高い連続制御。 | 学習データの範囲、計画の安全保証、実機分布シフト、訓練コストが課題。 |

**推奨順と理由**：まずDiehl（RTI）とTassa（iLQR系）で「実時間の非線形最適制御」を理解し、Di Carlo（四脚凸MPC）でロボット固有の接触制約を確認する。次にWilliams（MPPI）でサンプリング最適制御を比較し、PPO→TD-MPC2→Diffusion Policyでモデルフリー、World Model、生成方策という異なる学習方法を対比する。Posaの接触最適化とForsterの状態推定は、必要に応じてそれぞれ力学・推定の枝として読む。

**この区分の収録件数：9件。**

> 補足：AmesらのCBF-QP（2017）はこの会話の最初のロボティクス10本にも登場したが、二重計上を避けるためF節にまとめた。

<a id="sec-x"></a>
### X. その他の原著研究：古典的基盤と2026年の派生研究

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 先に提示した「古典的制御寄り10本」の一つ。 | 1984 / **[Automatic Tuning of Simple Regulators with Specifications on Phase and Amplitude Margins](https://doi.org/10.1016/0005-1098(84)90013-5)** | モデルを明示同定せずにPID制御器を自動調整し、ゲイン余裕・位相余裕に対応した性能を得る。 | リレー実験から限界振動の周波数と振幅を推定するPID自動調整を提案し、実用的な制御器の自動設計に道を開いた。改善率はシステムごとに異なる。 | 産業PID・ゲイン自動調整・低次元フィードバック制御のベースライン。 | 非線形リレー実験の許容、強い非線形性・遅延、周波数応答を1点で代表する限界。 |
| DGKFの原典。 | 1989 / **[State-Space Solutions to Standard H2 and H-infinity Control Problems](https://doi.org/10.1109/9.29425)** | 標準H2/H∞問題を状態空間・リカッチ方程式で解く。 | 可解条件と中央解をリカッチ方程式で表し、混合感度などのロバスト制御の数理的基盤を確立。主成果は可解性・構成法の定理。 | H∞混合感度、ロバストコントローラ、周波数整形。 | 標準問題の可解性条件、重み選択、コントローラ次数、非線形・飽和の扱いが課題。 |
| 間接データ駆動制御の歴史的基盤。 | 1994 / **[N4SID: Subspace Algorithms for the Identification of Combined Deterministic-Stochastic Systems](https://doi.org/10.1016/0005-1098(94)90030-2)** | 雑音を含む入出力系列からMIMO状態空間モデルを効率よく推定する。 | 過去・未来のデータ行列と射影・SVDから状態空間表現を復元するN4SID系の枠組みを提示。実験系別の誤差改善率は未確認。 | MIMOシステム同定、サブスペース予測器、DeePCとの比較。 | 線形性、モデル次数選択、励起条件、欠損・時変・非線形データへの対応が課題。 |
| MPCを明示的方策で置き換える研究の歴史的基盤。 | 2002 / **[The Explicit Linear Quadratic Regulator for Constrained Systems](https://doi.org/10.1016/S0005-1098(02)00174-9)** | 線形制約付きMPCの最適入力を状態の区分的アフィン関数としてオフラインで計算する。 | パラメトリックQPの解をExplicit MPCとして表現する枠組みを示し、オンラインQP求解を表引きに近い評価へ置換した。 | マイクロコントローラ向けMPC、MPC方策の学習代替との比較。 | 状態次元・制約数が増えると領域数が爆発し、メモリと探索コストが課題。 |
| 現代のRobust MPCの基本。 | 2005 / **[Robust Model Predictive Control of Constrained Linear Systems with Bounded Disturbances](https://doi.org/10.1016/j.automatica.2005.02.005)** | 有界外乱下の制約付き線形系を予測制御する。 | 名目軌道と誤差に対する固定安定化フィードバックを分離し、制約引き締めによる実行可能性・安定性の保証を示すTube MPCを定式化した。 | ロバストMPC、GP/学習モデルの誤差集合に対する制約保証。 | 外乱集合の既知性・保守性、チューブ計算、非線形系の扱いが課題。 |
| 会話で当初挙げた古典よりの選択肢。 | 2007 / **[L1 Adaptive Control: Theory and Applications](https://ieeexplore.ieee.org/search/searchresult.jsp?queryText=L1%20adaptive%20control%20Hovakimyan%20Cao%202007)** | モデル不確かさを有するシステムで高速適応とロバスト性を両立する。 | 適応推定と制御入力の帯域分離・フィルタ設計を特徴とするL1適応制御を体系化。論文題名・原典リンクは再確認が必要。 | 航空・ロボット・機械装置の適応フィードバック。 | 外乱とモデルクラスに関する仮定、入力フィルタによる性能制約、実機遅延。 |
| GP-MPCの実車／レース系への橋渡し。 | 2019 / **[Learning-Based Model Predictive Control for Autonomous Racing](https://arxiv.org/abs/1905.05122)** | 小型レーシングカーを対象に公称力学モデルと学習したモデル誤差を組み合わせ、限界走行でのMPC追従を改善。 | GPを含む学習ベースのモデル補正とMPCを実機レースへ適用し、データ学習と高性能走行を接続した。数値改善率は元回答で確認していない。 | 自動運転、タイヤ特性の変動を含むGP-MPCの実機適用。 | 学習データ範囲外・極限摩擦条件・確率的保証と実時間制約が課題。 |
| Differentiable MPCの応用例として紹介された研究。 | 2023 / **[DIPP: Differentiable Integrated Motion Prediction and Planning with Learnable Cost Function for Autonomous Driving](https://arxiv.org/abs/2207.10422)** | 自動運転で周辺車両の将来軌道の予測と自車の軌道最適化を一体化する問題。 | NN予測器→微分可能プランナ→走行軌道損失へ勾配を流し、予測と計画を別々に学習する方式より良好な軌道模倣・計画結果を報告。具体的な改善倍率は未確認。 | 予測・計画のEnd-to-end学習、自動運転のDifferentiable Optimization。 | 予測環境の分布シフト、非凸な最適化と衝突制約、模倣性能と閉ループ安全性の差が課題。 |
| 古典的Explicit MPCと新しいデータ駆動方策高速化を比較する。 | 2026 / **[Data-driven Acceleration of MPC with Guarantees](https://proceedings.mlr.press/v331/castellano26a.html)** | 多数のオフラインMPC解からオンラインで制御入力を高速に得る。 | データ被覆の条件下で実行可能性と最適性ギャップの保証を与え、前回説明では対象課題で通常のオンラインMPCより約100～1000倍高速だったと報告。 | Explicit MPCの近似、MPC蒸留、組込み制御器の高速化。 | 高次元空間で必要なデータ数、分布外状態、被覆保証、制約切替えが課題。 |
| Koopmanモデル学習よりもオンライン求解技術に重点を置く分岐。 | 2026 / **[Koopman-BoxQP: Solving Large-Scale NMPC at kHz Rates](https://proceedings.mlr.press/v331/wu26a.html)** | 高次元Koopman予測モデルに由来する構造化NMPCを、低遅延で解く。 | 持ち上げ変数の消去、BoxQP化、Warm Startと構造化内点法を組み合わせ、前回説明では1040変数・2080不等式制約の実験でkHz級の解法を報告。 | 高次元NMPC、高速線形代数、GPU/CPU最適化、Koopman制御。 | BoxQP構造への依存、学習モデルの予測誤差、一般NLPへの移植性。 |
| Neural Lyapunovの分散・時間遅れ方向への分岐。 | 2026 / **[Neural Vector Lyapunov–Razumikhin Certificates for Delayed Interconnected Systems](https://proceedings.mlr.press/v306/zhou26au.html)** | 複数サブシステムが遅延を伴って相互結合する系の安定性検証。 | Vector Lyapunov関数とRazumikhin型遅延条件をNN証明関数で扱い、車両隊列・マルチエージェント・電力系などで解析可能性を示したと前回説明。個別結果の再照合が必要。 | 通信遅延を持つ分散制御、ネットワーク化システム。 | 結合条件の保守性、遅延範囲の仮定、スケーラビリティ。 |
| Koopmanと統計的ロバスト性の接点。 | 2026 / **[Scalable Data-Driven Reachability Analysis and Control via Koopman Operators with Conformal Coverage Guarantees](https://proceedings.mlr.press/v331/nath26a.html)** | 学習したKoopmanモデルを用いて到達可能集合を予測し、近似誤差をデータで補正する。 | Conformal Predictionの誤差境界で到達可能集合を膨張させ、Hopper・Swimmer・クアッドコプター等で評価したと前回説明。被覆率の具体値は未転記。 | 学習ベースの安全なReachability、Koopman MPCのロバスト化。 | 較正データの交換可能性・分布シフトと有限データの制約、到達集合の保守性。 |
| Select-DPCと比較して読むと局所データの意味が分かる。 | 2026 / **[Taming Non-linearity with Local Data-driven Predictive Control](https://doi.org/10.1016/j.ifacsc.2026.100396)** | 非線形系で明示的な状態空間同定なしにローカルなデータ表現を作り、反復的にQPを解く。 | Sequential Data-driven QPという構成で局所モデルと最適化を更新し、倒立振子Swing-upで既存のデータ駆動・モデルベース手法と比較。数値改善率は未確認。 | 非線形DeePC、SQP型データ駆動最適制御。 | 局所線形性とデータ被覆、反復収束、実時間QP計算負荷。 |
| MPC解の微分と混同しない：DPCは予測ロールアウトで方策学習。 | 2026 / **[Zero-Shot Function Encoder-Based Differentiable Predictive Control](https://proceedings.mlr.press/v331/iqbal26a.html)** | パラメータの異なる非線形システム群で、追加訓練なしに適応する制御器を作る。 | Function Encoder型Neural ODEとDifferentiable Predictive Controlを組み合わせ、オフライン学習した明示的方策によるゼロショット適応を数値実験で検証。改善率の詳細は未転記。 | 類似装置間の制御方策の転移、オンライン最適化なしの高速制御。 | 事前学習分布外のモデルへの保証、安全性、学習モデル誤差、転移限界が課題。 |

**推奨順と理由**：最初にExplicit MPC（2002）・Tube MPC（2005）・N4SID（1994）などで、新規手法が置換・拡張した古典的な基準点を確認する。次にKabzanの学習レーシングMPC、DIPPの予測・計画の一体学習を読む。2026年の各研究は一本の直列発展ではなく、ゼロショット方策、遅延系保証、局所DeePC、Koopman高速QP、到達可能性、高速MPC代替という**分岐**として読む。

**この区分の収録件数：14件。**

> 補足：古典的な制御理論の原著6本と、2026年の派生研究等を同じ節に収録した。出版URLが個別論文ページでないものや書誌未確認のものは本文注記を参照。

<a id="sec-a"></a>
### A. Differentiable MPC・最適化層

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| QPのKKT感度を利用した微分可能な最適化層 | 2017 / **[OptNet: Differentiable Optimization as a Layer in Neural Networks](https://arxiv.org/abs/1703.00443)** | 画像分類・構造化予測などにQP層を組み込む実験と、GPUでのバッチQP求解を評価した。 | QPのKKT条件から厳密な局所解感度を計算する仕組みを実装し、制約を備えた学習可能層として機能することを示した。逆伝播の追加計算はQPの因数分解を再利用でき、著者らは「求解に対しごく小さい追加コスト」と報告している。 | 制約付き行動生成、学習可能なQP制御、逆最適化、制約付き機械学習に利用できる。 | QPの解の一意性、正則性および微分可能な有効制約集合を暗黙に仮定する。非凸NLPでは局所解・特異なKKT行列への対応が必要であり、改善率の一律な数値は示せない。 |
| 時系列最適制御の解感度を学習へ逆伝播 | 2018 / **[Differentiable MPC for End-to-end Planning and Control](https://arxiv.org/abs/1810.13400)** | 振子とCartPoleの模倣学習において、専門家の操作をMPC方策で再現する実験を行った。 | Box-DDP固定点のKKT条件から制御系列の感度を求め、コスト重みとダイナミクスを逆伝播で学習した。汎用NNより少ない教師データで学習でき、専門家がモデルクラス外の条件でも通常のシステム同定より優れる事例を示したが、具体的な改善率は要旨から確認できない。 | 逆最適制御、コスト学習、模倣学習、学習可能な非線形MPC層へ応用できる。 | 局所凸近似の固定点が得られることに依存し、非凸問題や有効制約集合の切替えで勾配が不安定になり得る。閉ループ安定性・安全性を自動で保証するものではない。 |
| 凸最適化層を汎用的に構築する枠組み | 2019 / **[Differentiable Convex Optimization Layers](https://papers.neurips.cc/paper_files/paper/2019/hash/9ce3c52fc54362e22053399d3181c638-Abstract.html)** | 画像処理等の学習課題で、パラメータ付き凸最適化問題をネットワークの内部に挿入して評価した。 | Disciplined Parametrized Programmingに従う凸問題を自動的に微分可能層へ変換し、Forwardの最適解とBackwardの感度を統合した。複数の学習実験で利用可能性を示しているが、汎用的な精度向上率は課題依存である。 | CVXPYを使った微分可能MPC、最適化を含む計算グラフ、パラメータ同定へ適用できる。 | 凸性とDPPに基づく定式化が必要で、一般の非凸NMPCは直接扱えない。大きなバッチや長ホライズンでは求解と感度計算のメモリが制約になる。 |
| 制約付きNMPCの解感度とacadosへの実装 | 2025 / **[Differentiable Nonlinear Model Predictive Control](https://arxiv.org/abs/2505.01353)** | 一般の制約付き最適制御問題で、SQP・内点法に基づく感度計算を既存の微分可能ソルバーと比較した。 | acadosに前向き・随伴解感度を実装し、原著要旨でmpc.pytorchおよびcvxpygenに対し3倍超の高速化を報告した。これは論文で設定したベンチマークにおける計算時間の比較である。 | 学習可能なNMPC、最適制御方策の勾配学習、モデル・コスト同定に利用できる。 | 最適性条件の正則性が必要であり、有効制約の切替えや局所最適解の非一意性は感度の信頼性を損なう。3倍超という数値は任意の問題・ハードウェアへ一般化できない。 |
| モデル誤差下で微分可能MPCを使って方策最適化 | 2026 / **[Policy Optimization for Unknown Systems using Differentiable MPC](https://proceedings.mlr.press/v331/zuliani26a.html)** | 状態が12次元の非線形クアッドコプター系で、未知・不正確なモデルに対するMPC方策学習を評価した。 | モデルから得る微分勾配とゼロ次推定を組み合わせ、モデルフリーのみの更新より初期の学習進行が速いことを示した。論文は一定の仮定下の収束解析も提示するが、改善率の数値は要旨では公開していない。 | モデル誤差を含むMPCの評価関数調整、閉ループ性能直接最適化、実機適応に適用可能である。 | ゼロ次勾配推定には試行回数を要し、未知実機への探索入力は安全性上の問題がある。収束保証は対象モデルと雑音に課された条件に依存する。 |

**推奨順と理由**：OptNetでQPのKKT条件と陰関数微分を押さえ、Amosで時系列のDDP/iLQRへ拡張する。Agrawalは一般の凸最適化層という並行発展として必要時に読む。Freyで実際の制約付きNMPCの感度計算とacados実装を学び、Zulianiで閉ループ性能最適化に進む。ポイントは「ソルバー内部のモデルヤコビアン」と「最適解のパラメータ感度」を区別すること。

**この区分の収録件数：5件。**


<a id="sec-b"></a>
### B. Neural Lyapunov・形式的検証

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 制御器とLyapunov関数の同時学習・反例探索 | 2019 / **[Neural Lyapunov Control](https://papers.neurips.cc/paper/8587-neural-lyapunov-control)** | 学習した安定化制御器とLyapunov関数について、数値的な反例探索と形式的条件検証を組み合わせた。 | ニューラル制御則とLyapunov候補を同時に更新し、反例が見つかる領域を修正して検証可能な安定領域を構築した。小規模な非線形系で方法を示したが、認証領域の倍率は今回の確認範囲では取得できなかった。 | ニューラル制御の安定領域推定、強化学習方策の証明付き設計に利用できる。 | 形式検証の計算負担が状態次元とNNサイズで増す。数値探索で反例が見つからないだけでは安定性証明にならず、モデル誤差も別途扱う必要がある。 |
| MIPを用いたNN制御器の安定性認証 | 2021 / **[Lyapunov-stable Neural-network Control](https://roboticsconference.org/2021/program/papers/063/index.html)** | 倒立振子、平面2次元・空間3次元のクアッドコプターを対象にLQRと比較した。 | NN制御器とLyapunov関数をMIPで検証し、対象とした3種類のロボット系で安定領域の内側近似を計算した。選定した条件下ではLQRを上回る制御性能を報告しているが、誤差低減率は要旨からは特定できない。 | 非線形ロボットの証明付き安定化、学習方策とLQRの性能比較に向く。 | MIPによる完全性を狙う検証はNN規模や状態次元の増加で高コストになる。保証は指定した力学・状態領域・モデル精度に限定される。 |
| 離散時間系の学習と反例誘導型検証 | 2023 / **[Neural Lyapunov Control for Discrete-Time Systems](https://proceedings.neurips.cc/paper_files/paper/2023/file/08bf1773e94763b6cc366ee7c6582f27-Paper-Conference.pdf)** | 離散時間非線形系で、Lyapunov関数と制御器の学習・反例に基づく検証を扱う。 | 連続時間を前提とする議論を離散時間の状態遷移に合わせ、Lyapunov減少条件に関する検証・学習の仕組みを示した。原著の具体的な改善率は今回の確認範囲では取得できていない。 | サンプル値制御、デジタル制御器、離散時間NN方策の安定性検証に役立つ。 | 離散時間の差分不等式を広い状態集合で保証するには高コストな検証が必要であり、外乱・モデル誤差への拡張が残る。 |
| 状態・出力フィードバックの保証付き学習 | 2024 / **[Lyapunov-stable Neural Control for State and Output Feedback: A Novel Formulation](https://proceedings.mlr.press/v235/yang24f.html)** | NN状態フィードバックだけでなく、NN観測器を含む出力フィードバック系を対象にした。 | 従来より大きな認証可能安定領域を持つ定式化を提案し、GPU上でBranch-and-Boundによる事後検証を実行した。著者らは出力フィードバックのNN制御器・観測器の安定性を形式的に示したが、要旨中に領域拡大倍率はない。 | センサだけから制御するNN出力フィードバック系、観測器と制御器の一体学習に応用できる。 | 認証は指定した領域・近似モデルに限定され、GPU利用でも高次元系の大域的な認証は難しい。定量的な改善倍率は本表では確認できていない。 |
| 検証境界を損失関数に組み込む認証型学習 | 2026 / **[Certified Training with Branch-and-Bound for Lyapunov-stable Neural Control](https://proceedings.mlr.press/v331/shi26a.html)** | 状態・出力フィードバックを含むNN制御器の認証可能な安定領域を、従来のCEGIS方式と比較した。 | 最大規模の実験として挙げた2Dクアッドローター出力フィードバック系で、従来比11倍超の検証高速化と164倍の認証可能ROA（引込み領域）を報告した。 | 形式検証を含むNN制御器の訓練、広い安定領域を必要とする制御設計に応用できる。 | 11倍超・164倍は論文中の特定の2D系での比較であり、他の次元・ネットワークでも達成されるとは限らない。分枝分割の組合せ的な計算負担は残る。 |

**推奨順と理由**：ChangでLyapunov減少条件を損失と反例探索に利用する基本を理解し、DaiでMIPによる**証明**との違いを確認する。Wuの離散時間系、Yangの出力フィードバックを経由し、ShiのCT-BaBで「訓練と検証の一体化」へ進む。最初は低次元系で証明領域と計算時間を必ず同時に測る。

**この区分の収録件数：5件。**


<a id="sec-c"></a>
### C. Control Contraction Metrics

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 非線形系の微分収縮性に基づくフィードバック合成 | 2017 / **[Control Contraction Metrics: Convex and Intrinsic Criteria for Nonlinear Feedback Design](https://arxiv.org/abs/1503.03144)** | 連続時間の非線形制御アフィン系について、収縮計量を使ったフィードバック安定化を理論解析した。 | 状態依存の正定値Metricが満たす凸条件から、参照軌道間の指数収縮を保証する制御則を導出した。実験の改善率ではなく、収縮率を示す指数境界が主要成果である。 | 非線形追従制御、ロバスト制御、軌道依存安定性の証明に使える。 | Metricの探索が難しく、高次元系では条件評価が重い。原論文の定理の一部には後続の技術的条件の補足があるため訂正論文との併読が必要である。 |
| 収縮理論の安定性・ロバスト性・学習制御への総説 | 2021 / **[Contraction Theory for Nonlinear Stability Analysis and Learning-based Control: A Tutorial Overview](https://doi.org/10.1016/j.arcontrol.2021.10.001)** | Contraction理論による非線形安定性・ロバスト制御・推定・学習応用の研究を横断的に整理した総説である。 | 収縮率、入力外乱に対する誤差上界、制御・推定の設計条件を統一的に説明している。新規ベンチマークや性能改善率を報告する実験論文ではない。 | CCM研究の理論整理、神経収縮計量やロバスト追従の先行研究探索に有用である。 | 総説自体は新しい制御器の実機性能を検証していない。扱う理論保証は正定値計量などの存在・検証可能性に依存する。 |
| Contraction Metricと制御方策の同時学習（C3M） | 2021 / **[Learning Certified Control Using Contraction Metric](https://proceedings.mlr.press/v155/sun21b.html)** | 学習済み非線形力学を含むロボットモデルで、SOS、RL、MPCとの比較を行った。 | 制御器と収縮MetricをNNで同時学習し、対象実験ではSOS・RL・MPC系比較手法より小さい追従誤差と速い実行を報告した。改善倍率は要旨で明記されていない。 | 非線形軌道追従、ロボットの有界外乱下での追従誤差保証に適用できる。 | 保証は制御アフィン系などのモデル仮定とMetric条件の検証に依存する。高次元での全領域検証、モデルバイアスへの耐性はなお課題である。 |
| 離散時間の学習Metricを形式的に検証 | 2025 / **[Neural Contraction Metrics with Formal Guarantees for Discrete-Time Nonlinear Dynamical Systems](https://proceedings.mlr.press/v283/li25e.html)** | Van der Pol系、多項式系、二機電力系、倒立振子でNN Contraction Metricを構築・検証した。 | Van der Polでは正規化検証領域の100%（一定Metric比較33.7%）を109秒で認証し、二機電力系では100%（一定Metric31.3%）を62秒で認証した。倒立振子では88.6%を認証したが、検証に5.8時間を要した。 | 連続微分できないNNを含む離散時間系の収縮性証明、安定領域の拡大に利用できる。 | 対象例によって検証時間が62秒から5.8時間へ大きく変動するため、高次元化・領域拡大に対する計算負荷が重い。証明は設定した検証領域の内部に限られる。 |
| Contraction保証とRLによる性能改善の統合 | 2026 / **[Contraction-Aware Reinforcement Learning for Nonlinear Control with Statistical Robustness](https://doi.org/10.1109/TRO.2026.3727138)** | シミュレーションと実ロボットの経路追従で、近似ダイナミクスを使ったCCM制御とRLの融合を比較した。 | 収縮性を利用した方策学習CARLを提案し、比較方式より追従性能・モデル誤差への頑健性が良好と報告した。改善百分率は公開要旨で確認できない。 | モデル誤差が残るロボットの追従制御や安定性を意識したRLへ利用できる。 | 理論保証はCCMを構成する近似モデルと仮定に依存し、報酬最適化自体が任意の未知外乱に対する安全保証を与えるわけではない。 |

**推奨順と理由**：Manchester & SlotineのCCMで収縮計量の不等式を導出する。Tsukamotoらの2021年チュートリアル総説でContraction理論の体系を確認してからSunのC3Mへ進み、2025年の形式的認証と2026年のRL統合を読み比べる。理論的な収縮率と実験的な平均追従誤差は別指標として扱う。

**この区分の収録件数：5件。**


<a id="sec-d"></a>
### D. DeePC・データ駆動予測制御

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 持続励起データからLTI軌道を表現する基本補題 | 2005 / **[A Note on Persistency of Excitation](https://doi.org/10.1016/j.sysconle.2004.09.003)** | LTI系の入出力列からHankel行列を作り、持続励起と軌道表現の関係を理論的に解析した。 | 入力が必要次数で持続励起されれば、長さLのすべての許容軌道が一つのHankelデータ行列の列空間で表現できることを示した。主要成果は実験改善率ではなくランク条件による厳密な表現性である。 | DeePC、データ駆動状態推定、線形システムの行動的同定の基礎になる。 | ノイズなしLTI系と十分な持続励起が本質的な前提であり、非線形系・入力飽和・ノイズ下ではそのままの等価性は成立しない。 |
| Hankelデータからモデルを明示せずMPCを構築 | 2019 / **[Data-Enabled Predictive Control: In the Shallows of the DeePC](https://arxiv.org/abs/1811.05890)** | 未知LTI系の軌道追従・入力制約付き制御と、非線形・確率系の数値シミュレーションを扱った。 | 有限の入出力データから予測軌道を最適化し、ノイズなし決定論的LTI系では適切な条件下で従来MPCと等価な入力を得ることを証明した。ノイズ系では正則化による実験的改善を示したが数値倍率は未確認。 | モデルを明示的に同定しないMPC、未知プラントのオンライン制御に利用できる。 | ノイズなしLTIという等価性の条件は強く、ノイズ・非線形・データ量増加により正則化・次元削減・安定性解析が必要になる。 |
| 直接データ駆動MPCの安定性・ロバスト性 | 2021 / **[Data-Driven Model Predictive Control with Stability and Robustness Guarantees](https://doi.org/10.1109/TAC.2020.3000182)** | ノイズのある未知LTIシステムの追従制御に対し、データ駆動MPCのロバスト安定性を解析した。 | 測定ノイズなどを考慮したMPC設計で、明示的な同定モデルに頼らずに閉ループ実用安定性を保証する十分条件を提示した。性能改善率より、ノイズ上界と設計条件の明確化が主要成果である。 | 制御保証を重視するDeePC系、未知線形プラントの実装に有用である。 | 小さなノイズ、十分な励起、特定の重みやホライズンなどの条件を要する。一般の非線形・時変系への厳密な保証はそのまま適用できない。 |
| 局所データ更新による非線形システムの追従制御 | 2022 / **[Linear Tracking MPC for Nonlinear Systems—Part II: The Data-Driven Case](https://arxiv.org/abs/2105.08567)** | 未知の非線形システムを、オンラインの入出力データで局所近似しながら参照追従する問題を扱った。 | 逐次更新するデータ駆動線形MPCにより、一定の局所条件下で非線形系の実用的追従保証を与え、数値例で追従を示した。定量的な追従誤差の改善率は要旨では確認できない。 | 局所線形化モデルを明示同定しないNonlinear Data-Driven MPCに適用できる。 | 動作点に近い局所データ、十分な励起、局所モデル誤差の小ささに依存する。大域非線形性や急激なモード遷移に対応するには追加工夫が必要である。 |
| 直接法と間接法の関係を正則化の観点から整理 | 2023 / **[Bridging Direct and Indirect Data-Driven Control Formulations via Regularizations and Relaxations](https://doi.org/10.1109/TAC.2022.3148374)** | 直接データ駆動制御とモデル同定を経由する間接法を、正則化・緩和を通して比較した。 | 両形式が特定の正則化と制約緩和の下で対応することを解析し、従来異なると見られていた設計問題の接点を明らかにした。主要成果は定理による関係の解明で、実験上の一律の改善率ではない。 | DeePCの正則化設計、間接同定型MPCと直接法の比較に利用できる。 | 対応関係は仮定したデータ・同定・正則化に依存する。非線形、強雑音、オンライン計算時間を含む総合性能差は別途評価が必要である。 |
| 局所的に有用なデータをオンライン選択するSelect-DPC | 2026 / **[Choose Wisely: Data-driven Predictive Control for Nonlinear Systems Using Online Data Selection](https://proceedings.mlr.press/v331/naf26a.html)** | ロケット着陸、ロボットアーム、CartPoleの振り上げという3種類の非線形シミュレータでDeePCとTime-Windowed DeePCを比較した。 | 現在の運転点に関連する軌道データをオンライン選択するSelect-DPCが、3種類の評価系で両比較法を上回る制御性能を示した。誤差削減率・成功率の具体値は公開要旨に記載されていない。 | 非線形系の局所データ駆動制御、履歴データが大量にある装置へのオンラインMPCに向く。 | 選択データの密度・多様性に性能が左右され、局所データが足りない領域で精度が落ちる。一般の非線形系について広域の安定性・制約保証はなお課題である。 |

**推奨順と理由**：Willemsの基本補題でHankel行列による有限長軌道表現の前提を理解し、CoulsonのDeePCを再実装する。Berberich（2021）でノイズ下の安定性・ロバスト性を確認し、2022年の非線形追従と2023年の直接法／間接法の比較へ進む。最後に2026年のSelect-DPCを読み、「局所データ選択がどの仮定を緩和し、何を未解決のまま残したか」を比較する。

**この区分の収録件数：6件。**


<a id="sec-e"></a>
### E. Koopman MPC・安定性保証

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 非線形系の埋め込み座標を深層学習 | 2018 / **[Deep Learning for Universal Linear Embeddings of Nonlinear Dynamics](https://doi.org/10.1038/s41467-018-07210-0)** | 非線形振動や流体系のデータで、線形化しやすい潜在座標をオートエンコーダに学習させた。 | 学習した座標上で近似線形の時間発展を表せることを数値例で示し、あらかじめ辞書を手作業で選ぶ必要性を減らした。汎用的な予測誤差改善率は問題ごとに異なる。 | Deep Koopman、潜在状態MPC、非線形システムの低次元表現学習に利用できる。 | 有限データとNN構造に依存し、安定性、可観測性、物理的意味、分布外入力下の予測を自動では保証しない。 |
| 持ち上げ線形モデルで非線形MPCを実現 | 2018 / **[Linear Predictors for Nonlinear Dynamical Systems: Koopman Operator Meets Model Predictive Control](https://arxiv.org/abs/1611.03537)** | 非線形振動系や高次元PDEなどに対し、データから持ち上げ線形予測器を構築しMPCに用いた。 | EDMD型の最小二乗同定で線形予測器を構築し、論文の数値例では通常の局所線形化等より良好な予測を報告した。MPCのオンライン最適化は持ち上げ状態の次元に応じた線形QPの構造にできる。 | 高速な非線形予測制御、線形MPCソルバーの再利用、PDEを含むデータ駆動制御に適用できる。 | 持ち上げ辞書と学習範囲によって予測精度が変わり、有限次元近似の誤差が閉ループ安定性・制約違反を引き起こし得る。 |
| Kernel EDMD予測誤差から実用安定性を解析 | 2025 / **[Kernel EDMD for Data-Driven Nonlinear Koopman MPC with Stability Guarantees](https://doi.org/10.1016/j.ifacol.2025.11.080)** | Kernel EDMDで非線形系の予測モデルを同定し、終端安定化制約を課さないMPCの数値シミュレーションを検証した。 | 状態に比例する点ごとのモデル誤差上界を前提に、終端安定化条件を置かずにKoopman MPCの原点への実用的漸近安定性を証明した。数値シミュレーションでも理論を検証したが、具体的な改善率は公開要旨にはない。 | Kernel Koopman MPC、学習モデルの誤差境界を利用した安定性保証に利用できる。 | 点ごとの誤差上界をデータから実証する必要があり、得られるのは原点付近の実用的漸近安定性である。大域的な厳密安定化や任意のモデル誤差には適用できない。 |
| 安定性を構造的に組み込んだKoopman埋め込み | 2026 / **[Learning Stable Koopman Embeddings for Identification and Control](https://doi.org/10.1016/j.automatica.2026.113003)** | 同定データから安定性・安定化可能性を持つKoopman埋め込みを学習し、制御課題を評価した。 | 安定性の構造を持ち上げモデルのパラメータ化に組み込むことにより、単に予測誤差を最小化するだけの設計と異なる制御向け表現を示した。数値的な誤差削減割合は原著要旨からは確認できない。 | 学習モデルに保証を埋め込むKoopman-MPC、構造保存型システム同定に使える。 | モデルの表現力と構造制約の両立が課題で、学習分布外での真の非線形系の安定性が無条件に保証されるわけではない。 |
| Koopman MPCの元の非線形系に関する指数安定性 | 2026 / **[On the Exponential Stability of Koopman Model Predictive Control](https://proceedings.mlr.press/v331/shang26a.html)** | 倒立振子に対してKoopman LQR/MPCとTaylor線形化型MPCを比較し、元の非線形系の安定性を解析した。 | 持ち上げ写像と1ステップ誤差が原点でLipschitzという条件などの下で、局所指数安定性を証明した。倒立振子の数値例では線形化MPCより収束挙動と累積コストが改善したが、要旨に具体的な割合はない。 | Koopman MPCの終端コスト・終端集合設計と閉ループ安定性評価に利用できる。 | 保証は原点近傍の局所結果であり、持ち上げ写像・誤差のLipschitz条件を実データから検証する難しさが残る。 |

**推奨順と理由**：Korda & MezićでEDMD→線形QPの実装を作り、Luschで持ち上げ座標の深層学習を試す。2026年のKoopman理論Tutorialを横に置いて有限データ誤差を整理し、Kernel EDMD、Fanの構造的安定性、Shangの真の非線形系の指数安定性を比較する。Koopman-BoxQPは安定性証明とは別の、高速求解法の枝として読む。

**この区分の収録件数：5件。**


<a id="sec-f"></a>
### F. Safety Filter・CBF

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| CBFの安全条件をQP制約へ組み込む | 2017 / **[Control Barrier Function Based Quadratic Programs for Safety Critical Systems](https://doi.org/10.1109/TAC.2016.2638961)** | ロボット・制約付き非線形系で、既存の目標追従制御入力を安全条件に従って修正するQPを扱った。 | CBFをQP制約に組み込むことで、適切な可行性・正則性条件下で安全集合の前方不変性を保証できる枠組みを与えた。制御誤差や実行速度の改善割合は主要成果ではない。 | 衝突回避、安全フィルタ、入力制限を持つ安全重要制御へ利用できる。 | CBF条件の可行性が入力飽和や複数障害物で失われる可能性がある。モデル誤差、測定誤差、高次相対次数への対応が必要である。 |
| 学習器の入力を予測軌道による安全制約で修正 | 2021 / **[A Predictive Safety Filter for Learning-Based Control of Constrained Nonlinear Dynamical Systems](https://doi.org/10.1016/j.automatica.2021.109597)** | 非線形システムで学習方策が提案する制御入力を、予測制約と終端安全性に照らして修正した。 | 有限ホライズンの安全計画で、元の学習方策からの入力偏差を小さくしながら安全制約を維持する仕組みを示した。実験の成功率や改善倍率は公開要旨に含まれていない。 | 学習済みRL方策に後付けする安全層、モデル予測型の安全入力修正に適用できる。 | 安全なバックアップ集合・終端条件とモデル妥当性を必要とする。計算時間や未知外乱が大きい場合、予測フィルタ自体の可行性が課題になる。 |
| CBF/HJ/MPC安全フィルタを比較・整理 | 2024 / **[The Safety Filter: A Unified View of Safety-Critical Control in Autonomous Systems](https://doi.org/10.1146/annurev-control-071723-102940)** | CBF、Reachability、予測型安全フィルタを統一的に比較した総説であり、独自の新規実験は目的としていない。 | 安全集合、入力修正、到達可能性、予測時間という観点で異なる安全フィルタの前提と保証を整理した。比較実験に基づく一つの改善率を報告する論文ではない。 | 制御対象の危険状態・モデル精度・計算予算に応じたSafety Filter設計に使える。 | 統一的な概念整理にとどまり、安全が成立するためのモデル妥当性・集合の非空性・リアルタイム性は個別方式で検証が必要である。 |
| HJ到達可能性とモデルフリー価値学習の融合 | 2026 / **[Deep QP Safety Filter: Model-free Learning for Reachability-based Safety Filter](https://proceedings.mlr.press/v331/kim26c.html)** | ハイブリッドシステムを含む複数の動的システムとRLタスクで、学習型HJ価値関数に基づく安全QPを評価した。 | モデル知識なしで価値関数とその導関数を学習し、比較方式より学習収束前の失敗を減らしつつ最終報酬を高める傾向を示した。要旨では削減率や報酬値の具体値を公表していない。 | 未知ダイナミクスの安全学習、モデルフリーRLに付加するQP型安全フィルタに適用できる。 | 価値関数の近似誤差や分布外状態での安全性保証は非自明である。理論の厳密な収束は指定された極限条件に依存し、有限データ実機での無条件保証ではない。 |
| 潜在状態上で滑らかなCBF安全フィルタを学習 | 2026 / **[How to Train Your Latent Control Barrier Function: Smooth Safety Filtering Under Hard-to-Model Constraints](https://proceedings.mlr.press/v331/nakamura26a.html)** | シミュレーションと画像ベースの実機マニピュレーションで、通常方策と安全方策を切り替える方式と比較した。 | 滑らかな潜在CBFを学習することで、従来のスイッチング型Safety Filterと比べて、対象実験の成功率を約2倍にしたと報告している。 | 画像観測からのロボット操作、安全な潜在状態フィルタ、スムーズな制御介入に利用できる。 | 成功率2倍は論文中の比較課題に依存し、画像の分布変化や潜在状態の不完全性の下で一般的な安全性が保証されるわけではない。 |

**推奨順と理由**：AmesのCBF-QPで前方不変性と入力補正の定式化を理解し、Wabersich & Zeilingerでホライズンを用いた予測的安全フィルタと比較する。安全フィルタ統一総説と2024年のCBF実装課題Tutorialを挟み、Deep QP Safety FilterとLatent CBFに進む。安全集合の数学的保証と実機の成功率を混同しないこと。

**この区分の収録件数：5件。**


<a id="sec-g"></a>
### G. SLS・分散／ロバスト制御

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 制御器ではなく閉ループ応答を直接設計する枠組み | 2016 / **[A System Level Approach to Controller Synthesis](https://arxiv.org/abs/1610.04815)** | 線形システムの閉ループ応答を直接最適化変数とする制御器合成を理論的に定式化した。 | 外乱から状態・入力への応答をアフィン等式で表し、内部安定性を満たす制御器を応答上の凸制約で扱えることを示した。主要成果は数値的な改善率ではなくパラメータ化の等価性である。 | 分散制御、通信制約付き制御、大規模ロバスト設計に適用できる。 | 理論の基本形は線形系を前提とする。応答の有限ホライズン近似や複雑な通信制約では問題サイズと実装負荷が増大する。 |
| SLSの安定化、局所性、分散制御、ロバスト性を体系化 | 2019 / **[System Level Synthesis](https://arxiv.org/abs/1904.01634)** | SLSによる集中・分散制御や局所制約を、理論と複数の設計例を通して体系化した。 | 閉ループ応答の自由度を直接制約することで、局所性・情報制約を表現する統一的設計枠組みをまとめた。総説として数値的な性能向上を新たに一律で実証したわけではない。 | ネットワーク化制御、車両隊列、分散MPC、ロバスト設計の学習基盤となる。 | 応答の近似次数と系統規模が計算負荷に影響し、強非線形・ハイブリッド力学には直接適用できない。 |
| Riccati再帰によりSLS-MPCのQP構造を高速処理 | 2024 / **[Fast System Level Synthesis: Robust Model Predictive Control using Riccati Recursions](https://doi.org/10.1016/j.ifacol.2024.09.027)** | 有限ホライズンのSLS型ロバストMPCについて、Riccati再帰を使う数値最適化アルゴリズムを扱った。 | 問題構造を利用した再帰計算で、汎用的な密行列QPより効率的にロバストMPCを解く方法を提示した。論文の具体的な計算時間比は今回の確認範囲で未検証。 | 高周期ロバストMPC、組込み向けSLSソルバーの設計に利用できる。 | 高速化はブロック構造などの特定の定式化に依存し、一般の非凸NLPにそのまま拡張できるわけではない。 |
| 多面体モデル不確かさを含むSLSロバストMPC | 2024 / **[Robust Model Predictive Control with Polytopic Model Uncertainty through System Level Synthesis](https://doi.org/10.1016/j.automatica.2023.111431)** | 行列A,Bがポリトープ状に変化する不確かさと加法外乱を持つ線形制約系で、SLSロバストMPCを評価した。 | 閉ループ外乱応答を用いた制約の引き締めによって、不確かなモデルに対して制約充足を設計できることを示した。定量的な比較倍率は公開情報から検証できなかった。 | モデル誤差が集合で評価できる多変数・分散系のロバストMPCに利用できる。 | 不確かさ集合を正しく指定する必要があり、ポリトープ頂点や予測ホライズンが増えると計算負荷と保守性が増す。 |
| Conformal Predictionによる誤差境界とSLSの融合 | 2026 / **[Safety Beyond the Training Data: Robust Out-of-Distribution MPC via Conformalized System Level Synthesis](https://proceedings.mlr.press/v331/srinivasan26a.html)** | 状態4次元の車両モデルと12次元のクアッドコプターで、学習分布外のモデル誤差を考慮したSLS-MPCを評価した。 | 重み付きConformal Predictionからモデル誤差集合を学び、SLSによる到達集合と制約引き締めへ反映した。対象の4D・12D非線形系では、固定誤差境界・ロバスト化なしの比較法より安全性・信頼性が改善したが、要旨に改善率はない。 | 学習済み動力学を使うロバストNMPC、データ密度に応じた不確実性境界の設計に利用できる。 | 被覆保証は較正データ、分布シフト仮定、誤差モデルの妥当性に依存する。いかなる未知環境でも無条件に安全となるわけではない。 |

**推奨順と理由**：2016年のSystem Level Approachで閉ループ応答を決定変数にする発想を学び、2019年の体系化論文で分散・局所性制約と対応づける。2024年のPolytopic Robust MPCとRiccati再帰による高速化を先に理解すると、2026年のConformalized SLSで統計的な誤差境界がどこに追加されたかが分かりやすい。

**この区分の収録件数：5件。**


<a id="sec-h"></a>
### H. GP-MPC・確率的モデル予測制御

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| GPの確率的予測と分散をMPCへ導入 | 2004 / **[Gaussian Process Model Based Predictive Control](https://mlg.eng.cam.ac.uk/pub/pdf/KocMurRasGir04.pdf)** | 非線形プロセスのGP予測モデルを利用し、将来出力の平均と不確実性をMPCに利用した。 | 物理モデルが不十分でもGPによって非線形入出力関係を表し、モデル不確実性を予測に組み込めることを初期の制御例で示した。改善の百分率は今回の確認範囲で取得できていない。 | 化学・プロセス制御の学習型MPC、未知非線形プラントの確率的予測に適用できる。 | GPの標準的な訓練計算はデータ数に対して概ね3乗で増え、長期分布伝播も重い。分布の正規近似が実系で正しいとは限らない。 |
| GP残差モデル・共分散伝播・確率制約 | 2020 / **[Cautious Model Predictive Control Using Gaussian Process Regression](https://arxiv.org/abs/1705.10702)** | 小型自動運転レーシングカー等を念頭に、GP残差・状態共分散・確率制約を統合したMPCを評価した。 | 公称力学とGP残差学習を組み合わせ、将来共分散を伝播して確率的制約を引き締めるCautious MPCを提案した。モデル誤差に慎重な挙動を示したが、要旨で汎用的な成功率・改善率は示していない。 | 不確かな摩擦特性を持つ車両、制約付きロボット、確率的な安全余裕設計に応用できる。 | 状態分布のガウス近似、共分散伝播の線形化、GP校正精度を仮定する。共分散まで最適化すると計算量が大きく、真の違反確率保証には追加条件が必要となる。 |
| 共分散関連のSQP感度を近似し計算量削減 | 2023 / **[Zero-Order Optimization for Gaussian Process-Based Model Predictive Control](https://arxiv.org/abs/2211.15522)** | GP共分散を最適化変数に含める従来の確率的MPCと、共分散感度の一部を近似するSQPを比較した。 | Zero-order SQPによって、対象の疎QP求解について状態次元n_xに対する理論的複雑度をO(n_x^6)からO(n_x^3)へ低減できると示し、数値例でスケーリングを検証した。 | 実時間GP-MPC、GP残差を含むChance-Constrained NMPC、SQP-RTIの高速化に使える。 | 計算量低減は特定構造のQPの各反復に関する値であり、GP推論・全システム時間の改善倍率を示さない。微分を近似するため通常のNLP局所最適性は一般に失われる。 |
| GPを用いた動的モデル学習とMPCの総説 | 2025 / **[Gaussian Processes for Dynamics Learning in Model Predictive Control](https://doi.org/10.1016/j.arcontrol.2025.101034)** | GP-MPCの予測平均、共分散、オンライン学習、ロバスト制約、計算効率などを既存研究から整理した総説である。 | GPの平均だけを使うMPCから、共分散を用いる確率的MPCと実時間近似までの技術的課題を体系化した。独自の比較実験に基づく一律の改善率はない。 | GP-MPCの研究課題整理、近似推論とロバストMPCの手法選択に適している。 | モデル誤差がGPの予測分散に十分反映されるとは限らず、オンライン更新時の計算負荷と安全性保証の両立が未解決である。 |
| GPモデルとacadosを接続した実時間GP-MPC | 2026 / **[L4acados: Learning-Based Models for acados, Applied to Gaussian Process-Based Predictive Control](https://arxiv.org/abs/2411.19258)** | ニューラルネットワークのモデルをacadosに埋め込むベンチマークと、小型レーシングカー・実車ISOレーンチェンジという2件の実機応用を扱った。 | Pythonで得た外部感度をacadosのSQPへ渡すL4acadosを実装し、既存ソフトより高速・良好な計算量スケーリングを報告した。GP-MPCを小型車およびフルスケール車の2種類のハードウェアで実証したが、全体の改善倍率は要旨で定量化されていない。 | GPyTorch・NNモデルを使うリアルタイムNMPC、acadosベースのモデル誤差補償制御に応用できる。 | 外部感度計算・ソルバー連携・GPU/CPU転送に性能が左右される。実機の2例は汎用的な安全保証を示すものではなく、別対象では計算時間と信頼性の評価が必要である。 |

**推奨順と理由**：2004年の初期GP-MPCで確率的予測の動機を確認し、Stochastic MPC TutorialとWang–ZhangのGP-MPC Tutorialで予測平均・共分散伝播を丁寧に導出する。HewingのCautious GP-MPCを実装し、LahrのZero-order近似によるSQP計算負荷の低減を加え、最後にL4acadosへ進む。モデルの予測分散と真の誤差確率の差にも注意する。

**この区分の収録件数：5件。**


<a id="sec-i"></a>
### I. Neural Operator・PDE制御

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| Fourier空間でPDEの作用素を学習 | 2021 / **[Fourier Neural Operator for Parametric Partial Differential Equations](https://arxiv.org/abs/2010.08895)** | Burgers方程式、Darcy流、Navier–Stokes方程式の数値解を学習し、高解像度の予測を比較した。 | Fourier Neural Operatorは固定解像度の比較で既存の学習法より高精度と報告され、乱流予測のゼロショット高解像度化を実証した。特定のベンチマークでは従来PDEソルバーより最大約10^3倍高速だった。 | 流体・熱・拡散PDEの高速代理モデル、シミュレーション高速化、分布定数系のMPCに適用できる。 | 最大10^3倍は特定実装と解像度での推論比較であり、学習時間は含まない。分布外パラメータ、長時間積分、保存則や誤差保証は別途検討が必要である。 |
| 入力関数から出力関数の非線形作用素を学習 | 2021 / **[Learning Nonlinear Operators via DeepONet Based on the Universal Approximation Theorem of Operators](https://doi.org/10.1038/s42256-021-00302-5)** | 多様な入力関数からPDE解を予測する例について、BranchネットとTrunkネットによる作用素近似を評価した。 | DeepONetが関数空間間の非線形写像を学習できることを複数のPDE・作用素ベンチマークで示した。原著の多様な課題を横断する単一の改善率はない。 | 熱・流体・構造解析の代理モデル、逆問題、PDE制御器の学習に応用できる。 | 測定点配置と訓練用関数の分布に強く依存する。高周波現象、未観測条件、長期安定性は別途保証が必要である。 |
| 物理法則の残差をNeural Operator学習に統合 | 2021 / **[Physics-Informed Neural Operator for Learning Partial Differential Equations](https://arxiv.org/abs/2111.03794)** | 粗い空間解像度のデータと、より細かい解像度のPDE残差を組み合わせ、流体等を予測した。 | Physics-Informed Neural Operatorは、粗い教師データだけで学ぶFNOに物理制約を加え、訓練解像度より細かなゼロショット予測でも精度の劣化を抑えた。Kolmogorov流など、通常のPINNで最適化が難しい設定への適用も示した。 | 物理制約付き代理モデル、限られた測定データを用いるPDE同定と予測制御に使える。 | 物理残差を高解像度で評価する学習コストが高く、PDE自体が未知・誤差を持つ場合は制約が逆効果になり得る。制御系の安定性は直接示していない。 |
| PDEモデルの微分可能予測に基づいて方策学習 | 2025 / **[Learning to Control PDEs with Differentiable Predictive Control and Time-Integrated Neural Operators](https://arxiv.org/abs/2511.08992)** | 熱伝導、非粘性Burgers、反応拡散の3種のPDEで追従・制約・曲率低減を学習制御器で試した。 | TI-DeepONetを通じたDifferentiable Predictive Controlでオフラインに方策を学習し、推論時の制御入力生成をNMPCベンチマークより約10^4〜10^5倍高速化したと著者らは報告した。制御対象と比較実装を固定した実験値である。 | 分布定数系の高速な明示的NN制御、熱・拡散・流体系でのオンライン最適化の代替に利用できる。 | オンライン速度は極めて速いがオフライン学習費用を含まず、予測器のモデル誤差と安全制約の一般的保証は残る。数値倍率は論文の3種のPDE設定に依存する。 |
| センサ・アクチュエータ数に依らない作用素方策 | 2026 / **[CINOC: Cardinality-Invariant Neural Operator Policies for Scalable PDE Control](https://proceedings.mlr.press/v306/zanotta26a.html)** | 線形・非線形・カオス・乱流PDEの追従、安定化、密度輸送で、センサ数・エージェント数を変えて評価した。 | Neural Operatorで状態場から制御場への方策を学習し、小規模群で訓練した同じパラメータを大規模群へ再学習なしで転移させた。部分的なエージェント故障に耐性がある例も示したが、群規模の具体倍率は公開要旨にない。 | センサ・アクチュエータの配置が変化するPDE制御、マルチエージェント協調制御に適用できる。 | ゼロショット転移は論文で扱った作用素・環境・群規模での実証であり、任意の密度や物理相互作用への適用を保証しない。訓練時の微分可能PDEソルバーも必要である。 |

**推奨順と理由**：まずFNOとDeepONetを比較して関数間写像の意味を理解し、Physics-Informed Neural Operatorで物理残差を導入する。その後、SarkarのPDE-DPCで学習代理モデルを方策学習に接続し、CINOCでセンサ・アクチュエータ構成の変化への汎化を学ぶ。数値PDEソルバーと比較する際は学習時間を含むか区別する。

**この区分の収録件数：5件。**


<a id="sec-j"></a>
### J. In-Context Dynamics Learning

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 系集合の事前学習を未知システムの予測に活用 | 2023 / **[From System Models to Class Models: An In-Context Learning Paradigm](https://arxiv.org/abs/2308.13380)** | 異なるパラメータを持つ多数の合成動的システムで、1ステップ予測と多段シミュレーションを評価した。 | Transformerを合成システム群で事前学習し、新しい系の入出力履歴だけからモデルの再学習なしに予測可能であることを示した。既存同定法に対する普遍的な誤差改善率は要旨では確認できない。 | 対象ごとに再同定を行わないメタ同定、オンライン予測器、少量データでの適応に利用できる。 | 事前学習時のシステム分布・Context長によって性能が変動し、真に未知な物理構造や分布外対象への安定した汎化は未解決である。 |
| ICL同定におけるTransformerの表現力と深さを理論解析 | 2025 / **[In-Context Learning of Linear Dynamical Systems with Transformers: Approximation Bounds and Depth-Separation](https://proceedings.neurips.cc/paper_files/paper/2025/file/f7ae4fe91d96f50abc2211f09b6a7e49-Paper-Conference.pdf)** | 線形状態空間モデルの集合を対象に、Transformerの層数とIn-Context同定能力を理論解析した。 | 適切な構成のTransformerによって線形力学の同定手続きを近似できる条件を示し、ネットワーク深さによって表現可能な処理が変わることを理論化した。主要成果は近似境界・深さ分離の定理であり、実機改善率ではない。 | メタシステム同定の理論設計、Transformer予測器の容量・深さ選択に活用できる。 | 主な理論対象は線形系であり、非線形・未観測状態・長期閉ループ安定性へ直接保証を拡張できない。 |
| 文脈長・環境多様性が未知系への適応に与える影響 | 2026 / **[Context and Diversity Matter: The Emergence of In-Context Learning in World Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/a46b9dcef23e9160f46d800a49a931d0-Abstract-Conference.html)** | 多様な環境のWorld Modelに対し、Contextの長さと訓練環境の多様性を変えて適応能力を評価した。 | 長い観測履歴と多様な学習環境が、未知環境のダイナミクスを文脈内で推定する能力の獲得に重要であると示した。比較改善率は確認した要旨からは取得できていない。 | 汎用World Model、未知動特性への文脈内適応、モデルベースRLの事前学習に有用である。 | 環境の同定と新しい力学則を学習する能力は異なり、訓練分布から大きく外れる場合の性能は不確実である。制御系の保証は扱わない。 |
| 確率予測・長い履歴・ベンチマーク評価 | 2026 / **[In-Context Learning for System Identification: Probabilistic Formulation, Long-Sequence Handling, and Benchmark Validation](https://arxiv.org/abs/2610.13411)** | 標準的な非線形同定ベンチマークと、分布外条件を含む新規システムの予測を評価した。 | 確率的Transformerメタ同定、長系列入力、少量実測データへの合成データ補完を統合し、比較法に競争力のある予測精度を示した。特定の分布シフトでは入力列のみの適応でも良好な汎化を報告しているが、定量誤差比は要旨にない。 | モデル同定の初期化、データ不足の装置、未知系への即応型予測器に応用できる。 | 2026年10月公開のプレプリントであり、事前学習分布への依存、長系列計算負荷、不確実性の校正、閉ループ制御への保証が残る。 |

**推奨順と理由**：ForgioneのClass Modelsで「未知系の履歴を文脈として使う」概念を掴む。Coleで線形系の理論的な表現・近似能力を確認し、WangでContext長と事前学習する環境の多様性を比較する。最後にRufoloの確率的システム同定と長系列問題へ進む。予測精度とMPCでの閉ループ安定性は別の課題である。

**この区分の収録件数：4件。**


<a id="sec-s"></a>
### S. 分野横断のサーベイ・総説

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 最初に読む総説の有力候補。 | 2020 / **[Learning-Based Model Predictive Control: Toward Safe Learning in Control](https://doi.org/10.1146/annurev-control-090419-075625)** | 学習したダイナミクス、MPCパラメータの学習、安全な探索の3視点から学習ベースMPCを整理。 | GP・MPC・安全フィルタ等を統一的に分類し、モデル誤差・安全保証と学習効率の緊張関係を明確化した。総説のため共通ベンチマーク値なし。 | 学習ベースMPC全体の地図、個別原著へ進む導入。 | モデル品質、システムの可変性、安全なデータ収集の両立。 |
| 安全制御全般の全体像。 | 2022 / **[Safe Learning in Robotics: From Learning-Based Control to Safe Reinforcement Learning](https://doi.org/10.1146/annurev-control-042920-020211)** | 学習中・学習後のロボットの安全性を、MPC、CBF、Reachability、RLの観点で整理。 | 安全探索と学習済み方策の認証の違いを明確化する総説。主成果は方法間の比較で独自改善倍率なし。 | ロボットの学習制御、安全制約の設計。 | モデル誤差の扱い、実機試行中の事故回避、環境変化への保証。 |
| ロボットRL分野の全体地図。 | 2023 / **[Model-Based Reinforcement Learning: A Survey](https://doi.org/10.1561/2200000086)** | World Modelの学習、計画、価値推定、潜在表現、不確実性処理を広く分類。 | モデルベースRLを一つの設計空間として体系化する長編総説。多くの先行研究の比較を含むが独自の一律な改善倍率はない。 | TD-MPC2、Dreamer、モデル学習とMPCの接続。 | モデル誤差の累積、オフラインデータの偏り、オンライン探索の安全性。 |
| 理論基盤として特に重要。 | 2023 / **[Statistical Learning Theory for Control: A Finite-Sample Perspective](https://arxiv.org/abs/2209.05423)** | 未知線形系の有限データから得る同定誤差とLQR学習性能を、統計理論として整理。 | サンプル複雑性、同定誤差上界、性能下界などの結果を系統立てて解説。改善倍率というより、必要データ数と誤差の評価が成果。 | 学習制御の性能保証、システム同定の実験計画。 | 非線形系、部分観測、現実の雑音・探索制約への一般化に研究余地。 |
| リカッチ方程式と方策勾配の関係を見る。 | 2023 / **[Toward a Theoretical Foundation of Policy Optimization for Learning Control Policies](https://doi.org/10.1146/annurev-control-042920-020021)** | LQR/LQGなどを題材に、方策勾配・Actor更新の最適化地形と収束を整理。 | 非凸な制御方策最適化でも特定条件では勾配法の大域収束を証明できる結果を解説。数値実験の改善率が主眼ではない。 | 制御工学からRLの理論へ進む導入。 | 非線形・部分観測・制約付き実システムでは大域保証が難しい。 |
| 「安全」の意味を数学的に区別する教材。 | 2024 / **[A Survey of Constraint Formulations in Safe Reinforcement Learning](https://doi.org/10.24963/ijcai.2024/913)** | 期待累積コスト・確率制約・時刻ごとの状態入力制約など、異なる安全性の定式化を比較。 | 各方式がどの事象にどの種の保証を与えるかを整理。研究間の成功率をそのまま比較できない理由が分かる。 | Constrained MDP、CBF付きRL、学習方策の仕様選定。 | 実装上の制約違反率と理論保証の差、分布シフト、高次元観測。 |
| DeePC以外も含めた大局観。 | 2024 / **[Data Science and Model Predictive Control: A Survey of Recent Advances on Data-Driven MPC Algorithms](https://doi.org/10.1016/j.jprocont.2024.103327)** | DeePC、同定ベースMPC、学習モデル、適応・強化学習などの方式を分野横断で比較。 | データ取得・構造化・学習モデル・最適化・閉ループ評価という関係を明確化する。独自実験の単一改善率はない。 | データ駆動MPCの設計方式選定・研究文献の探索。 | モデルを使う直接／間接法を同一条件で評価する際の比較軸が多い。 |
| 研究と現場運用のギャップを理解できる。 | 2024 / **[Machine Learning for Industrial Sensing and Control: A Survey and Practical Perspective](https://doi.org/10.1016/j.conengprac.2024.105841)** | 産業プロセスのSoft Sensor、ハイブリッドモデル、プロセス最適化、RL等の実用事例を比較。 | 技術の適用例と産業上の要件、解釈性と物理知識を組み込む意義を整理。独自の制御成功率改善を一律には述べない。 | 装置開発への学習制御の移行、産業での技術選定。 | データ品質、説明可能性、実時間性、安全性、保守運用。 |
| 最適化とフィードバックを横断する最重要の概念論文。 | 2024 / **[Optimization Algorithms as Robust Feedback Controllers](https://doi.org/10.1016/j.arcontrol.2024.100941)** | 数値最適化アルゴリズムを動的系として見なし、物理プラントと並列・閉ループ接続して解析する。 | 時間変動最適化、Online Optimization、フィードバックによる最適解追従・ロバスト性を制御理論の言葉で統一する総説。独自の速度改善率は目的としない。 | 最適化ベース制御、Online Gradient、SQP/RTI設計の概念整理。 | 反復速度・サンプリング時間・プラントの安定性を同時に扱うと仮定が複雑になる。 |
| 依頼者が引用したTutorial Review。 | 2025 / **[A Tutorial Review of Machine Learning-Based Model Predictive Control Methods](https://doi.org/10.1515/revce-2024-0055)** | 学習モデルの汎化誤差とMPC閉ループ安定性の関係を解析し、非線形化学プロセスの例で解説。 | モデルの構築と保証を統合したチュートリアルで、データ不足、次元の呪い、モデル誤差、計算効率・安全性まで体系化。独自の改善倍率は要照合。 | 学習モデル付きNMPCの設計、モデル誤差に敏感な制約付き制御。 | データ量、外挿、オンライン学習、リアルタイム最適化と閉ループ保証の両立。 |
| DeePC等の安定性・ロバスト性・制約保証を整理 | 2025 / **[An Overview of Systems-Theoretic Guarantees in Data-Driven Model Predictive Control](https://doi.org/10.1146/annurev-control-030323-024328)** | DeePCを含むデータ駆動MPCの安定性、ロバスト性、制約充足の研究を体系化した総説である。 | 未知LTIに対する厳密な理論からノイズや非線形系への拡張までを整理し、適用可能な保証条件を比較した。総説固有の追従誤差改善率は存在しない。 | データ駆動MPCの保証条件確認、DeePCとモデル同定型MPCの比較研究に利用できる。 | データ励起・ノイズ境界・モデルクラスの仮定がなお強く、大域非線形系の保証と計算効率の両立が課題である。 |
| システム同定の観点で機械学習を学ぶ基礎。 | 2025 / **[Deep Networks for System Identification: A Survey](https://doi.org/10.1016/j.automatica.2024.111907)** | RNN、状態空間型NN、学習構造、正則化、ノイズ下同定などを整理。 | 深層学習による同定を従来型のシステム同定と接続し、ネットワーク構造・学習手順と識別可能性の問題を概観。総説につき改善率なし。 | ニューラル状態空間モデル、制御用モデル学習。 | 同定の再現性、長期予測、物理整合性、閉ループ性能保証が不足。 |
| 非線形データ駆動制御の比較に使う。 | 2025 / **[Efficient Data-Driven Predictive Control of Nonlinear Systems: A Review and Perspectives](https://doi.org/10.1016/j.dche.2025.100219)** | 非線形系に対するKoopman MPCとDeePCなど、データからの凸予測制御方式を比較。 | 制御性能、凸最適化への落とし込み、計算時間、データ要件の観点から研究動向を整理。新規の一律な性能倍率はない。 | Koopman対DeePCの位置づけ、非線形データ駆動制御手法の選定。 | 非線形系での誤差上界、実行可能性、データ量とオンライン計算負荷。 |
| ロボティクスの将来像を把握する補助。 | 2026 / **[A Survey of World Models for Physical AI with Uncertainty Representation and Control](https://doi.org/10.1007/s44163-026-02122-1)** | 潜在状態、時系列予測、物理事前知識、不確実性、観測形式、制御器結合でWorld Modelを分類。 | Physical AIでのWorld Modelの設計要素を横断整理。独自の制御性能値の提示ではない。 | 自律ロボット、モデルベースRL、汎用的な環境予測器。 | 現実の分布シフト・長期予測・構造保存・安全保証が未解決。 |
| Koopman予測誤差から閉ループ保証に至る総説 | 2026 / **[An Overview of Koopman-Based Control: From Error Bounds to Closed-Loop Guarantees](https://doi.org/10.1016/j.arcontrol.2025.101035)** | Koopman予測モデルの誤差上界とMPCの閉ループ安定性・制約保証に関する研究を概観した総説である。 | 有限データ・有限次元近似の誤差解析から閉ループ保証へつながる研究を整理した。新たな制御ベンチマークの数値改善を報告するものではない。 | Koopmanモデルの学習・MPC設計・保証研究の文献マップとして使える。 | 学習した持ち上げ座標の誤差評価と真の非線形系への保証の橋渡しが難しく、高次元化による計算負荷も残る。 |
| 研究の大局観を得るために優先。 | 2026 / **[Control-Oriented System Identification: Classical, Learning, and Physics-Informed Approaches](https://arxiv.org/abs/2512.06315)** | 古典同定、データ駆動・NN同定、物理構造を保存するモデル学習を、閉ループ制御に必要なモデル品質という視点から比較。 | 予測誤差、安定性・散逸性・保存則などを「制御に適したモデル設計」という軸で体系化した総説。独自の改善倍率を報告するものではない。 | システム同定手法の選択、GP/Koopman/Neural Dynamicsの位置づけ整理。 | 開ループ予測精度と制御品質の乖離、物理制約の表現、有限データと閉ループ保証の両立。 |
| 古典的SQP型MPCと確率推論型MPCを比較できる。 | 2026 / **[Model Predictive Control via Probabilistic Inference: A Tutorial and Survey](https://doi.org/10.1016/j.arcontrol.2026.101052)** | 有限ホライズン最適制御をBoltzmann型の最適制御分布に関する推論問題として表す。 | MPPIを含むSampling MPCを事前分布・変分推論・多峰性・制約・理論解析という軸で整理し、代表的なサンプル更新を導出。総説につき共通の改善倍率はない。 | MPPI、サンプリング最適制御、分布に基づくロボット計画。 | サンプル効率、制約保証、計算時間、モデル不確かさが残る。 |
| 安定性・ロバスト性保証付きNeural Controlの総説 | 2026 / **[Neural Networks in the Loop: Learning with Stability and Robustness Guarantees](https://doi.org/10.1146/annurev-control-022624-032459)** | ニューラルネットワークを含む閉ループ制御の安定性とロバスト保証を既存研究から整理した総説である。 | Lyapunov、Contraction、検証付き学習などの制御保証の枠組みと必要条件を整理した。改善倍率・実験成功率を新規に報告する文献ではない。 | 保証付きニューラル制御器の研究計画、安定性証明手法の選定に使える。 | 大規模NNや高次元非線形系の証明可能領域は狭くなりやすく、モデル誤差・安全性・計算負荷の統一的な保証が難しい。 |
| FNO/DeepONetの背景の数理を補う。 | 2026 / **[Operator Learning: A Statistical Perspective](https://doi.org/10.1146/annurev-statistics-042424-070908)** | 関数から関数への作用素学習を統計回帰問題として整理し、関数空間での近似・汎化を扱う。 | PDE解写像の学習誤差・統計的難しさを体系化する総説。特定制御器の追従改善率ではない。 | Neural Operatorの理論、PDE制御モデルの学習。 | 無限次元関数のサンプル複雑性と実環境の一般化。 |
| MPCをTeacher・Policy・Criticに用いる研究の分類 | 2026 / **[Synthesis of Model Predictive Control and Reinforcement Learning: Survey and Classification](https://doi.org/10.1016/j.arcontrol.2026.101045)** | 既存のMPC・RL融合手法を分類する総説であり、独自のベンチマークによる制御性能向上は対象としない。 | MPCを教師、方策、価値関数等に利用する構成を整理し、手法間の前提と学習・計算上の特徴を比較した。特定手法の成功率を新たに測定した論文ではない。 | MPCとRLの融合アーキテクチャ設計、論文選定や研究課題の整理に有用である。 | 方式によってサンプル効率、オンライン計算量、安全性の仮定が異なり、各比較を同一条件で行うベンチマークが不足する。 |

**推奨順と理由**：最初は2020年HewingのLearning-Based MPCと2026年のA Decadeを読んで研究の分類を作る。次にControl-Oriented System ID→Statistical Learning Theoryでモデル学習の理論を補い、Optimization Algorithms as Robust Feedback Controllers→PI-MPC Tutorialで最適化側を俯瞰する。その後、DeePC・Koopman・Safety・RL・Neural Operator等の専門Surveyを必要な系譜に合わせて読む。全20件の完全通読を最初の目標にする必要はない。

**この区分の収録件数：20件。**


<a id="sec-t"></a>
### T. 方法を学ぶチュートリアル論文

| 読む価値・研究史上の位置 | 年・論文（原文リンク） | 対象・問題設定・実験 | 手法と主な成果・定量指標 | 期待される応用 | 残された課題・限界 |
| --- | --- | --- | --- | --- | --- |
| 実験入力設計と制御の両立を学ぶ。 | 2018 / **[Stochastic Model Predictive Control with Active Uncertainty Learning: A Survey on Dual Control](https://doi.org/10.1016/j.arcontrol.2017.11.001)** | 制御行為が状態を変えるだけでなく未知パラメータに関する情報を得るDual Controlを扱う。 | 不確実性学習と探索をMPCへ導入する方式を整理し、探索と制御のトレードオフを解説。独自倍率はない。 | Active Learning、オンラインシステム同定、GP-MPC。 | 探索時の安全性、情報利得評価の計算コスト、実機の励起制約。 |
| 確率的MPCへ進む前の導入。 | 2018 / **[Stochastic Model Predictive Control — How Does It Work?](https://doi.org/10.1016/j.compchemeng.2017.10.026)** | 確率的外乱を含む制約付きMPCで、リスクと性能を両立する設計を説明する。 | Chance Constraint、予測分布、名目MPCとの違いを例題で解説。チュートリアルに共通した改善倍率はない。 | GP-MPCの確率制約、SMPCの基礎理論。 | 分布仮定や近似共分散、制約違反確率の較正に依存する。 |
| ニューラルネットワークの知識を補うための実装志向教材。 | 2022 / **[A Tutorial Review of Neural Network Modeling Approaches for Model Predictive Control](https://www.sciencedirect.com/science/article/pii/S0098135422002927)** | FNN、RNN、LSTM/GRU、Encoder–Decoder等の時系列モデルを構築し、非線形化学プロセスのMPCで比較。 | 問題設定、訓練・パラメータ推定、性能検証、閉ループ実装を順に解説。異なるモデル構造による予測精度・閉ループ挙動の比較が含まれる。 | NNによるシステム同定、学習モデルをCasADi/acadosへ組み込む前段。 | モデル分布外の予測精度、モデル次数・アーキテクチャの選択、制御ループ内の計算負荷。 |
| 従来MPCとDeePCを制御目的の観点で結ぶ教材。 | 2022 / **[Offset-free Data-driven Predictive Control](https://doi.org/10.1109/CDC51059.2022.9992453)** | MPC、サブスペース予測制御、DeePCを定常偏差除去という課題から比較。 | 積分作用・増分入力やデータ駆動法の正則化を関連づけ、オフセットフリー追従を説明。論文の特定実験改善率は未確認。 | 精密サーボの定常誤差対策、DeePCへの積分構造導入。 | 観測ノイズ、ランク・励起条件、外乱推定に関する仮定。 |
| GP-MPCの入門から高度な論文への導入に適する。 | 2024 / **[A Tutorial on Gaussian Process Learning-based Model Predictive Control](https://arxiv.org/abs/2404.03689)** | GP回帰の平均・分散の導出、確率的な多段予測、MPCでの共分散伝播とChance Constraint。 | GP-MPCの確率近似と制約構築を一続きの数式で説明。一般の性能倍率は報告対象ではない。 | Cautious GP-MPC、Zero-order GP-MPC、L4acadosの事前学習。 | 分布のガウス近似、GP計算量、共分散の較正、オンライン制御。 |
| 基本CBF原論文の次に読む。 | 2024 / **[Advances in the Theory of Control Barrier Functions: Addressing Practical Challenges in Safe Control Synthesis for Autonomous and Robotic Systems](https://doi.org/10.1016/j.arcontrol.2024.100945)** | CBFの基本原理から入力制約、高相対次数、サンプル値制御、モデル誤差、保守性までを整理。 | CBF-QPの実装・解析で出会う問題と対策の理論を体系化。新規の統一改善率はない。 | ロボットの安全フィルタ設計、CBFの理論と実装の接続。 | QPの実行不能、誤差による安全集合の破れ、現実のアクチュエータ制約。 |
| MPPI研究の歴史とバリエーションを掴む。 | 2024 / **[Recent Advances in Path Integral Control for Trajectory Optimization: An Overview in Theoretical and Algorithmic Perspectives](https://doi.org/10.1016/j.arcontrol.2023.100931)** | Path Integral Control、MPPI、CEMなどの軌道最適化を理論とアルゴリズムから比較。 | サンプリング分布・ノイズ重み付け・最適制御の関係を整理し、実装教材も紹介。独自の一律性能倍率はない。 | MPPI、GPUサンプリングMPC、iLQRとの比較。 | 試行軌道数・確率分布の設計、ハード制約・安全性、長いホライズン。 |
| MPCとRLを価値関数から理解する補助。 | 2025 / **[A View on Learning Robust Goal-Conditioned Value Functions: Interplay Between RL and MPC](https://arxiv.org/abs/2502.06996)** | MPCの有限時間価値関数とRLの長期価値関数を共通の概念で比較する。 | 短いMPCホライズンへ学習した終端価値を組み込む構造と、頑健なGoal-conditioned価値学習の論点を整理。新規の統一改善倍率はない。 | RL終端コストを使うMPC、短ホライズン予測制御。 | 価値関数の外挿誤差、制約保証、MPCとRLの誤差相互作用。 |
| データ駆動研究への入口。 | 2025 / **[Tutorial: Data Driven and Learning Enabled Control](https://doi.org/10.1109/CDC57313.2025.11312267)** | 同定後に制御する間接法とデータから制御器を直接構築する直接法を比較する。 | データ駆動制御と学習ベース制御の共通構造を導入し、モデルの役割を整理。定量的な独自実験が主目的ではない。 | DeePC、Koopman、モデル同定ベース制御の統一的理解。 | 説明範囲が広いため、個別手法の詳細な数値再現には原論文が必要。 |
| 保証がない学習予測器を制御に導入する実践的な例。 | 2025 / **[Two-Component Controller Design to Safeguard Data-Driven Predictive Control](https://arxiv.org/abs/2505.19131)** | 出力制約を守る必要がある系で、DeePC/EDMD-MPCとモデルフリー高ゲインフィードバックを組み合わせる。 | 学習モデルが不十分なときに安全側フィードバックで介入する構成を提案し、設定値移行の数値例で出力拘束の扱いを実演。数値改善率は未確認。 | モデル学習の初期段階の安全化、DeePCと既知フィードバックの併用。 | 相対次数等の仮定、高ゲイン制御の騒音増幅、モデルベース予測器との切替え。 |
| 総合ロードマップの第一候補。 | 2026 / **[A Decade of Learning-Based Control: From Theoretic Foundations to Practical Deployment](https://arxiv.org/abs/2610.29187)** | 2016～2025年の学習ベース制御の10年間を、安定性・安全性保証、モデル学習、直接データ駆動方策・RL等の側面から俯瞰。 | 方法族ごとの必要な事前知識、学習対象、誤差の扱い、制御保証の対応を比較し、研究史をまとめる。182本のCDC招待セッション論文に関する集計にも言及。 | 今回の全研究系譜を俯瞰する最初の教材。 | 大規模な総説で個々のアルゴリズムの実装詳細は十分でない。掲載時点の新規研究・最新性能評価は別途追う必要。 |
| Operator Learning統計総説と補完関係。 | 2026 / **[From Theory to Application: A Practical Introduction to Neural Operators in Scientific Computing](https://doi.org/10.3390/math14132421)** | FNO、DeepONetを中心にNeural Operatorの構築・学習・PDE代理予測を実践的に概説。 | 理論とコード実装で生じる設計・計算上の問題を整理。PDE制御の性能改善を独自に一律評価する文献ではない。 | PDE制御へ進む前のScientific Machine Learning実装。 | 物理整合性、解像度間汎化、長時間予測誤差、学習計算量。 |
| 2026年時点の理論と実装をつなぐ最優先チュートリアル。 | 2026 / **[Koopman Operator Theory: Fundamentals, Control, and Applications](https://arxiv.org/abs/2607.01819)** | Koopman作用素と可観測関数、EDMD/Kernel EDMD、入力付き系への拡張、Koopman MPCまで段階的に説明。 | 有限データによるモデル近似誤差、EDMDとKoopman MPCの導出、実際のシミュレーションと公式GitHubのハンズオンを収録。チュートリアルなので一律の性能向上値はない。 | Koopman MPCの最初の自作、学習モデルの推定誤差と制御を結ぶ学習。 | 辞書選択、有限データの誤差、真の非線形系に対する閉ループ保証。 |
| 2024年のCBF実装課題チュートリアルの続きとして読む。 | 2026 / **[Learning for Feasible and Safe Control with Control Barrier Functions: A Tutorial](https://doi.org/10.26599/CAI.2025.9390009)** | 安全制約を満たすQPが空集合になる問題や高次CBFの設計で、学習をどこに使うか説明。 | CBF-QPの実行可能性・安全性と学習の関係を事例を交えて整理。改善率の統一値はない。 | 学習によるCBF設計・実行可能領域拡張、安全制御の自動化。 | 学習誤差と安全保証の整合、入力飽和、多数の安全制約との競合。 |

**推奨順と理由**：Koopman分野なら「2026年Koopman Tutorial→Korda原論文→最新の安定性研究」、GPなら「Stochastic MPC Tutorial→2024年GP-MPC Tutorial→Cautious→Zero-order」、NN-MPCなら「2022年NNモデリングTutorial→2025年ML-MPC Tutorial→Differentiable MPC原著」が読みやすい。安全制御なら「CBF原著→2024年CBF Tutorial→2026年学習CBF Tutorial」、MPPIなら「2017年原著→2024年Path Integral Review」の順を勧める。

**この区分の収録件数：14件。**


## 公開実装・講義・チュートリアル資料

**以下は文献件数に含めない。** 再現実装の入口として、この会話で紹介されたソフトウェア・講義資料を再録した。

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

## 文献評価・定量指標の読み方

**1. 実験結果と理論保証を分ける。** 「11倍高速化」は特定のベンチマークの処理時間比だが、「局所指数安定性」は仮定の下での数学的な性質であり、数値的な優劣を比較するものではない。

**2. 倍率には評価対象がある。** Online MPCの計算時間、NN推論だけの時間、オフライン学習を含む総時間、証明器による認証時間は別指標。例えばPDE-DPCの推論倍率を、開発総工数やリアルタイム性の普遍的な倍率と解釈しない。

**3. 原論文と総説を区別する。** 総説／チュートリアルの「成果」は手法体系化・理論導出・実装教材であり、新規ベンチマークの改善率が存在するとは限らない。

**4. 証明条件を確認する。** 有限データでの誤差上界、GP分散、Conformal Predictionの被覆率、Lyapunov/CCMの領域内認証、SLSの有界外乱仮定などは保証する対象と前提が異なる。

**5. 研究史上の直接の続編と、技術的な分岐を区別する。** たとえばDPCが「微分可能」と呼ばれていても、MPCソルバーの最適解に逆伝播する方式と、微分可能な予測ロールアウトで明示的方策を学習する方式は異なる。

**6. 書誌について。** 本表の大半は前回の会話と更新済みExcelから再掲した。今回、Koopman 2026 tutorial、2022年NN-MPC tutorial、2025年ML-MPC tutorial、2026年PI-MPCなど一部原典の要旨・出版社ページを確認したが、全件の題名・著者・巻号・DOI・ベンチマーク表を再検証したものではない。リンクが会議巻トップまたは検索画面になっているものは、論文タイトルを使って個別原典を確認すること。

---

**出典の基礎資料**：この会話の「論文54件・定量評価付きExcel」と、その前後に紹介されたロボティクス原著、古典的制御原著、2026年派生研究、総説15本、チュートリアル群。**文献本文の網羅的再査読ではないため、論文を引用したり数値を研究資料に再利用するときは、必ず原典の該当図表・定理を確認すること。**

