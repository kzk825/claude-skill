# AI News Pipeline

## 目的
AI関連ニュースを整理し、要約とSNS投稿コンテンツを生成するワークフロー。

## 入力
- ニュースURL
- 記事本文（任意）

## 出力
- ニュース要約
- 重要ポイント
- X投稿案

## ワークフロー

### Step1 ニュース取得
ニュースURLまたは記事本文を入力する。

### Step2 要約生成
以下のスキルを使用する

skills/ai-news-summary.md

出力：
- 3行要約
- 重要ポイント
- トレンド性

### Step3 投稿生成
要約結果を元にX投稿文を作成する

skills/x-post-generator.md

出力：
- X投稿案A
- X投稿案B
- X投稿案C

## 使用スキル
- skills/ai-news-summary.md
- skills/x-post-generator.md

## 参照プロンプト
- prompts/news-summary.md
- prompts/x-post.md
