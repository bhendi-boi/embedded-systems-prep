## Port Initialzations

### PortF

```c
void PortF_Init(void){
  volatile unsigned long delay;
  SYSCTL_RCGC2_R |= 0x00000020;     // 1) activate clock for Port F
  delay = SYSCTL_RCGC2_R;           // allow time for clock to start
  GPIO_PORTF_LOCK_R = 0x4C4F434B;   // 2) unlock GPIO Port F
  GPIO_PORTF_CR_R = 0x1F;           // allow changes to PF4-0
  // only PF0 needs to be unlocked, other bits can't be locked
  GPIO_PORTF_AMSEL_R = 0x00;        // 3) disable analog on PF
  GPIO_PORTF_PCTL_R = 0x00000000;   // 4) PCTL GPIO on PF4-0
  GPIO_PORTF_DIR_R = 0x0E;          // 5) PF4,PF0 in, PF3-1 out
  GPIO_PORTF_AFSEL_R = 0x00;        // 6) disable alt funct on PF7-0
  GPIO_PORTF_PUR_R = 0x11;          // enable pull-up on PF0 and PF4
  GPIO_PORTF_DEN_R = 0x1F;          // 7) enable digital I/O on PF4-0
}
```

### PortD

```c
void PortD_Init(void){
    volatile unsigned long delay;
    SYSCTL_RCGC2_R |= 0x00000008; // 1) activate clock for Port D
    delay = SYSCTL_RCGC2_R; // allow time for clock to start
    GPIO_PORTD_AMSEL_R &= ~(0x09); // 3) disable analog on PD0 and PD3
    GPIO_PORTD_PCTL_R &= ~(0x09); // 4) PCTL GPIO on PD0 and PD3
    GPIO_PORTD_DIR_R |= 0x09; // 5) enable PD0 and PD3 as output
    GPIO_PORTD_AFSEL_R&= ~(0x09); // 6) disable alt funct on PD0 and PD3
    GPIO_PORTD_DEN_R |= 0x09; // 7) enable digital I/O on PD0 and PD3
 }

```

## Toggle onboard LED (R or B or G). Observe the waveforms in debug mode

### Delay Function

```c
void Delay(){
    unsigned long volatile time = 145448;
    while(time){
        time--;
    }
}
```

## Interface SW1 and control LED(R or B or G). Observe the waveforms in debug mode

### Colors

- #### Red 02
- #### Blue 04
- #### Green 08

### Main Program

```c
int main(){
    PortF_Init();
    while(1){
        Switch = GPIO_PORTF_DATA_R & 0x11;
        if(Switch == 0x01){
            GPIO_PORTF_DATA_R = 0x02;
        }else{
            GPIO_PORTF_DATA_R = 0x00;
        }
    }
}
```

## On board Switch and on board LED

| Switch1 | Switch2 | Color |
| :-----: | :-----: | :---: |
|    1    |    1    |  Red  |
|    0    |    1    | Green |
|    1    |    0    | Blue  |
|    0    |    0    | White |

### Main Program

```c
int main(){
    PortF_Init();
    while(1){
        Switch = GPIO_PORTF_DATA_R & 0x11;
        if(Switch == 0x01){
            GPIO_PORTF_DATA_R = 0x08;
        }else if(Switch == 0x10){
            GPIO_PORTF_DATA_R = 0x04;
        }else if(Switch == 0x11){
            GPIO_PORTF_DATA_R = 0x02;
        }else{
            GPIO_PORTF_DATA_R = 0x0E;
        }
    }
}
```

## To control external LED using on board Switch

| Switch1 | Switch2 |        Color        |
| :-----: | :-----: | :-----------------: |
|    1    |    1    |         No          |
|    0    |    1    |       Port D3       |
|    1    |    0    |       Port D0       |
|    0    |    0    | Port D3 and Port D0 |

### Main Program

```c
int main(){
    PortF_Init();
    PortD_Init();
    while(1){
        Sw = GPIO_PORTF_DATA_R & 0x11;
        if(Sw == 0x01){
            GPIO_PORTD_DATA_R = 0x08;
        }else if(Sw == 0x10){
            GPIO_PORTD_DATA_R = 0x01;
        }else if(Sw == 0x00){
            GPIO_PORTD_DATA_R = 0x09;
        }else{
            GPIO_PORTD_DATA_R = 0x00;
        }
    }
}
```
