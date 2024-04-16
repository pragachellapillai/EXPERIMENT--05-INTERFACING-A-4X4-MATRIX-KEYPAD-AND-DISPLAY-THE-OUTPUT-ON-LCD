Name: PRAGAHARSHITHA NC

Ref No:212222110033
# EXPERIMENT 05 INTERFACING A 4X4 MATRIX KEYPAD AND DISPLAY THE OUTPUT ON LCD

## Aim: 
To Interface a 4X4 matrix keypad and show the output on 16X2 LCD display to ARM controller , and simulate it in Proteus
## Components required: 
STM32 CUBE IDE, Proteus 8 simulator .
## Theory:

![image](https://github.com/vasanthkumarch/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/36288975/2a4a795e-1674-4329-ae07-3f5e8d5073e2)

4×4 Keypad Module Pin Diagram
 
4x4 Keypad module Pin Diagram
4×4 Keypad module Pin Diagram
Pin Number	Pin Name	Description
1	R1	Taken out from 1st  ROW
2	R2	Taken out from 2nd  ROW
3	R3	Taken out from 3rd  ROW
4	R4	Taken out from 4th  ROW
5	C1	Taken out from 1st  COLUMN
6	C2	Taken out from 2nd COLUMN
7	C3	Taken out from 3rd  COLUMN
8	C4	Taken out from 4th  COLUMN
4×4 Matrix Keypad Module Hardware Overview
These Keypad modules are made of thin, flexible membrane material. The 4 x4 keypad module consists of 16 keys, these Keys are organized in a matrix of rows and columns. All these switches are connected to each other with a conductive trace. Normally there is no connection between rows and columns. When we will press a key, then a row and a column make contact.

## Procedure : 
 ## LCD 16X2 
   16×2 LCD is named so because; it has 16 Columns and 2 Rows. There are a lot of combinations available like,
   8×1, 8×2, 10×2, 16×1, etc. But the most used one is the 16*2 LCD, hence we are using it here.

All the above mentioned LCD display will have 16 Pins and the programming approach is also the same and hence the choice is left to you. 
Below is the Pinout and Pin Description of 16x2 LCD Module:

![image](https://user-images.githubusercontent.com/36288975/233858086-7b1a88a2-f941-475c-86c2-b3bae68bdf7e.png)
![image](https://user-images.githubusercontent.com/36288975/233857710-541ac1c2-786c-4dfc-b7b5-e3a4868a9cb6.png)
![image](https://user-images.githubusercontent.com/36288975/233857733-05df5dbf-1a1e-479e-85bb-8014a39ad878.png)

4-bit and 8-bit Mode of LCD:

The LCD can work in two different modes, namely the 4-bit mode and the 8-bit mode. In 4 bit mode we send the data nibble by nibble, first upper nibble and then lower nibble. For those of you who don’t know what a nibble is: a nibble is a group of four bits, so the lower four bits (D0-D3) of a byte form the lower nibble while the upper four bits (D4-D7) of a byte form the higher nibble. This enables us to send 8 bit data.

Whereas in 8 bit mode we can send the 8-bit data directly in one stroke since we use all the 8 data lines.

 8-bit mode is faster and flawless than 4-bit mode. But the major drawback is that it needs 8 data lines connected to the microcontroller. This will make us run out of I/O pins on our MCU, so 4-bit mode is widely used. No control pins are used to set these modes. 
 LCD Commands:

There are some preset commands instructions in LCD, which we need to send to LCD through some microcontroller. Some important command instructions are given below:

Hex Code

Command to LCD Instruction Register

0F

LCD ON, cursor ON

01

Clear display screen

02

Return home

04

Decrement cursor (shift cursor to left)

06

Increment cursor (shift cursor to right)

05

Shift display right

07

Shift display left

0E

Display ON, cursor blinking

80

Force cursor to beginning of first line

C0

Force cursor to beginning of second line

38

2 lines and 5×7 matrix

83

Cursor line 1 position 3

3C

Activate second line

08

Display OFF, cursor OFF

C1

Jump to second line, position 1

OC

Display ON, cursor OFF

C1

Jump to second line, position 1

C2

Jump to second line, position 2
 
## Procedure:
 1. click on STM 32 CUBE IDE, the following screen will appear 

 2. click on FILE, click on new stm 32 project 
 
3. select the target to be programmed  as shown below and click on next 

4.select the program name 

5. corresponding ioc file will be generated automatically 

6.select the appropriate pins as gipo, in or out, USART or required options and configure 

7.click on cntrl+S , automaticall C program will be generated 

8. edit the program and as per required 

9. Add necessary library files of LCD 16x2 , write the program and use project and build  

10. once the project is bulild 

11. click on debug option 

12.  Creating Proteus project and running the simulation
We are now at the last part of step by step guide on how to simulate STM32 project in Proteus.

13. Create a new Proteus project and place STM32F40xx i.e. the same MCU for which the project was created in STM32Cube IDE. 

14. After creation of the circuit as per requirement as shown below 

14. Double click on the the MCU part to open settings. Next to the Program File option, give full path to the Hex file generated using STM32Cube IDE. Then set the external crystal frequency to 8M (i.e. 8 MHz). Click OK to save the changes.
https://engineeringxpert.com/wp-content/uploads/2022/04/26.png

15. click on debug and simulate using simulation as shown below 


 

## STM 32 CUBE PROGRAM :

```
#include "main.h"
#include <stdbool.h>
#include "lcd.h"

bool col1,col2,col3,col4;
void key();

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
int main(void)
{
  
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  while (1)
  {
    
	  key();
	  HAL_Delay(500);
  }
}
void key()
{
	Lcd_PortType ports[] = { GPIOA, GPIOA, GPIOA, GPIOA };
	Lcd_PinType pins[] = {GPIO_PIN_3, GPIO_PIN_2, GPIO_PIN_1, GPIO_PIN_0};
	Lcd_HandleTypeDef lcd;
	lcd = Lcd_create(ports, pins, GPIOB, GPIO_PIN_0, GPIOB, GPIO_PIN_1, LCD_4_BIT_MODE);

	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_RESET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);

	col1 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
	col2 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
	col3 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
	col4 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);

 	if(!col1)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd, "key7\n");
		col1=1;
	}
	if(!col2)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd, "key8\n");
			col2=1;
		}
	if(!col3)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd, "key9\n");
			col3=1;
		}
	if(!col4)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd, "key/\n");
			col4=1;
		}

	    HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_SET);
		HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_RESET);
		HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
		HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);
		col1 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
		col2 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
		col3 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
		col4 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);

		if(!col1)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd, "key4\n");
			col1=1;
		}
		if(!col2)
			{
				Lcd_cursor(&lcd,0,1);
				Lcd_string(&lcd, "key5\n");
				col2=1;
			}
		if(!col3)
			{
				Lcd_cursor(&lcd,0,1);
				Lcd_string(&lcd, "key6\n");
				col3=1;
			}
		if(!col4)
			{
				Lcd_cursor(&lcd,0,1);
				Lcd_string(&lcd, "key*\n");
				col4=1;
			}
 		    HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_SET);
				HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
				HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_RESET);
				HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);

				col1 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
				col2 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
				col3 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
				col4 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);

				if(!col1)
				{
					Lcd_cursor(&lcd,0,1);
					Lcd_string(&lcd, "key1\n");
					col1=1;
				}
				if(!col2)
					{
						Lcd_cursor(&lcd,0,1);
						Lcd_string(&lcd, "key2\n");
						col2=1;
					}
				if(!col3)
					{
						Lcd_cursor(&lcd,0,1);
						Lcd_string(&lcd, "key3\n");
						col3=1;
					}
				if(!col4)
					{
						Lcd_cursor(&lcd,0,1);
						Lcd_string(&lcd, "key-\n");
						col4=1;
					}
 				        HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_SET);
						HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
						HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
						HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_RESET);

						col1 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
						col2 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
						col3 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
						col4 =HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);
						if(!col1)
						{
							Lcd_cursor(&lcd,0,1);
							Lcd_string(&lcd, "keyON/ac\n");
							col1=1;
						}
						if(!col2)
							{
								Lcd_cursor(&lcd,0,1);
								Lcd_string(&lcd, "key0\n");
								col2=1;
							}
						if(!col3)
							{
								Lcd_cursor(&lcd,0,1);
								Lcd_string(&lcd, "key=\n");
								col3=1;
							}
						if(!col4)
							{
								Lcd_cursor(&lcd,0,1);
								Lcd_string(&lcd, "key+\n");
								col4=1;
							}
						HAL_Delay(500);

}
```

## Output screen shots of proteus  :

 LCD OFF:
 
 ![Screenshot 2024-04-16 070626](https://github.com/pragachellapillai/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/148254952/c28ce788-3e8d-47e5-aa7f-8605901e4324)

LCD PN:

![Screenshot 2024-04-16 070652](https://github.com/pragachellapillai/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/148254952/fdf98853-fb0a-4d18-8e36-d14844709d2c)


 ## CIRCUIT DIAGRAM (EXPORT THE GRAPHICS TO PDF AND ADD THE SCREEN SHOT HERE): 

 ![Screenshot 2024-04-16 070714](https://github.com/pragachellapillai/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/148254952/e4e47160-892c-4d70-8c84-89dc3fd9c016)

 
## Result :
Interfacing a 4x4 keypad with ARM microcontroller are simulated in proteus and the results are verified.
