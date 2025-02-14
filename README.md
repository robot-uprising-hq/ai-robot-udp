# Micro Invaders Firmware (UDP)

This repository contains firmware for the [robots](https://github.com/robot-uprising-hq/ai-robot-hardware) that can be used in [Micro Invader competition](https://github.com/robot-uprising-hq/ai-guide).

The firmware supports Wifi and Bluetooth. If WiFi is used, the motor commands can be send via simple encoded UDP messages.

The firmware works well with [zero-ones-simulated](https://github.com/zero-ones-given/zero-ones-simulated) simulator.

There is also another alternative firmware for the robot, [ai-robot](https://github.com/robot-uprising-hq/ai-robot), which uses protocol buffers to serialize the data. It works well with [ai-simulator](https://github.com/robot-uprising-hq/ai-simulator).

## Getting started

Prepare yourself for an adventure in robotics! And remember, the real treasure in programming is not the result but the bugs we make along the way.

### Assembly

Connect the right motor to connector J2 and the left motor to J3.

### Configure WiFi or Bluetooth via serial

- Make sure the firmware has been flashed
- Connect board to computer via USB
- Establish [serial connection](https://docs.espressif.com/projects/esp-idf/en/v5.4/esp32/get-started/establish-serial-connection.html)

You can connect for example using screen or the monitor command if you have the Espressif toolchain installed:

    screen [port] 115200
    # or using the Espressif toolchain:
    idf.py -p [port] monitor

![screenshot](screenshot.png)

To connect to WiFi, press any key to open the terminal and then press the `w` key. The terminal will prompt you for the WiFi ssid and password and then reboot the robot. After rebooting, you can use the `i` command to check the ip that was assigned to the robot during startup.
In some cases the firmware has been known to report the wrong ip when using incompatible versions of esp-idf. If you don't get a local network ip (usually starting with 192.168), you can try finding out your ip from the logs the firmware outputs while booting or from the wifi routers admin interface. This version has been tested to work with esp-idf 5.4.

### Controlling the robot

Once you have set up the Wi-Fi connection, you should be able to control the robot by sending UDP messages to port 3000. The message should be a string consisting of two integers between 100 and -100 separated by a semicolon. You can try it out e.g. with the following command:

    echo -n '50;-50' | nc -u [robot ip] 3000

Or by modifying and running the [python example](examples/send-udp.py)


## Flashing the firmware

Install the Espressif toolchain v5.4: https://docs.espressif.com/projects/esp-idf/en/v5.4/esp32/get-started/index.html

Set up the ESP-IDF Environment variables, if you have not already done so:

    . $HOME/esp/esp-idf/export.sh

Build and flash the project:

    idf.py build
    idf.py -p [port] flash

The old precompiled build for esp-idf 5.3.0 can be found in [releases](https://github.com/robot-uprising-hq/ai-robot-udp/releases/)

## Troubleshooting

If you have trouble building, you can remove the build folder by running:

    idf.py fullclean


If you have trouble flashing, you can try running menuconfig and ensure that:

- bluetooth classic is enabled and BLE is disabled
- the custom partition table partitions.csv is selected
- at least 4MB flash size is selected from `Serial flasher config > Flash size`

```
    idf.py menuconfig
```

To take a look at the log output or connect to the built in console, you can:

    idf.py -p [port] monitor

To exit the monitor press `ctrl + ]` (`ctrl + 5` or `ctrl + å` On a Finnish keyboard)
