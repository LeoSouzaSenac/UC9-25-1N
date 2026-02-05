# Aplicação Java com Banco de Dados SQLite

## Introdução

Neste tutorial, vamos criar uma aplicação Java simples que conecta a um banco de dados SQLite e realiza algumas operações, como criar uma tabela de usuários, inserir novos usuários e listar os usuários existentes. Vamos entender como cada parte do código funciona, explicando tudo de forma clara e detalhada.

## Atenção:

Abra o arquivo build.gradle, procure a seção 'dependencies' e coloque esta linha:
```bash
// JDBC do SQLite
    implementation 'org.xerial:sqlite-jdbc:3.46.0.0'
```

Se for um projeto Maven, no arquilo 'pom.xml' adicione esta linha ao final, dentro de 'project':
```bash
<dependencies>
        <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
    </dependency>
    </dependencies>
```




Salve, e em 'Projects', clique com o botão esquerdo do mouse no projeto e selecione 'Clean and Build'. Isso baixará as dependências do JDBC automaticamente.

---

## Estrutura do Projeto

Aqui estão as cinco classes que compõem a nossa aplicação:

1. **ConexaoSQLite** - Responsável pela conexão com o banco de dados SQLite.
2. **CriarTabela** - Responsável por criar a tabela de usuários no banco de dados.
3. **InserirUsuario** - Responsável por inserir dados de novos usuários na tabela.
4. **ListarUsuarios** - Responsável por listar todos os usuários presentes na tabela.
5. **App** - A classe principal que orquestra a execução do programa.

---

Aqui está a explicação detalhada sobre o uso de **`try`** e **`catch`** com a adição da sua solicitação:

---

### 1. **ConexaoSQLite: Conectar ao Banco de Dados**

#### O que faz esta classe?

Esta classe tem o objetivo de estabelecer e fechar a conexão com o banco de dados SQLite. Vamos analisar o código passo a passo.

### Código:

```java
package Conexao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConexaoSQLite {
    // Método para conectar ao banco de dados
    public Connection conectar() {
        Connection conexao = null;
        String url = "jdbc:sqlite:usuarios.db"; // Caminho para o banco de dados
        
        try {
            conexao = DriverManager.getConnection(url);
            System.out.println("Conexão com SQLite estabelecida!");
        } catch (SQLException e) {
            System.out.println("Erro ao conectar ao banco: " + e.getMessage());
        }
        
        return conexao;
    }

    // Método para fechar a conexão
    public void desconectar(Connection conexao) {
        try {
            if (conexao != null) {
                conexao.close();
                System.out.println("Conexão fechada.");
            }
        } catch (SQLException e) {
            System.out.println("Erro ao fechar a conexão: " + e.getMessage());
        }
    }
}
```

### Explicação

- **`DriverManager.getConnection(url)`**: A classe **`DriverManager`** gerencia a conexão com o banco de dados. O método **`getConnection`** cria uma conexão com o banco SQLite localizado no arquivo **`usuarios.db`**.
  
- **`SQLException`**: Caso aconteça algum erro, como problemas na conexão, o Java lança uma exceção **`SQLException`**, que é tratada no bloco **`catch`**.

- **`conexao.close()`**: Para liberar os recursos do sistema, sempre fechamos a conexão quando terminamos de usá-la.

---

### **O que são `try` e `catch`?**

O bloco **`try`** e **`catch`** são usados em Java (e em muitas outras linguagens de programação) para **lidar com exceções** (erros que ocorrem durante a execução do programa). Vamos entender como eles funcionam:

#### 1. **`try`**:
- O bloco **`try`** é onde colocamos o código que pode gerar uma exceção (erro). Dentro do **`try`**, o Java tenta executar o código normalmente.
- Se o código dentro do **`try`** gerar um erro, o controle do programa passa imediatamente para o **`catch`**, onde a exceção será tratada.

#### 2. **`catch`**:
- O bloco **`catch`** é usado para capturar e tratar a exceção que ocorreu dentro do **`try`**. Se algum erro ocorrer no código dentro do **`try`**, ele será "pegado" aqui no **`catch`**, evitando que o programa trave ou se comporte de forma inesperada.
- O **`catch`** permite que o programador defina uma resposta personalizada para o erro, como exibir uma mensagem de erro mais amigável ou realizar outras ações de recuperação.

### Exemplo de como **`try`** e **`catch`** funcionam:

No exemplo da classe **`ConexaoSQLite`**:

```java
try {
    conexao = DriverManager.getConnection(url);
    System.out.println("Conexão com SQLite estabelecida!");
} catch (SQLException e) {
    System.out.println("Erro ao conectar ao banco: " + e.getMessage());
}
```

- **No bloco `try`**: O Java tenta conectar-se ao banco de dados SQLite usando o método **`getConnection`**. Se a conexão for bem-sucedida, a mensagem **"Conexão com SQLite estabelecida!"** será exibida.
  
- **Se ocorrer um erro (exceção)**: Se, por algum motivo, o banco de dados não puder ser acessado (como uma URL incorreta, ou problemas de rede), o Java gerará uma **`SQLException`**. O controle do programa será transferido para o **`catch`**, e a mensagem de erro será exibida na tela: **"Erro ao conectar ao banco: " + e.getMessage()**. A variável **`e`** contém detalhes sobre a exceção, como o tipo de erro e uma descrição.

### Por que usar **`try`** e **`catch`**?

1. **Evitar que o programa pare inesperadamente**: Se não usássemos **`try`** e **`catch`**, o programa poderia travar ao encontrar um erro, interrompendo sua execução. Usando **`try`** e **`catch`**, conseguimos tratar esses erros de maneira controlada e amigável.
   
2. **Fornecer feedback ao usuário**: Com o **`catch`**, podemos fornecer mensagens de erro mais amigáveis e informativas, o que melhora a experiência do usuário.

3. **Manter a aplicação funcionando**: O **`catch`** permite que o programa continue executando, mesmo após um erro. Por exemplo, em vez de travar a aplicação por causa de um erro de conexão, podemos tentar reconectar ou informar ao usuário o que ocorreu.
---

## 2. **CriarTabela**: Criar a Tabela de Usuários

### O que faz esta classe?

A classe **`CriarTabela`** é responsável por criar a tabela **`usuarios`** no banco de dados. Caso a tabela já exista, o código não gera erro, graças ao comando **`IF NOT EXISTS`**.

### Código:

```java
package Conexao;

import java.sql.Connection;
import java.sql.Statement;

public class CriarTabela {
    public static void criarTabelaUsuarios(Connection conexao) {
        String sql = "CREATE TABLE IF NOT EXISTS usuarios ("
                   + "id INTEGER PRIMARY KEY AUTOINCREMENT, "
                   + "nome TEXT NOT NULL, "
                   + "email TEXT NOT NULL)";
        
        try (Statement stmt = conexao.createStatement()) {
            stmt.execute(sql);
            System.out.println("Tabela 'usuarios' criada ou já existente.");
        } catch (Exception e) {
            System.out.println("Erro ao criar tabela: " + e.getMessage());
        }
    }
}
```

### Explicação

- **`CREATE TABLE IF NOT EXISTS usuarios`**: O comando SQL **`CREATE TABLE`** cria uma nova tabela. O **`IF NOT EXISTS`** garante que a tabela só será criada se ela ainda não existir, evitando erros.
- **`Statement`**: A classe **`Statement`** é usada para executar comandos SQL no banco de dados. Aqui, ela é criada através de **`conexao.createStatement()`**.
- **`stmt.execute(sql)`**: Executa o comando SQL que cria a tabela.
- **`Exception`**: Se ocorrer algum erro durante a execução do SQL (por exemplo, um erro de sintaxe), o erro é capturado no bloco **`catch`** e a mensagem é exibida.

---

## 3. **InserirUsuario**: Inserir Usuário na Tabela

### O que faz esta classe?

A classe **`InserirUsuario`** é responsável por inserir novos usuários na tabela **`usuarios`**. Ela recebe o nome e o e-mail do usuário como parâmetros e os insere no banco.

### Código:

```java
package Conexao;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class InserirUsuario {
    public static void inserirUsuario(Connection conexao, String nome, String email) {
        String sql = "INSERT INTO usuarios (nome, email) VALUES (?, ?)";
        
        try (PreparedStatement pstmt = conexao.prepareStatement(sql)) {
            pstmt.setString(1, nome); // Substitui o primeiro ? por 'nome'
            pstmt.setString(2, email); // Substitui o segundo ? por 'email'
            pstmt.executeUpdate();
            System.out.println("Usuário inserido com sucesso!");
        } catch (Exception e) {
            System.out.println("Erro ao inserir usuário: " + e.getMessage());
        }
    }
}
```

### Explicação

- **`INSERT INTO usuarios (nome, email) VALUES (?, ?)`**: Este comando SQL insere dados na tabela **`usuarios`**. O **`?`** é um placeholder (marcador) que será substituído pelos valores reais.
- **`PreparedStatement pstmt = conexao.prepareStatement(sql)`**: **`PreparedStatement`** é uma forma segura de executar comandos SQL, pois protege contra ataques de injeção de SQL.
- **`pstmt.setString(1, nome)`**: Aqui, estamos substituindo o primeiro **`?`** pelo valor de **`nome`**.
- **`pstmt.executeUpdate()`**: Executa o comando de inserção no banco de dados, adicionando o novo usuário.
- **`Exception`**: Caso ocorra algum erro ao tentar inserir os dados no banco, ele será capturado e a mensagem de erro será exibida.

---

## 4. **ListarUsuarios**: Listar Todos os Usuários

### O que faz esta classe?

A classe **`ListarUsuarios`** tem o objetivo de listar todos os usuários registrados na tabela **`usuarios`**. Ela executa um comando SQL para selecionar todos os registros e os imprime no console.

### Código:

```java
package Conexao;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

public class ListarUsuarios {
    public static void listarUsuarios(Connection conexao) {
        String sql = "SELECT * FROM usuarios";
        
        try (Statement stmt = conexao.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {
            
            System.out.println("ID | Nome | Email");
            while (rs.next()) {
                System.out.println(rs.getInt("id") + " | " + rs.getString("nome") + " | " + rs.getString("email"));
            }
        } catch (Exception e) {
            System.out.println("Erro ao listar usuários: " + e.getMessage());
        }
    }
}
```

### Explicação

- **`SELECT * FROM usuarios`**: O comando SQL **`SELECT`** seleciona todos os dados da tabela **`usuarios`**. O `*` significa "todos os campos".
- **`Statement stmt = conexao.createStatement()`**: Cria um **`Statement`** para executar o comando SQL.
- **`ResultSet rs = stmt.executeQuery(sql)`**: Executa a consulta e armazena os resultados em um **`ResultSet`**, que é um conjunto de dados retornado pelo banco de dados.
- **`rs.next()`**: O método **`next()`** move o cursor do **`ResultSet`** para o próximo registro. Se não houver mais registros, ele retorna **`false`**.
- **`rs.getInt("id")`**: Obtém o valor do campo **`id`** (que é um inteiro).
- **`rs.getString("nome")`** e **`rs.getString("email")`**: Obtêm os valores dos campos **`nome`** e **`email`** como texto.
- **`Exception`**: Caso ocorra algum erro ao executar a consulta ou ao percorrer os resultados, a exceção será capturada e a mensagem de erro será exibida.

---

## 5. **App**: A Classe Principal

### O que faz esta classe?

A classe **`App`** é o ponto de entrada da nossa aplicação. Ela orquestra a execução do programa, realizando as ações de conectar ao banco de dados, criar a tabela, inserir usuários e listar os usuários. Aqui está o código da classe **`App`**:

### Código:

```java
package Conexao;

import java.sql.Connection;

public class App {
    public static void main(String[] args) {
        ConexaoSQLite conexaoSQLite = new ConexaoSQLite(); 
        Connection conexao = conexaoSQLite.conectar();

        if (conexao != null) {
            // Criar tabela
            CriarTabela.criarTabelaUsuarios(conexao);

            // Inserir usuário
            InserirUsuario.inserirUsuario(conexao, "João", "joao@email.com");
            InserirUsuario.inserirUsuario(conexao,"Leo","leo@mail");

            // Listar usuários
            ListarUsuarios.listarUsuarios(conexao);

            // Fechar conexão
            conexaoSQLite.desconectar(conexao);
        }
    }
}
```

### Explicação

- **`ConexaoSQLite conexaoSQLite = new ConexaoSQLite();`**: Cria uma instância da classe **`ConexaoSQLite`**, que será usada para conectar ao banco de dados.
- **`Connection conexao = conexaoSQLite.conectar();`**: Estabelece a conexão com o banco de dados.
- **`CriarTabela.criarTabelaUsuarios(conexao);`**: Chama o método para criar a tabela de usuários, se ela ainda não existir.
- **`InserirUsuario.inserirUsuario(conexao, "João", "joao@email.com");`**: Insere um novo usuário na tabela.
- **`ListarUsuarios.listarUsuarios(conexao);`**: Exibe todos os usuários presentes na tabela.
- **`conexaoSQLite.desconectar(conexao);`**: Fecha a conexão com o banco de dados.

---

## 6. **AtualizarUsuario**: Atualizar Dados de um Usuário

### O que faz esta classe?

A classe **`AtualizarUsuario`** é responsável por **alterar os dados de um usuário existente** na tabela `usuarios`, utilizando o **ID** como referência.
Isso garante que apenas o usuário correto seja atualizado.

---

### Código:

```java
package Conexao;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class AtualizarUsuario {

    public static void atualizarUsuario(Connection conexao, int id, String novoNome, String novoEmail) {
        String sql = "UPDATE usuarios SET nome = ?, email = ? WHERE id = ?";

        try (PreparedStatement pstmt = conexao.prepareStatement(sql)) {

            pstmt.setString(1, novoNome);   // Novo nome
            pstmt.setString(2, novoEmail);  // Novo email
            pstmt.setInt(3, id);            // ID do usuário a ser atualizado

            int linhasAfetadas = pstmt.executeUpdate();

            if (linhasAfetadas > 0) {
                System.out.println("Usuário atualizado com sucesso!");
            } else {
                System.out.println("Nenhum usuário encontrado com esse ID.");
            }

        } catch (Exception e) {
            System.out.println("Erro ao atualizar usuário: " + e.getMessage());
        }
    }
}
```

---

### Explicação

* **`UPDATE usuarios SET nome = ?, email = ? WHERE id = ?`**
  Atualiza os campos `nome` e `email` **somente** do usuário cujo `id` for informado.

* **`pstmt.setString(1, novoNome)`**
  Substitui o primeiro `?` pelo novo nome.

* **`pstmt.setString(2, novoEmail)`**
  Substitui o segundo `?` pelo novo email.

* **`pstmt.setInt(3, id)`**
  Define qual usuário será atualizado.

* **`executeUpdate()`**
  Retorna quantas linhas foram alteradas. Isso permite saber se o usuário realmente existia.

---

## 7. **DeletarUsuario**: Remover um Usuário do Banco

### O que faz esta classe?

A classe **`DeletarUsuario`** é responsável por **excluir um usuário da tabela** usando o **ID** como critério.

---

### Código:

```java
package Conexao;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class DeletarUsuario {

    public static void deletarUsuario(Connection conexao, int id) {
        String sql = "DELETE FROM usuarios WHERE id = ?";

        try (PreparedStatement pstmt = conexao.prepareStatement(sql)) {

            pstmt.setInt(1, id); // ID do usuário a ser removido

            int linhasAfetadas = pstmt.executeUpdate();

            if (linhasAfetadas > 0) {
                System.out.println("Usuário deletado com sucesso!");
            } else {
                System.out.println("Nenhum usuário encontrado com esse ID.");
            }

        } catch (Exception e) {
            System.out.println("Erro ao deletar usuário: " + e.getMessage());
        }
    }
}
```

---


## Como usar no `App` (exemplo rápido)

```java
AtualizarUsuario.atualizarUsuario(conexao, 1, "João Silva", "joao@novoemail.com");
DeletarUsuario.deletarUsuario(conexao, 2);
```

---

### 💡 Observação didática importante (boa prática)

Você agora tem um **CRUD completo**:

* **Create** → InserirUsuario
* **Read** → ListarUsuarios
* **Update** → AtualizarUsuario
* **Delete** → DeletarUsuario

