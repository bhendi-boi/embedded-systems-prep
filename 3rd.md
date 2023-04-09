## Question

Generate a PWM at PB4 to rotate the servo motor from 00
to 1800 and 1800 to 00.

- 0deg ~ 0.7ms
- 180deg ~ 2.3ms

## Main Program

```c
int main(void)
{
    // ? Initializing clock
    PLL_Init();
    PortA_Init();
    SysInit();
    GPIO_PORTA_DATA_R = 0x20;
    while (1)
    {
        // ?  0 deg to 90 deg
        for (i = 5600; i <= 120000; i += 1000)
        {
            GPIO_PORTA_DATA_R = 0x20;
            SysLoad(i);
            GPIO_PORTA_DATA_R = 0x00;
            SysLoad(1600000 - i);
        }
        // ?  180 deg to 0 deg
        for (i = 184000; i >= 56000; i -= 1000)
        {
            GPIO_PORTA_DATA_R = 0x20;
            SysLoad(i);
            GPIO_PORTA_DATA_R = 0x00;
            SysLoad(1600000 - i);
        }
    }
}
```
