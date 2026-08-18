# Projeto-Integrador
Projeto integrador sobre plataforma para gerenciamento entre amigos, feito por Gabriel Félix, Otavio Avelino e João Vitor Romão.


import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.Scanner;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class teste{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String url ="https://store.steampowered.com/api/appdetails?appids="+sc.nextInt()+"&cc=br";

        HttpClient cliente = HttpClient.newHttpClient();
        HttpRequest pedido = HttpRequest.newBuilder().uri(URI.create(url)).GET().build();

        String json = "";

        try{

            HttpResponse<String> resposta = cliente.send(pedido, HttpResponse.BodyHandlers.ofString());
            json = resposta.body();

        }catch (IOException | InterruptedException e) {

            System.out.println("Ocorreu um erro:" + e.getMessage());

        }
        
        String nome = buscarCampo(json,sc.next());
        String preco = "true".equals(buscarCampo(json, "is_free")) ? "Grátis" : buscarCampo(json, "final_formatted");

        System.out.println("Dados do Jogo: \n" + nome + preco) ;
        sc.close();
    }


    public static String buscarCampo(String json, String campo){
        Pattern pattern = Pattern.compile("\""+campo+"\":\"(.*?)\"");
        Matcher matcher = pattern.matcher(json);
        if(matcher.find()){
            return matcher.group(1);
        }else{
            return null;
        }
    }
}
