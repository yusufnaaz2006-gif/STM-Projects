# Printing Using ITM Functionality

## Overview

This project demonstrates how to use ITM (Instrumentation Trace Macrocell) and SWV (Serial Wire Viewer) for debugging in STM32.

---

## ITM Send Character Function

```c
/////////////////////////////////////////////////////////////////////////////////////////////////////////
//                  Implementation of printf like feature using ARM Cortex M3/M4/ ITM functionality
//                  This function will not work for ARM Cortex M0/M0+
//                  If you are using Cortex M0, then you can use semihosting feature of openOCD
/////////////////////////////////////////////////////////////////////////////////////////////////////////

//Debug Exception and Monitor Control Register base address
#define DEMCR                  *((volatile uint32_t*) 0xE000EDFCU )

/* ITM register addresses */
#define ITM_STIMULUS_PORT0     *((volatile uint32_t*) 0xE0000000 )
#define ITM_TRACE_EN           *((volatile uint32_t*) 0xE0000E00 )

void ITM_SendChar(uint8_t ch)
{
    //Enable TRCENA
    DEMCR |= (1 << 24);

    //Enable stimulus port 0
    ITM_TRACE_EN |= (1 << 0);

    //Read FIFO status in bit[0]
    while(!(ITM_STIMULUS_PORT0 & 1));

    //Write character to ITM stimulus port0
    ITM_STIMULUS_PORT0 = ch;
}
```

---

## Features

* Real-time debugging using SWV
* No UART required
* Faster debug output
* Direct ARM Cortex-M register access

---

## Hardware Used

* STM32F446RE Nucleo Board
* ST-Link Debugger

---

## Software Used

* STM32CubeIDE
* SWV ITM Console

