# 展示品ページの増殖ガイド

このドキュメントは、`exhibit-nubuck` をひな形として、GitHub Pages に新しい展示品ページを追加する方法を説明します。

## 📋 前提条件

- GitHub アカウント
- ローカル環境に Git がインストール済み
- Python 3.8 以上がインストール済み

---

## 🎯 新しい展示品ページの作成手順

### 1. GitHub リポジトリを作成

1. GitHub にログインして新規リポジトリを作成
2. リポジトリ名：`exhibit-{商品名}` （例：`exhibit-suede`, `exhibit-leather`）
3. **Public** に設定
4. README は作成しない（後で追加）

### 2. ローカルに clone

```powershell
cd $env:USERPROFILE
git clone https://github.com/{ユーザー名}/exhibit-{商品名}.git
cd exhibit-{商品名}
```

### 3. ひな形ファイルをコピー

exhibit-nubuck から以下をコピー：

```powershell
# Windows PowerShell
Copy-Item "C:\Users\misaki_ogawa\exhibit-nubuck\index.html" .
Copy-Item "C:\Users\misaki_ogawa\exhibit-nubuck\generate_qr.py" .
Copy-Item "C:\Users\misaki_ogawa\exhibit-nubuck\remove_bg.py" .
mkdir images
Copy-Item "C:\Users\misaki_ogawa\exhibit-nubuck\images\nubuck-hero.jpg" images\hero.jpg
```

---

## 🖼️ 画像の準備

### hero.jpg（ヘロー画像）
- サイズ：**1200x400px** 以上推奨
- 形式：JPEG または PNG
- 用途：ページトップに表示される大きな画像

### logo.png（QR コード埋め込み用）
#### 白背景の場合：

```powershell
python remove_bg.py
```

自動的に白背景を透明に変換します。

#### 手動で透明化する場合：
1. Photoshop または GIMP で編集
2. 背景を削除
3. PNG 形式で保存（背景透明）

---

## 📝 index.html の編集

### 1. メタ情報の変更

```html
<title>exhibit-{商品名} | {商品説明}</title>
```

### 2. hero タイトルの変更

```html
<div class="hero-title">
    <h1>exhibit-{商品名}</h1>
    <p>{商品の説明}</p>
</div>
```

### 3. セクションの編集

各セクションを商品に合わせて編集：

```html
<section>
    <h2>🔍 {セクションタイトル}</h2>
    <ul>
        <li>特徴1</li>
        <li>特徴2</li>
        <li>特徴3</li>
    </ul>
</section>
```

### 4. footer-note の変更

```html
<div class="footer-note">
    <p><strong>注記：</strong>このページは展示品の解説ページです。</p>
    <p><strong>企業サイト：</strong><a href="https://www.{会社URL}/" target="_blank">会社名</a></p>
</div>
```

---

## 🎨 CSS のカスタマイズ（オプション）

### 色を変更したい場合

デスクトップの CSS 部分で以下を編集：

```css
section h2 {
    color: #667eea;  /* ← この色を変更 */
    border-bottom-color: #667eea;  /* ← この色も合わせる */
}

.en-section h2 {
    color: #764ba2;  /* ← 別の色に変更可 */
    border-bottom-color: #764ba2;
}
```

色コード例：
- 青：`#667eea`
- 紫：`#764ba2`
- 赤：`#e74c3c`
- 緑：`#27ae60`
- オレンジ：`#f39c12`

---

## 🔗 QR コードの生成

### 1. URL を確認

リポジトリの URL：`https://github.com/{ユーザー名}/exhibit-{商品名}`

GitHub Pages URL：`https://{ユーザー名}.github.io/exhibit-{商品名}/`

### 2. generate_qr.py を編集

```python
if __name__ == "__main__":
    GITHUB_URL = "https://{ユーザー名}.github.io/exhibit-{商品名}/"
    OUTPUT_PATH = Path(__file__).parent / "qr_code.png"
    LOGO_PATH = Path(__file__).parent / "logo.png"
    generate_qr_code(GITHUB_URL, OUTPUT_PATH, LOGO_PATH)
```

### 3. QR コード生成実行

```powershell
python generate_qr.py
```

**出力：** `qr_code.png` （ロゴ埋め込み版）

---

## 📤 GitHub に push

### 1. ファイルを git に追加

```powershell
git add .
git commit -m "初期化: exhibit-{商品名} のテンプレート作成"
```

### 2. push

```powershell
git push origin main
```

### 3. GitHub Pages を有効化

1. GitHub のリポジトリページ → **Settings**
2. **Pages** セクション
3. **Source** → `main branch` を選択
4. Save

**数分待つと**、`https://{ユーザー名}.github.io/exhibit-{商品名}/` でページが公開されます！

---

## 📋 チェックリスト

新しい展示品ページを作成する際のチェックリスト：

- [ ] GitHub リポジトリを作成
- [ ] ローカルに clone
- [ ] ひな形ファイルをコピー
- [ ] hero.jpg を準備（背景あり OK）
- [ ] logo.png を準備（背景透明）
- [ ] index.html を編集
  - [ ] タイトル変更
  - [ ] hero タイトル変更
  - [ ] セクション内容編集
  - [ ] footer-note 編集
- [ ] CSS 色をカスタマイズ（必要に応じて）
- [ ] generate_qr.py の URL を編集
- [ ] QR コードを生成
- [ ] git add / commit / push
- [ ] GitHub Pages を有効化
- [ ] 公開ページを確認

---

## 🚀 高速テンプレート作成スクリプト（オプション）

新規ページ作成を自動化したい場合は、以下の Python スクリプトを実行：

```python
# create_exhibit.py
import shutil
from pathlib import Path

def create_new_exhibit(product_name):
    # template から新しいディレクトリを作成
    template_path = Path("../exhibit-nubuck")
    new_path = Path(f"../exhibit-{product_name}")
    
    # ファイルをコピー
    shutil.copytree(template_path, new_path, ignore=shutil.ignore_patterns('.git'))
    print(f"✅ exhibit-{product_name} を作成しました！")
    print(f"📁 {new_path}")

# 使用例：
# create_new_exhibit("suede")
```

---

## 📸 例：複数ページの構成

```
fuku86.github.io (GitHub Pages リポジトリ)
├── exhibit-nubuck/
│   ├── index.html
│   ├── generate_qr.py
│   ├── qr_code.png
│   └── images/
│       └── nubuck-hero.jpg
├── exhibit-suede/
│   ├── index.html
│   ├── generate_qr.py
│   ├── qr_code.png
│   └── images/
│       └── suede-hero.jpg
└── exhibit-leather/
    ├── index.html
    ├── generate_qr.py
    ├── qr_code.png
    └── images/
        └── leather-hero.jpg
```

各ページは独立した GitHub リポジトリ、または 1 つのリポジトリで管理可能です。

---

## 🆘 トラブルシューティング

### GitHub Pages が表示されない
- Settings → Pages → Source が `main branch` に設定されているか確認
- 5 分待ってからリロード
- GitHub Status ページで障害がないか確認

### QR コードが生成されない
```powershell
python generate_qr.py  # エラーメッセージを確認
```

### ロゴが埋め込まれない
- `logo.png` が存在するか確認
- PNG 形式か確認（JPEG は不可）

---

## 💡 Tips

- セクションを追加する場合は、既存の section をコピーして編集
- emoji を活用すると見やすくなります（🖋️ 📏 🎨 など）
- 複数言語対応する場合は、`.en-section` クラスを使用
- モバイル横画面固定で表示されるため、横長レイアウトで テスト

---

**Happy Exhibiting! 🎉**
