## Config

Configuração para usar filesystem backend

    "backend": {
      "file": {
        "path": "vault/data"
      }
    },

TSL desabilitado
     "tls_disable": 1

UI habilitada

     "ui": true

## Uso

    docker-compose up -d --build

### Vault init & unseal

Você pode interagir com o container usando `docker-compose exec <nome-do-container>` ou entando dentro do container e executando os comandos diretamente lá `docker exec -it <nome-do-container> bash`

### Init

    docker-compose exec vault vault operator init

### Unseal


    docker-compose exec vault vault operator unseal <chave 1>
    docker-compose exec vault vault operator unseal <chave 2>
    docker-compose exec vault vault operator unseal <chave 3>

Após abrir o selo, é preciso autenticar usando algum dos metodos de autenticação configurados no vault

### Login

Se ainda não há nenhum metodo cadastrado, será necessario autenticar com o root token (obtido ao iniciar)

    docker-compose exec vault vault login <root token>

Saída:

    Key                  Value
    ---                  -----
    token                b81b2881-795d-aae0-b673-0c4ec9faae5b
    token_accessor       47c5a70b-6dc9-1e19-e6a4-795f95f63148
    token_duration       ∞
    token_renewable      false
    token_policies       ["root"]
    identity_policies    []
    policies             ["root"]


### Log

Habilitando log

    vault audit enable file file_path=/vault/logs/audit.log

## Armazenando chaves

#### Chaves estaticas \ Static secrets

##### Usando o CLI

    docker-compose exec vault vault kv put secret/foo bar=precious

    docker-compose exec vault vault kv get secret/foo

Usando a API
 export VAULT_TOKEN=213d7dad-c2fd-de83-61bf-bcc9ab43d480

 curl \
>     -H "X-Vault-Token: $VAULT_TOKEN" \
>     -H "Content-Type: application/json" \
>     -X POST \
>     -d '{ "data": { "foo": "world" } }' \
>     http://127.0.0.1:8200/v1/secret/data/hello


curl \
>     -H "X-Vault-Token: $VAULT_TOKEN" \
>     -X GET \
>     http://127.0.0.1:8200/v1/secret/data/hello

https://www.bogotobogo.com/DevOps/Docker/Docker-Vault-Consul.php