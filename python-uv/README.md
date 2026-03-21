# Python (uv)

## install

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## usage

```bash
# Python 3.14をインストール
uv python install 3.14
uv python list


uv init my_project # pyproject.tomlなど自動生成
uv python pin 3.14 # .python-versionファイルが作成され、3.14に固定される
uv add requests
uv sync # lockfileで完全再現

uv run python something.py  # uvのPython 3.14で実行される

uv venv --python 3.14 .venv
source .venv/bin/activate
python something.py  # activatedしたvenvのPython 3.14で実行される
```

## cron

```bash
0 9 * * * cd /path/to/project && uv run python something.py
```
