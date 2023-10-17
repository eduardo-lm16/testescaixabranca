import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

/**
 * Esta classe tem o intuito de representar o usuario dentro do sistema
 */
public class User {

    /**
     * Cria um conexão com o banco de dados
     *
     * @return a conexão do banco de dados
     */
    public Connection conectarDB(){

        /**
         * A conexão com banco de dados iniciada como nulo
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
             * O caminho para o banco de dados
             */
            String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";

            /**
             * Realiza a conexção com banco de dados
             */
            conn = DriverManager.getConnection(url);
        }
        /**
         * Trata exceção caso ocorra ao conectar ao banco de dados
         */
        catch (Exception e){}
        /**
         * @return conexão com banco de dados
         */
        return conn;
    }

    /**
     *  Nome do usuario
     */
    public String nome = "";
    /**
     * Resultado para a busca de usuario no banco de dados iniciada como falso
     */
    public boolean result = false;

    /**
     * Verifica se usuario está registrado no banco de dados
     *
     * @param login o login do usuario
     * @param senha a senha do usuario
     * @return result como verificação se o usuario está registrado no banco de dados
     */
    public boolean VerificarUsuario(String login, String senha){
        /**
         * Linha para realizar a busca no banco de dados
         */
        String sql = "";

        /**
         * Conecta com banco de dados
         */
        Connection conn = conectarDB();

        /**
         * Monta a linha como sendo um select para buscar no banco de dados
         */
        sql += "select nome from usuarios ";
        sql += "where login = " + "'" + login + "'";
        sql += " and senha = " + "'" + senha + "';";
        /**
         * Tenta buscar o usuario passado pela variavel sql no banco de dados
         */
        try{
            /**
             * Inicia as variavesi para realizar a busca no banco de dados e pega os resultados
             */
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(sql);
            /**
             * Caso um registro seja encontrado o seguinte if será executado
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
         * Trata Exceções lançadas na busca pelo registro do usuario
         */
        catch (Exception e){

        }
        /**
         * Retorna a variavel result
         */
        return  result;
    }

}
