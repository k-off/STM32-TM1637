# TM1637 7-segment 4-digit display library for STM32 MCU

![display image](https://i.ebayimg.com/images/g/BtQAAOSwX3BfHJdf/s-l300.jpg)

 * This library provides functionality for communication between a STM32F4 MCU and a 7-segment 4-digit display based on TM1637 chip.

 * It basically implements a minimal I2C protocol, that is enough for sending data to a slave device. It doesn't read from slave and ignores ACK bits.

 * It is aimed for cases when one wants to be more flexible with selection of CLK/DIO pins, or when I2C pins provided by the MCU are already busy.

## Added Crawler function

It formats the string as printf does, and shows it on the display:

![gif](GIF.gif)

## Example:
Switch to the example branch to see a complete STM-IDE project

## Usage:
```
#include "TM1637.h"
int main(void)
{
  SystemClock_Config();
  volatile uint16_t s = 0;
  uint8_t text[] = "0123456789ABCDEFG";
  uint8_t textSize = sizeof(text) - 4;
  uint8_t *ptr = text;             // used for ticker/crawler effect
  TM1637Init(GPIOA, GPIOA, 1, 2);  // works with any combination of two distinct available pins
  TM1637SetBrightness(Brightness1);
  while (1)
  {
    TM1637Crawl(750, 375, "Hello world, its %04d pm", 420); // rolls through the message and shows it char by char
    //TM1637DisplayText(ptr, textSize);	// display text
    //HAL_Delay(500);
    //TM1637DisplayNumber(s, s&1);       // display number with and without delimiter
    //HAL_Delay(500);
    ++ptr;                            // move beginning of the displayed text to the next character
    ++s;
    if (s > textSize) {s = 0; ptr = text;} //start from beginning
  }
  return 0;
}
```
