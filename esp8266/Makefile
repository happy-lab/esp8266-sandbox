PORT = --port /dev/tty.usbserial-A104992Z

# nodeMCU specific configuration
NODE_BIN = nodemcu-lua/nodemcu_float_0.9.5_20150318.bin

# espressif AT specific configuration
BINDIR=espressif/ESP8266_AT/bin

# generic esptool configuration
ESPTOOL = esptool.py
BAUD = --baud 115200

# adafruit huzzah ESP-12 specific configuration
FLASHFREQ = --flash_freq=40m
FLASHSIZE = --flash_size=4m
FLASHMODE = --flash_mode dio
FLASH_ARGS = --verify $(FLASHSIZE) $(FLASHFREQ) $(FLASHMODE)

espressif-at-image.bin : FORCE
	$(ESPTOOL) make_image \
		--segfile $(BINDIR)/boot_v1.1.bin --segaddr 0x000000 \
		--segfile $(BINDIR)/newest/user1.bin --segaddr 0x001000 \
		--segfile $(BINDIR)/esp_init_data_default.bin --segaddr 0x07c000 \
		--segfile $(BINDIR)/blank.bin --segaddr 0x07e000 \
		$@

#		--segfile $(BINDIR)/blank.bin --segaddr 0x3fe000 \
#		--segfile $(BINDIR)/esp_init_data_default.bin --segaddr 0x3fc000 \

flash-at : espressif-at-image.bin
	$(ESPTOOL) $(PORT) $(BAUD) write_flash $(FLASH_ARGS) 0x0 $<

flash-node : FORCE 
	$(ESPTOOL) $(PORT) $(BAUD) write_flash $(FLASH_ARGS) 0x0 $(NODE_BIN)

erase-flash : FORCE
	$(ESPTOOL) $(PORT) $(BAUD) erase_flash


# NOTE(bja, 2016-08) esptool.py only supports python2.7
py-env : FORCE
	virtualenv --python=python2.7 env

esptool-git :
	git clone https://github.com/themadinventor/esptool.git
	# . env/bin/activate
	# cd esptool
	# python setup.py install

FORCE :

