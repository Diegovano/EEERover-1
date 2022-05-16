# EEERover starter code

This project is an example of setting up WiFi communication with the Metro M0 Express module using HTTP.
The microcontroller connects to a WiFi network and acts as a simple web server, returning a very basic web page when it receives an HTTP request.
It also acts on certain path strings in the HTTP request to switch an LED on and off when the client clicks buttons on the webpage.

## Building the project

Download Microsoft Visual Studio Code and install the Platformio extension. Open Platformio Home using the link in the Platformio sidebar or the 🏠 (home) icon on the bottom toolbar. Choose 'Open Project' and select the directory containg this README.

Build the project using the Build option in the Platformio sidebar or the ✓ icon on the bottom toolbar. Platformio will download the required libraries and board support package the first time you build the project.

Run the code by choosing 'Upload' in the Platformio side bar. View the serial output using 'monitor'. A terminal will open to show the serial output from the microcontroller.

## Serving the webpage

A HTTP transaction begins when a client (your computer) sends some text (a request) to a server (the microcontroller module).
In response, the microcontroller sends some text back (the webpage, in HTML). A typical HTTP request looks like this:
```
GET / HTTP/1.1
Host: 192.168.0.3
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36 Edg/101.0.1210.39
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en;q=0.9,en-US;q=0.8
```
You will see each HTTP request printed on the serial terminal when it is received.

The starter code is set up to process HTTP requests. The line:
```
if (c == '\n')
```
detects a complete line in the request, and
```
if (currentLine.length() == 0)
```
detects an empty line, which denotes the end of the request.
When the request is complete, the server responds by sending a series of hardcoded text strings, which make up the simple webpage.

## Switching the LED

The HTTP request contains information including a string like:
```
GET /H
```
`GET` is the type of request and `/H` is the path requested, the part of the URL after the host name
The line:
```
if (currentLine.endsWith("GET /H"))
```
looks for this string in the HTTP request and, if it is found, switches the LED on.
So, entering the URL `http://192.168.0.10/H` (depending on your IP address) will switch the LED on and return the webpage.
The code, as written, will always return the same webpage no matter what URL is requested.

## Next steps

The code gives an example of sending information in both directions between the microcontroller and the remote client.
It can be extended to include all the data you need to control your rover and get data back.

### Improving the UI

The basic webpage gives a poor quality UI. A better UI could be written using more complex HTML, but storing that on the microconroller would slow down communication if it is transmitted on every request.
A better approach could be to include some logic so that the webpage is only sent if the root path `/index.html` is requested, and not other paths.
Some simple Javascript could allow the webpage to send requests for other paths like `/H` without refreshing the webpage.
You could also cretae an app that sends these requests.

### Alternatives to HTTP

Using HTTP creates quite a lot of overhead in the headers that are transmitted in every transaction.
You can use the WiFi101 library to send and receive raw data byte-by-byte.
However, a web browser will not let you control the internet traffic at such a low level so you would need to write a user interface with programming language to achieve this functionality.
