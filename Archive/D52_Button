#include <WiFi.h> //for esp library
#include <WiFiUdp.h>
#include <wifiInfo>
#include <OSCMessage.h>

WiFiUDP Udp;                                // A UDP instance to let us send and receive packets over UDP

int lastButtonState = LOW;

void setup() {
  Serial.begin(115200);
  pinMode(A2, INPUT);    // set the pushbutton pin to be an input

  // Connect to WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, pass);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");

  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());

  Serial.println("Starting UDP");
  Udp.begin(localPort);

}

void loop() {
  int currentButtonState = digitalRead(A2);
  Serial.println(currentButtonState);
  OSCMessage msg("/sensor");
  if (currentButtonState != lastButtonState) {
    msg.add(currentButtonState);
    lastButtonState = currentButtonState;
  }
  //msg.add("0"); // value will be between 0-4095
  Udp.beginPacket(outIp, outPort);
  msg.send(Udp);
  Udp.endPacket();
  msg.empty();
  delay(20);
}
