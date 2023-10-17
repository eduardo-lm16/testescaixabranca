import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

/**
 * Esta classe é responsável por representar o usuário no sistema.
 */
public class User {

    /**
     * O seguinte trecho cria a conexão com o banco de dados.
     *
     * @return a conexão do banco de dados
     */
    public Connection conectarDB(){

        /**
         * A conexão com o banco de dados é iniciada como nulo
         */
        Connection conn = null;

        /**
         * Método try catch para realizar a conexão com o banco de dados
         */
        try{
            /**
             * Método responasavel por garantir que o driver seja carregado
             */
            Class.forName("com.mysql.Driver.Manager").newInstance();

            /**
             * Definindo o caminho para o banco de dados.
             */
            String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";

            /**
             * Realizando a coneção com o banco de dados
             */
            conn = DriverManager.getConnection(url);
        }
        /**
         * Trata exceção caso ocorra erro ao conectar com o banco de dados
         */
        catch (Exception e){}
        /**
         * @return conexão com banco de dados
         */
        return conn;
    }

    /**
     * Nome de usuario
     */
    public String nome = "";
    /**
     * Resultado para a busca de usuario no banco de dados iniciada como falso
     */
    public boolean result = false;

    /**
     * Verifica se usuario possui registro no banco de dados
     *
     * @param login o login do usuario
     * @param senha a senha do usuario
     * @return result como verificação se o usuario está registrado no banco de dados
     */
    public boolean VerificarUsuario(String login, String senha){
        /**
         * Realiza a busca no banco de dados
         */
        String sql = "";

        /**
         * Conecta com banco de dados
         */
        Connection conn = conectarDB();

        /**
         * Monta a linha com um select para efetuar a busca no banco de dados
         */
        sql += "select nome from usuarios ";
        sql += "where login = " + "'" + login + "'";
        sql += " and senha = " + "'" + senha + "';";
        /**
         * Tenta buscar o usuario passado pela variável SQL no banco de dados
         */
        try{
            /**
             * Inicia as variáveis para realizar a busca no banco de dados e obtém os resultados
             */
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(sql);
            /**
             * Caso um registro seja encontrado, a seguinte variação será executada
             */
            if(rs.next()){
                /**
                 * Variavel result recebe valor true
                 * Variavel nome recebe o nome encontrado no banco de dados
                 */
                result  = true;
                nome = rs.getString("nome");
            }

        }
        /**
         * Trata exceções lançadas na busca pelo registro do usuário
         */
        catch (Exception e){

        }
        /**
         * Retorna a variável result
         */
        return  result;
    }

}