# Docker / Docker Compose

## install

```bash
# 古いバージョンのアンインストール（初回は不要）
sudo apt remove docker docker-engine docker.io containerd runc

# 必要なパッケージのインストール
sudo apt update
sudo apt install -y ca-certificates curl

# Docker の公式 GPG キーを追加
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# リポジトリを追加
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

# Docker Engine と Docker Compose をインストール
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

> 公式ドキュメント: <https://docs.docker.com/engine/install/ubuntu/>

```bash
# sudo なしで docker コマンドを使えるようにする
sudo usermod -aG docker $USER

# 変更を反映（再ログインでも可）
newgrp docker
```

```bash
# インストール確認
docker --version
docker compose version
```

## usage

```bash
# イメージを取得して起動
docker run hello-world

# コンテナ一覧（起動中）
docker ps

# コンテナ一覧（全て）
docker ps -a

# イメージ一覧
docker images

# コンテナを停止・削除
docker stop <container_id>
docker rm <container_id>

# イメージを削除
docker rmi <image_id>

# 未使用リソースをまとめて削除
docker system prune
```

## Docker Compose

```bash
# compose.yml に従ってコンテナを起動（バックグラウンド）
docker compose up -d

# ログを確認
docker compose logs -f

# コンテナを停止
docker compose down

# イメージも含めて削除
docker compose down --rmi all --volumes
```

`compose.yml` の例：

```yaml
services:
  app:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
```

## autostart

```bash
# Docker サービスを自動起動に設定
sudo systemctl enable docker
sudo systemctl start docker
```
