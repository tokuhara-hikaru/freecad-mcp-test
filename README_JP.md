# FreeCAD MCP - 日本語ドキュメント

[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/neka-nat-freecad-mcp-badge.png)](https://mseep.ai/app/neka-nat-freecad-mcp)

## 概要

FreeCAD MCPは、Model Context Protocol (MCP) を使用してClaude DesktopからFreeCADを制御することができるプロジェクトです。これにより、自然言語でCADの設計や操作を行うことができます。

## プロジェクトの特徴

### 🚀 主な機能
- **自然言語によるCAD操作**: Claude Desktopから日本語や英語でFreeCADを制御
- **リアルタイムな視覚フィードバック**: 作業結果をスクリーンショットで確認
- **豊富なツールセット**: オブジェクトの作成、編集、削除、コード実行など
- **パーツライブラリ連携**: FreeCADの部品ライブラリから直接部品を挿入

### 🎯 対象ユーザー
- CAD初心者から上級者まで
- 自然言語でのCAD操作を希望するユーザー
- プロトタイプ作成やアイデアの迅速な可視化が必要なユーザー
- AIアシスタントを活用したい設計者

## アーキテクチャ

```
┌─────────────────┐
│   Claude Desktop │ ← ユーザーが自然言語で操作
└─────────┬───────┘
          │ MCP Protocol (JSON-RPC over stdio)
          ▼
┌─────────────────┐
│ FreeCAD MCP     │ ← MCPプロトコルとXML-RPCを橋渡し
│ Server          │
└─────────┬───────┘
          │ XML-RPC (HTTP:9875)
          ▼
┌─────────────────┐
│ FreeCAD +       │ ← CADアプリケーション本体
│ FreeCADMCP      │   + RPCサーバーアドオン
│ Addon           │
└─────────────────┘
```

### コンポーネント説明

1. **Claude Desktop**: ユーザーインターフェース
   - 自然言語でCAD操作を入力
   - 結果を視覚的に確認

2. **FreeCAD MCP Server** (`src/freecad_mcp/`): 
   - MCPプロトコルとXML-RPCの橋渡し
   - Claude DesktopからのリクエストをFreeCADに翻訳
   - FreeCADからの応答をMCP形式で返す

3. **FreeCADMCP Addon** (`addon/FreeCADMCP/`): 
   - FreeCAD内で動作するRPCサーバー
   - ポート9875でXML-RPCリクエストを受信
   - FreeCAD APIを呼び出して実際のCAD操作を実行

4. **FreeCAD**: CADアプリケーション本体
   - 3Dモデリング、アセンブリ、図面作成
   - Pythonスクリプティング機能

## デモ

### フランジの設計
![demo](./assets/freecad_mcp4.gif)

### おもちゃの車の設計
![demo](./assets/make_toycar4.gif)

### 2D図面からの3Dパーツ作成

#### 入力図面
![input](./assets/b9-1.png)

#### 作成デモ
![demo](./assets/from_2ddrawing.gif)

実際の会話履歴は[こちら](https://claude.ai/share/7b48fd60-68ba-46fb-bb21-2fbb17399b48)で確認できます。

## インストール

### 1. FreeCADアドオンのインストール

#### FreeCADアドオンディレクトリの場所
- **Windows**: `%APPDATA%\FreeCAD\Mod\`
- **Mac**: `~/Library/Application Support/FreeCAD/Mod/`
- **Linux**:
  - Ubuntu: `~/.FreeCAD/Mod/` または `~/snap/freecad/common/Mod/` (snap版の場合)
  - Debian: `~/.local/share/FreeCAD/Mod`

#### インストール手順
```bash
git clone https://github.com/neka-nat/freecad-mcp.git
cd freecad-mcp
cp -r addon/FreeCADMCP ~/.FreeCAD/Mod/
```

アドオンをインストール後、FreeCADを再起動してください。

#### アドオンの使用方法
1. ワークベンチリストから「MCP Addon」を選択
   ![workbench_list](./assets/workbench_list.png)

2. 「FreeCAD MCP」ツールバーの「Start RPC Server」コマンドでRPCサーバーを起動
   ![start_rpc_server](./assets/start_rpc_server.png)

### 2. Claude Desktopの設定

#### 前提条件
[uvx](https://docs.astral.sh/uv/guides/tools/)の事前インストールが必要です。

#### 一般ユーザー向け設定
`claude_desktop_config.json`ファイルを編集：

```json
{
  "mcpServers": {
    "freecad": {
      "command": "uvx",
      "args": [
        "freecad-mcp"
      ]
    }
  }
}
```

#### トークンを節約したい場合
テキストフィードバックのみを使用する場合：

```json
{
  "mcpServers": {
    "freecad": {
      "command": "uvx",
      "args": [
        "freecad-mcp",
        "--only-text-feedback"
      ]
    }
  }
}
```

#### 開発者向け設定
まず、リポジトリをクローン：

```bash
git clone https://github.com/neka-nat/freecad-mcp.git
```

設定ファイル：
```json
{
  "mcpServers": {
    "freecad": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/freecad-mcp/",
        "run",
        "freecad-mcp"
      ]
    }
  }
}
```

## 利用可能なツール

### 📋 基本操作ツール
- **`create_document`**: FreeCADで新しいドキュメントを作成
- **`create_object`**: FreeCADで新しいオブジェクトを作成
- **`edit_object`**: FreeCADでオブジェクトを編集
- **`delete_object`**: FreeCADでオブジェクトを削除

### 🔧 高度な操作ツール
- **`execute_code`**: FreeCADで任意のPythonコードを実行
- **`insert_part_from_library`**: [パーツライブラリ](https://github.com/FreeCAD/FreeCAD-library)から部品を挿入

### 👀 情報取得ツール
- **`get_view`**: アクティブビューのスクリーンショットを取得
- **`get_objects`**: ドキュメント内の全オブジェクトを取得
- **`get_object`**: ドキュメント内の特定オブジェクトを取得
- **`get_parts_list`**: [パーツライブラリ](https://github.com/FreeCAD/FreeCAD-library)の部品リストを取得

## 使用方法

### 基本的なワークフロー

1. **FreeCADを起動**し、MCPアドオンでRPCサーバーを開始
2. **Claude Desktop**を開き、FreeCADと接続されていることを確認
3. **自然言語で指示**（例：「直径50mmの円柱を作成して」）
4. **結果を確認**し、必要に応じて修正指示

### 使用例

#### 基本的な形状作成
```
「高さ100mm、幅50mm、奥行き30mmの直方体を作成してください」
```

#### 複雑なアセンブリ
```
「フランジを設計してください。外径100mm、内径50mm、厚さ10mm、ボルト穴を8個（M8用）等間隔で配置してください」
```

#### 2D図面からの3D化
```
「この2D図面を基に3Dパーツを作成してください」
（画像をアップロード）
```

#### パーツライブラリの活用
```
「利用可能なボルトのパーツを教えてください」
「M8×25のボルトを挿入してください」
```

### アセット作成戦略

FreeCAD MCPは効率的なCAD設計のための戦略が組み込まれています：

1. **現状確認**: 作業開始前に`get_objects()`でドキュメントの現状を確認
2. **パーツライブラリ優先**: 
   - `get_parts_list()`で利用可能なパーツを確認
   - 必要な部品がライブラリにあれば`insert_part_from_library()`で挿入
3. **基本形状から作成**: ライブラリに適切な部品がない場合は基本形状（立方体、円柱、球など）を作成
4. **明確な命名**: オブジェクトには分かりやすい名前を付ける
5. **位置・スケール・回転の明示的設定**: `edit_object()`で正確な空間関係を定義
6. **設定確認**: オブジェクト編集後は`get_object()`で設定が正しく適用されているか確認
7. **高度なカスタマイズ**: 必要に応じて`execute_code()`でカスタムPythonスクリプトを実行

### 他のAIフレームワークとの統合

このプロジェクトには、他のAIフレームワークとの統合例も含まれています：

#### Google AI Developer Kit (ADK) との統合
```python
from google.adk.agents.llm_agent import LlmAgent
from google.adk.tools.mcp_tool.mcp_toolset import MCPToolset, StdioServerParameters

# エージェント設定
root_agent = LlmAgent(
    model="gemini-2.5-flash-lite",
    name="cad_design_agent",
    instruction="You are a CAD designer.",
    tools=[
        MCPToolset(
            connection_params=StdioServerParameters(
                command="uv",
                args=["--directory", "/path/to/freecad-mcp", "run", "freecad-mcp"]
            )
        )
    ]
)
```

#### LangChain との統合
```python
from langchain_groq import ChatGroq
from langchain_mcp_adapters.tools import load_mcp_tools
from langgraph.prebuilt import create_react_agent

# LLMの初期化
llm = ChatGroq(
    model="llama-3.1-8b-instant",
    temperature=0.7,
    name="cad_design_agent"
)

# MCPツールを読み込み、エージェントを作成
async with stdio_client(server_params) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()
        tools = await load_mcp_tools(session)
        agent = create_react_agent(llm, tools)
```

## 技術仕様

### 開発環境
- **Python**: >=3.12
- **主要依存関係**: 
  - `mcp[cli]>=1.12.2`
- **ビルドシステム**: Hatchling

### 通信プロトコル
- **Claude Desktop ↔ MCP Server**: Model Context Protocol (JSON-RPC over stdio)
- **MCP Server ↔ FreeCAD**: XML-RPC (HTTP)

### データシリアライゼーション
FreeCADオブジェクトは以下の情報で構造化されます：
- **基本情報**: Name, Label, TypeId
- **プロパティ**: すべてのオブジェクトプロパティ
- **配置情報**: Position, Rotation (Placement)
- **形状情報**: Volume, Area, Vertex/Edge/Face数
- **表示設定**: Color, Transparency, Visibility

### サポートされているFreeCADバージョン
- FreeCAD 0.20以降（推奨）

## プロジェクト構成

```
freecad-mcp/
├── README.md              # 英語ドキュメント
├── README_JP.md           # 日本語ドキュメント（このファイル）
├── pyproject.toml         # Python プロジェクト設定
├── src/
│   └── freecad_mcp/       # MCPサーバーのメインコード
│       ├── __init__.py
│       ├── server.py      # MCPサーバー実装
│       └── py.typed
├── addon/
│   └── FreeCADMCP/        # FreeCADアドオン
│       ├── Init.py
│       ├── InitGui.py
│       └── rpc_server/    # RPCサーバー実装
├── examples/              # 使用例
│   ├── adk/
│   └── langchain/
└── assets/                # デモ画像・GIF
```

## 貢献

このプロジェクトは[neka-nat](https://github.com/neka-nat)により開発されています。

<a href="https://github.com/neka-nat/freecad-mcp/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=neka-nat/freecad-mcp" />
</a>

Made with [contrib.rocks](https://contrib.rocks).

## ライセンス

MIT License

## 注意事項

- FreeCADとMCPサーバーの両方が同時に動作している必要があります
- 初回接続時は、FreeCADでRPCサーバーが起動されていることを確認してください
- 複雑な操作の場合、処理に時間がかかる場合があります

## トラブルシューティング

### よくある問題

1. **「Failed to connect to FreeCAD」エラー**
   - FreeCADが起動しているか確認
   - MCPアドオンでRPCサーバーが起動されているか確認
   - ポート9875が使用可能か確認（`netstat -an | grep 9875`）

2. **アドオンが表示されない**
   - FreeCADを再起動
   - アドオンが正しいディレクトリにインストールされているか確認
   - FreeCADのPythonバージョンと互換性があるか確認

3. **Claude Desktopが接続できない**
   - `claude_desktop_config.json`の設定を確認
   - uvxがインストールされているか確認（`uvx --version`）
   - パスが正しく設定されているか確認

4. **「Permission denied」エラー**
   - アドオンフォルダの権限を確認
   - 管理者権限でFreeCADを実行してみる

5. **パーツライブラリが見つからない**
   - FreeCAD Libraryアドオンがインストールされているか確認
   - ライブラリのパスが正しく設定されているか確認

### パフォーマンスの最適化

- **テキストフィードバックのみを使用**: `--only-text-feedback`オプションでトークン消費を削減
- **複雑な操作の分割**: 大きな操作は小さなステップに分けて実行
- **適切なビューの選択**: 必要に応じて適切なビューアングルを指定

### サポート

問題が解決しない場合は、以下の情報と共に[GitHub Issues](https://github.com/neka-nat/freecad-mcp/issues)でお気軽にお問い合わせください：
- FreeCADのバージョン
- オペレーティングシステム
- エラーメッセージの全文
- 実行しようとした操作の詳細

## FAQ（よくある質問）

### Q: このツールは商用利用可能ですか？
A: はい、MITライセンスの下で商用利用も可能です。

### Q: FreeCADのどのワークベンチがサポートされていますか？
A: 基本的にすべてのワークベンチで動作しますが、Part Design、Assembly、Draftワークベンチでの動作が特に最適化されています。

### Q: オフラインで使用できますか？
A: FreeCAD部分はオフラインで動作しますが、Claude Desktopはインターネット接続が必要です。

### Q: 他のCADソフトウェアとの統合予定はありますか？
A: 現在のところFreeCADのみサポートしていますが、将来的に他のCADソフトウェアとの統合も検討されています。

### Q: 大きなアセンブリファイルも処理できますか？
A: 可能ですが、パフォーマンスはファイルサイズとハードウェア性能に依存します。大きなファイルの場合は処理時間が長くなる可能性があります。

### Q: カスタムマクロやスクリプトは使用できますか？
A: はい、`execute_code`ツールを使用して任意のPythonコードを実行できます。