#include "DigitalOut.h"
#include "PinNames.h"
#include "mbed.h"
RawSerial pc(USBTX,USBRX); 

DigitalOut led(LED1);
int cnt = 0;
float kp = 0.;
float ki = 0.;
float kd = 0.;
volatile bool gotPacket = false;
volatile float data[3];

void serialEvent()
{   
    static char serialInBuffer[32];
    static int data_cnt= 0, buff_cnt = 0;
    if(pc.readable())
    {
        char byteIn = pc.getc();
        if(byteIn == ',')
        {
            serialInBuffer[buff_cnt] = '\0';
            data[data_cnt++] = atof(serialInBuffer); // string을 float으로 변환
            buff_cnt = 0;
        }
        else if(byteIn == '\n')
        {
            serialInBuffer[buff_cnt] = '\0';
            data[data_cnt] = atof(serialInBuffer);
            buff_cnt =0 ; data_cnt = 0;
            gotPacket = true;
            led=!led;
        }
        else
        {
            serialInBuffer[buff_cnt++] = byteIn;
        }
    }
}

int main(){
    pc.printf("loop\n"); 
    pc.attach(&serialEvent);
    while(1){
        if(gotPacket)
        {
            gotPacket = false;
            pc.printf("data = %.3f,%.3f,%.3f\n\r",data[0], data[1], data[2]);
        }
        
    }
}

