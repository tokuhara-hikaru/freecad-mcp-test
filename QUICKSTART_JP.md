# FreeCAD MCP - クイックスタートガイド

## 概要
FreeCAD MCPは、Claude DesktopからFreeCADを制御できるModel Context Protocol統合です。自然言語でCAD操作を行うことができます。

## 5分でセットアップ

### 1. 必要なソフトウェア
- [FreeCAD](https://www.freecad.org/) (v0.20以降)
- [Claude Desktop](https://claude.ai/)
- [uvx](https://docs.astral.sh/uv/guides/tools/) (Pythonパッケージマネージャー)

### 2. アドオンインストール（1分）
```bash
git clone https://github.com/neka-nat/freecad-mcp.git
cd freecad-mcp
cp -r addon/FreeCADMCP ~/.FreeCAD/Mod/    # Linux/Mac
# Windows: %APPDATA%\FreeCAD\Mod\ にコピー
```

### 3. Claude Desktop設定（2分）
`claude_desktop_config.json`に追加:
```json
{
  "mcpServers": {
    "freecad": {
      "command": "uvx",
      "args": ["freecad-mcp"]
    }
  }
}
```

### 4. 起動（2分）
1. FreeCADを起動
2. ワークベンチから「MCP Addon」を選択
3. 「Start RPC Server」をクリック
4. Claude Desktop再起動

## 基本的な使い方

### 簡単な例
Claude Desktopで以下を試してください:

```
「直径50mm、高さ100mmの円柱を作成してください」
```

```
「立方体（一辺30mm）を作成して、上面に直径10mmの穴を開けてください」
```

```
「利用可能なボルトのパーツを教えてください」
```

### 利用可能な主要操作
- ✅ 基本形状作成 (立方体、円柱、球など)
- ✅ オブジェクトの編集・移動・回転
- ✅ パーツライブラリからの部品挿入
- ✅ 複合操作 (ブーリアン演算など)
- ✅ 2D図面からの3Dパーツ作成
- ✅ カスタムPythonコード実行

## よくある問題と解決法

| 問題 | 解決法 |
|------|--------|
| 「Failed to connect to FreeCAD」 | FreeCADでRPCサーバーが起動しているか確認 |
| アドオンが表示されない | FreeCAD再起動、インストールパス確認 |
| Claude Desktopが反応しない | 設定ファイル確認、Claude Desktop再起動 |

## より詳しい情報
- [完全な日本語ドキュメント](./README_JP.md)
- [英語ドキュメント](./README.md)
- [GitHub Issues](https://github.com/neka-nat/freecad-mcp/issues)

---
💡 **ヒント**: 最初は簡単な形状作成から始めて、徐々に複雑な操作を試してみてください。