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

outside
