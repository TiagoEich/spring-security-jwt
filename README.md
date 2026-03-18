# 🔐 API REST com Spring Security e JWT

API REST desenvolvida com Spring Boot implementando autenticação e autorização com **Spring Security** e **JWT**. O projeto foi desenvolvido acompanhando o canal [Matheus Leandro Ferreira](https://www.youtube.com/@matheuslf) como estudo prático de segurança em APIs Java.

---

## 🛠️ Tecnologias Utilizadas

- **Java 21**
- **Spring Boot 3**
- **Spring Security** — autenticação e autorização
- **Spring Data JPA** — acesso ao banco de dados
- **PostgreSQL** — banco de dados relacional
- **JJWT** — geração e validação de tokens JWT
- **BCrypt** — criptografia de senhas
- **SpringDoc / Swagger UI** — documentação da API
- **Maven** — gerenciamento de dependências

---

## ✅ Funcionalidades

- Registro de usuários com senha criptografada via **BCrypt**
- Autenticação com geração de **token JWT**
- Filtro customizado (`JwtAuthFilter`) para interceptar e validar requisições
- Rotas públicas (`/auth/**`) e rotas protegidas (`/api/**`)
- CRUD de **Produtos** com endpoints protegidos por autenticação
- Tratamento global de exceções com `GlobalExceptionHandler`

---

## 🗂️ Estrutura do Projeto

```
src/
├── main/
│   ├── java/com/example/meu_primeiro_springboot/
│   │   ├── controller/         # Endpoints da API
│   │   ├── model/              # Entidades (Produto, Usuario)
│   │   ├── repository/         # Repositórios JPA
│   │   ├── service/            # Regras de negócio
│   │   ├── security/           # Filtro JWT, configuração de segurança
│   │   └── exceptions/         # Exceções customizadas e handler global
│   └── resources/
│       ├── application.properties
│       └── application-prod.properties
```

---

## 🗃️ Estrutura do Banco de Dados

```
produtos
├── id (PK)
├── nome
└── preco

usuarios
├── id (PK)
├── username (único)
└── password (BCrypt)
```

---

## 🔐 Fluxo de Autenticação

```
1. POST /auth/register → cria usuário com senha criptografada
2. POST /auth/login    → valida credenciais e retorna token JWT
3. GET  /api/produtos  → requer token no header Authorization
```

O token JWT deve ser enviado no header de todas as requisições protegidas:
```
Authorization: Bearer {seu_token_aqui}
```

---

## ▶️ Como Executar

### Pré-requisitos

- [Java 21+](https://www.oracle.com/java/technologies/downloads/)
- [PostgreSQL](https://www.postgresql.org/download/) instalado e rodando
- [Maven](https://maven.apache.org/) (ou usar o `./mvnw` incluído no projeto)

### Passo a passo

1. **Clone o repositório**
   ```bash
   git clone https://github.com/tiagoeich/spring-security-jwt.git
   ```

2. **Crie o banco de dados no PostgreSQL**
   ```sql
   CREATE DATABASE meubanco;
   ```

3. **Configure as variáveis de ambiente**

   Antes de executar, defina as variáveis de ambiente na sua máquina:
   - `DATASOURCE_URL` → ex: `jdbc:postgresql://localhost:5432/meubanco`
   - `DATASOURCE_USERNAME` → seu usuário do PostgreSQL
   - `DATASOURCE_PASSWORD` → sua senha do PostgreSQL

4. **Execute o projeto**
   ```bash
   ./mvnw spring-boot:run
   ```

5. **Acesse a documentação da API**

   Abra no navegador: `http://localhost:8080/swagger-ui.html`

---

## 📌 Endpoints

| Método | Rota | Descrição | Auth |
|--------|------|-----------|------|
| `POST` | `/auth/register` | Registrar novo usuário | ❌ |
| `POST` | `/auth/login` | Autenticar e obter token JWT | ❌ |
| `GET` | `/api/produtos` | Listar todos os produtos | ✅ |
| `GET` | `/api/produtos/{id}` | Buscar produto por ID | ✅ |
| `POST` | `/api/produtos` | Criar novo produto | ✅ |
| `DELETE` | `/api/produtos/{id}` | Deletar produto | ✅ |

> ✅ Requer token JWT no header: `Authorization: Bearer {token}`

---

## 📋 Exemplos de Requisição

**Registrar usuário**
```json
POST /auth/register
{
  "username": "tiago",
  "password": "senha123"
}
```

**Login**
```json
POST /auth/login
{
  "username": "tiago",
  "password": "senha123"
}
```

**Criar produto** *(requer token)*
```json
POST /api/produtos
{
  "nome": "Headphone",
  "preco": 299.99
}
```
