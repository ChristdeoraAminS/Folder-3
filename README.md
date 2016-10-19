# Folder-3
Percobaan A
Tugas dan Pertanyaan
====1.Master
#include <SoftwareSerial.h>
SoftwareSerial RS485Serial(10, 11);
int data;

void setup()
{
  Serial.begin(9600);
  Serial.println("Komunikasi RS485 :");
  pinMode(3, OUTPUT);
  digitalWrite(3, LOW);
  RS485Serial.begin(9600);
}
void loop ()
{
  if (RS485Serial.available())
  {
  data = RS485Serial.read();
  Serial.print(data);
  }
}

====Slave
#include <SoftwareSerial.h>
SoftwareSerial RS485Serial(10, 11);
int data = 123;

void setup()
{
  pinMode(3, OUTPUT);
  digitalWrite(3, LOW);
  RS485Serial.begin(9600);
}
void loop ()
{
 digitalWrite(3, HIGH);
 RS485Serial.write(data);
 delay(1000);
}

====2.Master
#include <SoftwareSerial.h>
SoftwareSerial RS485Serial(3, 2);
//int data;

void setup()
{
  Serial.begin(9600);
  Serial.println("Komunikasi RS485 :");
  pinMode(1, OUTPUT);
  digitalWrite(1, LOW);
  RS485Serial.begin(9600);
}
void loop ()
{
  if (RS485Serial.available())
  {
  int data = RS485Serial.read();
  int data1 = data*4;
    Serial.println(data1);
  }
}

====Slave
#include <SoftwareSerial.h>
SoftwareSerial RS485Serial(3, 2);
//int data;

void setup()
{
  pinMode(1, OUTPUT);
  digitalWrite(1, LOW);
  RS485Serial.begin(9600);
}
void loop ()
{
 int data =analogRead (A1);
  digitalWrite(1, HIGH);
  RS485Serial.write(data);
  delay(100);
}

Percobaan B
Tugas dan Pertanyaan
====Program Arduino
====Master
#include <SoftwareSerial.h>
SoftwareSerial RS485Serial(3,2); //RX,TX
int A;

void setup()
{
  Serial.begin(9600);
  Serial.println("komunikasi RS485:");
  pinMode(1,OUTPUT);
  digitalWrite(1,LOW); // set DE dan RE untuk terima data
  RS485Serial.begin(9600);
}

void loop()
{
  if(RS485Serial.available())
  {
    A = RS485Serial.read();
    Serial.print(A);
    Serial.println("");
  }
}

====Slave
#include <SoftwareSerial.h>
SoftwareSerial RS485Serial(3,2); //RX,TX
int data = A0;
int A;

void setup() {
  pinMode(1,OUTPUT);
  pinMode(data,INPUT);
  digitalWrite(1,LOW);
  RS485Serial.begin(9600);

}

void loop() {
 A = analogRead(A0);
 digitalWrite(1,HIGH);    //set DE dan RE untuk kirim data
 RS485Serial.write(A);
 delay(1000);

}
====Program C#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.IO.Ports;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        int a;
        string rxString;
        public Form1()
        {
            InitializeComponent();
        }
        
        private void button1_Click(object sender, EventArgs e)
        {
            if (button1.Text == "Connect")
            {
                button1.Text = "Disconnect";
                serialPort1.Open();
                richTextBox1.Text = "";
                a = 0;
            }
            else
            {
                button1.Text = "Connect";
                serialPort1.Close();
            }
        }

        private void serialPort1_DataReceived(object sender, System.IO.Ports.SerialDataReceivedEventArgs e)
        {
            rxString = serialPort1.ReadLine();
            this.Invoke(new EventHandler(DisplayText));

        }

        private void DisplayText(object sender, EventArgs e)
        {
            richTextBox1.AppendText(rxString);
            richTextBox1.ScrollToCaret();
            chart1.Series["Series1"].Points.AddXY(a, Convert.ToInt16(rxString));
            a++;
        }

        private void richTextBox1_TextChanged(object sender, EventArgs e)
        {

        }

        private void chart1_Click(object sender, EventArgs e)
        {

        }

        
    }
}

