# Análise do Código


    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.ResultSet;
    import java.sql.Statement;


        public class User {
        public Connection conectarDB(){
        Connection conn = null;
        try{
        Class.forName("com.mysql.Driver.Manager").newInstance();
        String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";
        conn = DriverManager.getConnection(url);
        }
    
            catch (Exception e){}
            return conn;
        }
        public String nome = "";
        public boolean result = false;
        public boolean VerificarUsuario(String login, String senha){
            String sql = "";
            Connection conn = conectarDB();
    
            sql += "select nome from usuarios ";
            sql += "where login = " + "'" + login + "'";
            sql += " and senha = " + "'" + senha + "';";
            try{
                Statement st = conn.createStatement();
                ResultSet rs = st.executeQuery(sql);
                if(rs.next()){
                    result  = true;
                    nome = rs.getString("nome");
                }
    
            }
            catch (Exception e){
    
            }
            return  result;
        }
    
    }     


# OBSERVAÇÕES
## 1-

<p>
    O código está sintetizado em apenas uma classe que realiza diversas operações diferentes, 
    como a conexão e a busca no banco de dados.
</p>
<p>
     Isso vai contra as boas práticas de programação, que dita que funções diferentes devem
     ser realizadas por classes diferentes, não pela mesma.
</p>

## 2-
<p>
    De acordo com o seguinte trecho do código, existe a possbilidade de ser aceito 
    um valor nulo como entrada:
</p>

        Connection conn = null;
        try{
        Class.forName("com.mysql.Driver.Manager").newInstance();
        String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";
        conn = DriverManager.getConnection(url);
        }
    
            catch (Exception e){}
            return conn;

<p>
    Isto ocorre pois a varável de conexão conn está recebendo o valor null, e logo após é feito um
    try/catch para estabelecer a conexão e caso haja algum problema o Exception não realizará nenhuma
    ação de tratamento, e ao fim, o return enviará uma variável com valor nulo.
</p>

## 3-
<p>
    O código não possui comentários explicativos, e algumas partes são de difícil entendimento,
    como por exemplo:
</p>

    Class.forName("com.mysql.Driver.Manager").newInstance();

<p>
    Para leigos em Java, este trecho de código pode não ser muito claro,
    dificultando o entendimento para posteriores manutenções que possam vir
    a ser realizadas no código.
</p>

## 4-
<p>
    A conexão com o banco de dados não é encerrada em momento algum, conforme 
    trecho de código abaixo demonstra. Isso pode acarretar em possíveis vulnerabilidades
    no código, pois a conexão se manterá aberta:
</p>

    public Connection conectarDB(){
        Connection conn = null;
        try{
        Class.forName("com.mysql.Driver.Manager").newInstance();
        String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";
        conn = DriverManager.getConnection(url);
        }
    
            catch (Exception e){}
            return conn;
        }



