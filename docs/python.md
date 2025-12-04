---
icon: simple/python
tags:
    - lang
---

# Python

??? info "Recursos"
    - [Roadmap Python](https://roadmap.sh/python)
    - [Learn Python in Y Minutes](https://learnxinyminutes.com/pt-br/python/)

## Gerenciador de Pacotes `pip`

O gerenciador de pacotes padrão para Python é o `pip`. Ele permite instalar e gerenciar pacotes que não fazem parte da biblioteca padrão do Python.

```bash
pip --version # (6)!

# instalação, atualização e deleção de pacotes
pip install nome_do_pacote
pip install nome_do_pacote==1.2.3
pip install --upgrade nome_do_pacote
pip uninstall nome_do_pacote

pip freeze > requirements.txt # (1)!
pip install -r requirements.txt # (2)!

pip list # (3)!
pip show nome_do_pacote # (4)!
pip freeze # (5)!
```

1. Gera arquivo `requirements.txt` com as dependências do projeto. Lista todos os pacotes instalados naquele ambiente, incluindo pacotes que você instalou manualmente com `pip` e pacotes que vieram como dependências de outros pacotes
2. Instala as dependências listadas (no requirements) do projeto
3. Lista pacotes instalados
4. Ver detalhes do pacote instalado
5. Mostrar todas as dependências com versões
6. Ver onde o pip está instalado

### Ambiente Virtual

`virtualenv` é uma ferramenta para criar ambientes Python isolados. Ela cria uma pasta que contém todos os executáveis ​​necessários para usar os pacotes que um projeto Python precisa. Quaisquer pacotes que você usar dentro desse ambiente serão independentes do interpretador do seu sistema. Isso significa que você pode manter as dependências do seu projeto separadas de outros projetos e do sistema em geral.

Após criar e ativar o ambiente virtual, você pode instalar pacotes nele.

```bash
python -m venv venv/ # (1)!
source venv/bin/activate # (2)!
pip install flask
```

1. Cria o ambiente `venv`, vai criar uma pasta com mesmo nome na raiz do projeto.
2. Ativa o ambiente, em windows fica `venv\Scripts\activate`


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