## Controls LED colors in port F with the help on on board switches while running the DC motor with nominal speed in clockwise direction

## SysTick Init

```c
void SysTick_Init(unsigned long period){
    // ? disabling systick timer before setup
    NVIC_ST_CTRL_R = 0;
    // ? -1 because counter goes till 0
    NVIC_ST_RELOAD_R = period - 1;
    NVIC_ST_CURRENT_R = 0;
    // ? priority 0
    NVIC_SYS_PRI3_R = (NVIC_SYS_PRI3_R & 0x00FFFFFF);
    // ? enable systick with interrupts
    NVIC_ST_CTRL_R = 0x07;
}
```

## SysTick Handler

```c
void SysTick_Handler(void)
{
    // ? toggle Pb4 after a time period of 10ms
    GPIO_PORTB_DATA_R ^= 0x01;
}
```

## Main Program

```c
int main(void)
{
    int i;
    PLL_Init();
    // ? Initialize systick with a reload of 10msec
    SysTick_Init(800000);
    PortF_Init();
    PortB_Init();
    while (1)
    {
        // ? Reading inputs from switches
        Switch = GPIO_PORTF_DATA_R;
        Switch = GPIO_PORTF_DATA_R & 0x11;
        if (Switch == 0x11)
            GPIO_PORTF_DATA_R = 0x02;
        else if (Switch == 0x01)
            GPIO_PORTF_DATA_R = 0x08;
        else if (Switch == 0x10)
            GPIO_PORTF_DATA_R = 0x04;
        else if (Switch == 0x00)
            GPIO_PORTF_DATA_R = 0x0E;
    }
}

```
