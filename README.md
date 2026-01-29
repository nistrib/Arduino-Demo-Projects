# Arduino Demo Projects

A collection of Arduino projects demonstrating embedded systems programming, digital I/O control, state management, and interactive game logic. These projects showcase proficiency in C/C++ for microcontroller development.

## 🎮 Projects Overview

### 1. Rock-Paper-Scissors Game (`rps-program.ino`)
An interactive Rock-Paper-Scissors game implementation featuring Arduino AI opponent with memory capabilities.

### 2. Hardware Test Program (`wire-test-program.ino`)
A comprehensive hardware validation suite for testing LED outputs and button inputs.

---

## 🎲 Project 1: Rock-Paper-Scissors Game

### Features

#### Game Mechanics
- **Interactive Gameplay**: Human vs. Arduino opponent with button-based input
- **Visual Feedback**: LED patterns indicate game states, choices, and results
- **Memory System**: Arduino remembers up to 50 previous rounds
- **Customizable AI**: Modular `makeRPSChoice()` function for implementing different strategies

#### State Management
- **WaitingForRound**: All LEDs blinking - waiting for round start
- **WaitingForOpponent**: LED5 blinking - Arduino made choice, waiting for opponent
- **ArduinoWonRound**: LED1 solid - Arduino wins
- **OpponentWonRound**: LED5 solid - Opponent wins
- **Draw**: LED1 + LED5 solid - Tie game
- **Choice Display**: Visual representation of Rock/Paper/Scissors choices

#### Technical Highlights

**Advanced Bit Manipulation**
```cpp
// Double-bit LED pattern system
// 00 -> OFF, 01 -> BLINKING, 11 -> ON
enum LEDDoubleBitPattern {
  LED1_ON = 0b011 << 8,
  LED2_BLINKING = 0b001 << 6,
  ALL_LEDS_BLINKING = 0b0101010101
};
```

**Cyclic Game Logic**
```cpp
// Mathematical approach to RPS logic
// Rock=0, Paper=1, Scissors=2
// Winner = (choice + 1) % 3
RoundResult determineRoundResults(RPSChoice arduino, RPSChoice opponent){
  if (opponent == (arduino+1) % 3) return Round_Lost;
  else if (opponent == arduino) return Round_Draw;
  else return Round_Won;
}
```

**Non-Blocking Input with LED Animation**
```cpp
// Simultaneously blinks LEDs while monitoring button presses
int waitForBtnPressAndBlinkLEDs(int btns[], int btnCount, unsigned int ledPattern)
```

### Hardware Configuration

#### Components
- **5 LEDs**: Visual output for game states and choices
- **4 Buttons**: User input for game control and RPS selection

#### Pin Mapping
```cpp
// Buttons
BTN1 (Pin 3) → Paper
BTN2 (Pin 4) → Scissors
BTN3 (Pin 5) → Start Round
BTN4 (Pin 2) → Rock

// LEDs (Left to Right)
LED1 (Pin 12) → Arduino/Rock indicator
LED2 (Pin 11) → Rock display
LED3 (Pin 10) → Paper display
LED4 (Pin 9)  → Scissors indicator
LED5 (Pin 8)  → Opponent/Scissors indicator
```

### How to Play

1. **Start Round**: Press BTN3 (Start Round button)
2. **Arduino Chooses**: Arduino makes its choice (LEDs show the choice)
3. **Player Input**: Press your choice button (BTN4=Rock, BTN1=Paper, BTN2=Scissors)
4. **View Result**: LEDs display the winner
5. **End Round**: Press BTN3 again to proceed to next round

### Game Flow Diagram

```
[Press START] → [Arduino Chooses] → [Display Arduino Choice] 
     ↓
[Wait for Player Choice] → [Determine Winner] → [Display Result]
     ↓
[Save Round Data] → [Press START to Continue]
```

### Extensibility

The AI decision-making is isolated in the `makeRPSChoice()` function, allowing easy implementation of different strategies:

- **Random Strategy**: Use `randRPS()` for unpredictable choices
- **Counter-Pick Strategy**: Analyze opponent patterns and counter
- **Frequency Analysis**: Track most-used opponent choice
- **Adaptive Learning**: Adjust based on win/loss history

```cpp
RPSChoice makeRPSChoice(){
  // Access to game history:
  // - arduinoChoices[] : Previous Arduino choices
  // - opponentChoices[] : Previous opponent choices
  // - roundResults[] : Previous round outcomes
  // - roundsPlayed : Number of rounds completed
  
  // Implement your strategy here
  return RPS_Paper; // Current: always plays Paper
}
```

---

## 🔧 Project 2: Hardware Test Program

### Purpose
Validates correct wiring and functionality of the Arduino hardware setup before running complex programs.

### Test Cases

#### Test Case 1: LED Sequential Test
**Purpose**: Verify all LEDs are properly connected and functioning

**Behavior**:
1. All LEDs turn on simultaneously (1 second)
2. LEDs turn off sequentially from left to right (1 second each)
3. Validates LED polarity and pin connections

```cpp
// All LEDs on → LED1 → LED2 → LED3 → LED4 → LED5 → Repeat
```

#### Test Case 2: Button-LED Mapping Test
**Purpose**: Verify button inputs are correctly wired and debounced

**Behavior**:
- Initial state: LED5 blinking
- BTN1 press → LED1 turns on
- BTN2 press → LED2 turns on
- BTN3 press → LED3 turns on
- BTN4 press → LED4 turns on
- Validates button debouncing and input detection

### Configuration

Change test case by modifying:
```cpp
const int TEST_CASE = 1;  // Set to 1 or 2
```

### Why This Matters

Hardware testing is critical in embedded systems:
- **Prevents Logic Errors**: Ensures code failures aren't due to wiring issues
- **Saves Debug Time**: Quickly identifies hardware problems
- **Validates Connections**: Confirms all components are operational
- **Professional Practice**: Standard procedure in embedded development

---

## 🛠️ Technical Skills Demonstrated

### Embedded C/C++ Programming
- **Strong typing** with enums for state management
- **Bit manipulation** for efficient LED control
- **Modular design** with reusable functions
- **Memory management** with fixed-size arrays
- **Const correctness** for configuration values

### Digital I/O Control
- **pinMode()** configuration for inputs/outputs
- **digitalWrite()** for LED control
- **digitalRead()** for button sensing
- **Pull-up resistor** understanding (BTN_PRESSED_SIGNAL = LOW)

### Timing & Interrupts
- **Non-blocking delays** using `millis()`
- **Custom delay functions** (`millisDelay()`)
- **Interval-based LED blinking** without `delay()`
- **Debounce handling** for button presses

### State Machine Design
- **Enum-based states** for clear state definition
- **State transitions** based on user input
- **Visual state feedback** via LED patterns

### Algorithm Implementation
- **Cyclic logic** using modulo arithmetic
- **Bit shifting** for pattern generation
- **Boolean logic** for bit testing
- **Data structure** design for game history

---

## 📋 Getting Started

### Requirements
- **Arduino Board** (Uno, Nano, Mega, etc.)
- **5 LEDs** + appropriate resistors (220Ω - 1kΩ)
- **4 Push Buttons** (momentary, normally open)
- **Breadboard** and jumper wires
- **Arduino IDE** (1.8.x or 2.x)

### Installation

1. **Hardware Setup**:
   - Connect LEDs to pins 12, 11, 10, 9, 8 (with current-limiting resistors)
   - Connect buttons to pins 2, 3, 4, 5 (with pull-down resistors or use internal pull-ups)
   - Connect common ground

2. **Upload Code**:
   ```bash
   # Open Arduino IDE
   # Load wire-test-program.ino first
   # Upload to board
   # Verify all LEDs and buttons work
   
   # Then load rps-program.ino
   # Upload and enjoy!
   ```

3. **Serial Monitor**:
   - Open Serial Monitor (Ctrl+Shift+M)
   - Set baud rate to 9600
   - View game state messages and debug info

### Wiring Diagram

```
Arduino Pin → Component
----------------------------
Pin 12 → LED1 (+ resistor) → GND
Pin 11 → LED2 (+ resistor) → GND
Pin 10 → LED3 (+ resistor) → GND
Pin 9  → LED4 (+ resistor) → GND
Pin 8  → LED5 (+ resistor) → GND

Pin 3  → Button 1 → GND
Pin 4  → Button 2 → GND
Pin 5  → Button 3 → GND
Pin 2  → Button 4 → GND
```

---

## 💡 Code Highlights

### Smart Bit Manipulation
```cpp
bool isBitHigh(long n, int pos){
  unsigned long bt = 1 << pos;
  return ((n & bt) != 0);
}
```

### Efficient Pattern Decoding
```cpp
void setLEDsToDoubleBitPattern(unsigned int pattern, int blinkingLEDValue){
  unsigned int mask = 0b11;
  for(int ledPos = 0; ledPos < LED_COUNT; ledPos++){
    unsigned int curLedBits = (pattern >> (ledPos*2)) & mask;
    // Process 2-bit pattern for each LED
  }
}
```

### Non-Blocking Multi-Function Loop
```cpp
// Simultaneously:
// - Checks multiple buttons
// - Blinks LEDs at fixed intervals
// - Returns on first button press
int waitForBtnPressAndBlinkLEDs(int btns[], int btnCount, unsigned int ledPattern)
```

---

## 🎯 Learning Outcomes

### Core Concepts Mastered
✅ Digital I/O programming (GPIO control)  
✅ State machine implementation  
✅ Non-blocking timing patterns  
✅ Bit manipulation for efficiency  
✅ Debouncing techniques  
✅ Modular code architecture  
✅ Hardware-software integration  
✅ Debugging with Serial output  

### Best Practices Applied
✅ Meaningful variable/function names  
✅ Comprehensive inline documentation  
✅ Const correctness for configuration  
✅ Separation of concerns (logic vs. I/O)  
✅ Extensible design patterns  
✅ Hardware validation procedures  

---

## 🔮 Future Enhancements

### Game Features
- [ ] Add difficulty levels (Easy/Medium/Hard AI)
- [ ] Implement score tracking across sessions (EEPROM)
- [ ] Add sound effects using piezo buzzer
- [ ] Multi-player mode (2 humans vs. Arduino referee)
- [ ] LCD display for detailed game information

### Technical Improvements
- [ ] Port to ESP32 for WiFi multiplayer
- [ ] Add Bluetooth connectivity for mobile app control
- [ ] Implement advanced AI using pattern recognition
- [ ] Create configuration menu via Serial commands
- [ ] Add statistics tracking and analysis

### Hardware Additions
- [ ] RGB LEDs for colorful feedback
- [ ] 7-segment display for score
- [ ] Rotary encoder for menu navigation
- [ ] SD card logging of all games

---

## 📝 Notes

These programs were developed as demonstration projects to showcase embedded systems programming capabilities. The code emphasizes:
- **Clean, maintainable architecture**
- **Efficient resource usage**
- **Professional documentation**
- **Hardware-first development** (test before implementing logic)

The modular design allows easy adaptation for different Arduino boards and hardware configurations.

---

## 🙏 Acknowledgments

- Arduino community for excellent documentation and examples
- Embedded systems best practices from industry standards
- Built with attention to real-world embedded development workflows

---

## 📄 License

These projects are created for educational and demonstration purposes.

---

**Author**: Daniil Nistribenko
**Purpose**: Arduino Skills Demonstration  
**Hardware**: Arduino Uno/Nano compatible boards
