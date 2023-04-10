## Generate a contol sequence at PE0-3 to rotate the stepper motor 180deg in clock wise direction, followed by a pause for 1sec and anticlock wise by 180deg

## Calculating Step angle

- Step angle = 360 / S
- S = m \* no of teeth
- m = 1 for uni polar and m = 2 for bipolar
- ### Step angle for the motor given to us S = 360 / 2 \* 100 = 1.8deg
- No of steps required for 90deg rotation = 1.8 \* 50

## Sequences for rotating

- ### Clockwise - 5, 6, A, 9
- ### Anti clockwise - 9, A, 6, 5

## Main program

```c
int i = 0;
int main(void)
{
    PLL_Init();
    PortD_Init();
    SysInit();

    // ? 0 to 180deg clockwise
    for (i = 0; i < 12; i++)
    {
        GPIO_PORTD_DATA_R = 0x05;
        SysLoad(800000);
        GPIO_PORTD_DATA_R = 0x06;
        SysLoad(800000);
        GPIO_PORTD_DATA_R = 0x0A;
        SysLoad(800000);
        GPIO_PORTD_DATA_R = 0x09;
        SysLoad(800000);
    }
    GPIO_PORTD_DATA_R = 0x05;
    SysLoad(800000);
    GPIO_PORTD_DATA_R = 0x06;
    SysLoad(800000);

    // ? 1 sec khaali
    SysLoad(16000000);
    SysLoad(16000000);
    SysLoad(16000000);
    SysLoad(16000000);
    SysLoad(16000000);

    // ? 0 to 180deg anti clockwise
    for (i = 0; i < 12; i++)
    {
        GPIO_PORTD_DATA_R = 0x09;
        SysLoad(800000);
        GPIO_PORTD_DATA_R = 0x0A;
        SysLoad(800000);
        GPIO_PORTD_DATA_R = 0x06;
        SysLoad(800000);
        GPIO_PORTD_DATA_R = 0x05;
        SysLoad(800000);
    }
    GPIO_PORTD_DATA_R = 0x09;
    SysLoad(800000);
    GPIO_PORTD_DATA_R = 0x0A;
    SysLoad(800000);
}
```
