

```
MyProject/
├── scripts/
│   └── run_engine.py
├── sub_package/
│   ├── sub1/
│   │   ├── __init__.py
│   │   └── common.py
│   └── sub2/
│       ├── __init__.py
│       └── common.py
└── my_package/
    ├── __init__.py
    └── utils/
        ├── __init__.py
        └── common.py
```

たとえば、`run_engine.py` で、

```python
sys.path.insert(0, str(PROJECT_ROOT / "sub_package"))
sys.path.insert(0, str(PROJECT_ROOT))
```

このような`insert` 順番だとして、

```py
from my_package.utils.common import common_function

import sub1.common
```

このようにimport する。しかし、`my_package.utils.common.py` 内で、`sub1.common` をimport してる関係値である場合には、`insert` 順番はこのままでいいの？