# ECE 528 Homework 2
## Section I: Review Questions

1. **What is a hardware timer? What kinds of tasks do embedded systems commonly use hardware timers for?**  
   A hardware timer is a peripheral used to generate periodic events or measure time intervals. In embedded systems, they can be used to create delays or trigger interrupts.
   
2. **What is a clock signal used for in embedded systems? List the primary clock sources available on the MSP432P401R microcontroller and briefly describe each clock source.**  
   A clock signal is used to synchronize operations within a system. For the MSP432P401R microcontroller, there is the auxiliary clock, master clock, subsystem master clock, low-speed subsytem master clock, and low-speed backup domain clock.
   
3. **Consider a 16-bit timer (e.g. Timer_A configured in up mode). If it counts upward, what is the maximum count value before it rolls over? Show your work.**
   The maximum count value before rollover is 0xFFFF.  
   2^16 - 1 = 65,536 -> 0xFFFF  
4. **If a 24-bit timer counts upward from zero at 10 MHz, what is its overflow period? Show your work.**  
   The overflow period is 1.68 s.  
   T = (2^24)/(10^6) = 1.68 s  
5. **Describe the SysTick timer on the MSP432P401R microcontroller. Provide two example applications for SysTick in embedded systems. Which clock sources can the SysTick timer use and how can they be selected?**  
   The SysTick timer is a 24-bit timer that can be used to generate periodic interrupts. In embedded systems, the SysTick timer can be used as a counter or can delay functions. The SysTick timer uses the CPU's free running clock which can be selected when CLKSOURCE = 1.
   
6. **How many registers does the SysTick timer have on the MSP432P401R microcontroller and what is the purpose of each? Name them and provide a short description per register.**
   The SysTick timer has 4 registers. STCSR is the SysTick control and status register, STRVR is the SysTick reload value register, STCVR is the SysTick current value register, and STCR is the SysTick calibration value register.
   
7. **Describe the Timer_A peripheral on the MSP432P401R microcontroller. List the main operating modes of Timer_A and briefly explain each.**
   The Timer_A peripheral is a 16-bit timer with four operating modes. Stop mode means the timer is halted. Up mode means the timer counts from 0 to TAxCCR0, then resets to 0. Continuous mode means the timer counts from 0 to 0xFFFF. In Up/Down mode, the timer counts from 0 to TAxCCR0m then back to 0.  
   
8. **Briefly state the purpose of the following Timer_A registers.**  
- TAxCTL
- TAxCCTLn
- TAxR
- TAxCCRn
- TaxIV
- TAxEX0  
   The TAxCTL register configures the operation of Timer_A, TAxCCTLn controls the Timer_A output behavior, and TAxR contains the Timer_A count. TAxCCRn has two modes: compare mode and capture mode. In compare mode, it contains the value compared against a register. In capture mode, TAxR is copied into TAxCCRn when a capture occurs. The register TaxIV is used for interrupts, and TAxEX0 provides additional clock division.
  
9. **What is PWM? Provide at least three example applications that use PWM in embedded systems.**  
  PWM is a technique used to vary the amount of power delivered to a device. In embedded systems, PWM can be used to adjust an LED's brightness, control the speed of motors, and for audio generation.

10. **How many Timer_A modules are available and how many PWM outputs can each module provide?**  
    There are 4 Timer A modules, and each module provides 5 PWM outputs.

11. **Describe the steps needed to initialize P2.4 as a PWM output driven by Timer_A0 CCR1 (TA0.1).**
    ```c
      void Timer_A0_PWM_Init(uint16_t period_constant, uint16_t duty_cycle_1, uint16_t duty_cycle_2)
      {
          // Return immediately if either duty cycle values are greater than
          // or equal to the given period_constant
          if (duty_cycle_1 >= period_constant) return;
          
          // Configure pin P2.4 (PM_TA0.1) to use peripheral function mode
          // by setting Bit 4 of the SEL0 register for P2
          // and clearing Bit 4 of the SEL1 register for P2
          P2->SEL0 |= 0x10;
          P2->SEL1 &= ~0x10;
      
          // Configure pin P2.4 as an output GPIO pin to drive the PWM signals
          // Set Bit 4 of the DIR register for P2
          P2->DIR |= 0x10;
      
          // Set the Timer A0 Capture/Compare register to the specified period_constant
          // Assign the value of period_constant to the CCR[0] register
          TIMER_A0->CCR[0] = period_constant;
      
          // Configure the Timer A0 expansion register to divide the clock frequency by 1
          // Clear all bits of the EX0 register
          TIMER_A0->EX0 = 0x0000;
      
          // Configure the output mode as Toggle / Reset for CCR[1]
          // Set the bits of the OUTMOD field of the CCTL[1] register to 010b
          TIMER_A0->CCTL[1] |= 0x0040;
      
          // Assign the value of duty_cycle_1 to the CCR[1] register
          // Duty Cycle %: duty_cycle_1 / period_constant
          TIMER_A0->CCR[1] = duty_cycle_1;
      
          // Modify the following bits in the CTL register
          // Select SMCLK = 12 MHz as timer clock source
          // Set ID = 1 (Divide timer clock by 2)
          // Set MC = 3 (Up/Down Mode)
          TIMER_A0->CTL = 0x0270;
      }
    ```
12. ** **
    ```c
       void Timer_A0_Interrupt_Init(void(*task)(void), uint16_t period)
       {
          // Store the user-defined task function for use during interrupt handling
          Timer_A0_Task = task;
      
          // Halt Timer A0 by clearing the MC bits in the CTL register
          TIMER_A0->CTL &= ~0x0030;
      
          // In the CTL register, set the TASSEL and ID bits
          TIMER_A0->CTL |= 0x0200;
      
          // Enable interrupt request of the
          // corresponding Capture/Compare interrupt flag
          TIMER_A0->CCTL[0] |= 0x0010;
      
          // Store the period in the CCR0 register
          TIMER_A0->CCR[0] = (period - 1);
      
          // Divide the SMCLK frequency by 1
          TIMER_A0->EX0 = 0x0000;
      
          // Interrupt priority level = 2
          // Timer A0 has an IRQ number of 8
          NVIC->IP[2] = (NVIC->IP[2] & 0xFFFFF00F) | 0x00000060;
      
          // Enable Interrupt 8 in NVIC
          NVIC->ISER[0] |= 0x00000100;
      
          TIMER_A0->CTL |= 0x0014;
      }
```
