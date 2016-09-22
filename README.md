ESP8266 embedded systems projects


# ESP8266 MicroPython #

* (mem check fail)[http://cholla.mmto.org/esp8266/sdk/examples/mem_error.html]

## building firmware ##

(basic directions)[https://learn.adafruit.com/building-and-running-micropython-on-the-esp8266/build-firmware]

deviations from directions

* add firmware vagrant repo as subtree:

    git subtree add --prefix esp8266/micropython-vagrant https://github.com/adafruit/esp8266-micropython-vagrant.git master

* use esptool.py to flash firmware

    git clone https://github.com/themadinventor/esptool.git
    virtualenv --python=python2.7 env
    . env/bin/activate
    cd esptool/
    python setup.py install
    esptool.py -h

### flash adafruit huzzah breakout board with FTDI cable ###

* press gpio0 button then reset button

* run commands to flash

    esptool.py --port /dev/ttyXXX erase_flash
    esptool.py -p /dev/tty.usbserial-FTF0GKWQ --baud 460800 write_flash --verify --flash_size=8m 0 micropython-vagrant/firmware-combined.bin 


### adafruit huzzah breakout board ###

* board 1 - micropython:

  * chip id:

      esptool.py --port /dev/tty.usbserial-A104992Z chip_id
      esptool.py v1.2-dev
      Connecting...
      Chip ID: 0x00db5844
    
  * flash id:

      esptool.py --port /dev/tty.usbserial-A104992Z flash_id
      esptool.py v1.2-dev
      Connecting...
      Manufacturer: e0
      Device: 4016

  * mac address:

      esptool.py --port /dev/tty.usbserial-A104992Z read_mac
      esptool.py v1.2-dev
      Connecting...
      MAC: 18:fe:34:db:58:44


* board 2 - nodemcu lua:

    esptool.py --port /dev/tty.usbserial-FTF0GKWQ chip_id
    esptool.py v1.2-dev
    Connecting...
    Chip ID: 0x001217e7

    esptool.py --port /dev/tty.usbserial-FTF0GKWQ flash_id
    esptool.py v1.2-dev
    Connecting...
    Manufacturer: e0
    Device: 4016

    esptool.py --port /dev/tty.usbserial-FTF0GKWQ read_mac
    esptool.py v1.2-dev
    Connecting...
    MAC: 5c:cf:7f:12:17:e7

    -- lua
    > print(wifi.sta.getmac())
    18-FE-34-12-17-E7
    > majorVer, minorVer, devVer, chipid, flashid, flashsize, flashmode, flashspeed = node.info()
    > print("NodeMCU "..majorVer.."."..minorVer.."."..devVer)
    NodeMCU 0.9.5
    > print("chipid = "..chipid)
    chipid = 1185767
    > print("flashid = "..flashid)
    flashid = 1458400
    > print("flash mode = "..flashmode)
    flash mode = 0
    > print("flash speed = "..flashspeed)
    flash speed = 40000000
    > print("flash size = "..flashsize)
    flash size = 4096
