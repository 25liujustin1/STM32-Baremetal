# STM32 Bare-Metal

Firmware for the STM32F446RE (Nucleo-F446RE) written against the chip's registers instead of ST's HAL, one peripheral at a time. Each folder is a separate STM32CubeIDE project.

## Projects

In order:

| Project | What it does |
|---|---|
| [`Blinky`](Blinky/Src/main.c) | Basically Hello World lol |
| [`baremetal_template`](baremetal_template/Src/main.c) | Takes care of setup |
| [`blinky_cmsis`](blinky_cmsis/Src/main.c) | LED blink on PA5 through the CMSIS register definitions |
| [`systick_timer`](systick_timer/Src/main.c) | The same blink with an accurate millisecond delay from the SysTick timer |
| [`timer2`](timer2/Src/main.c) | Copy of `systick_timer` used as the starting point for TIM2 |
| [`timer_interrupt`](timer_interrupt/Src/main.c) | LED toggled from a TIM2 update interrupt |
| [`Usart2`](Usart2/Src/main.c) | USART2 at 115200 baud, transmitting a character over the ST-LINK virtual COM port |
| [`usart2_echo`](usart2_echo/Src/main.c) | Reads a line over USART2 and echoes it back, with `printf` retargeted to the UART |
| [`uart_adc`](uart_adc/Src/main.c) | Polls ADC1 on PA0 and prints each reading over USART2 |
| [`adc_dma`](adc_dma/Src/main.c) | ADC1 -> DMA2 Stream 0 into a double-buffered circular buffer, streamed over USART2 |

## Building

Open this folder as a workspace in STM32CubeIDE and import the projects (File → Import → Existing Projects into Workspace). The register-level projects use the CMSIS headers from the STM32Cube F4 firmware package (`STM32Cube_FW_F4_V1.28.0`), so install it through the IDE first and update the include paths if it lives somewhere else on your machine.