# ✅ **Introdução ao Padrão MVC e Criação do Projeto no NetBeans**

## 📌 O que é o Padrão MVC?

O padrão **MVC (Model-View-Controller)** é uma forma organizada de estruturar sistemas dividindo a aplicação em três partes principais:

* **Model (Modelo):** Representa os dados e suas regras básicas.
* **View (Visão):** Interface gráfica, tudo o que o usuário vê e interage.
* **Controller (Controlador):** Faz a ponte entre View e Model, coordenando ações.

O MVC é muito usado porque ajuda a manter o código organizado, facilita manutenção e permite reaproveitar partes do sistema.

---

# 📚 Nosso Projeto: *Gerenciador de Biblioteca*

Vamos criar um sistema simples onde o usuário poderá:

✔ Cadastrar livros
✔ Consultar livros
✔ Atualizar informações
✔ Excluir livros

### **Tecnologias Utilizadas**

* **Java (Java with Gradle)**
* **NetBeans IDE 19**
* **GUI Builder (Swing)**
* **MySQL + JDBC**

---

# 🏗 Criando o Projeto no NetBeans

### **1. Abra o NetBeans 19**

### **2. Crie um novo projeto**

* Clique em **File > New Project**
* Selecione **Java with Gradle > Java Application**
* Nome do projeto: `GerenciadorBiblioteca`
* Clique em **Finish**

### **3. Criar os Pacotes da Arquitetura**

No painel **Projects**:

```
src/main/java
 ├── model
 ├── view
 ├── controller
 ├── dao            (opcional, mas recomendado)
 ├── service        (opcional para lógicas mais complexas)
 └── database
```

### **4. Adicionar dependência JDBC ao `build.gradle`**

```gradle
dependencies {
    implementation 'mysql:mysql-connector-java:8.0.33'
}
```

---

# 🧩 **Arquitetura em Camadas (MVC aprimorado)**

A divisão ideal para projetos Java + Swing é:

1. **Model**
2. **DAO (Data Access Object)**
3. **Controller**
4. **View**
5. **Services** (opcional, recomendado em sistemas maiores)
6. **Database (conexão)**

Essa estrutura deixa o projeto limpo e fácil de dar manutenção.

---

# ✅ 1. **MODEL**

### ✔ O que deve ter:

* Classes que representam entidades do sistema:

  * `Livro`, `Usuario`, `Categoria`, etc.
* Atributos
* Getters e Setters
* Construtores
* `toString()` quando necessário

### ❌ O que NÃO deve ter:

* SQL
* Conexão com o banco
* Lógica de negócio complexa

> Ex: A classe `Livro` deve ter somente id, titulo, autor, ano, categoria.

---

# ✅ 2. **DAO (Data Access Object)**

### ✔ O que deve ter:

* Métodos de CRUD (Create, Read, Update, Delete)
* Uso de:

  * `Connection`
  * `PreparedStatement`
  * `ResultSet`
* Cada classe DAO gerencia *somente uma entidade*

  * `LivroDAO`
  * `UsuarioDAO`

### ❌ O que NÃO deve ter:

* Regras de negócio
* Código da interface visual
* Decisões de fluxo

> DAO = ponte DIRETA entre seu sistema e o banco.

---

# ✅ 3. **CONTROLLER**

### ✔ O que deve ter:

* Lógica de controle entre View → Service → DAO
* Processar dados vindos da tela
* Chamadas ao DAO ou Service
* Validações simples (ex: campo vazio)

### ❌ O que NÃO deve ter:

* SQL
* Código de interface gráfica (como `JFrame`, `JTextField`)

> Controller decide *o que fazer* quando o usuário clica em um botão.

---

# ✅ 4. **VIEW**

### ✔ O que deve ter:

* Telas criadas com Swing/GUI Builder
* Botões
* Campos de texto
* Coleta dos dados digitados
* Chamadas ao controller

### ❌ O que NÃO deve ter:

* SQL
* Regras de negócio
* Cálculos importantes

> View = onde o usuário interage (tela).

---

# ✅ 5. **SERVICES** (opcional, mas recomendado)

Use quando o controller começar a ficar grande ou quando houver lógica mais complexa.

### ✔ O que deve ter:

* Regras de negócio
* Cálculos
* Validações complexas
* Processamento de dados

### ❌ O que NÃO deve ter:

* SQL direto
* Acesso ao banco (isso é responsabilidade do DAO)

> Service = o “cérebro” do sistema.

---

# ✅ 6. **DATABASE (conexão)**

### ✔ O que deve ter:

* Uma classe responsável por **abrir e fechar conexões**:

  * `Conexao.java`
* Método padrão:

```java
public static Connection getConnection() {
    // retorna a conexão
}
```

### ❌ O que NÃO deve ter:

* CRUD
* Telas
* Regras

> A camada Database serve apenas para fornecer a conexão ao DAO.

---

# 📌 **Resumo Geral da Arquitetura**

| Camada         | Responsabilidade Principal                 | Exemplos                      |
| -------------- | ------------------------------------------ | ----------------------------- |
| **Model**      | Representa dados/entidades                 | `Livro`, `Usuario`            |
| **DAO**        | Acesso ao banco de dados (CRUD)            | `LivroDAO.listar()`           |
| **Controller** | Controla o fluxo entre View, Service e DAO | `LivroController.cadastrar()` |
| **View**       | Interface gráfica com o usuário            | Telas Swing                   |
| **Service**    | Regras de negócio complexas                | `validarCadastro()`           |
| **Database**   | Fornece conexão JDBC                       | `Conexao.getConnection()`     |


