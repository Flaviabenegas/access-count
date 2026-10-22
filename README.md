# Access Count

Aplicação web simples que conta quantas vezes uma página foi acessada. A cada requisição, um contador é incrementado no Redis e exibido na tela.

## Descrição

O projeto é composto por dois serviços:

- **web**: aplicação Flask (`app.py`) que incrementa e exibe o contador de acessos.
- **redis**: banco de dados em memória usado para armazenar o contador.

A comunicação entre os serviços é feita pela rede interna do Docker, onde o serviço Flask se conecta ao Redis pelo host `redis` (configurável pela variável de ambiente `REDIS_HOST`).

## Estrutura do projeto

```
access-count/
├── app.py              # Aplicação Flask
├── requirements.txt    # Dependências Python
├── Dockerfile          # Imagem da aplicação web
├── docker-compose.yml  # Orquestração dos serviços web e redis
└── README.md
```

## Tecnologias

- [Python 3.11](https://www.python.org/)
- [Flask 3.0.2](https://flask.palletsprojects.com/)
- [Redis 5.0.3](https://redis-py.readthedocs.io/)
- [Docker](https://www.docker.com/) e [Docker Compose](https://docs.docker.com/compose/)

## Pré-requisitos

- Docker
- Docker Compose (plugin `docker compose`)

## Como executar

### Com Docker Compose (recomendado)

Na raiz do projeto, execute:

```bash
docker compose up --build
```

A aplicação estará disponível em [http://localhost:5000](http://localhost:5000).

Para parar e remover os containers:

```bash
docker compose down
```

Para parar e remover também os dados armazenados no Redis:

```bash
docker compose down -v
```

### Sem Docker (ambiente local)

Certifique-se de ter um servidor Redis em execução e instale as dependências:

```bash
pip install -r requirements.txt
REDIS_HOST=localhost python app.py
```

## Uso

Acesse a rota raiz `GET /` no navegador. A cada acesso, o contador é incrementado.

Resposta esperada:

```html
<h1>Olá, Docker!</h1><p>Esta página foi visitada <strong>1</strong> vezes.</p>
```

## Variáveis de ambiente

| Variável      | Padrão   | Descrição                          |
| ------------- | -------- | ---------------------------------- |
| `REDIS_HOST`  | `redis`  | Host do servidor Redis             |

## Dockerfile

O `Dockerfile` cria a imagem da aplicação a partir de `python:3.11-alpine`, instala as dependências de `requirements.txt`, copia o código da aplicação e expõe a porta `5000`.

## docker-compose.yml

O `docker-compose.yml` define os serviços:

- **web**: construído a partir do `Dockerfile`, expõe a porta `5000` e depende do serviço `redis`.
- **redis**: utiliza a imagem oficial `redis` e armazena os dados em um volume nomeado.
