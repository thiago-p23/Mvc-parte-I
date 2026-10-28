# TechByte Solutions - System de Controle de Estoque

Sistema de gestão de inventário e controlo de estoque via consola/terminal, desenvolvido em Java aplicando a arquitetura em camadas (**MVC** - *Model-View-Controller*) e o padrão **Repository**.

---

## 🚀 Funcionalidades

- **Cadastrar Produto:** Registo de novos produtos com validação de dados.
- **Listar Produtos:** Exibição de todos os produtos cadastrados formatados em tabela.
- **Buscar por ID:** Consulta detalhada das informações de um produto específico.
- **Atualizar Produto:** Edição do nome, descrição e preço de um produto existente.
- **Remover Produto:** Exclusão de um produto do sistema através do seu ID.
- **Regras de Negócio Automáticas:** 
  - Adição automática da tag `[VALOR ALTO]` na descrição para produtos com preço igual ou superior a R$ 5.000,00.
  - Validação estrita para impedir nomes com menos de 3 caracteres ou preços negativos.

---

## 🏗️ Arquitetura do Projeto

O projeto segue a estrutura de pacotes `br.com.senai.techbyte` dividida pelas seguintes responsabilidades:

```text
src/
 └── br/
      └── com/
           └── senai/
                └── techbyte/
                     ├── model/       # Entidades de domínio (Produto)
                     ├── repository/  # Camada de acesso a dados (Interface e Memória)
                     ├── service/     # Regras de negócio e validações
                     ├── controller/  # Intermediário entre a View e o Service
                     └── view/        # Interface com o utilizador via Consola
```

## Descrição do Projeto

O **TechByte Solutions** é um sistema desenvolvido em **Java** para realizar o gerenciamento de produtos de um estoque através do terminal.

O projeto permite cadastrar, consultar, listar, atualizar e remover produtos, aplicando conceitos de **Programação Orientada a Objetos (POO)** e separação de responsabilidades através de diferentes camadas.

Os produtos são armazenados temporariamente em memória utilizando uma lista, sem a utilização de um banco de dados.

---

## Objetivo

O objetivo do projeto é desenvolver um sistema organizado para gerenciamento de produtos, aplicando conceitos importantes do desenvolvimento Java, como:

- Programação Orientada a Objetos;
- Encapsulamento;
- Interfaces;
- Implementação de interfaces;
- Separação de responsabilidades;
- Organização do projeto em pacotes;
- Validação de dados;
- Tratamento de exceções;
- Collections;
- `ArrayList`;
- `Optional`;
- `BigDecimal`;
- CRUD;
- Entrada de dados utilizando `Scanner`.

---

## Funcionalidades

O sistema possui as seguintes funcionalidades:

### 1. Cadastrar Produto

Permite cadastrar um novo produto informando:

- Nome;
- Descrição;
- Preço.

O ID do produto é gerado automaticamente pelo sistema.

### 2. Listar Produtos

Exibe todos os produtos cadastrados no estoque, mostrando:

- ID;
- Nome;
- Descrição;
- Preço.

### 3. Buscar Produto por ID

Permite informar o ID de um produto para consultar seus dados.

Caso o produto não exista, o sistema informa que nenhum produto foi encontrado.

### 4. Atualizar Produto

Permite alterar os dados de um produto já cadastrado.

O usuário informa o ID do produto e depois fornece:

- Novo nome;
- Nova descrição;
- Novo preço.

### 5. Remover Produto

Permite remover um produto utilizando seu ID.

Caso o ID não seja encontrado, o sistema informa que o produto não foi localizado.

---

## Menu do Sistema

Ao executar o programa, o usuário possui acesso ao seguinte menu:

```text
==========================================
  TECHBYTE SOLUTIONS - CONTROLE DE ESTOQUE
==========================================
1. Cadastrar Produto
2. Listar Todos os Produtos
3. Buscar Produto por ID
4. Atualizar Produto
5. Remover Produto
0. Sair
==========================================
```

---

## Estrutura do Projeto

O projeto está organizado em diferentes pacotes para separar as responsabilidades de cada classe.

```text
br.com.senai.techbyte
│
├── model
│   └── Produto.java
│
├── repository
│   ├── ProdutoRepository.java
│   └── ProdutoMemoryRepository.java
│
├── service
│   └── ProdutoService.java
│
├── controller
│   └── ProdutoController.java
│
└── view
    └── ProdutoConsoleView.java
```

---

## Explicação das Classes

### Produto.java

Representa o produto dentro do sistema.

Possui os atributos:

```java
private Long id;
private String nome;
private String descricao;
private BigDecimal preco;
```

Também possui:

- Construtores;
- Getters e setters;
- `equals()`;
- `hashCode()`;
- `toString()`.

O `BigDecimal` é utilizado para representar o preço dos produtos.

---

### ProdutoRepository.java

É uma interface responsável por definir as operações que um repositório de produtos deve possuir.

Ela contém os seguintes métodos:

```java
Produto salvar(Produto produto);

List<Produto> listarTodos();

Optional<Produto> buscarPorId(Long id);

boolean atualizar(Produto produto);

boolean deletar(Long id);
```

A interface funciona como um contrato para as classes responsáveis pelo armazenamento dos produtos.

---

### ProdutoMemoryRepository.java

Implementa a interface `ProdutoRepository`.

Essa classe simula um banco de dados utilizando:

```java
ArrayList<Produto>
```

Os produtos ficam armazenados em memória enquanto o programa estiver sendo executado.

Ela também possui:

```java
private long proximoId = 1;
```

Esse atributo é utilizado para gerar automaticamente um novo ID para cada produto cadastrado.

A classe implementa as operações de:

- Salvar;
- Listar;
- Buscar;
- Atualizar;
- Deletar.

Como o armazenamento é feito em memória, os dados são perdidos quando o programa é encerrado.

---

### ProdutoService.java

É responsável pelas **regras de negócio** e validações do sistema.

Antes de cadastrar ou atualizar um produto, os dados são verificados.

Entre as validações estão:

- Nome obrigatório;
- Nome com no mínimo 3 caracteres;
- Descrição obrigatória;
- Preço não pode ser menor que zero;
- ID deve ser válido e positivo.

A classe também possui uma regra especial para produtos de alto valor.

Quando determinado produto possui preço igual ou superior a:

```text
R$ 5.000,00
```

uma identificação é adicionada automaticamente à descrição do produto.

---

## Regra de Negócio para Produtos de Alto Valor

Durante o cadastro, produtos com preço maior ou igual a R$ 5.000 recebem uma identificação na descrição.

Exemplo:

```text
Produto:
Notebook Gamer

Descrição informada:
Notebook de alto desempenho

Preço:
R$ 7.000,00
```

O sistema poderá armazenar:

```text
[VALOR ALTO] Notebook de alto desempenho
```

Essa regra é executada pela camada `ProdutoService`.

---

### ProdutoController.java

O `ProdutoController` funciona como intermediário entre a interface do usuário e a camada de serviço.

Ele possui métodos para:

```java
cadastrarProduto()
listarProdutos()
buscarPorId()
atualizarProduto()
removerProduto()
```

Dessa forma, a interface do sistema não precisa acessar diretamente o repositório.

---

### ProdutoConsoleView.java

É responsável pela interação com o usuário através do terminal.

A classe utiliza:

```java
Scanner
```

para receber as informações digitadas.

Ela apresenta o menu principal e possui métodos responsáveis pelas operações:

```java
cadastrar()
listar()
buscarPorId()
atualizar()
remover()
```

Também realiza o tratamento de erros durante a entrada dos dados.

---

## Fluxo do Sistema

A comunicação entre as camadas ocorre
