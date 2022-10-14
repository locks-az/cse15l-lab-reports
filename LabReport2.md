# Part 1:

## SearchEngine.java code

```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.

    ArrayList<String> lib = new ArrayList<String>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
          return String.format("Home");
        } else {
          System.out.println("Path: " + url.getPath());
          if (url.getPath().contains("/add")) {
            // get after "="
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
              lib.add(parameters[1]);
              return String.format(parameters[1] + " was added");
            }
          } else if (url.getPath().contains("/search")) {
            String[] parameters = url.getQuery().split("=");
            String temp = "";
            if (parameters[0].equals("s")) {
              // search for parameters 1
              for (int i = 0; i < lib.size(); i++) {
                if (lib.get(i).contains(parameters[1])) {
                  temp += lib.get(i);
                  temp += " ";
                }
              }
              return String.format(temp);
            }
          }
          return "404 Not Found!";
        }
    }

}

class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }
        int port = Integer.parseInt(args[0]);
        Server.start(port, new Handler());
    }
}
```

## Sever.java code

```
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.URI;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

interface URLHandler {
    String handleRequest(URI url);
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        // form return body after being handled by program
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            // form the return string and write it on the browser
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        //create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        //start the server
        server.start();
        System.out.println("Server Started! Visit http://localhost:" + port + " to visit.");
    }
}
```

## Search Engine Home Screen

![Home Screenshot](/LabReport2Imgs/SearchEngineHome.png)

Upon launching the server with no additions or queries, the webpage will display the home page. SearchEngine.java first runs throught the main method taking in an input string in order to host the server. If no input string was provided, the webpage displays a "missing port number" error. Otherwise, the string is converted into an int and the Handler() method is called.

In the Handler() method,

Since the url is "localhost:6001" with a base path of "/",

## Adding

![Adding Screenshot](/LabReport2Imgs/AddingPineapple.png)

## Querying

![Querying Screenshot](/LabReport2Imgs/SearchingApp.png)
