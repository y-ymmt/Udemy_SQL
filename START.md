# PostgreSQL Docker環境構築手順

## 前提条件
- Dockerがインストールされていること
- Docker Desktopが起動していること（Windows環境）

## 手順

1. プロジェクトのルートディレクトリに`docker-compose.yml`ファイルを作成します。

```yaml
version: '3.8'

services:
  db:
    image: postgres:latest
    container_name: postgres_db
    environment:
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
      POSTGRES_DB: test
      PGDATA: /var/lib/postgresql/data/pgdata
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    restart: unless-stopped

volumes:
  postgres_data:
    driver: local
```

2. Docker Composeでコンテナを起動します：
```bash
docker-compose up -d
```

3. コンテナの状態を確認します：
```bash
docker-compose ps
```

4. PostgreSQLに接続するには以下の情報を使用します：
- ホスト: localhost
- ポート: 5432
- ユーザー名: test
- パスワード: test
- データベース名: test

## データの永続化について
- データは`postgres_data`という名前のDockerボリュームに保存されます
- コンテナを削除しても、データは保持されます
- ボリュームの確認は以下のコマンドで行えます：
```bash
docker volume ls
```

## 注意事項
- パスワードは本番環境では必ず変更してください
- 必要に応じて環境変数ファイル（.env）を使用してください

## コンテナの停止方法
```bash
docker-compose down
```

## PostgreSQLコンテナでSQLを実行する方法

### 方法1: psqlコマンドを使用してコンテナ内に入る
```bash
docker exec -it postgres_db psql -U test -d test
```

### 方法2: bashでコンテナに入り、その後psqlを実行
```bash
# コンテナのbashに入る
docker exec -it postgres_db bash

# コンテナ内でpsqlを実行
psql -U test -d test
```

### よく使うpsqlコマンド
- `\l` - データベース一覧の表示
- `\c データベース名` - データベースの切り替え
- `\dt` - テーブル一覧の表示
- `\d テーブル名` - テーブルの構造を表示
- `\q` - psqlを終了

### SQLの実行例
```sql
-- テーブルの作成
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);

-- データの挿入
INSERT INTO users (name, email) VALUES ('テストユーザー', 'test@example.com');

-- データの確認
SELECT * FROM users;
```

### 注意事項
- コンテナ名（postgres_db）は`docker-compose.yml`で設定した名前と一致している必要があります
- ユーザー名（postgres）とデータベース名（mydatabase）は環境設定に応じて変更してください

このセットアップでは以下の特徴があります：

1. PostgreSQLの最新バージョンを使用
2. データの永続化にDockerボリュームを使用
3. デフォルトのポート5432を使用
4. コンテナの自動再起動設定
5. Windows環境での実行を考慮した設定

必要に応じて、以下のようなカスタマイズも可能です：
- PostgreSQLのバージョンの指定
- ポート番号の変更
- 環境変数の外部ファイル化
- バックアップスクリプトの追加

これらの設定について、さらに詳しい説明や特定の設定の変更が必要な場合は、お申し付けください。