## DEN DIGITAL ENABLE

- 1 for using a pin as digital
- 0 otherwise

## SYSCTL_RCGC2_R

- LSB - Port A
- Move a bit towards left go to the next alphabet

## DIR DIRECTION REGISTER

- 1 for output
- 0 for input

## PCTL Port control

- contains 32 bits
- LSN - Least Significant nibble for port 0
- load `F` for the port you are using
- ex: using porta2 `GPIO_PORTA_PCTL_R &= ~ 0x00000F00`

## AFSEL Alternative fucntion select

- ex: `GPIO_PORTA_AFSEL_R = ~0x20` for porta5

## AMSEL

- ex: `GPIO_PORTA_AMSEL_R &= ~0x20` for porta5

## PortF_Init

```
void PortF_Init(){
    volatile long delay;
    SYSCTL_RCGC2_R |= 0x00000020;
    delay = SYSCTL_RCGC2_R;
    GPIO_PORTF_LOCK_R = 0x4C4F434B;
    GPIO_PORTF_PUR_R = 0X11;
    GPIO_PORTF_CR_R = 0X1F;
    GPIO_PORTF_DEN_R = 0X1F;
    GPIO_PORTF_DIR_R = 0X0E;
    GPIO_PORTF_AFSEL_R = 0X00;
    GPIO_PORTF_AMSEL_R = 0X00;
    GPIO_PORTF_PCTL_R = 0X00000000;
}
```
