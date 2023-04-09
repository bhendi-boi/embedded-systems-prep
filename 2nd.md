# Timers

## PLL

Micro controllers have a PLL and it allows us to change the clock frequency of the micro controller through software.

- PLL produces **80** MHz clock

## Systick Timer

- uses 24 bit register
- used to create time delays and generate perodic interrupts
- configured using 3 registers
  - **STCTRL** Systick control and status
  - **STRELOAD** Systick reload register
  - **STCURRENT** Systick currentvalue register
- Reload value = desired time delay / 12.5 ns

## Initialization

- Turn off the SysTick by clearing ENABLE in STCTRL

```c
    void SysInit(void)
    {
        NVIC_ST_CTRL_R = 0;
        NVIC_ST_CURRENT_R = 0;
        NVIC_ST_CTRL_R = 0x00000005;
    }
```

- 5 represents 101
  - 1 for CLK_SRC so the system clock is used
  - 0 for INTEN to disable interrupts
  - 1 for ENABLE to start SysTick timer

## Using SysTick

```c
    void SysLoad(unsigned long peroiod)
    {
        // ? period -1 cuz counter goes til 0
        NVIC_ST_RELOAD_R = peroiod - 1;
        NVIC_ST_CURRENT_R = 0;
        // ? when 16th bit is 1 => counter completed a cycle
        while ((NVIC_ST_CURRENT_R & 0x00010000) == 0)
        {
        }
    }
```
