# OGC.Engineering
## ogc_deploy_thermal_inspection_tool_nrtps
Developer contact - dustin < at > ogc.engineering

---
### Location:

- Deployment using NRTPS OS:
    - location: deployments/ogc_deploy_thermal_inspection_tool_nrtps/
    - repo: https://github.com/OGC-dustin/ogc_deploy_thermal_inspection_tool_nrtps

---
### Deployment Options:
- at compile time, default options within repositories that are not redefined in deployment will provide warnings

---
### Resources:
- Software - Applications:
    - ogc_app_thermal_inspection_tool - directs system functionality
- Software - Libraries:
    - ogc_lib_os_nrtps - library providing the non real time polled scheduler ( nrtps ) operating system ( os ) functionality
    - ogc_lib_sd_card - libraries to handle sd card memory devices
    - ogc_lib_indicator_status_single - library to handle single color status indicators ( individual RGB Colors )
    - ogc_lib_indicator_status_multi - library to handle multi color status indicators ( mix of RGB to make specific colors )
    - ogc_lib_rgb_color_mixing - library handling simple and advanced color mixing calculations ( reference library to mix colors )
    - ogc_lib_encoder_dual_phase - library to track direction and position of a two phase encoder
- Firmware - Drivers:
    - ogc_drv_oled_ssd1351 - driver for SSD1351 RGB OLED
    - ogc_drv_ov7670 - driver for color camera sensor
    - ogc_drv_neopixel - driver to NeoPixel LEDs
    - ogc_drv_mlx90640 - driver for the MLX90640 IR Camera
    - ogc_drv_garmin_lidar_lite_v3 - driver for the Garmin Lidar Lite v3
- Firmware - Hal:
    - ogc_fw_thermal_inspection_tool - provides base level functionality and hardware abstraction to easy to use functions
- Firmware - CSP:
    - ogc_csp_ti_tm4c123g - provides chip level definitions and support drivers specific to the microprocessor family
- Hardware - Description:
    - ogc_hw_thermal_inspection_tool - provides hardware definitions, planned module definitions and pin assignment definitions

### Planning: ( Erase once documentation is updated throughout repositories )

NRTPS main calls the following in order:

hal_config() - setup the hardware
    core - clocks
        setup watchdog ( stop for now, but make the application reset it regularly in the future )
        set system clock
        start system_tick timer
        start RTC timer
    UI daughter board
        init gpio and comms for oled
        init gpio and comms for sd card
        init and turn off RGB leds
        init encoder gpio and enable change_of_state interrupts, attach change_of_state handlers from software debounce library
    Sensor daughter board
        init gpio and comms for lidar distance sensor ( is there a low power state?)
        init comms for thermal cameral ( is there a low power state? )
        init gpio and comms for color camera ( put in low power state )
        init gpio for neopixel ( default state is off, so no action needed )

hal_init() - start firmware level tasks
    ( optional ).... detect if encode click is pressed ( enter a special boot mode ) blocking before continuation to application
    schedule an immediate one shot task to fill in boot screen ( provide a built in OGC.Engineering boot logo that will remain on screen until the application takes over control ( displaying it's logo and then transitioning to app use ) )

app_init() - start application level tasks
    schedule a delayed application task ( after a short delay for the boot logo to show ) to start app logic

boot_screen_one_shot_task()
    clear oled
    set background to black
    set to mid brightness
    draw logo ( adapt to hardware description of height, width, color depth, orientation, etc. )

app_task()
    From the view of SOFTWARE we have the following generic features and their associated specific items and the path to get around

Application <----------------------------------------------------------------------------------------------------------> Hardware

System
    Command Line Interface <-> command line library ( register commands and callbacks ) <-> Serial UART
    External Storage <-> ?? <-> sd card library <-> hal
User Interface
    Input <- dual encoder tracker ( up/R or down/L with count and click ) as a library <- button debounce library <- hal
    Status Indicator -> sw_status_indicator_multi_lib ( provides simple and advanced control of RGB leds ) -> driver combines individual R, G, and B into single calls for gpio/pwm sync -> hal
    Display -> SW_window_manager ( tracks what is on screen ) -> sw_graphics_library ( provides advanced drawing )-> ssd1351_driver ( provides primatives ) -> hal
Sensors
    Light -> reuse sw-status_indicator_multi to set color and state -> NeoPixel driver to handle protocol generation -> hal
    Camera ( color ) <- image processor library ( overlays ) <-
    Camera ( ir ) <- image processor library ( overlays ) <-
    Camera ( Distance Sensor ) ( single pixel camera ) <- image processor library ( overlays ) <-
