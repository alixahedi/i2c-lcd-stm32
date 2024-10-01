# I2C LCD Library for STM32

This is a simple and efficient I2C LCD library for STM32 microcontrollers, designed to control one or multiple I2C LCD displays. The library is object-oriented, allowing you to manage multiple LCDs simultaneously by creating instances for each display.

## Features

- **Multiple LCD Support**: Control multiple LCDs by creating separate instances.
- **4-bit Mode Communication**: Uses 4-bit mode to communicate with the LCD.
- **Customizable I2C Addresses**: Specify different I2C addresses for each LCD.
- **Basic LCD Functions**: Initialize, clear display, set cursor position, display strings and characters.
- **Easy Integration**: Simple API for quick integration into your STM32 projects.

## Table of Contents

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
  - [Initialization](#initialization)
  - [Displaying Text](#displaying-text)
  - [Example](#example)
- [API Reference](#api-reference)
- [Contributing](#contributing)
- [License](#license)

## Getting Started

### Prerequisites

- **Hardware**: STM32 microcontroller, I2C LCD display(s).
- **Software**: STM32CubeIDE or any compatible development environment.
- **Knowledge**: Basic understanding of C programming and STM32 HAL libraries.

### Installation

1. **Clone the Repository**: Download or clone this repository to your local machine.
   ```bash
   git clone https://github.com/alixahedi/i2c-lcd-stm32.git
   ```

2. **Add Files to Project**: Copy `i2clcd.c` and `i2clcd.h` into your project's source and header directories, respectively.

3. **Include Header File**: Include the `i2clcd.h` in your main program or wherever you plan to use the LCD functions.
   ```c
   #include "i2clcd.h"
   ```

4. **Configure I2C Peripheral**: Ensure that the I2C peripheral is initialized using STM32CubeMX or manually in your code.

## Usage

### Initialization

Create an instance of `I2C_LCD_HandleTypeDef` for each LCD you plan to use. Initialize each LCD by specifying its I2C handler and address.

```c
I2C_LCD_HandleTypeDef lcd1;
I2C_LCD_HandleTypeDef lcd2;

void init_lcds(void) {
    lcd1.hi2c = &hi2c1;     // hi2c1 is your I2C handler
    lcd1.address = 0x4E;    // I2C address for the first LCD
    lcd_init(&lcd1);        // Initialize the first LCD

    lcd2.hi2c = &hi2c1;     // Use the same or different I2C handler
    lcd2.address = 0x7E;    // I2C address for the second LCD
    lcd_init(&lcd2);        // Initialize the second LCD
}
```

### Displaying Text

Use the provided functions to display text, move the cursor, and clear the display.

```c
lcd_puts(&lcd1, "Hello, LCD 1!");
lcd_gotoxy(&lcd2, 0, 1);
lcd_puts(&lcd2, "LCD 2 here!");
```

### Example

```c
#include "main.h"
#include "i2c_lcd.h"

I2C_HandleTypeDef hi2c1;
I2C_LCD_HandleTypeDef lcd1;

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_I2C1_Init();

    lcd1.hi2c = &hi2c1;
    lcd1.address = 0x4E;
    lcd_init(&lcd1);

    lcd_clear(&lcd1);
    lcd_puts(&lcd1, "STM32 I2C LCD");
    lcd_gotoxy(&lcd1, 0, 1);
    lcd_puts(&lcd1, "Library Demo");

    while (1) {
        // Main loop
    }
}
```

## API Reference

### Structures

- **`I2C_LCD_HandleTypeDef`**

  | Member    | Description                      |
  |-----------|----------------------------------|
  | `hi2c`    | Pointer to the I2C handler       |
  | `address` | I2C address of the LCD (8-bit)   |

### Functions

- **`void lcd_init(I2C_LCD_HandleTypeDef *lcd);`**

  Initializes the LCD display.

- **`void lcd_send_cmd(I2C_LCD_HandleTypeDef *lcd, char cmd);`**

  Sends a command byte to the LCD.

- **`void lcd_send_data(I2C_LCD_HandleTypeDef *lcd, char data);`**

  Sends a data byte to the LCD.

- **`void lcd_clear(I2C_LCD_HandleTypeDef *lcd);`**

  Clears the display.

- **`void lcd_gotoxy(I2C_LCD_HandleTypeDef *lcd, int col, int row);`**

  Sets the cursor position.

- **`void lcd_puts(I2C_LCD_HandleTypeDef *lcd, char *str);`**

  Displays a string.

- **`void lcd_putchar(I2C_LCD_HandleTypeDef *lcd, char ch);`**

  Displays a single character.

## Contributing

Contributions are welcome! Please fork this repository and submit a pull request for any improvements.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/NewFeature`)
3. Commit your Changes (`git commit -m 'Add NewFeature'`)
4. Push to the Branch (`git push origin feature/NewFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Disclaimer**: This library is provided "as is", without warranty of any kind. Use it at your own risk.
