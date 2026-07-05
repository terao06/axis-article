# 🚀 RAG（Retrieval Augmented Generation）完全入門ガイド

> **「AIにあなた専用の知識を教えて、もっと賢く使おう！」**

こんにちは！この記事では、RAG（Retrieval Augmented Generation）という技術を、初心者の方でも楽しく学べるように解説していきます。難しい専門用語は最小限に、実際に手を動かしながら学んでいきましょう！

---

## 📚 目次
1. [はじめに](#はじめに)
2. [環境構築](#環境構築)
3. [第1章：AI基礎知識](#第1章ai基礎知識)
4. [第2章：LangChain入門](#第2章langchain入門)
5. [第3章：RAGの基礎](#第3章ragの基礎)
6. [第4章：Advanced RAG](#第4章advanced-rag)
7. [第5章：評価とテスト](#第5章評価とテスト)
8. [第6章：LangGraphで複雑なワークフロー](#第6章langgraphで複雑なワークフロー)
9. [トラブルシューティング](#トラブルシューティング)

---

## はじめに

### 🤔 RAGって何？ざっくり説明

**RAG（Retrieval Augmented Generation）** を一言で言うと、**「AIに参考書を渡して、そこから答えを探させる技術」** です！

想像してみてください。テスト前に友達に質問したとき、「えーっと、確か教科書に書いてあったはず...」って言いながらページをめくって答えてくれたこと、ありませんか？RAGはまさにそれをAIにやらせる技術なんです。

#### 💡 なぜRAGが必要なの？

ChatGPTなどのAIは賢いですが、実は困った問題があります：

1. **📝 覚えられる量に限界がある**: 一度に読める文字数に制限があります
2. **⏰ 最新情報を知らない**: 学習データに含まれていない情報は答えられません
3. **🎭 たまに嘘をつく**: 「ハルシネーション」と呼ばれる、事実と異なる情報を自信満々に答えることがあります

RAGを使えば、これらの問題を解決できます！

#### 🔄 RAGの仕組み（超シンプル版）

```
あなた: 「Pythonの使い方教えて！」
    ↓
RAG: 「ちょっと待ってね、資料を探すよ...」
    ↓
RAG: 「見つけた！これを参考に答えるね」
    ↓
AI: 「Pythonは○○で...（資料に基づいた正確な回答）」
```

---

## 環境構築

### 🛠️ 何を準備すればいい？

料理をするには材料と道具が必要ですよね。プログラミングも同じです！以下のものを準備しましょう。

#### 必要なもの一覧

- ✅ **Python 3.10以上** → プログラミング言語（無料）
- ✅ **OpenAI APIキー** → AIを使うために必須（有料、無料クレジットの有無は時期による）
- ✅ **Git**（あれば便利）

**💰 お金はかかるの？**
- OpenAI APIは従量課金制（使った分だけ）
- 価格は **1Mトークンあたり** の表記が基本です
- 例: **GPT-5 mini** 入力 $0.25 / 出力 $2.00、**GPT-5.1** 入力 $1.25 / 出力 $10.00（いずれも 1Mトークンあたり）
- Embeddings例: **text-embedding-3-small** は $0.02 / 1Mトークン
- 無料クレジットの有無や金額は時期・アカウントによって変わるので、請求画面で確認してください

---

### ステップ1: Pythonをインストールしよう 🐍

#### Windowsの場合

1. [Python公式サイト](https://www.python.org/downloads/)にアクセス
2. 「Download Python 3.xx.x」ボタンをクリック
3. ダウンロードしたファイルを実行
4. **超重要！** → **"Add Python to PATH"** に必ずチェック ✅

#### ちゃんと入ったか確認！

PowerShellを開いて（Windowsキー + X → 「Windows PowerShell」）、以下を入力：

```powershell
python --version
```

`Python 3.10.x` 以上が表示されればOK！👍

**❌ 表示されない場合**
- Pythonをインストールし直す
- "Add Python to PATH" にチェックを入れ忘れていないか確認

---

### ステップ2: OpenAI APIキーを取得しよう 🔑

#### OpenAI APIって何？

簡単に言うと、**「ChatGPTをプログラムから使えるようにするチケット」** です！

OpenAIは、ChatGPTを作っている会社で、その技術をAPIとして提供しています。

#### 💰 料金はどのくらい？

- **従量課金制**: 使った分だけ支払う（月額制じゃない！）
- **GPT-5 mini**: 入力 $0.25 / 出力 $2.00（1Mトークンあたり）
- **GPT-5.1**: 入力 $1.25 / 出力 $10.00（1Mトークンあたり）
- **Embedding**: text-embedding-3-small は $0.02 / 1Mトークン
- 無料クレジットの有無・金額は時期により変動するため、最新の請求画面で確認してください

#### 🎯 APIキーを手に入れよう！

1. [OpenAI公式サイト](https://platform.openai.com/)にアクセス
2. 右上の「Sign up」からアカウント作成
   - Googleアカウントでサクッと登録できます！
3. ログインしたら、右上のアカウントメニューから「**API keys**」をクリック
4. 「**Create new secret key**」ボタンをクリック
5. 名前を付けて（例: "RAG-Tutorial"）「**Create secret key**」
6. 表示されたキーをコピー（**このタイミングでしか見られない！**）

**⚠️ 超重要な注意事項**:
- ❗ APIキーは絶対に他人に見せない！
- ❗ TwitterやGitHubに公開しない！
- ❗ `.env`ファイルで安全に管理しましょう

#### 📊 使用量を確認する方法

[Usage Dashboard](https://platform.openai.com/usage)で、使用量とコストをリアルタイムで確認できます📈

---

### ステップ3: プロジェクトフォルダを作ろう 📁

#### フォルダを作成

```powershell
mkdir rag_training
cd rag_training
```

#### 仮想環境を作成（これ、なんで必要なの？）

**仮想環境** = 「このプロジェクト専用の作業スペース」

例えば、部屋の中に小さなテント⛺を張って、その中でだけ作業するイメージです。他のプロジェクトと干渉しないので、安心して実験できます。

```powershell
python -m venv venv
```

#### 仮想環境を有効化

```powershell
.\venv\Scripts\Activate.ps1
```

成功すると、プロンプトの前に `(venv)` が表示されます：

```
(venv) PS C:\Users\...\rag_training>
```

**⚠️ エラーが出た場合**

PowerShellで実行ポリシーエラーが出たら：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

もう一度 `.\venv\Scripts\Activate.ps1` を実行してください。

---

### ステップ4: 必要なパッケージをインストール 📦

#### requirements.txt を作成

プロジェクトフォルダ内に `requirements.txt` というファイルを作成して、以下をコピペ：

```txt
# 基本パッケージ（LangChainの心臓部）
langchain==1.2.7
langchain-openai==1.1.7
langchain-community==0.4.1
langchain-core==1.2.7
langchain-chroma==1.1.0
openai==2.16.0

# ドキュメント処理（ファイルを読み込むやつ）
unstructured==0.18.31
markdown==3.10.1

# ベクトルストア（データを保存する場所）
chromadb==1.4.1
faiss-cpu==1.13.2

# Embedding（文章を数値に変換する魔法）
sentence-transformers==5.2.2

# テキスト分割（長い文章を小分けにする）
langchain-text-splitters==1.1.0

# 評価・テスト（ちゃんと動いてるか確認）
ragas==0.4.3
langsmith==0.6.7

# LangGraph（複雑な処理を組み立てる）
langgraph==1.0.7

# ユーティリティ（便利ツール集）
python-dotenv==1.2.1
pydantic==2.12.5
```

#### インストール実行

```powershell
pip install -r requirements.txt
```

**⏳ 待ち時間**: 5〜10分かかることがあります。YouTubeでも見ながら待ちましょう😊

---

### ステップ5: 環境変数の設定 🔑

#### .env ファイルを作成

プロジェクトフォルダ内に `.env` ファイルを作成：

```env
# OpenAI API（必須）
OPENAI_API_KEY=your_openai_api_key_here

# OpenAI設定（オプション）
# 使用するモデルを指定（デフォルト: gpt-5-mini）
OPENAI_MODEL=gpt-5-mini
# または高精度モデル
# OPENAI_MODEL=gpt-5.1

# OpenAI Embeddings設定（オプション）
# 使用するEmbeddingsモデルを指定（デフォルト: text-embedding-3-small）
OPENAI_EMBEDDING_MODEL=text-embedding-3-small
# または高精度モデル
# OPENAI_EMBEDDING_MODEL=text-embedding-3-large

# LangSmith（評価ツール、なくてもOK）
# 推奨: LANGSMITH_*（最新表記）
LANGSMITH_TRACING=false
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
LANGSMITH_API_KEY=your_langsmith_api_key_here
LANGSMITH_PROJECT=rag-tutorial
#
# 互換: 旧表記（環境によってはまだ有効）
# LANGCHAIN_TRACING_V2=false
# LANGCHAIN_ENDPOINT=https://api.smith.langchain.com
# LANGCHAIN_API_KEY=your_langsmith_api_key_here
# LANGCHAIN_PROJECT=rag-tutorial
```

**💡 OpenAI APIキーの取得方法**
1. [OpenAI公式サイト](https://platform.openai.com/)でアカウント作成
2. APIキーを発行
3. `.env` ファイルの `your_openai_api_key_here` を置き換え

#### 環境変数読み込み用ファイル

`set_env.py` を作成：

```python
import os
from dotenv import load_dotenv

def setup_environment():
    """環境変数を読み込む（おまじない的なやつ）"""
    load_dotenv()
    
    # OpenAI API（必須）
    openai_key = os.getenv("OPENAI_API_KEY")
    if not openai_key:
        raise ValueError("OPENAI_API_KEY が設定されていません。.envファイルを確認してください。")
    os.environ["OPENAI_API_KEY"] = openai_key
    
    # OpenAI モデル設定
    os.environ["OPENAI_MODEL"] = os.getenv("OPENAI_MODEL", "gpt-5-mini")
    os.environ["OPENAI_EMBEDDING_MODEL"] = os.getenv("OPENAI_EMBEDDING_MODEL", "text-embedding-3-small")
    
    # LangSmith設定（使わなくてもOK）
    # 最新: LANGSMITH_* を優先
    if os.getenv("LANGSMITH_API_KEY"):
        os.environ["LANGSMITH_TRACING"] = os.getenv("LANGSMITH_TRACING", "true")
        os.environ["LANGSMITH_ENDPOINT"] = os.getenv("LANGSMITH_ENDPOINT", "https://api.smith.langchain.com")
        os.environ["LANGSMITH_API_KEY"] = os.getenv("LANGSMITH_API_KEY")
        os.environ["LANGSMITH_PROJECT"] = os.getenv("LANGSMITH_PROJECT", "rag-tutorial")
    # 互換: 旧表記も拾う
    elif os.getenv("LANGCHAIN_API_KEY"):
        os.environ["LANGSMITH_TRACING"] = "true"
        os.environ["LANGSMITH_ENDPOINT"] = "https://api.smith.langchain.com"
        os.environ["LANGSMITH_API_KEY"] = os.getenv("LANGCHAIN_API_KEY")
        os.environ["LANGSMITH_PROJECT"] = os.getenv("LANGCHAIN_PROJECT", "rag-tutorial")
    
    print("✓ 環境変数を読み込みました！準備完了です🎉")
    print(f"✓ 使用モデル: {os.environ['OPENAI_MODEL']}")
    print(f"✓ 使用Embeddingsモデル: {os.environ['OPENAI_EMBEDDING_MODEL']}")

if __name__ == "__main__":
    setup_environment()
```

---

## 第1章：AI基礎知識

### 1.1 AIと初めての会話 🗣️

まずは、OpenAI APIを使ってAIと会話してみましょう！

#### openai_basic.py を作成

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

# OpenAIに接続
client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
)

# AIに話しかけてみよう！
response = client.chat.completions.create(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    messages=[
        {"role": "system", "content": "あなたは親切で面白いアシスタントです。絵文字も使ってフレンドリーに答えてください。"},
        {"role": "user", "content": "こんにちは！自己紹介をしてください。"}
    ],
)

print(response.choices[0].message.content)
```

#### 実行してみよう

```powershell
python openai_basic.py
```

AIがフレンドリーに自己紹介してくれるはずです！😊

**💡 豆知識**: `temperature` パラメータ
- **0〜0.3**: 真面目で一貫性のある回答（ビジネス向け）
- **0.5〜0.7**: バランスが良い（おすすめ）
- **0.8〜1.0**: クリエイティブで多様な回答（創作向け）

---

### 1.2 JSON形式で出力させてみよう 📊

AIに「決まった形式」で答えてもらう方法を学びます。

#### json_output.py を作成

```python
import os
from openai import OpenAI
from dotenv import load_dotenv
import json

# 環境変数を読み込む
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
)

response = client.chat.completions.create(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    messages=[
        {
            "role": "system", 
            "content": "人物一覧を次のJSON形式で出力してください。\n{'people': [{'name': '名前', 'role': '役割'}]}"
        },
        {
            "role": "user", 
            "content": "昔あるところにおじいさんとおばあさんがいました。おじいさんは山へ芝刈りに、おばあさんは川へ洗濯に行きました。"
        }
    ],
    response_format={"type": "json_object"},  # JSON形式を指定
)

# 補足: json_schema に対応しているモデルでは、厳密な形式指定が可能です。
# response_format={"type": "json_schema", "json_schema": {...}}

result = json.loads(response.choices[0].message.content)
print(json.dumps(result, ensure_ascii=False, indent=2))
```

#### 実行結果（例）

```json
{
  "people": [
    {
      "name": "おじいさん",
      "role": "主人公・芝刈り"
    },
    {
      "name": "おばあさん",
      "role": "主人公・洗濯"
    }
  ]
}
```

**🎯 活用例**: 
- ニュース記事から人物を抽出
- レビューから感情を分析
- 商品情報を構造化

---

### 1.3 プロンプトエンジニアリング入門 🎨

**プロンプトエンジニアリング** = 「AIへの上手な質問の仕方」

同じ質問でも、聞き方を変えるだけで回答の質が劇的に変わります！

#### Few-shot学習（お手本を見せる）

「こういう風に答えてね」という例を見せる方法です。

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
)

# お手本を見せて学習させる
messages = [
    {"role": "system", "content": "入力がAIに関するかTrue/Falseで回答してください。"},
    {"role": "user", "content": "AIの進化はすごい"},
    {"role": "assistant", "content": "True"},
    {"role": "user", "content": "今日はいい天気だ"},
    {"role": "assistant", "content": "False"},
    {"role": "user", "content": "機械学習のアルゴリズムを学びたい"},
    {"role": "assistant", "content": "True"},
    {"role": "user", "content": "明日は雨が降るだろう"}  # これを判定
]

response = client.chat.completions.create(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    messages=messages,
)

print(response.choices[0].message.content)  # "False" が返ってくるはず
```

#### Chain-of-Thought（考える過程を見せる）

「一歩ずつ考えてね」とお願いすると、AIがより正確に答えてくれます。

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
)

messages = [
    {"role": "system", "content": "ステップバイステップで考えてください。途中の計算過程も示してください。"},
    {"role": "user", "content": "25人のクラスで、男子が女子より5人多いです。女子は何人ですか？"}
]

response = client.chat.completions.create(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    messages=messages,
)

print(response.choices[0].message.content)
```

**出力例:**
```
ステップ1: 女子をx人とすると、男子は(x+5)人
ステップ2: 合計は x + (x+5) = 25
ステップ3: 2x + 5 = 25
ステップ4: 2x = 20
ステップ5: x = 10

答え: 女子は10人です。
```

---

## 第2章：LangChain入門

### 2.1 LangChainって何？🔗

**LangChain** = 「AIアプリを作るためのレゴブロック」

LangChainを使うと、複雑なAIアプリケーションを簡単に組み立てられます。

#### LangChainで何ができる？

- 🎯 **プロンプト管理**: テンプレート化して使い回し
- 🔗 **Chain**: 複数の処理を連結（検索→要約→翻訳など）
- 🤖 **Agent**: ツールを使って自律的に問題解決
- 📚 **RAG**: ドキュメント検索と回答生成を統合

---

### 2.2 最初のLangChainプログラム

#### langchain_hello.py を作成

```python
import os
from langchain_openai import ChatOpenAI
from langchain_core.messages import SystemMessage, HumanMessage
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

# モデルを作成（OpenAI使用）
model = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)

# メッセージを作成
messages = [
    SystemMessage(content="あなたは親切なプログラミング講師です。初心者にもわかりやすく説明してください。"),
    HumanMessage(content="Pythonでファイルを読み込む方法を教えてください。")
]

# AIを呼び出す
response = model.invoke(messages)
print(response.content)
```

**実行:**

```powershell
python langchain_hello.py
```

---

### 2.3 リアルタイムで文字が出てくる（ストリーミング） ⚡

ChatGPTみたいに、文字が一文字ずつ表示される機能を実装してみましょう！

#### streaming_example.py

```python
import os
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

model = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
    streaming=True  # ストリーミングを有効化
)

messages = [HumanMessage(content="日本の歴史について200字で説明してください。")]

print("AIが考え中", end="")

# リアルタイムで出力
for chunk in model.stream(messages):
    print(chunk.content, end='', flush=True)

print("\n\n完了！✨")
```

**実行すると...**

文字が一文字ずつ表示されて、まるでAIが本当に考えているみたいに見えます！

---

### 2.4 プロンプトテンプレート（使い回せる質問フォーマット） 📝

毎回同じような質問をするなら、テンプレート化しましょう！

#### prompt_template.py

```python
import os
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI
from langchain_core.output_parsers import StrOutputParser
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

# モデルとパーサー
model = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)
parser = StrOutputParser()

# プロンプトテンプレート（{dish} の部分が変わる）
prompt = ChatPromptTemplate.from_messages([
    ("system", "あなたは経験豊富な料理人です。ユーザーが入力した料理のレシピを、わかりやすく楽しく説明してください。絵文字も使ってOKです！"),
    ("human", "{dish}")
])

# Chain（流れ作業）を作成
chain = prompt | model | parser

# 実行（料理名を変えるだけ！）
recipe = chain.invoke({"dish": "カレーライス"})
print(recipe)
```

**💡 応用例:**
- `{"dish": "オムライス"}` → オムライスのレシピ
- `{"dish": "唐揚げ"}` → 唐揚げのレシピ

---

### 2.5 構造化された出力（決まった形式で返してもらう） 🎯

AIに「この形式で答えて！」と指定する方法です。

#### structured_output.py

```python
import os
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI
from pydantic import BaseModel, Field
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

# 出力の「型」を定義（こういう形で返してね！）
class Recipe(BaseModel):
    dish_name: str = Field(description="料理名")
    ingredients: list[str] = Field(description="材料のリスト")
    steps: list[str] = Field(description="調理手順のリスト")
    cooking_time: int = Field(description="調理時間（分）")

# モデル
model = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)

# プロンプト
prompt = ChatPromptTemplate.from_messages([
    ("system", "あなたは料理の専門家です。レシピを提供してください。"),
    ("human", "{dish}")
])

# Chainに構造化出力を組み込む
chain = prompt | model.with_structured_output(Recipe)

# 実行
recipe = chain.invoke({"dish": "オムライス"})

# きれいに表示
print(f"🍳 料理名: {recipe.dish_name}")
print(f"⏱️  調理時間: {recipe.cooking_time}分")
print(f"\n📝 材料:")
for ingredient in recipe.ingredients:
    print(f"  • {ingredient}")
print(f"\n👨‍🍳 手順:")
for i, step in enumerate(recipe.steps, 1):
    print(f"  {i}. {step}")
```

**出力例:**
```
🍳 料理名: ふわふわオムライス
⏱️  調理時間: 20分

📝 材料:
  • 卵 3個
  • ご飯 1杯
  • 玉ねぎ 1/4個
  • ケチャップ 大さじ3

👨‍🍳 手順:
  1. 玉ねぎをみじん切りにする
  2. フライパンで玉ねぎを炒める
  3. ご飯を加えてケチャップで味付け
  4. 卵を溶いて薄く焼く
  5. ケチャップライスを卵で包む
```

---

## 第3章：RAGの基礎

### 3.1 RAGの5つのパーツ 🧩

RAGは以下の5つのコンポーネントで構成されます。レゴブロックを組み立てるイメージです！

1. **📄 Document Loader**: ファイルを読み込む（.txt、.md、.pdfなど）
2. **✂️ Document Transformer**: 長い文章を小さく分割
3. **🔢 Embedding Model**: 文章を数値（ベクトル）に変換
4. **💾 Vector Store**: ベクトルを保存・検索できる場所
5. **🔍 Retriever**: 質問に関連する文章を探してくる

---

### 3.2 Embeddingって何？魔法の数値変換 ✨

#### Embeddingを超わかりやすく説明

**Embedding** = 「文章を数の配列に変換する魔法」

なぜこんなことをするかというと...

**コンピューターは言葉がわかりません。数字しかわかりません。**

だから、「機械学習は面白い」という文章を、こんな感じで数値に変換します：

```
テキスト: "機械学習は面白い"
    ↓ 魔法の変換（Embedding）
ベクトル: [0.23, -0.45, 0.78, ..., 0.12]  （1536個の数字）
```

#### なぜこれが便利？

数値になれば、「似ている度合い」を計算できます！

```
"機械学習は面白い"      [0.2, -0.4, 0.7, ...]
"AIについて学ぶ"        [0.3, -0.5, 0.8, ...]  ← 似てる！
"今日は良い天気"        [0.9,  0.2, -0.1, ...] ← 全然違う！
```

#### 実際に試してみよう！

##### embedding_demo.py

```python
import os
from langchain_openai import OpenAIEmbeddings
import numpy as np
from dotenv import load_dotenv

# 環境変数を読み込む
load_dotenv()

# Embeddingモデルの準備
embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)

# いろんなテキストを用意
texts = [
    "機械学習は人工知能の一分野です",
    "深層学習はニューラルネットワークを使います",
    "今日は良い天気ですね",
    "AIと機械学習について学んでいます"
]

print("🔢 テキストをベクトル化しています...\n")

# 各テキストをベクトルに変換
vectors = []
for text in texts:
    vector = embeddings.embed_query(text)
    vectors.append(vector)
    print(f"📝 テキスト: {text}")
    print(f"   ベクトル次元: {len(vector)}個の数字")
    print(f"   最初の5個: {[f'{v:.3f}' for v in vector[:5]]}")
    print()

# ========================================
# 類似度を計算してみよう！
# ========================================
def cosine_similarity(vec1, vec2):
    """2つのベクトルがどれだけ似ているか計算"""
    dot_product = np.dot(vec1, vec2)
    norm1 = np.linalg.norm(vec1)
    norm2 = np.linalg.norm(vec2)
    return dot_product / (norm1 * norm2)

print("="*60)
print("🔍 類似度チェック！")
print("="*60)

base_text = texts[0]
base_vector = vectors[0]

print(f"\n基準: 「{base_text}」\n")

for i, (text, vector) in enumerate(zip(texts[1:], vectors[1:]), 1):
    similarity = cosine_similarity(base_vector, vector)
    
    # 絵文字で視覚化
    if similarity > 0.8:
        emoji = "🔥 超似てる！"
    elif similarity > 0.6:
        emoji = "✅ 似てる"
    elif similarity > 0.4:
        emoji = "🤔 ちょっと似てる"
    else:
        emoji = "❌ 全然違う"
    
    print(f"{emoji} 比較 {i}: 「{text}」")
    print(f"        類似度: {similarity:.4f}")
    print()

print("="*60)
print("💡 結論:")
print("- AIに関する文章同士は類似度が高い（0.8以上）")
print("- 天気の話は類似度が低い（0.4以下）")
print("- Embeddingは意味を理解している！すごい！")
print("="*60)
```

---

### 3.3 ベクトル検索の仕組み 🔍

#### ベクトル検索の全体の流れ

```
1. 📚 ドキュメントを小さく分割（チャンク）
   例: 長い記事を段落ごとに分ける
   ↓
2. 🔢 各チャンクをベクトル化
   例: 各段落を数値の配列に変換
   ↓
3. 💾 ベクトルをデータベースに保存
   例: Chromaというツールに保存
   ↓
4. ❓ ユーザーの質問もベクトル化
   例: 「Pythonとは？」→ [0.1, 0.5, ...]
   ↓
5. 🔍 似ているベクトル（チャンク）を検索
   例: 一番近い3つを取得
   ↓
6. 🤖 検索結果をAIに渡して回答生成
```

#### 類似度の計算方法

##### 1. コサイン類似度（一番よく使われる）

「ベクトルの方向がどれだけ似ているか」を測ります。

```python
def cosine_similarity(vec1, vec2):
    """
    コサイン類似度 = cos(θ)
    範囲: -1 ～ 1
    1に近いほど似ている
    """
    return np.dot(vec1, vec2) / (np.linalg.norm(vec1) * np.linalg.norm(vec2))
```

##### 2. ユークリッド距離

「ベクトル間の直線距離」を測ります。

```python
def euclidean_distance(vec1, vec2):
    """
    ユークリッド距離 = √(Σ(a-b)²)
    小さいほど似ている
    """
    return np.linalg.norm(np.array(vec1) - np.array(vec2))
```

---

### 3.4 実際にRAGを作ってみよう！🚀

いよいよ本番です！RAGシステムを実際に作ります。

#### ステップ1: サンプルドキュメントを準備

まず、AIに学習させる「教科書」を作りましょう。

```powershell
mkdir documents
```

`documents` フォルダ内に、以下の3つのMarkdownファイルを作成：

##### ai_basics.md

```markdown
# AI基礎知識 🤖

## AIとは

人工知能（AI）は、人間の知的な振る舞いをコンピューターで真似する技術です。

## 機械学習

機械学習は、データから自動的にパターンを学ぶ技術です。まるでコンピューターが勉強するみたい！

### 教師あり学習

「これが正解だよ」と教えながら学習させる方法です。

例: 猫の写真を見せて「これは猫だよ」と教える

### 教師なし学習

正解を教えずに、データの中から自分でパターンを見つけさせる方法です。

例: たくさんの写真を見せて、勝手にグループ分けさせる

## 深層学習

人間の脳を真似した「ニューラルネットワーク」を使った機械学習です。
めちゃくちゃ賢いけど、計算量が多いのが難点。
```

##### python_guide.md

```markdown
# Python プログラミングガイド 🐍

## Pythonとは

Pythonは、シンプルで読みやすいプログラミング言語です。
初心者にも優しい！

## ファイル操作

### ファイルの読み込み

```python
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()
    print(content)
```

### ファイルの書き込み

```python
with open('file.txt', 'w', encoding='utf-8') as f:
    f.write('Hello, World!')
```

## リスト操作

リストは複数のデータをまとめて扱えます。

```python
fruits = ['apple', 'banana', 'orange']
fruits.append('grape')  # 追加
print(fruits[0])  # 最初の要素
```

##### langchain_intro.md

```markdown
# LangChain入門 🔗

## LangChainとは

LangChainは、LLM（大規模言語モデル）を使ったアプリを簡単に作れるフレームワークです。

## 主な機能

### Chains（チェーン）

複数の処理をつなげて、パイプラインを作れます。

例: 検索 → 要約 → 翻訳

### Agents（エージェント）

ツールを使って自律的にタスクを実行するAIです。

例: 計算機を使って数学の問題を解く

### RAG

文書検索と回答生成を組み合わせた仕組み。
専門知識が必要な質問に答えるのに最適！
```

#### ステップ2: 基本的なRAGシステムを実装

##### basic_rag.py

```python
import os
from dotenv import load_dotenv
from langchain_community.document_loaders import DirectoryLoader
from langchain_text_splitters import CharacterTextSplitter
from langchain_openai import OpenAIEmbeddings
from langchain_chroma import Chroma
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough

load_dotenv()

print("="*60)
print("🚀 RAGシステムを起動中...")
print("="*60)

# ========================================
# 1. 📄 ドキュメントを読み込む
# ========================================
print("\n📚 ステップ1: ドキュメントを読み込んでいます...")
loader = DirectoryLoader(
    './documents/',
    glob='**/*.md',
    show_progress=True
)
documents = loader.load()
print(f"✅ {len(documents)}個のドキュメントを読み込みました！")

# ========================================
# 2. ✂️ テキストを分割
# ========================================
print("\n✂️  ステップ2: ドキュメントを小さく分割しています...")
text_splitter = CharacterTextSplitter(
    chunk_size=500,      # 1チャンク500文字
    chunk_overlap=50,    # 50文字ずつ重複（文脈を保つため）
    separator="\n"
)
splits = text_splitter.split_documents(documents)
print(f"✅ {len(splits)}個のチャンクに分割しました！")

# ========================================
# 3. 🔢 Embeddingモデルを準備
# ========================================
print("\n🔢 ステップ3: Embeddingモデルを初期化しています...")

# OpenAI（精度高いけど有料）
embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)

# 無料で使いたい場合（Ollama）
# from langchain_community.embeddings import OllamaEmbeddings
# embeddings = OllamaEmbeddings(model="llama3.2")

print("✅ Embeddingモデルの準備OK！")

# ========================================
# 4. 💾 ベクトルストアを作成
# ========================================
print("\n💾 ステップ4: ベクトルストアを作成しています...")
print("   （初回は時間がかかります。コーヒータイム☕）")
vectorstore = Chroma.from_documents(
    documents=splits,
    embedding=embeddings,
    persist_directory="./chroma_db"  # ここに保存
)
print("✅ ベクトルストアを作成しました！")

# ========================================
# 5. 🔍 Retriever（検索機能）を作成
# ========================================
retriever = vectorstore.as_retriever(
    search_type="similarity",
    search_kwargs={"k": 3}  # 上位3件を取得
)

# ========================================
# 6. 🤖 RAG Chainを組み立てる
# ========================================
print("\n🔗 ステップ5: RAG Chainを構築しています...")

# LLMモデル（OpenAI使用）
llm = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
    temperature=0  # 正確性重視
)

# プロンプトテンプレート
template = """あなたは親切なアシスタントです。
以下の文脈だけを使って質問に回答してください。
文脈に情報がない場合は「申し訳ありませんが、その情報は資料にありません😅」と答えてください。

文脈:
{context}

質問: {question}

回答（わかりやすく、絵文字も使ってOK）:"""

prompt = ChatPromptTemplate.from_template(template)

# ドキュメントをフォーマット
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

# Chain（流れ作業）を構築
rag_chain = (
    {
        "context": retriever | format_docs,
        "question": RunnablePassthrough()
    }
    | prompt
    | llm
    | StrOutputParser()
)

print("✅ RAG Chainの構築完了！")

# ========================================
# 7. 🎉 質問してみよう！
# ========================================
print("\n" + "="*60)
print("🎉 RAGシステムが準備できました！")
print("="*60)

questions = [
    "機械学習とは何ですか？",
    "Pythonでファイルを読み込む方法を教えてください",
    "LangChainの主な機能は何ですか？",
    "今日の天気は？"  # これは資料にないので「わからない」と答えるはず
]

for question in questions:
    print(f"\n❓ 質問: {question}")
    print("-" * 60)
    
    answer = rag_chain.invoke(question)
    print(f"🤖 回答: {answer}")
    print()

print("="*60)
print("✨ すべての質問が完了しました！")
print("="*60)
```

#### 実行してみよう

```powershell
python basic_rag.py
```

**期待される動作:**
- AI関連の質問には正確に答える
- Pythonの質問にはコード例付きで答える
- 資料にない質問（天気など）には「わからない」と答える

---

## 第4章：Advanced RAG

さらに賢いRAGシステムを作りましょう！🚀

### 4.1 HyDE（仮説的ドキュメント埋め込み）🎯

**HyDE** = 「まず仮の答えを作って、それで検索する」

#### なぜこれが有効？

質問文よりも、答えの文章の方が、実際の資料に似ているからです。

```
質問: 「機械学習って何？」
  ↓ そのまま検索 ❌
資料: 「機械学習は...です」← マッチしづらい

質問: 「機械学習って何？」
  ↓ まず仮の答えを生成
仮答: 「機械学習はデータから学習する技術です」
  ↓ これで検索 ✅
資料: 「機械学習はデータから...」← マッチしやすい！
```

#### hyde_rag.py

```python
import os
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_chroma import Chroma
from langchain_openai import OpenAIEmbeddings
from langchain_core.runnables import RunnablePassthrough
from dotenv import load_dotenv

print("🎯 HyDE（仮説的ドキュメント埋め込み）RAGシステム\n")

# 環境変数を読み込む
load_dotenv()

# モデルとベクトルストア（OpenAI使用）
llm = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)

embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)
vectorstore = Chroma(
    persist_directory="./chroma_db",
    embedding_function=embeddings
)
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

# ========================================
# HyDE: まず仮の回答を生成
# ========================================
hyde_prompt = ChatPromptTemplate.from_template(
    """次の質問に対する回答を、簡潔に1〜2文で書いてください。
正確でなくても構いません。仮の回答で大丈夫です。
    
質問: {question}

仮の回答:"""
)

hyde_chain = hyde_prompt | llm | StrOutputParser()

# ========================================
# RAG Chain with HyDE
# ========================================
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

answer_prompt = ChatPromptTemplate.from_template(
    """以下の文脈を使って質問に回答してください。

文脈:
{context}

質問: {question}

回答:"""
)

# HyDEを組み込んだChain
hyde_rag_chain = (
    {
        "question": RunnablePassthrough(),
        "context": hyde_chain | retriever | format_docs
    }
    | answer_prompt
    | llm
    | StrOutputParser()
)

# 実行
question = "教師なし学習とは？"
print(f"❓ 質問: {question}\n")
print("🔄 処理中...")
print("  1. 仮の回答を生成")
print("  2. 仮の回答で検索")
print("  3. 正確な回答を生成\n")

answer = hyde_rag_chain.invoke(question)
print(f"🤖 回答: {answer}")
```

---

### 4.2 マルチクエリRAG（いろんな角度から検索）🔍

**マルチクエリ** = 「1つの質問から複数の検索クエリを作る」

#### なぜ有効？

同じことでも、言い方を変えると違う情報が見つかることがあります。

```
元の質問: 「Pythonとは？」
  ↓ 複数のクエリに変換
- 「Pythonの特徴」
- 「Pythonプログラミング言語」
- 「Pythonの使い方」
  ↓ それぞれで検索
より多くの関連情報をゲット！✨
```

#### multi_query_rag.py

```python
import os
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_chroma import Chroma
from langchain_openai import OpenAIEmbeddings
from pydantic import BaseModel, Field
from langchain_core.runnables import RunnablePassthrough
from dotenv import load_dotenv
from langchain_core.output_parsers import StrOutputParser

print("🔍 マルチクエリRAGシステム\n")

# 環境変数を読み込む
load_dotenv()

# モデルとベクトルストア（OpenAI使用）
llm = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)

embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)
vectorstore = Chroma(
    persist_directory="./chroma_db",
    embedding_function=embeddings
)
retriever = vectorstore.as_retriever(search_kwargs={"k": 2})

# ========================================
# 複数のクエリを生成
# ========================================
class Queries(BaseModel):
    queries: list[str] = Field(description="生成された検索クエリのリスト")

query_prompt = ChatPromptTemplate.from_template(
    """元の質問に対して、ベクトルデータベースから関連文書を検索するための
異なる3つの検索クエリを生成してください。
異なる角度からアプローチしてください。

元の質問: {question}

検索クエリ（3つ）:"""
)

query_chain = (
    query_prompt 
    | llm.with_structured_output(Queries)
    | (lambda x: x.queries)
)

# ========================================
# 複数クエリで検索
# ========================================
def multi_retrieve(queries: list[str]):
    """複数のクエリでそれぞれ検索して、結果をまとめる"""
    print(f"📝 生成されたクエリ:")
    for i, q in enumerate(queries, 1):
        print(f"   {i}. {q}")
    print()
    
    all_docs = []
    for query in queries:
        docs = retriever.invoke(query)
        all_docs.extend(docs)
    
    # 重複を除去
    unique_docs = []
    seen = set()
    for doc in all_docs:
        if doc.page_content not in seen:
            seen.add(doc.page_content)
            unique_docs.append(doc)
    
    print(f"🔍 検索結果: {len(unique_docs)}件の重複なし文書\n")
    return unique_docs

def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

answer_prompt = ChatPromptTemplate.from_template(
    """以下の文脈を使って質問に回答してください。

文脈:
{context}

質問: {question}

回答:"""
)

# Multi-Query RAG Chain
multi_query_rag_chain = (
    {
        "question": RunnablePassthrough(),
        "context": query_chain | multi_retrieve | format_docs
    }
    | answer_prompt
    | llm
    | StrOutputParser()
)

# 実行
question = "Pythonとは何ですか？"
print(f"❓ 元の質問: {question}\n")
print("="*60)

answer = multi_query_rag_chain.invoke(question)

print("="*60)
print(f"\n🤖 最終回答:\n{answer}")
```

---

### 4.3 リランキング（検索結果を再評価）📊

**リランキング** = 「検索結果をもう一度並び替える」

#### なぜ必要？

最初の検索（Embedding）は速いけど精度がイマイチ。
リランキングモデルでもう一度チェックすると、より関連性の高い順に並びます。

```
初回検索（速い）
  ↓
10件取得
  ↓
リランキング（精度高い）
  ↓
上位3件だけ選ぶ
```

#### reranking_rag.py

```python
import os
from dotenv import load_dotenv
from sentence_transformers import CrossEncoder
from langchain_chroma import Chroma
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough, RunnableLambda

load_dotenv()

print("📊 リランキングRAGシステム\n")

# ========================================
# リランキングモデルの準備
# ========================================
print("🔧 リランキングモデルを読み込んでいます...")

reranker = CrossEncoder(
    'cross-encoder/ms-marco-MiniLM-L-6-v2',
    max_length=512
)

print("✅ リランキングモデルの準備完了！\n")

# ベクトルストア（多めに取得）
embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)
vectorstore = Chroma(
    persist_directory="./chroma_db",
    embedding_function=embeddings
)
base_retriever = vectorstore.as_retriever(search_kwargs={"k": 10})

# LLM（OpenAI使用）
llm = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)

# ========================================
# リランキング関数
# ========================================
def rerank_documents(query: str, documents, top_k: int = 3):
    """リランキングを実行"""
    print(f"🔄 リランキング中...")
    
    # クエリとドキュメントのペアを作成
    pairs = [[query, doc.page_content] for doc in documents]
    
    # スコアを計算
    scores = reranker.predict(pairs)
    
    # スコアでソート
    ranked = sorted(
        zip(documents, scores),
        key=lambda x: x[1],
        reverse=True
    )
    
    print(f"📊 スコア:")
    for i, (doc, score) in enumerate(ranked[:top_k], 1):
        print(f"   {i}. スコア {score:.4f}: {doc.page_content[:50]}...")
    print()
    
    # 上位K件を返す
    return [doc for doc, score in ranked[:top_k]]

# ========================================
# RAG Chain with Reranking
# ========================================
def retrieve_and_rerank(question: str):
    """検索 → リランキング"""
    print(f"🔍 初回検索中...")
    docs = base_retriever.invoke(question)
    print(f"   {len(docs)}件取得\n")
    
    reranked_docs = rerank_documents(question, docs, top_k=3)
    print(f"✅ 上位3件に絞り込み完了\n")
    
    return reranked_docs

def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

answer_prompt = ChatPromptTemplate.from_template(
    """以下の文脈を使って質問に回答してください。

文脈:
{context}

質問: {question}

回答:"""
)

reranking_rag_chain = (
    {
        "question": RunnablePassthrough(),
        "context": RunnableLambda(retrieve_and_rerank) | RunnableLambda(format_docs)
    }
    | answer_prompt
    | llm
    | StrOutputParser()
)

# 実行
question = "深層学習について説明してください"
print(f"❓ 質問: {question}\n")
print("="*60)

answer = reranking_rag_chain.invoke(question)

print("="*60)
print(f"\n🤖 最終回答:\n{answer}")
```

---

## 第5章：評価とテスト

### 5.1 なぜ評価が大切？📏

RAGシステムを作ったら、「ちゃんと動いてるかな？」をチェックする必要があります。

#### 評価の3つのポイント

1. **🎯 関連性**: 検索された文書は質問に関連してる？
2. **✅ 正確性**: 回答は文脈に基づいて正確？
3. **📖 完全性**: 必要な情報を全て含んでる？

---

### 5.2 手動評価（自分で確認）👀

#### manual_evaluation.py

```python
import os
from langchain_chroma import Chroma
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough
from dotenv import load_dotenv

print("📝 手動評価システム\n")

# セットアップ
load_dotenv()
embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)
vectorstore = Chroma(
    persist_directory="./chroma_db",
    embedding_function=embeddings
)
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

# LLM（OpenAI使用）
llm = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)

def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

answer_prompt = ChatPromptTemplate.from_template(
    """以下の文脈を使って質問に回答してください。

文脈:
{context}

質問: {question}

回答:"""
)

rag_chain = (
    {
        "question": RunnablePassthrough(),
        "context": retriever | format_docs
    }
    | answer_prompt
    | llm
    | StrOutputParser()
)

# ========================================
# テストケース
# ========================================
test_cases = [
    {
        "question": "機械学習とは何ですか？",
        "expected_keywords": ["データ", "学習", "パターン"]
    },
    {
        "question": "Pythonでファイルを読み込むには？",
        "expected_keywords": ["open", "read", "with"]
    },
    {
        "question": "LangChainでできることは？",
        "expected_keywords": ["Chain", "Agent", "RAG"]
    },
]

# ========================================
# 評価の実行
# ========================================
print("🧪 テストを開始します...\n")
print("="*60)

results = []

for i, test in enumerate(test_cases, 1):
    print(f"\n📋 テストケース {i}")
    print("-"*60)
    print(f"❓ 質問: {test['question']}")
    print(f"🎯 期待されるキーワード: {', '.join(test['expected_keywords'])}")
    
    # RAGで回答生成
    answer = rag_chain.invoke(test['question'])
    print(f"\n🤖 生成された回答:\n{answer}\n")
    
    # キーワードチェック
    found_keywords = [kw for kw in test['expected_keywords'] if kw in answer]
    score = len(found_keywords) / len(test['expected_keywords']) * 100
    
    print(f"📊 自動評価:")
    print(f"   見つかったキーワード: {found_keywords}")
    print(f"   スコア: {score:.0f}%")
    
    # 手動評価
    print(f"\n👤 あなたの評価:")
    print("   1️⃣ : 完璧！")
    print("   2️⃣ : 良い")
    print("   3️⃣ : まあまあ")
    print("   4️⃣ : ダメ")
    
    manual_score = input("   スコア (1-4): ")
    
    results.append({
        "question": test['question'],
        "auto_score": score,
        "manual_score": manual_score
    })
    
    print("="*60)

# 結果サマリー
print("\n📊 評価結果サマリー\n")
for i, result in enumerate(results, 1):
    print(f"{i}. {result['question']}")
    print(f"   自動: {result['auto_score']:.0f}% | 手動: {result['manual_score']}/4")
```

---

## 第6章：LangGraphで複雑なワークフロー

### 6.1 LangGraphって何？🕸️

**LangGraph** = 「処理の流れをグラフ（フローチャート）で表現できるツール」

#### できること

- 🔄 **ループ**: 品質チェックして、ダメなら再実行
- 🔀 **条件分岐**: 状況に応じて処理を変更
- 💾 **ステート管理**: 会話履歴や中間結果を保持
- ⏸️ **チェックポイント**: 途中から再開可能

---

### 6.2 シンプルなグラフの例

#### simple_graph.py

```python
from langgraph.graph import StateGraph, END
from pydantic import BaseModel, Field, ConfigDict

print("🕸️ LangGraphシンプル例\n")

# ========================================
# ステート（状態）の定義
# ========================================
class State(BaseModel):
    model_config = ConfigDict(arbitrary_types_allowed=True)
    question: str = Field(description="ユーザーの質問")
    answer: str = Field(default="", description="生成された回答")
    score: int = Field(default=0, description="品質スコア")
    retry_count: int = Field(default=0, description="再試行回数")

# ========================================
# ノード（処理）の定義
# ========================================
def answer_node(state: State) -> dict:
    """回答を生成するノード"""
    retry_info = f" (試行{state.retry_count + 1}回目)" if state.retry_count > 0 else ""
    print(f"🤖 回答を生成中{retry_info}: {state.question}")
    
    # 実際にはLLMを呼び出す（ここでは簡易版）
    # 再試行時はより詳細な回答を生成
    base_answer = f"「{state.question}」に関する詳しい回答を生成しました。"
    if state.retry_count > 0:
        extra_detail = "さらに詳細な情報を追加して、より充実した内容にしました。" * state.retry_count
        answer = base_answer + extra_detail
    else:
        answer = base_answer
    
    print(f"   ✅ 完了")
    return {"answer": answer, "retry_count": state.retry_count + 1}

def check_node(state: State) -> dict:
    """品質をチェックするノード"""
    print(f"🔍 品質をチェック中...")
    
    # 簡易的なチェック（実際にはLLMで評価）
    score = len(state.answer)
    
    if score >= 50:
        emoji = "✅"
    else:
        emoji = "❌"
    
    print(f"   {emoji} スコア: {score}")
    return {"score": score}

# ========================================
# グラフの構築
# ========================================
print("🔧 グラフを構築中...\n")

workflow = StateGraph(State)

# ノードを追加
workflow.add_node("answer", answer_node)
workflow.add_node("check", check_node)

# 開始ポイント
workflow.set_entry_point("answer")

# エッジ（流れ）を追加
workflow.add_edge("answer", "check")

# 条件分岐
def should_continue(state: State) -> str:
    """スコアが50以上なら終了、未満なら再生成（最大3回）"""
    if state.score >= 50:
        return "end"
    elif state.retry_count >= 3:
        print("   ⚠️ 最大試行回数に達しました\n")
        return "end"
    else:
        print("   🔄 スコアが低いので再生成します...\n")
        return "retry"

workflow.add_conditional_edges(
    "check",
    should_continue,
    {
        "end": END,
        "retry": "answer"
    }
)

# コンパイル
app = workflow.compile()

# ========================================
# 実行
# ========================================
print("="*60)
print("実行開始")
print("="*60)

initial_state = State(question="Pythonとは？")
result = app.invoke(initial_state)

print("\n" + "="*60)
print("✨ 最終結果")
print("="*60)
print(f"❓ 質問: {result['question']}")
print(f"🤖 回答: {result['answer']}")
print(f"📊 スコア: {result['score']}")
```

---

### 6.3 RAG + LangGraphの実装（超強力！）💪

品質チェックして、ダメなら自動で再検索する賢いRAGを作りましょう！

#### rag_with_graph.py

```python
import os
from langgraph.graph import StateGraph, END
from pydantic import BaseModel, Field, ConfigDict
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_chroma import Chroma
from langchain_openai import OpenAIEmbeddings
from typing import List
from dotenv import load_dotenv

print("🚀 RAG + LangGraph 最強システム\n")

# ========================================
# ステートの定義
# ========================================
class RAGState(BaseModel):
    question: str = Field(description="ユーザーの質問")
    documents: List[str] = Field(default=[], description="検索されたドキュメント")
    answer: str = Field(default="", description="生成された回答")
    is_satisfactory: bool = Field(default=False, description="回答が満足できるか")
    retry_count: int = Field(default=0, description="再試行回数")
    model_config = ConfigDict(arbitrary_types_allowed=True)

# ========================================
# セットアップ
# ========================================
# 環境変数を読み込む
load_dotenv()

# LLM（OpenAI使用）
llm = ChatOpenAI(
    model=os.getenv("OPENAI_MODEL", "gpt-5-mini"),
    openai_api_key=os.getenv("OPENAI_API_KEY"),
)

embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.getenv("OPENAI_API_KEY")
)
vectorstore = Chroma(
    persist_directory="./chroma_db",
    embedding_function=embeddings
)
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

# ========================================
# ノードの定義
# ========================================
def retrieve_node(state: RAGState) -> dict:
    """ドキュメントを検索"""
    print(f"🔍 検索中: {state.question}")
    
    docs = retriever.invoke(state.question)
    doc_texts = [doc.page_content for doc in docs]
    
    print(f"   ✅ {len(doc_texts)}件取得")
    return {"documents": doc_texts}

def generate_node(state: RAGState) -> dict:
    """回答を生成"""
    print(f"🤖 回答を生成中...")
    
    context = "\n\n".join(state.documents)
    
    prompt = ChatPromptTemplate.from_template(
        """以下の文脈を使って質問に回答してください。

文脈:
{context}

質問: {question}

回答:"""
    )
    
    chain = prompt | llm | StrOutputParser()
    answer = chain.invoke({
        "context": context,
        "question": state.question
    })
    
    print(f"   ✅ 生成完了")
    return {"answer": answer}

def evaluate_node(state: RAGState) -> dict:
    """回答の品質を評価"""
    print(f"📊 品質を評価中...")
    
    # 簡易的な評価（実際にはLLMで評価）
    is_good = len(state.answer) > 50 and "わかりません" not in state.answer
    
    emoji = "✅ 合格" if is_good else "❌ 不合格"
    print(f"   {emoji}")
    
    return {
        "is_satisfactory": is_good,
        "retry_count": state.retry_count + 1
    }

# ========================================
# グラフの構築
# ========================================
print("🔧 グラフを構築中...\n")

workflow = StateGraph(RAGState)

# ノードを追加
workflow.add_node("retrieve", retrieve_node)
workflow.add_node("generate", generate_node)
workflow.add_node("evaluate", evaluate_node)

# 開始ポイント
workflow.set_entry_point("retrieve")

# エッジ
workflow.add_edge("retrieve", "generate")
workflow.add_edge("generate", "evaluate")

# 条件分岐
def should_retry(state: RAGState) -> str:
    """再試行するか判定"""
    if state.is_satisfactory:
        return "end"
    elif state.retry_count >= 3:
        print("   ⚠️ 最大試行回数に達しました")
        return "end"
    else:
        print("   🔄 再試行します...\n")
        return "retry"

workflow.add_conditional_edges(
    "evaluate",
    should_retry,
    {
        "end": END,
        "retry": "retrieve"
    }
)

# コンパイル
app = workflow.compile()

# ========================================
# 実行
# ========================================
print("="*60)
print("実行開始")
print("="*60)

question = "機械学習とは何ですか？"
initial_state = RAGState(question=question)

print(f"\n❓ 質問: {question}\n")

result = app.invoke(initial_state)

print("\n" + "="*60)
print("✨ 最終結果")
print("="*60)
print(f"🤖 回答:\n{result['answer']}\n")
print(f"📊 試行回数: {result['retry_count']}")
print(f"✅ 満足度: {'合格' if result['is_satisfactory'] else '不合格'}")
```

---

## トラブルシューティング

### よくあるエラーと解決方法 🔧

#### 1. `ModuleNotFoundError: No module named 'langchain'`

**原因**: パッケージがインストールされていない

**解決**:
```powershell
pip install langchain langchain-openai langchain-community
```

---

#### 2. Ollamaに接続できない 🦙

**エラー**: `Connection refused` または `404 Not Found`

**解決方法（ステップバイステップ）**:

```powershell
# 1. Ollamaが起動しているか確認
ollama list

# 2. 起動していなければ起動
ollama serve

# 3. 別のPowerShellウィンドウで、モデルがあるか確認
ollama list

# 4. なければダウンロード
ollama pull llama3.2
```

---

#### 3. OpenAI APIキーのエラー 🔑

**エラー**: `AuthenticationError: Invalid API key`

**解決**:

1. `.env` ファイルを確認
2. APIキーが正しいか確認:

```python
import os
from dotenv import load_dotenv

load_dotenv()
print(os.getenv("OPENAI_API_KEY"))
```

3. 空だったら `.env` ファイルを修正

---

#### 4. Chromaのエラー 💾

**エラー**: `sqlite3.OperationalError: database is locked`

**解決**:

```powershell
# データベースを削除して再作成
Remove-Item -Recurse -Force .\chroma_db

# スクリプトを再実行
python basic_rag.py
```

---

#### 5. メモリ不足 💻

**エラー**: 動作が遅い、フリーズする

**解決**:

```python
# チャンクサイズを小さくする
text_splitter = CharacterTextSplitter(
    chunk_size=300,  # 500 → 300に減らす
    chunk_overlap=30
)

# 検索件数を減らす
retriever = vectorstore.as_retriever(
    search_kwargs={"k": 2}  # 3 → 2に減らす
)
```

---

#### 6. 日本語が文字化けする 🈁

**解決**:

ファイル操作時に必ず `encoding='utf-8'` を指定：

```python
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()
```

---

## まとめ 🎓

### 🎉 お疲れ様でした！

ここまで学んだこと：

1. **RAGの基礎** 📚
   - ドキュメント検索 + AI回答生成
   - Embeddingの仕組み
   - ベクトル検索

2. **LangChainの使い方** 🔗
   - Chain（処理の連結）
   - Prompt（質問のテンプレート化）
   - Retriever（検索機能）

3. **Advanced RAG** 🚀
   - HyDE（仮説的検索）
   - マルチクエリ（複数角度から検索）
   - リランキング（検索結果の再評価）

4. **LangGraph** 🕸️
   - 複雑なワークフローの構築
   - 品質チェック & 再試行

5. **評価とテスト** 📊
   - システムの品質管理

### 🚀 次のステップ

1. **もっと大きなドキュメント** 📖
   - 実際のマニュアルやドキュメントで試してみよう

2. **Webアプリ化** 🌐
   - Streamlit や FastAPI でWeb UIを作ってみよう

3. **本番環境へのデプロイ** ☁️
   - Docker、AWS、Azureなどで公開

4. **高度な機能** 💎
   - PDF読み込み
   - 画像検索
   - 音声入力

### 📚 おすすめ学習リソース

- [LangChain公式ドキュメント](https://python.langchain.com/) - 詳しい使い方
- [Ollama公式サイト](https://ollama.com/) - ローカルLLMの活用
- [LangGraph](https://langchain-ai.github.io/langgraph/) - 高度なワークフロー
- [OpenAI API](https://platform.openai.com/docs/) - API仕様

---

### 💬 最後に

RAGは進化し続けている技術です。この記事で基礎を学んだあなたは、もう立派な「RAG使い」です！🎉

困ったことがあったら：
1. エラーメッセージをよく読む
2. Google検索（エラーメッセージをコピペ）
3. 公式ドキュメントを確認
4. コミュニティで質問（GitHub Issues、Stack Overflow）

**楽しいRAGライフを！✨**

---

## 付録：実践プロジェクト例 🎯

このガイドで学んだ技術を実際に使っているプロジェクトを紹介します！
社会のどんなところで貢献できそうかを自分なりに考えて作ってみました！

### Terao Navi プロジェクト

**Terao Navi** は、このガイドで学んだRAG技術を活用した、AIチャットシステムです。
Webサイトに簡単に組み込めるヘルプチャットボットとして設計されています。

---

### 付録A: Terao Navi API

**リポジトリ**: [https://github.com/terao06/terao_navi_api](https://github.com/terao06/terao_navi_api)

#### プロジェクト概要

**Terao Navi API** は、FastAPIを使って構築されたバックエンドAPIです。
このガイドで学んだRAG技術を実装したプロジェクトです！

---

### 付録B: Terao Navi SDK

**リポジトリ**: [https://github.com/terao06/terao_navi_sdk](https://github.com/terao06/terao_navi_sdk)

#### プロジェクト概要

**Terao Navi SDK** は、上記のAPIを活用したフロントエンド（チャットウィジェット）です。
わずか2行のコードで、任意のWebサイトにAIチャット機能を組み込めます！

###  付録C: Terao Navi Web

**リポジトリ**: [https://github.com/terao06/terao_navi_web](https://github.com/terao06/terao_navi_web)

#### プロジェクト概要

**Terao Navi Web** はドキュメントを登録する管理画面です。簡単にドキュメントを登録できます。


#### 🎯 実践課題

これらのプロジェクトを使った学習課題：

**初級編:**
1. デモを動かしてみる
2. チャットの色を変えてみる
3. 質問を投げて、どう処理されているか追いかける

**中級編:**
1. 新しいドキュメントを追加してみる
2. プロンプトをカスタマイズして回答の質を改善
3. エラーハンドリングを追加

**上級編:**
1. 新しい機能を追加（画像アップロード、音声入力など）
2. 別のLLMモデルに切り替えてみる
3. パフォーマンスチューニング

**フィードバック歓迎！**

* このガイドは継続的に改善しています。誤りや改善点があれば、お気軽にご指摘ください！

---

<div align="center">

**作成者より**

この記事が役に立ったら、ぜひ実際に手を動かして試してみてください。
失敗を恐れず、どんどん実験しましょう！🚀

あなたも、このガイドで学んだことを活かして、
素晴らしいRAGシステムを作ってください！

**Happy Coding! 🎉**

</div>
