# OpenAIのLangChain入門 〜真面目に楽しくLLMと遊ぶ〜

## LangChainとは
「LLM（大規模言語モデル）ってすごいけど、なんか単体で使うのは勿体ないよね…」  
そんなあなたのモヤモヤを解決してくれるのが **LangChain** です。

一言でいえば、  
**「LLMをただの“おしゃべりAI”から、“仕事ができる相棒”に変えてくれるライブラリ」**。  

例えば…  
- ChatGPTに「電卓して！」って言ったら「私は計算苦手なんだよね…」となるのを、LangChainなら解決。  
- 自社のマニュアルを丸ごと読み込ませて「社内Q&Aボット」を作れる。  
- ちょっと賢いエージェントにして「検索→計算→答えをまとめる」みたいなタスクもこなせる。  

つまり、LangChainは **「LLMの能力を拡張する魔法のツールボックス」** なんです。  

---

## 主な機能とその詳細

### 🎨 プロンプトテンプレート（PromptTemplates）
毎回同じような指示文を書くの、面倒じゃないですか？  
**PromptTemplate**は「雛形」を作っておいて、必要な部分だけ差し替えできる仕組みです。  

> 例えるなら「お年玉袋に名前を書き換えて毎年使い回す」感じです（※現実では怒られるので注意）。

```python
from langchain.prompts import PromptTemplate
from langchain.chat_models import ChatOpenAI

template = PromptTemplate(
    input_variables=["language"],
    template="{language}について簡潔に説明してください。"
)
prompt = template.format(language="Python")

llm = ChatOpenAI(temperature=0)
response = llm.predict(prompt)
print(response)
```

---

### 🔗 LLMチェーン（LLMChains）
PromptTemplateとLLMを組み合わせて「流れ作業」にするのが**LLMChain**。  

「質問を受ける → プロンプトを作る → モデルに聞く → 答えを返す」  
これを**一発で**やってくれます。  

> つまり「ワンオペ店員」から「分業チーム」への進化。

```python
from langchain.prompts import PromptTemplate
from langchain.llms import OpenAI
from langchain.chains import LLMChain

llm = OpenAI(temperature=0)
prompt = PromptTemplate(
    input_variables=["topic"],
    template="{topic}に関する3つのトリビアを教えてください。"
)

chain = LLMChain(llm=llm, prompt=prompt)
print(chain.run("富士山"))
```

---

### 🕵️ エージェント（Agents）
**エージェントは「LLMに自分で考えさせてツールを使わせる」仕組み。**  

「ニュース調べて → 必要なら計算して → まとめて」みたいに、**AIが勝手にタスクを分解して動く**んです。  
> つまり「指示待ち人間」じゃなく「自律行動型社員」へ進化。

```python
from langchain.agents import load_tools, initialize_agent, AgentType
from langchain.llms import OpenAI

llm = OpenAI(temperature=0)
tools = load_tools(["llm-math"], llm=llm)

agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION)
print(agent.run("144の平方根は？"))
```

---

### 🔧 ツール（Tools）
ツールは**エージェントの「手足」**です。  
電卓、Web検索、DBアクセスなど、外部の力を借りられます。  

しかも自作可能！例えば「文字を逆さにするツール」を作ったら、AIがそれも使えるようになります。  
> ちょっと悪用したら「逆さ言葉しか喋らないAI」とか作れそうですね（役立つかは別問題）。

```python
from langchain.agents import Tool

def reverse_text(text: str) -> str:
    return text[::-1]

reverse_tool = Tool(
    name="reverse",
    func=reverse_text,
    description="与えられたテキストを逆順に返すツール"
)

print(reverse_tool.func("Hello"))  # "olleH"
```

---

### 🧠 メモリ（Memory）
普通のLLMは「金魚並みの記憶力」。  
会話が終わるとすぐ忘れます。  

でもLangChainの**Memory**を使えば、会話の履歴を保持して「前に言ったことを覚えてるAI」にできます。  
> まさに「昨日の夕飯も覚えてる頼れる相棒」。

```python
from langchain import OpenAI, ConversationChain

conversation = ConversationChain(llm=OpenAI(temperature=0), verbose=True)

print(conversation.predict(input="こんにちは、私の名前は太郎です。"))
print(conversation.predict(input="私の名前は何ですか？"))  # → 「太郎」と返してくれる
```

---

### 📚 ドキュメントローダー & テキスト分割
「LLMに100ページのマニュアル読ませたい！」  
→ そのまま突っ込むと「ごめん、長すぎる…」と怒られます。  

そこで **Document Loader**でデータを読み込み、**Text Splitter**で小分けにします。  
> 例えるなら「1本丸ごとのバゲット」じゃなく「スライスして食べやすくする」感じ。

```python
from langchain.document_loaders import TextLoader
from langchain.text_splitter import CharacterTextSplitter

loader = TextLoader("example.txt", encoding="utf-8")
documents = loader.load()

splitter = CharacterTextSplitter(chunk_size=500, chunk_overlap=0)
docs = splitter.split_documents(documents)
print(len(docs))
```

---

### 🗂 ベクターストア & リトリーバル
LLMは賢いけど「昨日のニュース」や「自社マニュアル」までは知らない。  
そこで登場するのが**ベクターストア**。  

文章をベクトル化して保存し、似てるものを高速検索！  
これで「社内特化型ChatGPT」も作れちゃう。  
> 要するに「AIの脳にUSBメモリを差す」イメージです。

```python
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import FAISS

texts = [
    "東京都は日本の首都です。",
    "パリはフランスの首都です。",
    "富士山は日本の最高峰の山です。"
]

vector_store = FAISS.from_texts(texts, embedding=OpenAIEmbeddings())
result = vector_store.similarity_search("日本の首都はどこ？", k=1)
print(result[0].page_content)
```

---

## まとめ
LangChainは、  
- PromptTemplates → **言葉のテンプレ職人**  
- LLMChains → **自動化された流れ作業**  
- Agents & Tools → **自律的に動くAI社員**  
- Memory → **記憶力を与える脳**  
- Document Loaders & Splitters → **情報の仕込み包丁**  
- Vector Stores → **知識USB**  

といった感じで、LLMを「ただの会話相手」から「万能アシスタント」へ進化させるライブラリです。  

AIに「もっとちゃんと働け！」と思ったら、ぜひLangChainで遊んでみてください。  

## 付録

### OpenAIのオープンモデル「gpt-oss」（8月5日発表）

2025年8月5日、OpenAIは「gpt-oss」というオープンモデル（オープンウェイト）を公開しました。ローカルやオンプレでの運用、細かなカスタマイズ、既存スタックとの柔軟な統合を重視した位置づけのモデルです。

主なポイントは以下のとおりです。

- 概要
    - オープンウェイトとして提供され、クラウドに依存しない実行が可能（自社環境での運用に向く）。
    - OpenAI互換のAPIやツール群から利用しやすい設計が想定され、既存のワークフローへ置き換え／併用がしやすい。

- できること（能力の目安）
    - 指示追従（Instruction Following）：要約、リライト、説明、翻訳などの汎用タスク。
    - コード関連：サンプル生成、変換、説明、簡易リファクタリング。
    - 構造化出力：JSONなどのフォーマットでの整形出力の誘導がしやすい。
    - 長文コンテキスト：RAG（社内文書検索＋要約）との相性がよく、業務知識ベースのQAに有用。
    - エージェント連携：検索や電卓などのツール実行と組み合わせたタスク分解・自動化と親和性が高い。

- LangChainとの相性と活用アイデア
    - Chat/LLMインターフェースに接続して、PromptTemplatesやLLMChainをそのまま活用可能。
    - Embeddings＋Vector StoreでRAG基盤を構築し、社内特化型のQA／ナレッジ検索を強化。
    - Agents＋Tools＋Memoryと組み合わせて「検索→要約→整形→配信」などの業務フローを自動化。

- セットアップのヒント（OpenAI互換エンドポイント例）
    - OpenAI互換APIが用意されている場合は、ベースURLとモデル名を切り替えるだけで最小変更で移行可能。
    - ローカル実行では量子化（例：int4/8）でメモリ削減、GPUがあればレイテンシ短縮が期待できる。

```python
# 例：OpenAI互換エンドポイント経由での接続（概念例）
from langchain.chat_models import ChatOpenAI

llm = ChatOpenAI(
        model_name="gpt-oss",
        openai_api_base="http://localhost:8000/v1",  # 提供されるエンドポイントに合わせて調整
        openai_api_key="dummy",                      # ローカル/自己ホスト時はダミーでOKな場合あり
        temperature=0.2,
)

print(llm.predict("このモデルの特徴を3点で教えて"))
```

注意：具体的なモデルID、対応するAPIパラメータ、コンテキスト長、ライセンス条件などは公式アナウンスに従ってください。上記コードは接続方法のイメージです。

### gpt-ossを組み込んだアプリを作ってみた!

gpt-ossとLangChainを活用して、**自然言語でレストラン予約ができるAIアシスタント**を作成しました！

#### 📱 アプリ概要
「今日の夜、和食でおいしいところない？」  
「渋谷で3人で入れるイタリアンを予約したい」  

こんな自然な会話でレストラン検索から予約確認まで、AIが自動で処理してくれるアシスタントアプリです。

#### 🛠️ システム構成

**AIエージェント部分（gpt-oss + LangChain）**
- **役割**: ユーザーの自然言語を理解し、適切なAPIを選択・実行
- **使用技術**: gpt-oss（ローカル実行）, LangChain Agents & Tools
- **処理フロー**: 質問解析 → API選択 → 結果整理 → 自然な回答生成

**API構成（5つのエンドポイント）**

1. おすすめレストラン取得API
   - 用途: 「おいしいお店を教えて」系の質問に対応
   - 返却: エリア・ジャンル別のおすすめ店舗リスト

2. レストラン名検索API  
   - 用途: 「〇〇というお店を探して」系の質問に対応
   - 返却: 店舗ID + 基本情報（住所、電話番号、ジャンル等）

3. 店舗詳細情報取得API
   - 用途: 特定店舗の詳しい情報が必要な場合
   - 返却: 営業時間、メニュー、料金帯、設備情報等

4. 予約可能性確認API
   - 用途: 「予約できる？」の確認（AIが自動実行）
   - 返却: 予約可否 + 空席状況 + 予約条件

(5. 実際の予約実行API )⚠️
   - 用途: 最終確認後の予約処理
   - **重要**: AIの誤作動防止のため、フロントエンドから直接実行
   - 実行タイミング: ユーザーが予約内容を確認し「予約する」ボタンを押下後
#### 💡 実際の動作例

**ユーザー**: 「今夜8時に新宿で2人でイタリアンを食べたいんですが」

**AIの思考プロセス**:
1. 🔍 `/recommend` で新宿のイタリアンを検索
2. 📋 複数候補から条件に合うものを絞り込み
3. ✅ `/can_reserve` で各店舗の予約可能性を確認
4. 💬 「〇〇というお店が空いています。予約しますか？」と提案

**安全機能**: 
- 予約確認まではAIが自動処理
- 実際の予約は人間が最終判断（誤操作防止）
- ローカル実行により、機密情報の外部送信を回避

#### 🎬 デモ動画

実際のアプリの動作を動画で確認できます：

*📺 [デモ動画を見る](https://youtu.be/ST5RmIt7vKk) - 自然言語での問い合わせから予約確認までの一連の流れ*

#### 🎯 このアプリの価値
- **自然な対話**: 「検索条件を入力」ではなく「普通の会話」で利用可能
- **コンテキスト理解**: 「2人で」「今夜」などの情報を総合的に処理  
- **安全性**: 重要な操作は人間が最終確認
- **プライバシー**: gpt-ossのローカル実行で情報漏洩リスクを最小化

> まさに「AIコンシェルジュが店選びから予約確認まで丸ごとお手伝い」する未来の予約体験！

---

## あなたも gpt-oss でアプリを作ってみませんか？

今回ご紹介したレストラン予約AIアシスタントは、gpt-oss、LangChainと自作APIの組み合わせで実現できました。

**gpt-oss の魅力**：
- 🏠 **ローカル実行** → プライバシー保護 & コスト削減
- 🔧 **カスタマイズ自由** → 自社データで追加学習も可能
- 🔗 **既存システム連携** → OpenAI互換APIで簡単移行
- ⚡ **リアルタイム処理** → レスポンス速度の最適化

**あなたなら何を作りますか？**
- 📚 社内FAQ自動回答システム
- 🛒 商品レコメンデーションAI
- 📊 データ分析レポート生成ツール
- 🎯 カスタマーサポート自動化
- 💡 アイデア整理 & 企画書作成アシスタント

LangChainの豊富なツールと、gpt-ossのローカル実行の安心感。  
この組み合わせなら、**「こんなAIツールがあったらいいな」**を現実にできます。

**ぜひ挑戦してみてください！**
**あなたの創造力 × AI の力で、きっと面白いものが生まれるはずです。**

