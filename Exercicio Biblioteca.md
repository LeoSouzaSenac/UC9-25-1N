# Atividade: Cadastro de Livros com Java, SQLite e Interface Gráfica (Swing)

## Objetivo

Desenvolver uma aplicação em **Java com interface gráfica (Swing)** para **cadastro de livros**, utilizando **SQLite** como banco de dados.

O foco da atividade é aprender:

* Criar interfaces gráficas com **Java Swing**
* Conectar Java com banco de dados **SQLite**
* Executar operações básicas de banco de dados (CRUD)
* Organizar o código de forma simples e funcional

---

## Estrutura sugerida do projeto

Projeto simples, **sem MVC**:

```
CadastroLivros/
├── Main.java
├── Livro.java
├── LivroOperacoes.java
├── LivroForm.java
```

---

## Passo 1: Criar o banco de dados SQLite

O SQLite utiliza um arquivo local como banco de dados.

Nome do banco:

```
biblioteca.db
```

Tabela `livros`:

Campos:

* `id`
* `titulo`
* `autor`

SQL sugerido:

```sql
CREATE TABLE IF NOT EXISTS livros (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    titulo TEXT NOT NULL,
    autor TEXT NOT NULL
);
```

---

## Passo 2: Criar a classe Livro

Crie a classe `Livro.java`.

Essa classe representa um livro no sistema.

Ela deve conter:

* Atributos: `id`, `titulo`, `autor`
* Construtor
* Getters e setters

Pergunta:

> O `id` deve ser informado ao cadastrar um livro?

Resposta:

> Não. O `id` é gerado automaticamente pelo SQLite.

---

## Passo 3: Criar a classe LivroOperacoes

Crie a classe `LivroOperacoes.java`.

Essa classe será responsável por **acessar o banco de dados SQLite**.

Ela deve conter:

* Método para abrir conexão com o banco
* Método para criar a tabela
* Método para cadastrar um livro
* Método para listar todos os livros
* Método para listar um livro em específico
* Método para atualizar um livro
* Método para remover um livro

Exemplo de SQL para inserir um livro:

```sql
INSERT INTO livros (titulo, autor) VALUES (?, ?);
```

---

## Passo 4: Criar a Interface Gráfica (Swing)

Crie a classe `LivroForm.java`.

Essa classe será responsável pela **interface gráfica** da aplicação.

A interface deve conter:

### Campos

* Campo de texto para **Título**
* Campo de texto para **Autor**
* Campo de texto para **ID** (usado para atualizar ou remover)

### Botões

* **Cadastrar**
* **Listar**
* **Atualizar**
* **Remover**

### Área de exibição

* Uma **tabela (`JTable`)** ou **área de texto (`JTextArea`)** para mostrar os livros cadastrados

Regras importantes:

* A interface **não deve conter comandos SQL**
* Todas as operações devem chamar métodos da `LivroOperacoes`
* Mostrar mensagens de sucesso ou erro com `JOptionPane`

---

## Passo 5: Integração da interface com o banco

A interface deve:

* Ler os dados digitados pelo usuário
* Criar objetos `Livro`
* Chamar os métodos da `LivroOperacoes`
* Atualizar a lista de livros na tela após cada operação

Exemplo de comportamento esperado:

* Ao clicar em **Cadastrar**, o livro é salvo no banco
* Ao clicar em **Listar**, todos os livros aparecem na tela
* Ao clicar em **Atualizar**, o livro com o ID informado é alterado
* Ao clicar em **Remover**, o livro com o ID informado é apagado

---

## Passo 6: Classe Main

No `Main.java`:

* Iniciar a aplicação
* Abrir a janela `LivroForm`
* Garantir que a tabela seja criada ao iniciar o programa

---

## Testes obrigatórios

Antes de entregar, teste:

* Cadastro de pelo menos 3 livros
* Listagem correta na interface gráfica
* Atualização de um livro existente
* Remoção de um livro pelo ID
* Persistência dos dados após fechar e abrir o programa

---

## Regras da atividade

* Interface obrigatoriamente em **Java Swing**
* Banco de dados obrigatoriamente em **SQLite**
* Código organizado e legível
* Cada botão deve executar apenas uma função
