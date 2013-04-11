ArduinoTimer
============

ArduinoTimer is a library, which aims at providing developers with means to schedule asynchronous timer execution, pretty much like timer services provided by real OSes. In fact, it is a set of libraries, one for each timer available in ATmega2560 (1, 3, 4 and 5). By dividing the full library into four smaller libraries, the developer can save both RAM and Flash memory.

Each library has five functions:
- void startTimerNNN(unsigned long microsecondsInterval): Starts the timer, and schedules the first notification. On 16 MHz Arduino boards, this function has a resolution of 4us, for intervals <= 260000, and a resolution of 16us for other intervals. On 8 MHz Arduino boards, this function has a resolution of 8us, for intervals <= 520000, and a resolution of 32us for other intervals. 
- void resetTimerNNN(): Resets the timer's counter. This function should be called as soon as a notification is received, in order to properly prepare the timer for the next notification.
- void pauseTimerNNN(): Pauses the timer, preventing any further notifications.
- void resumeTimerNNN(): Resumes the timer.
- unsigned int readTimerNNN(): Returns the current value of the timer's counter.

Where NNN is 1, 3, 4 or 5, depending on the chosen library.

There are also two other functions common to all libraries:
- disableMillis(): Disables Arduino's default millisecond counter, rendering millis(), micros(), delay() and delayMicroseconds() useless, while saving some processing power.
- enableMillis(): Enables Arduino's default millisecond counter.

In order to receive the notifications, an interrupt handler must be setup as shown below:
ISR(timerNNNEvent)
{
  resetTimerNNN();
  // Rest of code
} 

