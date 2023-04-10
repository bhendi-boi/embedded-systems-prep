## Generate a control signal sequence at PA3 & PA7 to perform the following

- Switch Direction Speed
- SW1 clock 50%
- SW2 anticlock 75%
- Both clock 90%
- None motor off

## Main Program

```c
unsigned long int sw;
int main(void)
{
    PLL_Init(); // 80 MHz
    SysInit();
    PortA_Init();
    PortF_Init();

    while (1)
    {
        // ? taking input
        sw = GPIO_PORTF_DATA_R;
        if (sw == 0x01)
        {
            GPIO_PORTA_DATA_R |= (0x08);
            SysLoad(400000); // wait 5ms
            GPIO_PORTA_DATA_R &= ~(0x08);
            SysLoad(400000); // wait 5ms
        }
        else if (sw == 0x10)
        {
            GPIO_PORTA_DATA_R |= (0x80);
            SysLoad(600000); // wait 7.5ms
            GPIO_PORTA_DATA_R &= ~(0x80);
            SysLoad(200000); // wait 2.5ms
        }
        else if (sw == 0x00)
        {
            GPIO_PORTA_DATA_R |= (0x08);
            SysLoad(720000); // wait 9ms
            GPIO_PORTA_DATA_R &= ~(0x08);
            SysLoad(80000); // wait 1ms
        }
        else if (sw == 0x11)
        {
            GPIO_PORTA_DATA_R |= (0x00);
        }
    }
}

```
