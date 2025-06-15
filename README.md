# atv-06-12

**Código fonte**
-import java.util.List;

public class Main {

    public static void main(String[] args) {
        Database db = new Database();

        RepositorioEquipamento repo = new RepositorioEquipamento(db);

        Equipamento eq1 = new Equipamento();
        eq1.setNome("Motor");
        eq1.setTipo("Elétrico");
        eq1.setLocalizacao("Sala A");

        repo.criar(eq1);  // Inserir no banco
        System.out.println("Criado: " + eq1.getNome());

        System.out.println("\nListando todos equipamentos:");
        List<Equipamento> lista = repo.listarTodos();
        for (Equipamento e : lista) {
            System.out.println(e.getId() + ": " + e.getNome() + ", " + e.getTipo() + ", " + e.getLocalizacao());
        }

        eq1.setLocalizacao("Sala B");
        repo.atualizar(eq1);
        System.out.println("\nAtualizado equipamento id " + eq1.getId() + " para localização: " + eq1.getLocalizacao());

        Equipamento buscado = repo.buscarPorId(eq1.getId());
        if (buscado != null) {
            System.out.println("Buscado por ID: " + buscado.getId() + ", localização: " + buscado.getLocalizacao());
        }

        repo.deletar(eq1);
        System.out.println("\nEquipamento deletado id: " + eq1.getId());

        System.out.println("\nListando todos equipamentos após exclusão:");
        lista = repo.listarTodos();
        for (Equipamento e : lista) {
            System.out.println(e.getId() + ": " + e.getNome() + ", " + e.getTipo() + ", " + e.getLocalizacao());
        }

        db.fecharConexao();
    }
}




import com.j256.ormlite.jdbc.JdbcConnectionSource;
import com.j256.ormlite.support.ConnectionSource;

public class Database {

    private static final String URL = "jdbc:sqlite:equipamentos.db";
    private ConnectionSource connection;

    public Database() {
        try {
            connection = new JdbcConnectionSource(URL);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public ConnectionSource getConnection() {
        return connection;
    }

    public void fecharConexao() {
        try {
            if (connection != null) {
                connection.close();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}




import com.j256.ormlite.table.DatabaseTable;
import com.j256.ormlite.field.DatabaseField;

@DatabaseTable(tableName = "equipamento")
public class Equipamento {

    @DatabaseField(generatedId = true)
    private int id;

    @DatabaseField
    private String nome;

    @DatabaseField
    private String tipo;

    @DatabaseField
    private String localizacao;

    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }

    public String getTipo() { return tipo; }
    public void setTipo(String tipo) { this.tipo = tipo; }

    public String getLocalizacao() { return localizacao; }
    public void setLocalizacao(String localizacao) { this.localizacao = localizacao; }
}





import com.j256.ormlite.dao.Dao;
import com.j256.ormlite.dao.DaoManager;
import com.j256.ormlite.table.TableUtils;
import java.sql.SQLException;
import java.util.List;

public class RepositorioEquipamento {

    private static Dao<Equipamento, Integer> dao;

    public RepositorioEquipamento(Database db) {
        try {
            dao = DaoManager.createDao(db.getConnection(), Equipamento.class);
            TableUtils.createTableIfNotExists(db.getConnection(), Equipamento.class);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public Equipamento criar(Equipamento equipamento) {
        try {
        dao.create(equipamento);
        System.out.println("Equipamento inserido com sucesso!");
        } catch (SQLException e) {
        e.printStackTrace();
        }
    return equipamento;
    }


    public Equipamento buscarPorId(int id) {
        try {
            return dao.queryForId(id);
        } catch (SQLException e) {
            e.printStackTrace();
            return null;
        }
    }

    public List<Equipamento> listarTodos() {
        try {
            return dao.queryForAll();
        } catch (SQLException e) {
            e.printStackTrace();
            return null;
        }
    }

    public void atualizar(Equipamento equipamento) {
        try {
            dao.update(equipamento);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public void deletar(Equipamento equipamento) {
        try {
            dao.delete(equipamento);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}




**Prints**

![](![Capturar](https://github.com/user-attachments/assets/7053e098-4f09-4cef-b017-768f44fc6a17))

![](![Capturar1](https://github.com/user-attachments/assets/495e932e-e78c-44e9-abb8-bbcf3fd0cbe8))


