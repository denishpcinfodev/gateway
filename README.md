# Spring Cloud Gateway e Spring Kafka

Este projeto é uma arquitetura baseada em microsserviços que utiliza Spring Cloud Gateway como API Gateway e Spring Kafka para comunicação assíncrona. Os microsserviços gerenciados incluem Profile, Users, Food Catalogue e Order.

## Funcionalidades
- **API Gateway**: Roteamento de solicitações para diferentes microsserviços.
- **Comunicação Assíncrona**: Integração via Kafka para envio de eventos entre serviços.
- **Gerenciamento de Perfis**: Operações CRUD de perfis de usuário.
- **Catálogo de Alimentos**: Adição e consulta de itens do catálogo.
- **Pedidos**: Gerenciamento e busca de pedidos de clientes.

## Microsserviços e Endpoints
### Profile Service (`/api/profile`)
1. **Buscar Perfil**: Recupera informações de um perfil pelo identificador.
   - **GET** `/api/profile/{item}`

2. **Atualizar Cadastro**: Atualiza os dados de um perfil.
   - **PUT** `/api/profile/atualizar-cadastro`

3. **Deletar Perfil**: Remove um perfil por ID.
   - **DELETE** `/api/profile/{id}`

---

### Users Service (`/api/users`)
1. **Buscar Todos os Usuários**: Recupera usuários com filtros como nome, CPF ou email.
   - **GET** `/api/users/todos-usuarios`
   - **Parâmetros**:
     - `page`: Número da página (opcional)
     - `size`: Tamanho da página
     - `sort`: Ordenação
     - `busca`: Critério de busca (opcional)
     - `global`: Campo de busca (nome, cpf, email)

---

### Food Catalogue Service (`/api/catalogo-food`)
1. **Adicionar Item ao Catálogo**: Adiciona um novo item ao catálogo de alimentos.
   - **POST** `/api/catalogo-food/add-food`

2. **Buscar Cardápio do Restaurante**: Recupera detalhes de um restaurante com seu menu.
   - **GET** `/api/catalogo-food/buscar-restaurante-food/{restaurantId}`

---

### Order Service (`/api/pedido`)
1. **Salvar Pedido**: Cria um novo pedido.
   - **POST** `/api/pedido/salvar-pedido`

2. **Buscar Todos os Pedidos**: Recupera todos os pedidos com filtros como data ou número do pedido.
   - **GET** `/api/pedido/todos`
   - **Parâmetros**:
     - `page`: Número da página (opcional)
     - `size`: Tamanho da página
     - `sort`: Ordenação
     - `busca`: Critério de busca (opcional)
     - `global`: Campo de busca (data, numeroPedido, nomeRestaurante, nomeCliente)

3. **Buscar Pedidos do Usuário**: Filtra pedidos específicos de um usuário por email.
   - **GET** `/api/pedido/todos-meus-pedidos`
   - **Parâmetros**:
     - `email`: Email do usuário
     - `page`, `size`, `sort`, `busca`, `global` (semelhantes aos acima)

4. **Atualizar Pedido**: Atualiza um pedido existente.
   - **PUT** `/api/pedido/atualizar-pedido`

## Configuração de Kafka
### Tópico Kafka: `api-events`
- **Producer**: Os eventos são publicados nos endpoints para notificar outros serviços.
- **Configuração**: Inclui suporte a Kafka Producer e Serialização JSON.

## Configuração do Projeto
1. **API Gateway**
   - Roteia solicitações para os serviços:
     - `/api/profile/**` → Profile Service
     - `/api/users/**` → Users Service
     - `/api/catalogo-food/**` → Food Catalogue Service
     - `/api/pedido/**` → Order Service

2. **Spring Cloud Gateway**
   - Configuração de rotas no `application.yml`.

3. **Dependências Principais**
   - `spring-boot-starter-webflux`
   - `spring-cloud-starter-gateway`
   - `spring-kafka`
   - `lombok`

## Como Executar
1. Configure um ambiente Kafka local:
   ```bash
   # Iniciar o Zookeeper
   bin/zookeeper-server-start.sh config/zookeeper.properties

   # Iniciar o Kafka Broker
   bin/kafka-server-start.sh config/server.properties

   # Criar o tópico
   bin/kafka-topics.sh --create --topic api-events --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
   ```

2. Execute cada serviço individualmente com Maven ou sua IDE.

3. Acesse o Gateway API em `http://localhost:8080`.

4. Use ferramentas como Postman para testar os endpoints dos serviços.

## Estrutura do Repositório
```
microsservicos/
├── gateway/         # Projeto Spring Cloud Gateway
├── profile-service/ # Gerenciamento de perfis
├── users-service/   # Gerenciamento de usuários
├── food-catalogue/  # Catálogo de alimentos
├── order-service/   # Gerenciamento de pedidos
```

## Melhorias Futuras
- **Monitoramento**: Integração com ferramentas como Prometheus e Grafana.

