# task2
#include <Servo.h>
Servo gripper;
Servo wrist;
Servo elbow;
Servo shoulder;
Servo base;

double base_angle=90;
double shoulder_angle=90;
double elbow_angle=90;
double wrist_angle=90;


void setup() {
 Serial.begin(115200);
   base.attach(8);
  shoulder.attach(9); 
  elbow.attach(10);
  wrist.attach(11);
  gripper.attach(12); 

  base.write(base_angle);
  shoulder.write(shoulder_angle);
  elbow.write(elbow_angle);
  wrist.write(wrist_angle);

}



String getValue(String data, char separator, int index)
{
  int found = 0;
  int strIndex[] = {0, -1};
  int maxIndex = data.length()-1;

  for(int i=0; i<=maxIndex && found<=index; i++){
    if(data.charAt(i)==separator || i==maxIndex){
        found++;
        strIndex[0] = strIndex[1]+1;
        strIndex[1] = (i == maxIndex) ? i+1 : i;
    }
  }

  return found>index ? data.substring(strIndex[0], strIndex[1]) : "";
}

void loop() {
  
  String computerText = Serial.readStringUntil('@');
  computerText.trim();
  if (computerText.length() == 0) {
    return;
  }
  // 92-0-130
  String command = getValue(computerText, ' ',0);

    if (command == "right"  command == "رايت"  command == "Right") {
      base.write(base_angle -= 20);
    }
    if (command == "left"  command == "Left"  command == "لفت") {
     base.write(base_angle += 20);
    }

    if (command == "top"  command == "توب"  command == "Top") {
      shoulder.write(shoulder_angle -= 20);
    }

   if (command == "bottom"|| command == "بوتوم" || command == "Bottom") {
     shoulder.write(shoulder_angle += 20);
    }
    Serial.println(command);
  Serial.println("WORKING");
  delay(1000);

}

let isConnectted = false;
      let port;
      let writer;
      var target_id;
      const enc = new TextEncoder();

      async function onChangespeech() {
        if (!isConnectted) {
          alert("you must connect to the usb in order to use this.");
          return;
        }
       
        try {
          const commandlist = content;
          const commandSplit = commandlist.split(" ")
          const command = commandSplit.slice(-1);
          const computerText = ${command}@;
          await writer.write(enc.encode(computerText));
        } catch (e) {
          console.log(e);
        }
      }
    
      

    async function onConnectUsb() {
      try {
        const requestOptions = {
          // Filter on devices with the Arduino USB vendor ID.
          filters: [{ usbVendorId: 0x2341 }],
        };

        // Request an Arduino from the user.
        port = await navigator.serial.requestPort(requestOptions);
        await port.open({ baudRate: 115200 });
        writer = port.writable.getWriter();
        isConnectted = true;
      } catch (e) {
        console.log("err", e);
      }
    }

<!DOCTYPE html>

<html lang="en" dir="ltr">

<head>

 <meta charset="utf-8">

 <title>speech to text in javascript</title>

 <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css">

</head>

<body>

 <div class="container">

 <h1 class="text-center mt-5">

 Speech to Text 
 </h1>

 <div class="form-group">

 <textarea  id="textbox" rows="6" class="form-control"></textarea>

 </div>

 <div class="form-group">

 <button id="start-btn"  class="btn btn-danger btn-block" title="" >
    Start
 </button>
<button class = "btn btn-danger btn-block" onclick="onConnectUsb()" id="connect-usb">
    connect
</button>


 
 <p id="instructions">Press the Start button</p>

 </div>

 </div>

 <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>

 <script src="script.js"></script>
<script src="arduino.js"></script>
<script>


</script>
</body>

</html>
