以下はPostgreSQLの最新版（現時点ではPostgreSQL 15.3）をDockerで構築する手順です。

---

## **手順**

### **1. Dockerのインストール**
- Dockerがインストールされていない場合は、[公式ページ](https://www.docker.com/)からインストールしてください。

---

### **2. 最新版PostgreSQLイメージの取得**
- PostgreSQLの最新版を取得するには以下のコマンドを実行します。

```bash
docker pull postgres:latest
```

- または、特定のバージョン（例: 15.3）を指定して取得する場合は以下を使用します。

```bash
docker pull postgres:15.3
```

---

### **3. コンテナの起動**
以下のコマンドでPostgreSQLコンテナを起動します。

```bash
docker run -d --name postgres-latest -e POSTGRES_USER=test -e POSTGRES_PASSWORD=test -e POSTGRES_DB=test -v C:\Users\yimam\Desktop\git\Udemy_SQL\db_volume:/var/lib/postgresql/data -p 5432:5432 postgres:latest
```

#### **オプション説明**
- `--name`: コンテナ名（例: `postgres-latest`）。
- `-e POSTGRES_USER`: PostgreSQLのユーザー名。
- `-e POSTGRES_PASSWORD`: PostgreSQLのパスワード。
- `-e POSTGRES_DB`: デフォルトで作成されるデータベース名。
- `-p 5432:5432`: ホストとコンテナ間でポート5432をマッピング。

---

### **4. コンテナの状態確認**
コンテナが正常に起動しているか確認します。

```bash
docker ps
```

出力例:
```plaintext
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                    NAMES
aeb38b69100e   postgres:latest "docker-entrypoint.s…"   10 seconds ago  Up 10 seconds  0.0.0.0:5432->5432/tcp   postgres-latest
```

---

### **5. 接続確認**
#### **a) ホストマシンから接続**
PostgreSQLクライアントツール（例: `psql`）を使用して接続します。

```bash
psql -h localhost -p 5432 -U your_user -d your_database
```

#### **b) Dockerコンテナ内で確認**
コンテナに入ってPostgreSQLに接続します。

```bash
docker exec -it postgres-latest bash
psql -U your_user -d your_database
```

---

### **6. データ永続化（オプション）**
データを永続化するには、ボリュームをマウントします。以下のように設定してください。

```bash
docker run -d \
  --name postgres-latest \
  -e POSTGRES_USER=your_user \
  -e POSTGRES_PASSWORD=your_password \
  -e POSTGRES_DB=your_database \
  -v /path/to/local/data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:latest
```

`/path/to/local/data` はホスト側のデータ保存先ディレクトリです。
