# nas

```bash
# Samba のインストール
sudo apt update
sudo apt install samba

# 共有用フォルダを作成（場所は任意）
sudo mkdir -p /home/$USER/share
# 書き込み可能にする
sudo chmod 0777 /home/$USER/share

# パスワードを2回入力
sudo smbpasswd -a $USER

sudo usermod -aG sambashare $USER

sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
sudo nano /etc/samba/smb.conf
```

**グローバルセクション**（`[global]` の下あたり）に以下を追記：

```ini
   unix charset = UTF-8
   dos charset = CP932
   hosts allow = 192.168.1.0/24   # ← 自分のLANのアドレス範囲に変更
```

**ファイルの末尾**に共有セクションを追加：

```ini
[share]
   comment = My NAS Share
   path = /home/ユーザー名/share
   browseable = yes
   writable = yes
   guest ok = no
   valid users = ユーザー名
   force create mode = 0777
   force directory mode = 0777
```

> `guest ok = yes` にするとパスワードなしでアクセス可能（セキュリティ的に注意）


```bash
# 設定ファイルの構文チェック
testparm

# Samba を再起動
sudo systemctl restart smbd
sudo systemctl restart nmbd

# 自動起動を有効化
sudo systemctl enable smbd
sudo systemctl enable nmbd

sudo ufw allow samba
sudo ufw reload
```

---

## 各デバイスからの接続方法

| デバイス                        | アクセス方法                                                                  |
| ------------------------------- | ----------------------------------------------------------------------------- |
| **Windows**                     | エクスプローラーのアドレスバーに `\\サーバーのIPアドレス\share` と入力        |
| **Windows（ドライブ割り当て）** | エクスプローラー → PC → ネットワークドライブの割り当て → `\\IPアドレス\share` |
| **Linux（Nemo）**               | ファイル → サーバーに接続 → `smb://IPアドレス/share`                          |
| **Mac**                         | Finder → 移動 → サーバーへ接続 → `smb://IPアドレス/share`                     |
