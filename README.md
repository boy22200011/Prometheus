# Prometheus 監控系統

這是一個使用 Docker Compose 部署的 Prometheus 監控系統，包含 Prometheus 和 Grafana 兩個核心服務。

## 📋 專案簡介

本專案提供了一個完整的監控解決方案，使用 Prometheus 進行指標收集與儲存，並透過 Grafana 進行視覺化展示。

## ✨ 功能特色

- **Prometheus**: 開源的監控與告警系統，用於收集和儲存時間序列資料
- **Grafana**: 強大的資料視覺化與分析平台
- **Docker Compose**: 一鍵啟動所有服務，簡化部署流程
- **資料保留**: 預設保留 30 天的監控資料
- **自動重啟**: 容器異常時自動重啟，確保服務穩定性

## 🔧 系統需求

- Docker (版本 20.10 或以上)
- Docker Compose (版本 2.0 或以上)
- 至少 2GB 可用記憶體
- 至少 10GB 可用磁碟空間（視資料保留時間而定）

## 🚀 快速開始

### 1. 啟動服務

```bash
docker-compose up -d
```

### 2. 驗證服務狀態

```bash
docker-compose ps
```

### 3. 存取服務

- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3000
  - 預設帳號: `admin`
  - 預設密碼: `admin` (首次登入後會要求修改)

## 📊 服務端口

| 服務 | 端口 | 說明 |
|------|------|------|
| Prometheus | 9090 | Prometheus Web UI |
| Grafana | 3000 | Grafana Dashboard |

## ⚙️ 配置說明

### Prometheus 配置 (`prometheus.yml`)

- **scrape_interval**: 每 15 秒抓取一次指標資料
- **evaluation_interval**: 每 15 秒計算一次告警規則
- **預設監控目標**: Prometheus 自身 (localhost:9090)

### 資料保留設定

在 `docker-compose.yml` 中可調整資料保留時間：

```yaml
- "--storage.tsdb.retention.time=30d"  # 修改為所需天數，例如 7d、60d
```

### 新增監控目標

編輯 `prometheus.yml` 檔案，在 `scrape_configs` 區段新增監控目標：

```yaml
scrape_configs:
  - job_name: 'your-service'
    static_configs:
      - targets: ['your-service:port']
```

## 📁 專案結構

```
Prometheus/
├── docker-compose.yml    # Docker Compose 配置檔
├── prometheus.yml        # Prometheus 配置檔
├── README.md            # 本文件
└── LICENSE              # 授權資訊
```

## 🛠️ 常用指令

### 啟動服務
```bash
docker-compose up -d
```

### 停止服務
```bash
docker-compose down
```

### 查看日誌
```bash
# 查看所有服務日誌
docker-compose logs -f

# 查看特定服務日誌
docker-compose logs -f prometheus
docker-compose logs -f grafana
```

### 重啟服務
```bash
docker-compose restart
```

### 停止並刪除資料卷（⚠️ 會刪除所有監控資料）
```bash
docker-compose down -v
```

## 🔐 安全注意事項

1. **Grafana 密碼**: 首次登入後請立即修改預設密碼
2. **生產環境**: 建議修改預設密碼並使用環境變數管理敏感資訊
3. **網路安全**: 生產環境請考慮使用反向代理（如 Nginx）並啟用 HTTPS
4. **防火牆**: 確保僅允許授權的 IP 存取服務端口

## 📝 後續擴充建議

### 新增 Node Exporter

如需監控系統資源（CPU、記憶體、磁碟等），可新增 Node Exporter：

1. 在 `docker-compose.yml` 中新增服務：

```yaml
  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    restart: unless-stopped
    ports:
      - "9100:9100"
```

2. 在 `prometheus.yml` 中新增監控配置：

```yaml
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

### 整合其他服務

可根據需求新增其他 Exporter，例如：
- **MySQL Exporter**: 監控 MySQL 資料庫
- **Redis Exporter**: 監控 Redis 快取
- **Nginx Exporter**: 監控 Nginx Web 伺服器

## 🐛 疑難排解

### 服務無法啟動

1. 檢查 Docker 是否正常運行：
   ```bash
   docker ps
   ```

2. 檢查端口是否被占用：
   ```bash
   # Windows PowerShell
   netstat -ano | findstr :9090
   netstat -ano | findstr :3000
   ```

3. 查看詳細錯誤日誌：
   ```bash
   docker-compose logs
   ```

### 無法存取 Web UI

1. 確認服務已正常啟動
2. 檢查防火牆設定
3. 確認端口映射正確

### 資料未顯示

1. 檢查 Prometheus 配置檔語法是否正確
2. 確認監控目標可正常連線
3. 查看 Prometheus 的 Targets 頁面確認抓取狀態

## 📚 相關資源

- [Prometheus 官方文件](https://prometheus.io/docs/)
- [Grafana 官方文件](https://grafana.com/docs/)
- [Docker Compose 文件](https://docs.docker.com/compose/)

## 📄 授權

詳見 [LICENSE](LICENSE) 檔案。

## 🤝 貢獻

歡迎提交 Issue 或 Pull Request 來改善此專案。

---

**注意**: 本專案僅供學習與開發環境使用，生產環境請根據實際需求調整配置與安全設定。
