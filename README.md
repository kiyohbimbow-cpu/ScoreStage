# ScoreStage
譜面閲覧・手書き注釈・セットリスト管理ができる譜面ビューア
演奏者が本番で使えるAndroid向けPDF譜面ビューアアプリ。
ストレスフリーな操作感で、ライブやリハーサル中でも快適に譜面を閲覧できます。

## 🎯 プロジェクト目標

演奏中にストレスなく譜面を閲覧・操作できることを最優先にした、ミュージシャン向けの「本番用譜面アプリ」です。

## ✨ 主な機能

### MVP（最小限の実装）
現在実装済みの機能:

- ✅ **PDFインポート**: Storage Access Frameworkを使用したPDFファイルの読み込み
- ✅ **譜面ライブラリ**: タイトル、アーティスト、作曲者による管理
- ✅ **譜面閲覧**: Android PdfRendererによる高品質なPDF表示
- ✅ **ページ送り**: 左右タップ、スワイプによる直感的なページ操作
- ✅ **検索機能**: タイトル、アーティスト、作曲者による検索
- ✅ **お気に入り**: 頻繁に使用する譜面をマーク
- ✅ **最近使用**: 最近開いた譜面を素早くアクセス
- ✅ **ブックマーク**: 特定のページに素早くジャンプ
- ✅ **Performance Mode**: 誤タップ防止のためのミニマルUI

### 今後実装予定の機能

- ⏳ **半ページ送りモード**: 縦長の譜面を上下に分割して表示
- ⏳ **2ページ表示モード**: タブレット/横向きでの見開き表示
- ⏳ **Bluetoothページターナー対応**: ペダルでのページ送り
- ⏳ **注釈機能**: ペン、蛍光ペン、消しゴム、undo/redo
- ⏳ **余白トリミング**: ページ単位または一括でのトリミング
- ⏳ **セットリスト**: ライブ用の譜面リスト管理
- ⏳ **ジャンプターゲット**: D.S. / D.C. / リピート対応

## 🏗️ アーキテクチャ

### 技術スタック

- **言語**: Kotlin
- **UI**: Jetpack Compose + Material 3
- **アーキテクチャ**: MVVM (Model-View-ViewModel)
- **DI**: Hilt
- **データベース**: Room
- **PDF描画**: Android PdfRenderer（将来的にPDFiumへの差し替え可能な設計）
- **非同期処理**: Kotlin Coroutines + Flow
- **ナビゲーション**: Jetpack Navigation Compose

### プロジェクト構成

```
ScoreStage/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/musicsheet/viewer/
│   │   │   │   ├── MusicSheetApp.kt          # Application class
│   │   │   │   ├── MainActivity.kt            # Main activity
│   │   │   │   ├── core/                      # Core utilities
│   │   │   │   │   ├── pdf/                   # PDF abstraction layer
│   │   │   │   │   │   ├── PdfRenderer.kt
│   │   │   │   │   │   └── AndroidPdfRendererImpl.kt
│   │   │   │   │   └── util/                  # Utility classes
│   │   │   │   ├── data/                      # Data layer
│   │   │   │   │   ├── local/
│   │   │   │   │   │   ├── entity/            # Room entities
│   │   │   │   │   │   ├── dao/               # Room DAOs
│   │   │   │   │   │   └── database/          # Database
│   │   │   │   │   └── repository/            # Repository implementations
│   │   │   │   ├── domain/                    # Domain layer
│   │   │   │   │   ├── model/                 # Domain models
│   │   │   │   │   ├── repository/            # Repository interfaces
│   │   │   │   │   └── usecase/               # Use cases (future)
│   │   │   │   ├── di/                        # Hilt modules
│   │   │   │   └── ui/                        # UI layer
│   │   │   │       ├── navigation/            # Navigation setup
│   │   │   │       ├── theme/                 # Theme & styling
│   │   │   │       ├── library/               # Library screen
│   │   │   │       └── reader/                # Reader screen
│   │   │   └── res/                           # Resources
│   │   └── AndroidManifest.xml
│   └── build.gradle.kts
├── gradle/
│   └── libs.versions.toml                     # Version catalog
└── build.gradle.kts
```

## 🚀 ビルドと実行

### 必要環境

- Android Studio Hedgehog (2023.1.1) 以上
- JDK 11 以上
- Android SDK 35 (コンパイル)
- 最小SDK: 29 (Android 10)

### ビルド手順

1. プロジェクトをAndroid Studioで開く
2. Gradle Syncを実行
3. エミュレータまたは実機を接続
4. Run ボタンでアプリを起動

```bash
# コマンドラインからのビルド
./gradlew assembleDebug

# インストール
./gradlew installDebug
```

## 📱 使い方

### 1. PDFのインポート
- 右下の「+」ボタンをタップ
- デバイスからPDFファイルを選択
- 自動的にライブラリに追加されます

### 2. 譜面の閲覧
- ライブラリから譜面をタップして開く
- **左タップ**: 前のページへ
- **右タップ**: 次のページへ
- **中央タップ**: UI表示/非表示切り替え
- 下部スライダーでページジャンプ

### 3. お気に入りとブックマーク
- 星アイコンでお気に入りに追加
- 閲覧中にブックマークアイコンでページをマーク

## 🎨 UX/UI設計の重点

### 演奏中の使いやすさ
- **大きなタップエリア**: 指が太くても確実に操作できる
- **誤タップ防止**: Performance Modeで余計なUIを最小化
- **1アクション到達**: よく使う機能は1タップで実行
- **直感的な操作**: 学習コスト不要、本番中でも迷わない

### パフォーマンス最適化
- PDFページの先読みキャッシュ（前後2ページ）
- Bitmapの適切なリサイクルでメモリ節約
- 画面回転に強い設計

## 📊 データモデル

### Score (譜面)
- タイトル、アーティスト、作曲者
- PDFファイルURI、ページ数
- お気に入りフラグ、最終閲覧日時

### Bookmark (ブックマーク)
- 譜面IDとページインデックス
- ブックマーク名

### その他
- Tag (タグ分類)
- Setlist (セットリスト)
- AnnotationStroke (注釈ストローク)
- PageEdit (ページ編集情報)
- JumpTarget (ジャンプポイント)

## 🎼 MusicXML Reader の思想

ScoreStage の MusicXML Reader は、作譜ソフトではなく「現場で読むためのビューア」です。  
そのため、XML を機械的に忠実復元することよりも、**人間が読む譜面として破綻しないように安定して解釈すること** を優先します。

### 役割の違い
- **ScoreWeave**: 譜面を作るための解釈
- **ScoreStage**: 譜面を読むための解釈

どちらも MusicXML を扱いますが、重心が違います。  
ScoreStage では編集向けの厳密内部モデルよりも、閲覧・注釈・本番運用に向いた正規化済みの表示モデルを優先します。

### 進め方
- MusicXML の記述をそのまま鵜呑みにしない
- XML 上の揺れや不足があっても、譜面として自然に表示する
- 読めることを優先し、必要なら補正する
- 補正した箇所は内部で追跡できるようにする
- ただし元データは破壊しない

### 推奨パイプライン
MusicXMLParser  
→ RawMusicXmlModel  
→ MusicXmlNormalizationEngine  
→ ReadableScoreModel  
→ DisplayLayoutEngine  
→ RenderedScorePageModel

### 重点要件
- 壊れ気味の MusicXML でもなるべく読める
- `attributes` の継承や不足を補完する
- `tie / beam / accidental / rest / voice` の整合性を正規化する
- 改行や段組は表示用に最適化できる
- recoverable warning を持つ
- ページ単位・段単位でキャッシュしやすい
- 注釈レイヤーを後から重ねやすい

### 補足
ScoreStage は「読めること」「事故らないこと」「重くならないこと」を最優先にします。  
MusicXML の意味論は壊さず、表示補正は内部で分離して持ちます。

## 🔒 パーミッション

### 必要なパーミッション
- `READ_EXTERNAL_STORAGE` (Android 12以下)
- `READ_MEDIA_IMAGES` (Android 13以上)
- `WAKE_LOCK` (画面常時ON用)

※ Storage Access Frameworkを使用するため、ファイルアクセス権限は最小限

## 📝 今後の開発ロードマップ

### Phase 1: MVP完成 ✅
- [x] 基本的なPDF閲覧
- [x] ライブラリ管理
- [x] 検索・お気に入り

### Phase 2: 本番向け機能
- [ ] 半ページ送りモード
- [ ] 2ページ表示
- [ ] Bluetoothペダル対応
- [ ] 画面常時ON設定

### Phase 3: 編集機能
- [ ] 注釈（ペン、蛍光ペン）
- [ ] 余白トリミング
- [ ] ページ並び替え

### Phase 4: 高度な機能
- [ ] セットリスト管理
- [ ] ジャンプターゲット
- [ ] クラウド同期

## 🧪 テスト

現在、基本的なビルドが通ることを確認済み。
今後、UIテストとUnit Testを追加予定。

## 📄 ライセンス

このプロジェクトは演奏者向けの譜面ビューアアプリとして、オリジナル実装で開発されています。
既存アプリの模倣ではなく、独自の設計思想に基づいた実装です。

## 👥 開発者向けメモ

### コードスタイル
- Kotlin公式スタイルガイドに準拠
- Jetpack Composeのベストプラクティスを適用
- クリーンアーキテクチャの簡易版（過度な抽象化は避ける）

### 設計判断
- **PDF描画の抽象化**: 将来的なPDFiumへの差し替えを考慮
- **Room使用**: オフライン完結、高速なクエリ
- **Compose Navigation**: モダンなナビゲーション管理
- **Material 3**: 最新のデザインシステム

## 📞 お問い合わせ

バグ報告や機能要望は、Issuesセクションまでお願いします。

---

**ScoreStage** - 演奏者のための、演奏者による譜面アプリ 🎵
