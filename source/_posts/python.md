---
title: python
date: 2017-07-18 08:20:53
tags: [note, python]
---
### python alt env
* install
```
pip3 install virtualenv
```

* setup
```
#venv is virtual env name
virtualenv --no-site-packages venv
```

* enter
```
source venv/bin/activate
```

* exit
```
deactivate
```

## others
### file server
```
python3 -m http.server 8000
```



## codes
### CORS in flask
```
from functools import wraps
from flask import make_response


def allow_cross_domain(fun):
    @wraps(fun)
    def wrapper_fun(*args, **kwargs):
        rst = make_response(fun(*args, **kwargs))
        rst.headers['Access-Control-Allow-Origin'] = '*'
        rst.headers['Access-Control-Allow-Methods'] = 'PUT,GET,POST,DELETE'
        allow_headers = "Referer,Accept,Origin,User-Agent"
        rst.headers['Access-Control-Allow-Headers'] = allow_headers
        return rst
    return wrapper_fun



@app.route('/hosts/')
@allow_cross_domain
def domains():
    pass
```

