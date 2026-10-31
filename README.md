# mydjango

Django 部落格練習專案：文章列表與文章详情页，外加 Django admin 後台。

## 專案結構

```
mydjango/
├── pyproject.toml      # uv 專案設定（Python 3.10，Django 5.2）
├── main.py             # 佔檔腳本，不是進入口
└── mblog/              # Django 專案
    ├── manage.py
    ├── mblog/          # 專案設定（settings / urls / wsgi / asgi）
    ├── mysite/         # app：Post 模型、views、admin、migrations
    ├── templates/      # base / header / footer / index / post
    └── db.sqlite3
```

## 模型

`mysite.models.Post`

| 欄位 | 型別 |
| --- | --- |
| `title` | `CharField(200)` |
| `slug` | `CharField(200)` |
| `body` | `TextField` |
| `pub_date` | `DateTimeField(auto_now_add=True)` |

依 `pub_date` 由新到舊排序。

## 路由

| 路徑 | 視圖 | 說明 |
| --- | --- | --- |
| `` | `homepage` | 列出所有文章 |
| `post/<slug:slug>/` | `showpost` | 單篇文章（找不到就轉回首頁） |
| `admin/` | Django admin | 後台 |

語言 `zh-hant`、時區 `Asia/Taipei`。靜態檔案由 WhiteNoise 提供（`STATIC_ROOT = staticfiles`）。

## 執行

```bash
uv sync                  # 或 pip install django gunicorn whitenoise

cd mblog
uv run python manage.py migrate
uv run python manage.py createsuperuser
uv run python manage.py runserver
```

開 `http://127.0.0.1:8000/`，後台在 `/admin/`。

## 部署

`gunicorn` 與 `whitenoise` 已在依賴裡：

```bash
uv run python mblog/manage.py collectstatic --noinput
uv run gunicorn mblog.wsgi:application   # 於 mblog/ 目錄執行
```

## 注意事項

- `settings.py` 目前為 `DEBUG = True`、`ALLOWED_HOSTS = ["*"]`，上線前务必改掉。
- `db.sqlite3` 與 `__pycache__/*.pyc` 已被提交进 repo，建議加进 `.gitignore` 並从版本控制移除。
