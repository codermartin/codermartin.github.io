---
title: test
date: 2020-01-14 10:28:54
tags:
comments: true
---
## 准备

### 改造测试函数

```python
#url.py
actuator_bp = Blueprint("actuator", __name__)
actuator_bp.add_url_rule(
    "/info/<dlgId>",
    view_func=views.HealthCheck.as_view(name="health_check"),
    methods=["GET"],
)

#view.py
class HealthCheck(MethodView):
    @ensure_dlg_id_serialization
    def get(self, dlgId):
        import time
        time.sleep(10)
        return Response(status=200)
```

### gunicorn配置修改

```python
import os

"""
请参考gunicorn配置
http://docs.gunicorn.org/en/stable/
"""

bind = "0.0.0.0:5006"

workers = int(os.environ.get("gunicorn.workers", 1))

# port = 5000
worker_class = os.environ.get("gunicorn.worker_class", "gthread")

#
threads = int(os.environ.get("gunicorn.threads", 20))
#
timeout = 20

# The maximum size of HTTP request line in bytes.
limit_request_line = 4096

# Limit the number of HTTP headers fields in a request.
limit_request_fields = 100

# Limit the allowed size of an HTTP request header field.
limit_request_field_size = 8190

graceful_timeout = 5

# reload = False


# chdir = '/opt/pangu'

# access_logfile = ''

# access_log_format = '%(h)s %(l)s %(u)s %(t)s "%(r)s" %(s)s %(b)s "%(f)s" "%(a)s"'

# error_logfile=''

log_level = "info"

proc_name = "nlp_server"

keepalive = int(os.environ.get("KEEPALIVE_TIMEOUT", 30))
```
