/*
  Web Server
 
 A simple web server that shows the value of the analog input pins.
 using an Arduino Wiznet Ethernet shield. 
 
 Circuit:
 * Ethernet shield attached to pins 10, 11, 12, 13
 * Analog inputs attached to pins A0 through A5 (optional)
 
 created 18 Dec 2009
 by David A. Mellis
 modified 9 Apr 2012
 by Tom Igoe
 
 */

#include <SPI.h>
#include <Ethernet.h>

// Enter a MAC address and IP address for your controller below.
// The IP address will be dependent on your local network:
byte mac[] = { 
  0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
IPAddress ip(192,168,1, 177);
String buffer;
boolean Dstart=false;
boolean Dend=false;

// Initialize the Ethernet server library
// with the IP address and port you want to use 
// (port 80 is default for HTTP):
EthernetServer server(80);

void setup() {
 // Open serial communications and wait for port to open:
  Serial.begin(9600);
  pinMode(13,OUTPUT);
   while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }


  // start the Ethernet connection and the server:
  Ethernet.begin(mac, ip);
  server.begin();
  Serial.print("server is at ");
  Serial.println(Ethernet.localIP());
}


void loop() {
  // listen for incoming clients
  EthernetClient client = server.available();
  if (client) {
    Serial.println("new client");
    // an http request ends with a blank line
    boolean currentLineIsBlank = true;
    while (client.connected()) {
      if (client.available()) {
        char c = client.read();
       // Serial.write(c);
 
        // if you've gotten to the end of the line (received a newline
        // character) and the line is blank, the http request has ended,
        // so you can send a reply
        if(c=='*'){ //start of data
          Dstart=true;
          Serial.println("Start of Data");
        }
        else if(c=='?'){  
          Dstart=false;
          Dend=true;
       //  Serial.println("K="+buffer);
           buffer="";
         Serial.println("End Of Data");
        }
        else if(Dstart){
          if(c=='K'){
            Serial.println("K="+buffer);
            switch(buffer.toInt()) {
            case 1:
            digitalWrite(13,HIGH);
            break;
            
            case 2:
            //digitalWrite(13,HIGH);
            break;
            
            case 3:
            digitalWrite(13,LOW);
            break;
            
            case 4:
           // digitalWrite(13,LOW);
            break;           

            
            }
          }
          if(c=='D')
          {
            Serial.println("D="+buffer);
            analogWrite(14,buffer.toInt());
          }
          
          if(c=='B')
          {
            Serial.println("B="+buffer);
            analogWrite(13,buffer.toInt());
          }
          else{
            buffer += c;
          }
        }
        
        else if (c == '\n' && currentLineIsBlank) {
          // send a standard http response header
          client.println("HTTP/1.1 200 OK");
          client.println("Content-Type: text/html");
          client.println("Connection: close");
          client.println();
          client.println("Hello User");
          break;
         // Serial.print(buffer);
         
        }
        else if (c == '\n') {
          // you're starting a new line
          currentLineIsBlank = true;
         // buffer="";
        } 
        else if (c != '\r') {
          // you've gotten a character on the current line
          currentLineIsBlank = false;
        }
      }
    }
    // give the web browser time to receive the data
    delay(1);
    // close the connection:
    client.stop();
    Serial.println("client disonnected");
  }
}
