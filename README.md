### 註冊鏈接

https://account.network3.ai/register_page?rc=c796427d

# 使用 Network3 節點的步驟

## 1. 安裝 Docker 和 Docker Compose

如果還未安裝Docker，請按照以下步驟進行安裝：

- Docker 安裝腳本：
  1. 下載安裝腳本

     ```
     wget https://get.docker.com/ -O docker.sh
     ```
  2. 執行腳本

     ```
     sudo sh docker.sh
     ```


## 2. 創建 docker-compose.yml 文件

在您的主機上創建一個目錄來存放docker-compose.yml文件及相關資料，例如 Network3-node。然後在該目錄中創建 docker-compose.yml 文件。

```bash
mkdir network3
cd network3
touch docker-compose.yml
```

## 3. 編輯 docker-compose.yml 文件

使用您喜歡的編輯器（例如 nano、vim 或 Visual Studio Code）打開 docker-compose.yml 文件，並將以下內容粘貼到文件中。請根據實際情況修改配置。

```yaml
version: '3.3'

services:
  network3-01:
    image: aron666/network3-ai
    container_name: network3-01
    environment:
      - EMAIL=your-email-to-bind
    ports:
      - 8080:8080/tcp
    volumes:
      - 路徑/wireguard:/usr/local/etc/wireguard
    healthcheck:
      test: curl -fs http://localhost:8080/ || exit 1
      interval: 30s
      timeout: 5s
      retries: 5
      start_period: 30s
    privileged: true
    devices:
      - /dev/net/tun
    cap_add:
      - NET_ADMIN
    restart: always

  autoheal:
    restart: always
    image: willfarrell/autoheal
    container_name: autoheal
    environment:
      - AUTOHEAL_CONTAINER_LABEL=all
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

## 4. 修改配置


- 路徑設定：修改 "路徑" 為您主機上的實際路徑，用來存放挖礦資料。
- 綁定設定：修改 "your-email-to-bind" 為你的network3 email

## 5. 啟動 Network3 節點

使用以下指令啟動 Network3 節點：

```bash
docker compose up -d
```

此指令將根據docker-compose.yml文件中的設定啟動所有Network3節點服務。

## 6. 查看容器狀態

您可以使用以下指令查看容器的運行狀態：

```bash
docker compose ps
```

## 7. 停止 Network3 節點

如果需要停止Network3節點，使用以下指令：

```bash
docker compose down
```

## 8. 更新程式

如果需要更新程式，請按照以下步驟：

##### 更新步驟

1. 停止現有的容器

   ```bash
   docker compose down
   ```
2. 更新程式

   ```bash
   docker compose pull
   ```
3. 重新啟動容器

   ```bash
   docker compose up -d
   ```
