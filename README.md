# Analytics Platform

Este projeto é a minha resolução do Nola God Level Coder Challenge.

## Considerações Importantes

Considerei que o banco de dados PostgreSQL do desafio já estaria rodando na máquina do avaliador a fim de facilitar casos de teste com dados controlados.

Para que a aplicação funcione é importante executar o seguinte código SQL dentro do banco de dados utilizado no desafio:

```sql
-- --- Chaves Estrangeiras (para JOINs) ---
CREATE INDEX IF NOT EXISTS idx_sales_store_id ON sales(store_id);
CREATE INDEX IF NOT EXISTS idx_sales_channel_id ON sales(channel_id);
CREATE INDEX IF NOT EXISTS idx_sales_customer_id ON sales(customer_id);
CREATE INDEX IF NOT EXISTS idx_product_sales_sale_id ON product_sales(sale_id);
CREATE INDEX IF NOT EXISTS idx_product_sales_product_id ON product_sales(product_id);

-- --- Colunas de Filtro Comuns (para WHERE) ---
CREATE INDEX IF NOT EXISTS idx_sales_sale_status_desc ON sales(sale_status_desc);
CREATE INDEX IF NOT EXISTS idx_sales_created_at ON sales(created_at); -- Para filtros de DATA (ex: "últimos 30 dias")
CREATE INDEX IF NOT EXISTS idx_sales_delivery_seconds ON sales(delivery_seconds);

-- --- Índices Funcionais ---
CREATE INDEX IF NOT EXISTS idx_sales_created_at_dow ON sales (EXTRACT(DOW FROM created_at));
CREATE INDEX IF NOT EXISTS idx_sales_created_at_hour ON sales (EXTRACT(HOUR FROM created_at));
CREATE INDEX IF NOT EXISTS idx_sales_customer_id_created_at ON sales(customer_id, created_at DESC);
```

O código SQL acima cria alguns índices importantes a fim de acelerar algumas consultas no banco.

## Como executar o projeto

1 - Clone o repositório e os seus respectivos submódulos na sua máquina:

```bash
  git clone --recursive https://github.com/lucasNSF/food-analytics-platform-meta-repository.git
```

2 - Dentro da pasta `analytics-engine` adicione um arquivo `.env` com o seguinte esqueleto:

```bash
CUBEJS_DEV_MODE=true

CUBEJS_DB_HOST=host.docker.internal
CUBEJS_DB_TYPE=postgres
CUBEJS_DB_NAME= # Nome do banco postgres
CUBEJS_DB_USER= # Nome do usuário postgres
CUBEJS_DB_PASS= # Senha do usuário postgres

CUBEJS_API_SECRET= # Chave de API gerada para o CubeJS
CUBEJS_EXTERNAL_DEFAULT=true
CUBEJS_SCHEDULED_REFRESH_DEFAULT=true
CUBEJS_SCHEMA_PATH=model
CUBEJS_WEB_SOCKETS=true
```

Altere os valores comentados para as suas especificações. Para a variável `CUBEJS_API_SECRET` é recomendado que você utilize uma string longa e difícil de adivinhar.

3 - Dentro da pasta `backend` crie uma pasta `environments` e adicione um arquivo `.env.production` com o seguinte conteúdo:

```bash
NODE_ENV=production
PORT=3000
CUBE_URL=http://engine:4000
```

4 - Na pasta raiz do projeto, rode o seguinte comando:

```bash
docker compose up -d
```

5 - Acesse o projeto em `http://localhost:8080`.
