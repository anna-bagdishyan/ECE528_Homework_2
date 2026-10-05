# ECE 528 Homework 2
## Section I: Review Questions

1. **What is a hardware timer? What kinds of tasks do embedded systems commonly use hardware timers for?**  
   A hardware timer is a peripheral used to generate periodic events or measure time intervals. In embedded systems, they can be used to create delays or trigger interrupts.
   
2. **What is a clock signal used for in embedded systems? List the primary clock sources available on the MSP432P401R microcontroller and briefly describe each clock source.**  
   A clock signal is used to synchronize operations within a system. For the MSP432P401R microcontroller, there is the auxiliary clock, master clock, subsystem master clock, low-speed subsytem master clock, and low-speed backup domain clock.
   
3. **Consider a 16-bit timer (e.g. Timer_A configured in up mode). If it counts upward, what is the maximum count value before it rolls over? Show your work.**
   The maximum count value before rollver is 0xFFFF.  
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
   The TAxCTL register configures the oepration of Timer_A, TAxCCTLn controls the Timer_A output behavior, and TAxR contains the Timer_A count. TAxCCRn has two modes: compare mode and capture mode. In compare mode, it contains the value compared against a register. In capture mode, TAxR is copied into TAxCCRn when a capture occurs. The register TaxIV is used for interrupts, and TAxEX0 provides additional clock division.
  
9. **What is PWM? Provide at least three example applications that use PWM in embedded systems.**  
  PWM is a technique used to vary the amount of power delivered to a device. In embedded systems, PWM can be used to adjust an LED's brightness, control the speed of motors, and for audio generation.

10. **How many Timer_A modules are available and how many PWM outputs can each module provide?**  
    There are 4 Timer A modules, and each module provides 5 PWM outputs.

11. 
