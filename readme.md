# GPIO Linux Device Driver for Raspberry Pi 5

This project demonstrates how to create a simple GPIO Linux device driver for the Raspberry Pi 5. It includes:

- A kernel module (`ldd_gpio.c`) to control GPIO21.
- A user-space application (`usr_toggle_gpio.c`) to toggle the GPIO from user space.

---

## Project Structure

- **ldd_gpio.c**: Kernel module source code. Controls GPIO21 and creates a device file for user-space interaction.
- **usr_toggle_gpio.c**: User-space application. Toggles GPIO21 by writing to the device file.
- **Makefile**: Automates compilation of both kernel module and user-space application.
- **readme.md**: This guide.

---

## Prerequisites

- Raspberry Pi 5 running Linux.
- Kernel headers installed (`sudo apt install raspberrypi-kernel-headers`).
- Basic build tools (`make`, `gcc`).

---

## Step-by-Step Instructions

### 1. Compile the Kernel Module and User Application

Run the following command in the project directory:

```sh
make
```

This will build:
- `ldd_gpio.ko` (kernel module)
- `usr_toggle_gpio` (user-space application)

---

### 2. Load the Kernel Module

Insert the kernel module into the running kernel:

```sh
sudo insmod ldd_gpio.ko
```

**Tip:** If you need to reload, first remove it with `sudo rmmod ldd_gpio`.

---

### 3. Verify Module Loading

Check that the module loaded correctly:

```sh
dmesg | tail
lsmod | grep ldd_gpio
```

You should see messages indicating successful initialization.

---

### 4. Run the User-Space Application

Start toggling GPIO21:

```sh
sudo ./usr_toggle_gpio
```

- The application will toggle GPIO21 every second.
- Press `CTRL+C` to stop.

---

### 5. Test GPIO Functionality

- Connect an LED (with resistor) to GPIO21 and GND.
- The LED should blink on and off every second.

---

## Troubleshooting

- **Device file not found:** Ensure `/dev/ldd_gpio21_device` exists after loading the module.
- **Permission errors:** Run the user application with `sudo`.
- **GPIO not toggling:** Double-check wiring and that GPIO21 is correct for your hardware.

---

## Notes

- The kernel module uses GPIO number 592, which maps to GPIO21 on Raspberry Pi 5. If your board revision differs, check the correct GPIO number in `/sys/kernel/debug/gpio`.
- You can manually control the GPIO using:
  ```sh
  echo 1 | sudo tee /dev/ldd_gpio21_device   # Set GPIO high
  echo 0 | sudo tee /dev/ldd_gpio21_device   # Set GPIO low
  cat /dev/ldd_gpio21_device                 # Read GPIO state
  ```

---

## Clean Up

To remove the kernel module and clean build files:

```sh
sudo rmmod ldd_gpio
make clean
```

---

## References

- [Raspberry Pi GPIO documentation](https://www.raspberrypi.com/documentation/computers/os.html#gpio)
- [Linux Device Drivers, 3rd Edition](https://lwn.net/Kernel/LDD3/)