Arvin Zhang

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

## Server.java code

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

## Overview

Upon launching the server with no additions or queries, the webpage will display the home page. SearchEngine.java first runs throught the main method taking in an input string in order to host the server. If no input string was provided, the webpage displays a "missing port number" error. Otherwise, the string is converted into an int and the start method from the Server class is called.

Server.java handles the backend and eventually called the handleRequest() method in SearchEngine.java. handleRequest() first checks for the path section of the URL. Then, based on the path, the method enters different conditions, adding to a 'lib' array list or searching and outputing contents already in the 'lib' array list.

## Search Engine Home Screen

![Home Screenshot](/LabReport2Imgs/SearchEngineHome.png)

Since the url is "localhost:6001" with a base path of "/", handleRequest() immediately enters the first condition and simply returns the string "Home" for display on the webpage.

No significant values changed since storing information was not needed as the method directly returned the result rather than a variable.

## Adding

![Adding Screenshot](/LabReport2Imgs/AddingPineapple.png)

When the URL path contains the key term "/add", handleRequest() then splits the URL string by the character "=" into a string array using .split(). handleRequest() continues by checking for the character "s" which indicates an intentional word will be used as input. Lastly, handleRequest adds the word give after "=" into the parameters string array and displays "X was added."

One variable that changed by the time the request finished processing was the 'lib' variable. The 'lib' variable is used to store the strings added by the URL. It starts off as empty and adds a string with each request.

## Querying

![Querying Screenshot](/LabReport2Imgs/SearchingApp.png)

When the URL path contains the key term "/search", the handleRequest() method travels into another if else statement and begins searching for the element. In order to search for a certain word in the 'lib' array list, it uses a for loop, iterating through all strings in 'lib'. For each string, it checks if the string contains parts of the word given. If it does, it then adds to a temp string and returns it after finishing the loop.

One variable that changed was the value of the 'temp' variable which stores the output. It started off as an empty string and with each string in 'lib' that fit the conditions, 'temp' would increase in size and store that string in addtion to the previous strings. At the end, temp held the desired search results was used in the return statement.

# Part 2:

## ArrayExample.java reverseInPlace() Error

Orignal Code:

```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

Bug:
The bug was that the elements of the array were being overwritten while iterting through it. For example, the first element would be set as the last element and its value would be lost. By the time it iterates to the last element, the method will set the last element as the first element, which is already the last element, thus losing the first element entirely. This resulted in a symptom of a mirrored array where the front half is the same as the back half reversed.

Failure Inducing Input and Symptom:

![symptom](/LabReport2Imgs/testReversedInPlace.png)

![fail input example](/LabReport2Imgs/ReversedFailure.png)

When using {1,2,3} as a failure-inducing input, the expected should be {3,2,1}. However, the symptom is {3,2,3};

Fix:

```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }
```

Solution:
In order to prevent the overwritting bug, I used a temp variable to swap the two opposing side elements. Since I was swapping the elements instead of overwritting them, I no longer needed to iterate to the end of the array, so I change the condition to arr.length/2. As a result, the relationship between the bug and the symptom was fixed as no data was being overwritten.

## LinkedList.java append() Error

Orignal Code:

```
public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
            n.next = new Node(value, null);
        }
    }
```

Bug:
The bug was that inside the append() method, there was a while loop traveling to the last node. Specifically, the line adding the node was inside the while loop. This means that the loop will run infinitely since each time the link list is adding/overwriting a node and then checking if there is a next node. This relationship between the bug and the symptom of the while loop results in there always being a next node, and the condition will always be true, thus haveing an overall program symptom of running infinitely.

Failure Inducing Input and Symptom:

![symptom](/LabReport2Imgs/testLLAppend.png)

![fail input example](/LabReport2Imgs/LLAppendFailure.png)

Any input to the code is considered a failure-inducing input since the symptom is that the program runs infinitely and does not return a new linked list. As seen in the example images above, the failure inducing input was "temp.append(1);" The infinite loop symptom can be seen in the second example image above as no results were outputted. A manuel stop of the program was performed instead.

Fix:

```
public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
        }
        n.next = new Node(value, null);

    }
```

Solution:

To fix the relationship between the bug and the symptom, I took out the line that adds the node out of the while loop so that when it reaches the end of the linked list, it will exit the loop and then add the new node.
