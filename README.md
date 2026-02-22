# JIMA2025_TCU-mlab

経営科学系研究部会連合協議会（JIMA）2025年度 発表用実験結果リポジトリ

## 概要

書籍の日次販売数を予測する時系列予測タスクに取り組んだ研究の実験結果をまとめたリポジトリです。
時系列基盤モデル **Chronos** をベースに、PEFT（Parameter-Efficient Fine-Tuning）手法および **RAF（Retrieval-Augmented Forecasting）** の有効性を検証しています。

## ディレクトリ構成

```
実験結果/
├── 実験1/
│   ├── 実験1-1/          # 事後学習手法の比較
│   │   ├── zero-shot/
│   │   ├── LoRA/
│   │   ├── IA3/
│   │   ├── DoRA/
│   │   └── 指標比較/
│   └── 実験1-2/          # モデルサイズの比較（DoRA + Gate-RAF）
│       ├── tiny(DoRA)+Gate-RAF/
│       ├── small(DoRA)+Gate-RAF/
│       ├── mini(DoRA)+Gate-RAF/
│       ├── base(DoRA)+Gate-RAF/
│       └── 指標比較/
├── 実験2/                 # RAF手法の比較
│   ├── no-RAF/
│   ├── Proto-RAF/
│   ├── Gate-RAF/
│   └── 指標比較/
└── 実験3/                 # スパイク・非スパイク日の分析
    ├── Gate-RAF/
    ├── SARIMA/
    └── 指標比較/
```

## 実験概要

### 実験1-1：事後学習手法の比較

Chronos（base）に対して異なるPEFT手法を適用し、書籍販売数予測の精度を比較しました。

| 手法 | 説明 |
|------|------|
| zero-shot | 事後学習なし |
| LoRA | Low-Rank Adaptation |
| IA3 | Infused Adapter by Inhibiting and Amplifying Inner Activations |
| DoRA | Weight-Decomposed Low-Rank Adaptation |

各手法のディレクトリには以下が含まれます：
- `evaluation_all.csv`：全書籍の評価結果
- `All_predict/`：全書籍の予測グラフ
- `{XX}%_書籍名.png`：総販売数の百分位数ごとの代表的な予測グラフ（5%〜100%）

### 実験1-2：モデルサイズの比較（DoRA + Gate-RAF）

DoRAで事後学習したChronosにGate-RAFを適用し、モデルサイズ（tiny / small / mini / base）による精度差を検証しました。

### 実験2：RAF手法の比較

Chronos（DoRA fine-tuned, base）に対して異なるRAFの組み合わせを比較しました。

| 手法 | 説明 |
|------|------|
| no-RAF | RAFなし |
| Proto-RAF | プロトタイプベースの検索拡張予測 |
| Gate-RAF | ゲーティング機構を用いた検索拡張予測 |

### 実験3：スパイク・非スパイク日の分析

z-scoreによるスパイク検出（閾値 z = 2.0）を適用し、スパイク日と非スパイク日に分けて予測精度を評価しました。Gate-RAFとSARIMAを比較しています。

## 評価指標

### 実験1・2

| 指標 | 説明 |
|------|------|
| MAE | 平均絶対誤差 |
| RMSE | 二乗平均平方根誤差 |
| wQL_0.1 | 加重分位損失（10パーセンタイル） |
| wQL_0.5 | 加重分位損失（50パーセンタイル） |
| wQL_0.9 | 加重分位損失（90パーセンタイル） |
| Coverage_80% | 80%予測区間のカバレッジ |

### 実験3

| 指標 | 説明 |
|------|------|
| MAE_spike / RMSE_spike | スパイク日のMAE・RMSE |
| MAE_non_spike / RMSE_non_spike | 非スパイク日のMAE・RMSE |

各実験の `指標比較/` には各指標のモデル間比較グラフが格納されています。
