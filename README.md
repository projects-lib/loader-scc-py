# scc-loader

Spring Cloud Config Server loader for Python applications. Fetches properties
from a Config Server and returns them as a single flat `dict`.

## Installation

From Git (recommended for teams; pin to a version tag):

```
scc-loader @ git+https://git-internal/xl-axiata/loader-scc-py.git@v0.1.0
```

Local (single-machine development):

```bash
pip install -e D:\XL-Axiata\RND\RAG\loader-scc-py
```

## Usage

The library reads configuration from environment variables. The consuming
application loads `.env` (e.g. with `python-dotenv`), then:

```python
from dotenv import load_dotenv
from scc_loader import ConfigServerClient

load_dotenv()                       # load .env into os.environ
remote = ConfigServerClient().fetch()
db_host = remote.get("spring.datasource.host")
```

Or inject values directly without environment variables:

```python
remote = ConfigServerClient(
    uri="http://config-server",
    application="vektor-creator-service",
    profile="sit",
    label="tencent",
).fetch()
```

## Environment variables

| Variable | Role | Default |
|----------|------|---------|
| `SPRING_CLOUD_CONFIG_URI` | Config Server URI | `http://localhost:8888` |
| `APPLICATION_NAME` | application name (URL segment 2) | `application` |
| `SPRING_CLOUD_CONFIG_PROFILE` | profile (URL segment 3) | `default` |
| `LABEL` | label (URL segment 4, optional) | _(empty)_ |
| `SPRING_CLOUD_CONFIG_FAIL_FAST` | `true` raises on failure | `true` |
| `SPRING_CLOUD_CONFIG_REQUEST_TIMEOUT` | timeout in seconds | `10` |

Requested URL: `GET {uri}/{application}/{profile}[/{label}]`

## Testing

```bash
pip install -e .[test]
pytest
```

## License

MIT
