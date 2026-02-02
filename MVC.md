# ✅ Introdução ao Padrão MVC e Criação do Projeto no NetBeans

## 📌 O que é o Padrão MVC?

O padrão **MVC (Model-View-Controller)** é uma forma organizada de estruturar sistemas dividindo a aplicação em três partes principais:

* **Model (Modelo):** Representa os dados e suas regras mais básicas.
* **View (Visão):** Interface gráfica, tudo o que o usuário vê e interage.
* **Controller (Controlador):** Faz a ponte entre View e Model, coordenando as ações do sistema.

O MVC é muito utilizado porque:

✔ mantém o código organizado
✔ facilita manutenção
✔ evita retrabalho
✔ deixa claro **quem faz o quê** no sistema

> **Ideia central:** cada parte do sistema tem uma responsabilidade bem definida.

---

# 📚 Nosso Projeto: *Gerenciador de Biblioteca*

Vamos criar um sistema simples onde o usuário poderá:

✔ Cadastrar livros
✔ Consultar livros
✔ Atualizar informações
✔ Excluir livros

Esse projeto será usado para **aprender arquitetura**, não apenas Java.

---

# 🏗 Criando o Projeto no NetBeans

## 1️⃣ Abrir o NetBeans 19

## 2️⃣ Criar um novo projeto

* **File > New Project**
* **Java with Gradle > Java Application**
* Nome do projeto: `GerenciadorBiblioteca`
* Clique em **Finish**

---

## 3️⃣ Criar os Pacotes da Arquitetura

No painel **Projects**:

```
src/main/java
 ├── model
 ├── view
 ├── controller
 ├── dao
 ├── service
 └── database
```

> Mesmo que algumas camadas sejam simples no início, **a estrutura já deve existir**.

---

## 4️⃣ Adicionar dependência JDBC ao `build.gradle`

```gradle
dependencies {
    implementation 'mysql:mysql-connector-java:8.0.33'
}
```

Após salvar, o Gradle irá baixar o driver automaticamente.

---

# 🧩 Arquitetura em Camadas (MVC Aprimorado)

No Java + Swing, o MVC puro costuma ser expandido para **MVC em camadas**, ficando assim:

1. **Model**
2. **DAO (Data Access Object)**
3. **Controller**
4. **View**
5. **Service** (opcional, mas recomendado)
6. **Database (Conexão)**

Essa divisão deixa o projeto:

✔ organizado
✔ escalável
✔ fácil de corrigir erros
✔ profissional

---

## 🔁 Fluxo Geral do Sistema

```
Usuário
  ↓
VIEW (Swing)
  ↓
CONTROLLER
  ↓
SERVICE (quando existir regra)
  ↓
DAO
  ↓
DATABASE (MySQL)
```

O retorno dos dados acontece no caminho inverso.

---

# ✅ 1. MODEL (Modelo)

## 🎯 O que é a camada Model?

A camada **Model** representa os **dados do sistema**. Ela é o reflexo das tabelas do banco de dados dentro do Java.

> Se existe uma tabela `livro`, existe uma classe `Livro`.

---

## 📦 O que vai no Model?

✔ Atributos (colunas da tabela)
✔ Construtores
✔ Getters e Setters
✔ `toString()` (quando necessário)

### Exemplo conceitual – Classe `Livro`

* id
* titulo
* autor
* anoPublicacao
* categoria

Cada atributo representa **um dado**, nada mais.

---

## ❌ O que NÃO vai no Model?

❌ SQL
❌ JDBC
❌ Conexão com banco
❌ Telas (Swing)
❌ Regras de negócio

> O Model **não sabe** como os dados são salvos nem exibidos.

---

## 🧠 Analogia

O Model é como uma **ficha de cadastro em papel**:

* só guarda informações
* não decide nada
* não executa ações

---

# ✅ 2. DAO (Data Access Object)

## 🎯 O que é o DAO?

O DAO é a camada responsável por **acessar o banco de dados**.

Ele traduz:

* objetos Java → SQL
* SQL → objetos Java

---

## 📦 O que vai no DAO?

✔ Métodos de CRUD
✔ SQL (`INSERT`, `SELECT`, `UPDATE`, `DELETE`)
✔ JDBC:

* `Connection`
* `PreparedStatement`
* `ResultSet`

✔ Uma classe DAO por entidade

Exemplos:

* `LivroDAO`
* `UsuarioDAO`

---

## ❌ O que NÃO vai no DAO?

❌ Telas
❌ Botões
❌ `JOptionPane`
❌ Regras de negócio

> DAO **não decide**, apenas executa comandos no banco.

---

## ⚠ Erro comum

❌ Validar formulário no DAO
✔ Validação é Controller ou Service

---

# ✅ 3. CONTROLLER (Controlador)

## 🎯 O que é o Controller?

O Controller coordena o sistema. Ele recebe ações da View e decide o fluxo.

---

## 📦 O que vai no Controller?

✔ Receber dados da View
✔ Validações simples (campo vazio, formato)
✔ Chamar Service ou DAO
✔ Controlar o fluxo

Exemplos:

* cadastrarLivro()
* atualizarLivro()
* excluirLivro()

---

## ❌ O que NÃO vai no Controller?

❌ SQL
❌ JDBC
❌ Código Swing

---

## 🧠 Analogia

O Controller é o **gerente**:

* recebe pedidos
* confere
* manda executar

---

# ✅ 4. VIEW (Visão)

## 🎯 O que é a View?

A View é a **interface gráfica** do sistema.

No nosso projeto:

* Swing
* GUI Builder do NetBeans

---

## 📦 O que vai na View?

✔ `JFrame`, `JPanel`, `JButton`, `JTextField`
✔ Layout
✔ Captura dos dados digitados
✔ Chamada do Controller

---

## ❌ O que NÃO vai na View?

❌ SQL
❌ JDBC
❌ Regras de negócio

> A View **não pensa**, apenas mostra e coleta dados.

---

# ✅ 5. SERVICE (Serviços)

## 🎯 Para que serve?

Centralizar **regras de negócio** quando o sistema cresce.

---

## 📦 O que vai no Service?

✔ Regras do sistema
✔ Validações complexas
✔ Processamentos

Exemplos:

* verificar duplicidade
* regras específicas do domínio

---

## ❌ O que NÃO vai no Service?

❌ SQL direto
❌ Código Swing

---

# ✅ 6. DATABASE (Conexão)

## 🎯 Função da camada

Centralizar a conexão com o banco.

---

## 📦 O que vai na Database?

✔ Classe `Conexao`
✔ Método `getConnection()`

```java
public static Connection getConnection() {
    // retorna a conexão
}
```

---

## ❌ O que NÃO vai na Database?

❌ CRUD
❌ Telas
❌ Regras

---

# 📌 Resumo Geral da Arquitetura

| Camada     | Responsabilidade  |
| ---------- | ----------------- |
| Model      | Representar dados |
| DAO        | Acesso ao banco   |
| Controller | Controlar ações   |
| View       | Interface         |
| Service    | Regras do sistema |
| Database   | Conexão JDBC      |

---

# 🧪 Exemplo Prático

## 📁 Estrutura

```
GerenciadorBiblioteca
└── src/main/java
    ├── model
    │   └── Livro.java
    ├── dao
    │   └── LivroDAO.java
    ├── service
    │   └── LivroService.java
    ├── controller
    │   └── LivroController.java
    ├── view
    │   └── TelaLivro.java
    └── database
        └── Conexao.java
```

---

# ✅ MODEL – `Livro.java`

```java
package model;

public class Livro {

    private int id;
    private String titulo;
    private String autor;
    private int anoPublicacao;
    private String categoria;

    public Livro() {}

    public Livro(String titulo, String autor, int anoPublicacao, String categoria) {
        this.titulo = titulo;
        this.autor = autor;
        this.anoPublicacao = anoPublicacao;
        this.categoria = categoria;
    }

    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getTitulo() { return titulo; }
    public void setTitulo(String titulo) { this.titulo = titulo; }

    public String getAutor() { return autor; }
    public void setAutor(String autor) { this.autor = autor; }

    public int getAnoPublicacao() { return anoPublicacao; }
    public void setAnoPublicacao(int anoPublicacao) { this.anoPublicacao = anoPublicacao; }

    public String getCategoria() { return categoria; }
    public void setCategoria(String categoria) { this.categoria = categoria; }
}
```

📌 **Model apenas representa dados.**

---

# ✅ DATABASE – `Conexao.java`

```java
package database;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Conexao {

    private static final String URL = "jdbc:mysql://localhost:3306/biblioteca";
    private static final String USER = "root";
    private static final String PASSWORD = "root";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}
```

📌 **Centraliza a conexão com o banco.**

---

# ✅ DAO – `LivroDAO.java`

```java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package GerenciadorBiblioteca.dao;

import GerenciadorBiblioteca.database.Conexao;
import GerenciadorBiblioteca.model.Livro;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;

/**
 *
 * @author Professor
 */
// DAO significa Data Access Objetc (Objeto de Acesso a Dados)
// Nesta camada, vão classes que contém métodos para trabalhar com o banco
// Por exemplo: inserir, ler, atualizar, deletar informações (CRUD)
public class LivroDAO {

    public static void cadastrar(Livro livro) {
        String sql = "INSERT INTO livros (titulo, autor, ano_publi, categoria) VALUES (?,?,?,?)";

        try (Connection con = Conexao.getConnection(); 
             PreparedStatement pstmt = con.prepareStatement(sql)) {

            pstmt.setString(1, livro.getTitulo());
            pstmt.setString(2, livro.getAutor());
            pstmt.setInt(3, livro.getAnoPublicacao());
            pstmt.setString(4, livro.getCategoria());

            pstmt.executeUpdate();
            System.out.println("Livro cadastrado com sucesso!");

        } catch (SQLException error) {
            System.out.println("Erro: " + error.getMessage());
        }
    }

    public static ArrayList<Livro> listar() {
        ArrayList<Livro> livros = new ArrayList<>();
        String sql = "SELECT * FROM livros";

        try (Connection con = Conexao.getConnection(); 
             Statement stmt = con.createStatement()) {

            ResultSet rs = stmt.executeQuery(sql);

            while (rs.next()) {
                Livro livro = new Livro();
                livro.setId(rs.getInt("id"));
                livro.setTitulo(rs.getString("titulo"));
                livro.setAutor(rs.getString("autor"));
                livro.setAnoPublicacao(rs.getInt("ano_publi"));
                livro.setCategoria(rs.getString("categoria"));
                livros.add(livro);
            }

        } catch (SQLException error) {
            System.out.println("Erro: " + error.getMessage());
        }

        return livros;
    }

    public static void atualizar(Livro livro) {
        String sql = "UPDATE livros SET titulo = ?, autor = ?, "
                + "ano_publi = ?, categoria = ? WHERE id = ?";

        try (Connection con = Conexao.getConnection(); 
            PreparedStatement pstmt = con.prepareStatement(sql)) {

            pstmt.setString(1, livro.getTitulo());
            pstmt.setString(2, livro.getAutor());
            pstmt.setInt(3, livro.getAnoPublicacao());
            pstmt.setString(4, livro.getCategoria());
            pstmt.setInt(5, livro.getId());
            
            int linhasAtualizadas = pstmt.executeUpdate();
            
            if (linhasAtualizadas > 0) {
                System.out.println("Livro atualizado com sucesso!");
            } else {
                System.out.println("Livro não encontrado!");
            }

        } catch (SQLException error) {
            System.out.println("Erro: " + error.getMessage());
        }
    }
    
    public static void deletar(int id){
        String sql = "DELETE from livros WHERE id = ?";
        
        try (Connection con = Conexao.getConnection(); 
            PreparedStatement pstmt = con.prepareStatement(sql)) {

            pstmt.setInt(1, id);
            
            int linhasDeletadas = pstmt.executeUpdate();
            
            if (linhasDeletadas > 0) {
                System.out.println("Livro removido com sucesso!");
            } else {
                System.out.println("Livro não encontrado!");
            }

        } catch (SQLException error) {
            System.out.println("Erro: " + error.getMessage());
        }
    }
}

```

📌 **DAO só acessa o banco.**

---

# ✅ SERVICE – `LivroService.java`

```java
/*
    O Service é a camada que aplica as regras do sistema 
    e decide se uma ação pode ou não acontecer. É ele quem chama os métodos
    das classes da camada DAO e valida.
*/
public class LivroService {
    // Aqui ficam as REGRAS DE NEGÓCIO do sistema
    // Não tem SQL, não tem interface gráfica e não acessa banco diretamente


    // Método responsável por cadastrar um livro
    // Antes de salvar, ele valida as regras de negócio
    public void cadastrarLivro(Livro livro) {

        // Regra de negócio:
        // Um livro NÃO pode ser cadastrado sem título
        if (livro.getTitulo() == null || livro.getTitulo().isEmpty()) {

            // Lança uma exceção se a regra for violada
            // O DAO NÃO faz validação, quem valida é o Service
            // IllegalArgumentException é uma exceção padrão do Java usada quando
            // um método recebe um argumento inválido
            // O Service é o guardião das regras.
            // Ao lançar a excessão, interrompemos o método imediatamente, forçamos
            // quem chamou a lidar com o erro e evitamos que o sistema entre em estado inválido
            // Não usamos if/else e System.out.println porque com ele o código continua 
            // “silenciosamente”, e quem chamou o método não sabe que falhou
            throw new IllegalArgumentException("Título é obrigatório");
        }

        // Regra de negócio:
        // Ano de publicação precisa ser válido
        // Essa regra não tem relação com banco, por isso fica no Service
        if (livro.getAnoPublicacao() < 1500) {
            throw new IllegalArgumentException("Ano de publicação inválido");
        }
        // Se todas as regras passaram, o Service manda o DAO salvar no banco
        LivroDAO.cadastrar(livro);
    }

        // Aqui não existe regra de negócio
        // O Service apenas pede para o DAO buscar os dados
    public ArrayList<Livro> listarLivros() {
        return LivroDAO.listar();
    }
    
    
    // Atualizar também precisa validar regras
    public void atualizarLivro(Livro livro) {

        // Regra de negócio:
        // Para atualizar, o livro precisa ter ID
        if (livro.getId() <= 0) {
            throw new IllegalArgumentException("ID do livro inválido");
        }

        // Reaproveitamos regras parecidas com o cadastro
        if (livro.getTitulo() == null || livro.getTitulo().isEmpty()) {
            throw new IllegalArgumentException("Título é obrigatório");
        }

        if (livro.getAutor() == null || livro.getAutor().isEmpty()) {
            throw new IllegalArgumentException("Autor é obrigatório");
        }

        if (livro.getAnoPublicacao() < 1500) {
            throw new IllegalArgumentException("Ano de publicação inválido");
        }

        // Se tudo estiver correto,
        // o Service manda o DAO atualizar no banco
        LivroDAO.atualizar(livro);
    }

    
    // Aqui também existe regra de negócio
    public void deletarLivro(int id) {

        // Regra de negócio:
        // Não faz sentido deletar um livro sem ID válido
        if (id <= 0) {
            throw new IllegalArgumentException("ID inválido para exclusão");
        }

        // Service autoriza o DAO a remover do banco
        LivroDAO.deletar(id);
    }
}
```

📌 **Service contém regras de negócio.**

---

# ✅ CONTROLLER – `LivroController.java`

```java
package controller;

import model.Livro;
import service.LivroService;
import java.util.List;

public class LivroController {

    private LivroService service = new LivroService();

    public void cadastrarLivro(String titulo, String autor, int ano, String categoria) {
        Livro livro = new Livro(titulo, autor, ano, categoria);
        service.cadastrarLivro(livro);
    }

    public List<Livro> listarLivros() {
        return service.listarLivros();
    }
}
```

📌 **Controller controla o fluxo.**

---

# ✅ VIEW – `TelaLivro.java`

```java
package view;

import controller.LivroController;
import javax.swing.*;

public class TelaLivro extends JFrame {

    private JTextField txtTitulo = new JTextField();
    private JTextField txtAutor = new JTextField();
    private JTextField txtAno = new JTextField();
    private JTextField txtCategoria = new JTextField();
    private JButton btnSalvar = new JButton("Salvar");

    private LivroController controller = new LivroController();

    public TelaLivro() {
        setTitle("Cadastro de Livro");
        setSize(300, 300);
        setLayout(null);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        txtTitulo.setBounds(20, 20, 200, 25);
        txtAutor.setBounds(20, 60, 200, 25);
        txtAno.setBounds(20, 100, 200, 25);
        txtCategoria.setBounds(20, 140, 200, 25);
        btnSalvar.setBounds(20, 180, 200, 30);

        add(txtTitulo);
        add(txtAutor);
        add(txtAno);
        add(txtCategoria);
        add(btnSalvar);

        btnSalvar.addActionListener(e -> {
            controller.cadastrarLivro(
                txtTitulo.getText(),
                txtAutor.getText(),
                Integer.parseInt(txtAno.getText()),
                txtCategoria.getText()
            );
            JOptionPane.showMessageDialog(this, "Livro cadastrado com sucesso");
        });
    }
}
```

📌 **View apenas interage com o usuário.**

---

