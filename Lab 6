// BranchingFunctionsDelays.c Lab 6
// Runs on LM4F120/TM4C123
// Use simple programming structures in C to 
// toggle an LED while a button is pressed and 
// turn the LED on when the button is released.  
// This lab will use the hardware already built into the LaunchPad.
// Kristijan Vidovic
// February 23, 2017

// built-in connection: PF0 connected to negative logic momentary switch, SW2
// built-in connection: PF1 connected to red LED
// built-in connection: PF2 connected to blue LED
// built-in connection: PF3 connected to green LED
// built-in connection: PF4 connected to negative logic momentary switch, SW1

#include "TExaS.h"

#define GPIO_PORTF_DATA_R       (*((volatile unsigned long *)0x400253FC))
#define GPIO_PORTF_DIR_R        (*((volatile unsigned long *)0x40025400))
#define GPIO_PORTF_AFSEL_R      (*((volatile unsigned long *)0x40025420))
#define GPIO_PORTF_PUR_R        (*((volatile unsigned long *)0x40025510))
#define GPIO_PORTF_DEN_R        (*((volatile unsigned long *)0x4002551C))
#define GPIO_PORTF_AMSEL_R      (*((volatile unsigned long *)0x40025528))
#define GPIO_PORTF_PCTL_R       (*((volatile unsigned long *)0x4002552C))
#define SYSCTL_RCGC2_R          (*((volatile unsigned long *)0x400FE108))
#define SYSCTL_RCGC2_GPIOF      0x00000020  // port F Clock Gating Control
#define PORTF2									(*((volatile unsigned long *)0x40025010))	//izlaz
#define PORTF4									(*((volatile unsigned long *)0x40025040))	//ulaz
// basic functions defined at end of startup.s
void DisableInterrupts(void); // Disable interrupts
void EnableInterrupts(void);  // Enable interrupts
//declarations of functions
void Delay100ms(unsigned long time);
void init_PortF(void);
int main(void){ 
  TExaS_Init(SW_PIN_PF4, LED_PIN_PF2);  // activate grader and set system clock to 80 MHz
	init_PortF ();
	PORTF2 = 0x04;								//LED starts ON
  EnableInterrupts();           //enable interrupts for the grader
  while(1){
		
		Delay100ms(1);
		if(!PORTF4){					//switch is pressed
			PORTF2 ^= 0x04;		//toggle the LED
		}
		else{
			PORTF2 = 0x04;		//turn the LED ON
		}
		
  }
}
void init_PortF(void){unsigned long volatile delay;
	SYSCTL_RCGC2_R |= SYSCTL_RCGC2_GPIOF;	//activate clock for PortF
	delay = SYSCTL_RCGC2_R;
	GPIO_PORTF_AMSEL_R &= ~0x14;		//disable analog function on both ports
	GPIO_PORTF_PCTL_R &=~0x14;
	GPIO_PORTF_DIR_R |= 0x04;				//set PortF2 as output
	GPIO_PORTF_DIR_R &= ~0x10;			//set PortF4 as input
	GPIO_PORTF_AFSEL_R &= ~0x14;		//disable alt function on PortF2 and PortF4
	GPIO_PORTF_PUR_R |= 0x10;				//enable PUR on PortF4
	GPIO_PORTF_DEN_R |= 0x14;				//set PortF2 and PortF4 as digital input/output
	
}

void Delay100ms(unsigned long time){
  unsigned long i;
  while(time > 0){
    i = 1333333;  // this number means 100ms
    while(i > 0){
      i = i - 1;
    }
    time = time - 1; // decrements every 100 ms
  }
}
