# Estrutura

```
src/
└── com.exemplo
    ├── model
    │   └── Usuario.java
    │
    ├── data
    │   ├── Conexao.java
    │   └── UsuarioDAO.java
    │
    ├── service
    │   └── UsuarioService.java
    │
    ├── controller
    │   └── UsuarioController.java
    │
    ├── view
    │   ├── TelaLogin.java
    │   └── TelaPrincipal.java
    │
    └── App.java
```

---

#  1. MODEL

## Usuario.java

Agora vamos deixar o objeto completo e mais correto.

```java
package com.exemplo.model;

public class Usuario {

    private int id;
    private String usuario;
    private String senha;

    public Usuario() {
    }

    public Usuario(String usuario, String senha) {
        this.usuario = usuario;
        this.senha = senha;
    }

    public Usuario(int id, String usuario, String senha) {
        this.id = id;
        this.usuario = usuario;
        this.senha = senha;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsuario() {
        return usuario;
    }

    public void setUsuario(String usuario) {
        this.usuario = usuario;
    }

    public String getSenha() {
        return senha;
    }

    public void setSenha(String senha) {
        this.senha = senha;
    }
}
```

---

#  2. CAMADA DATA

## Conexao.java

```java
package com.exemplo.data;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Conexao {

    private static final String URL = "jdbc:mysql://localhost:3306/seu_banco";
    private static final String USUARIO = "root";
    private static final String SENHA = "root";

    public static Connection conectar() {
        try {
            return DriverManager.getConnection(URL, USUARIO, SENHA);
        } catch (SQLException e) {
            throw new RuntimeException("Erro ao conectar ao banco", e);
        }
    }
}
```

---

## UsuarioDAO.java (Agora usando objeto Usuario)

Aqui é onde a arquitetura começa a ficar correta.

```java
package com.exemplo.data;

import com.exemplo.model.Usuario;
import org.mindrot.jbcrypt.BCrypt;

import java.sql.*;

public class UsuarioDAO {

    public boolean salvar(Usuario usuario) {

        String sql = "INSERT INTO usuarios (usuario, senha) VALUES (?, ?)";

        String senhaHash = BCrypt.hashpw(usuario.getSenha(), BCrypt.gensalt());

        try (Connection conn = Conexao.conectar();
             PreparedStatement stmt = conn.prepareStatement(sql)) {

            stmt.setString(1, usuario.getUsuario());
            stmt.setString(2, senhaHash);

            stmt.executeUpdate();
            return true;

        } catch (SQLException e) {
            e.printStackTrace();
            return false;
        }
    }

    public Usuario buscarPorUsuario(String nomeUsuario) {

        String sql = "SELECT * FROM usuarios WHERE usuario = ?";

        try (Connection conn = Conexao.conectar();
             PreparedStatement stmt = conn.prepareStatement(sql)) {

            stmt.setString(1, nomeUsuario);
            ResultSet rs = stmt.executeQuery();

            if (rs.next()) {
                return new Usuario(
                        rs.getInt("id"),
                        rs.getString("usuario"),
                        rs.getString("senha")
                );
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

        return null;
    }
}
```

---

#  3. SERVICE (Regra de Negócio)

Aqui vai a inteligência da aplicação.

```java
package com.exemplo.service;

import com.exemplo.data.UsuarioDAO;
import com.exemplo.model.Usuario;
import org.mindrot.jbcrypt.BCrypt;

public class UsuarioService {

    private UsuarioDAO usuarioDAO = new UsuarioDAO();

    public boolean registrar(Usuario usuario) {

        if (usuario.getUsuario().isEmpty() || usuario.getSenha().isEmpty()) {
            return false;
        }

        return usuarioDAO.salvar(usuario);
    }

    public Usuario login(String nomeUsuario, String senhaDigitada) {

        Usuario usuarioBanco = usuarioDAO.buscarPorUsuario(nomeUsuario);

        if (usuarioBanco != null) {
            if (BCrypt.checkpw(senhaDigitada, usuarioBanco.getSenha())) {
                return usuarioBanco;
            }
        }

        return null;
    }
}
```

Percebe a diferença?

* DAO → só banco
* Service → regra
* Controller → ponte com a View

---

#  4. CONTROLLER

```java
package com.exemplo.controller;

import com.exemplo.model.Usuario;
import com.exemplo.service.UsuarioService;

public class UsuarioController {

    private UsuarioService service = new UsuarioService();

    public boolean registrar(String usuario, String senha) {
        Usuario novoUsuario = new Usuario(usuario, senha);
        return service.registrar(novoUsuario);
    }

    public Usuario login(String usuario, String senha) {
        return service.login(usuario, senha);
    }
}
```

Controller não faz regra.
Ele só organiza o fluxo.

---

#  5. EVENTO DO BOTÃO LOGIN (Swing)

```java
private void btnLoginActionPerformed(java.awt.event.ActionEvent evt) {

    String usuario = txtUsuario.getText().trim();
    String senha = new String(txtSenha.getPassword()).trim();

    Usuario usuarioLogado = controller.login(usuario, senha);

    if (usuarioLogado != null) {

        JOptionPane.showMessageDialog(this, "Bem-vindo " + usuarioLogado.getUsuario());

        TelaPrincipal tela = new TelaPrincipal(usuarioLogado);
        tela.setVisible(true);

        this.dispose();

    } else {
        JOptionPane.showMessageDialog(this, "Usuário ou senha inválidos");
    }
}
```

---

#  6. TelaPrincipal recebendo o usuário

```java
public class TelaPrincipal extends javax.swing.JFrame {

    private Usuario usuarioLogado;

    public TelaPrincipal(Usuario usuario) {
        initComponents();
        this.usuarioLogado = usuario;

        lblBemVindo.setText("Bem-vindo " + usuario.getUsuario());
    }
}
```
