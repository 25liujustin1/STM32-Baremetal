# STM32 Bare-Metal

Firmware for the STM32F446RE (Nucleo-F446RE) written against the chip's registers instead of ST's HAL, one peripheral at a time. Each folder is a separate STM32CubeIDE project.

The main result is [`adc_dma`](adc_dma/Src/main.c): the ADC converts continuously, DMA copies every sample into a 1000-sample circular buffer, and the half-transfer and transfer-complete interrupts let the CPU stream one half over UART while DMA fills the other. On the PC side, a small Python script plots the stream live with Matplotlib.

## Projects

In the order I built them:

| Project | What it does |
|---|---|
| [`blinky_v2`](blinky_v2/Core/Src/main.c) | LED blink using ST's HAL, as a baseline to compare against |
| [`baremetal_template`](baremetal_template/Src/main.c) | Empty starting point for the register-level projects |
| [`blinky_cmsis`](blinky_cmsis/Src/main.c) | LED blink on PA5 through the CMSIS register definitions, with a busy-wait delay |
| [`systick_timer`](systick_timer/Src/main.c) | The same blink with an accurate millisecond delay from the SysTick timer |
| [`timer2`](timer2/Src/main.c) | Copy of `systick_timer` used as the starting point for TIM2 |
| [`timer_interrupt`](timer_interrupt/Src/main.c) | LED toggled from a TIM2 update interrupt, with an empty main loop |
| [`Usart2`](Usart2/Src/main.c) | USART2 at 115200 baud, transmitting a character over the ST-LINK virtual COM port |
| [`usart2_echo`](usart2_echo/Src/main.c) | Reads a line over USART2 and echoes it back, with `printf` retargeted to the UART |
| [`uart_adc`](uart_adc/Src/main.c) | Polls ADC1 on PA0 and prints each reading over USART2 |
| [`adc_dma`](adc_dma/Src/main.c) | ADC1 → DMA2 Stream 0 into a double-buffered circular buffer, streamed over USART2 |
| [`Blinky`](Blinky/Src/main.c) | LED blink with my own register address definitions, without the CMSIS header |

## Building

Open this folder as a workspace in STM32CubeIDE and import the projects (File → Import → Existing Projects into Workspace). The register-level projects use the CMSIS headers from the STM32Cube F4 firmware package (`STM32Cube_FW_F4_V1.28.0`), so install it through the IDE first and update the include paths if it lives somewhere else on your machine.

## Trying `uart_adc` or `adc_dma`

1. Wire a potentiometer between 3V3 and GND, with the wiper on A0 (PA0).
2. Flash the project and open the board's ST-LINK virtual COM port at 115200 baud (8N1).
3. Turning the knob sweeps the readings from about 0 to 4095.

The ADC in `adc_dma` samples much faster than UART can send text, so the stream is a series of snapshots rather than every sample. Triggering the ADC from a timer at a fixed rate is the next step.
