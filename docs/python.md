---
icon: simple/python
tags:
    - lang
---

# Python

## Arquivo `__init__`

O arquivo `__init__.py` marca um diretório como um **diretório de pacote Python**, o que significa que o Python conseguirá importar os arquivos dentro desse diretório como parte de um pacote.

```
minha_pasta/
    subpasta/
        __init__.py
        modulo.py
```

Com isso você pode importar `modulo.py` das seguintes formas:

```py
import subpasta.modulo
# ou
from subpasta import modulo
```

Esse arquivo é frequentemente usado para fornecer uma interface conveniente para o pacote, importando componentes-chave para que os usuários não precisem conhecer a estrutura exata do seu pacote.

Por exemplo, se você tem vários módulos no seu pacote, você pode importar suas funções em `__init__.py` para que os usuários possam acessá-las diretamente do pacote:

```py
# Arquivo: seu_pacote/__init__.py
from .arquivo1 import adicionar, subtrair
from .arquivo2 import funcao_importante
from submodulo.arquivo3 import *
# ...
```

Isso permite que os usuários façam algo como isto:

```py
from seu_pacote import adicionar
# em vez de
from seu_pacote.arquivo1 import adicionar
```

O arquivo `__init__.py` também pode ser usado para realizar **tarefas de inicialização de pacotes e para definir funções de conveniência**. Por exemplo, se você tem um pacote que interage com um banco de dados, você pode ter um arquivo `__init__.py` que se parece com isto:

```py
# Arquivo: database/__init__.py
import os
from sqlalchemy.orm import sessionmaker
from sqlalchemy import create_engine
engine = create_engine(os.environ['DATABASE_URL'])
Session = sessionmaker(bind=engine)
```

Com essa configuração, você pode iniciar uma nova sessão de banco de dados de qualquer lugar do seu projeto usando:

```py
from database import Session
sessao = Session()
```