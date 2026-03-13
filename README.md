
# Template Guia — Leitura de API e Construção de API com FastAPI

### Aprenda comigo:

## Manual da engenharia de dados:  https://manual-engenharia-dados.lovable.app/

Este material é um guia prático para entender dois lados muito comuns em projetos de dados e back-end:

- Consumir APIs externas
- Criar APIs próprias

Tecnologias utilizadas:

- Python
- Requests
- FastAPI

A ideia não é apenas copiar código, mas entender **a estrutura mental necessária para trabalhar com APIs**.

---

# Visão Geral

Fluxo comum em projetos:

API externa  
↓  
Script Python consome dados  
↓  
Processamento / transformação  
↓  
API própria expõe os dados

Ou seja:

**consumo + processamento + exposição**

---

# 1 — Leitura de API em Python

Para consumir APIs em Python normalmente usamos a biblioteca **requests**.

Instalação:

```
pip install requests
```

---

# Template Base — Leitura de API

```python
import requests

url = "https://api.exemplo.com/dados"

response = requests.get(url)

if response.status_code == 200:
    dados = response.json()
    print(dados)
else:
    print(f"Erro: {response.status_code}")
```

Elementos importantes:

- URL da API
- Método HTTP (GET, POST, etc)
- Código de status
- Conversão JSON

---

# Exemplo 1 — Leitura simples

```python
import requests

url = "https://jsonplaceholder.typicode.com/users"

response = requests.get(url)

dados = response.json()

print(dados)
```

---

# Exemplo 2 — Leitura com parâmetros

```python
import requests

url = "https://jsonplaceholder.typicode.com/comments"

params = {
    "postId": 1
}

response = requests.get(url, params=params)

dados = response.json()

print(dados)
```

---

# Exemplo 3 — Converter resposta em DataFrame

```python
import requests
import pandas as pd

url = "https://jsonplaceholder.typicode.com/posts"

response = requests.get(url)

dados = response.json()

df = pd.DataFrame(dados)

print(df.head())
```

---

# Template — API com autenticação por token

```python
import requests

url = "https://api.exemplo.com/dados"

headers = {
    "Authorization": "Bearer SEU_TOKEN"
}

response = requests.get(url, headers=headers)

dados = response.json()

print(dados)
```

---

# Template — Requisição POST

```python
import requests

url = "https://api.exemplo.com/processar"

payload = {
    "nome": "Walter",
    "idade": 30
}

response = requests.post(url, json=payload)

print(response.json())
```

---

# 2 — Construção de API com FastAPI

FastAPI é um framework moderno para criar APIs em Python.

Instalação:

```
pip install fastapi
pip install uvicorn
```

---

# Template Base — FastAPI

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"mensagem": "API funcionando"}
```

---

# Como executar a API

```
uvicorn app:app --reload
```

Explicação:

- primeiro **app** → nome do arquivo
- segundo **app** → variável da aplicação

---

# Exemplo 1 — Endpoint simples

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/status")
def status():
    return {"status": "ok"}
```

---

# Exemplo 2 — Parâmetro na URL

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/usuario/{usuario_id}")
def buscar_usuario(usuario_id: int):

    return {
        "usuario_id": usuario_id
    }
```

Exemplo de chamada:

```
/usuario/10
```

---

# Exemplo 3 — Recebendo dados com POST

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Cliente(BaseModel):

    nome: str
    idade: int
    email: str

@app.post("/clientes")
def criar_cliente(cliente: Cliente):

    return {
        "mensagem": "Cliente recebido",
        "dados": cliente
    }
```

---

# Template — Autenticação por header

```python
from fastapi import FastAPI, Header, HTTPException

app = FastAPI()

TOKEN_VALIDO = "segredo123"

@app.get("/dados")
def acessar_dados(x_token: str = Header()):

    if x_token != TOKEN_VALIDO:
        raise HTTPException(status_code=401, detail="Token inválido")

    return {"dados": "acesso permitido"}
```

---

# Template — API com lista em memória

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

clientes = []

class Cliente(BaseModel):
    nome: str
    idade: int

@app.post("/clientes")
def adicionar_cliente(cliente: Cliente):

    clientes.append(cliente.dict())

    return {"mensagem": "cliente adicionado"}

@app.get("/clientes")
def listar_clientes():

    return clientes
```

---

# Estrutura mental para trabalhar com APIs

## Consumindo API

Você precisa definir:

- URL
- método HTTP
- parâmetros
- headers
- autenticação
- tratamento de resposta
- tratamento de erro

---

## Criando API

Você precisa definir:

- aplicação
- rotas
- métodos HTTP
- parâmetros de entrada
- validação
- resposta JSON

---

# Conclusão

Entender **o template mental de consumo e criação de APIs** facilita muito o desenvolvimento de integrações.

Você passa a enxergar os dois lados:

- quem consome
- quem fornece os dados

Esse fluxo aparece muito em:

- pipelines de dados
- integrações entre sistemas
- produtos digitais
