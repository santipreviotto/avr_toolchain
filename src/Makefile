MAINFILE = main
LIBFILE = blink
PORT = /dev/ttyUSB0
DEVICE = atmega328p
FRECUENCY = 16000000UL
PROGRAMMER = arduino
BAUD = 57600
COMPILER = avr-gcc
UPLOADER = avrdude

LINT_TOOL = cpplint
LINT_FLAGS = --recursive --quiet


dockeride:
ifndef YOUARECONTAINERIZEDBUDDY
	../tools/dockeride -p $(PWD)/..
else
	echo "Alredy dockerized, pal!"
endif

	$(info )
	$(info Welcome to the docker image for programming AVR microcontrollers.)
	$(info )
	$(info Copyright 2022 Previotto Santiago.)
	$(info )

build:
	$(COMPILER) -Wall -Os -DF_CPU=$(FRECUENCY) -mmcu=$(DEVICE) -c $(MAINFILE).c -o $(MAINFILE).o
	cd lib/src/ && $(COMPILER) -Wall -Os -DF_CPU=$(FRECUENCY) -mmcu=$(DEVICE) -c $(LIBFILE).c -o $(LIBFILE).o
	cd lib/src/ && mv $(LIBFILE).o ../../
	$(COMPILER) -Wall -Os -DF_CPU=$(FRECUENCY) -mmcu=$(DEVICE) -o $(MAINFILE).elf $(MAINFILE).o $(LIBFILE).o
	avr-objcopy -j .text -j .data -O ihex $(MAINFILE).elf $(MAINFILE).hex
	avr-size --format=avr --mcu=$(DEVICE) $(MAINFILE).elf

flash:
	$(UPLOADER) -v -p $(DEVICE) -c $(PROGRAMMER) -P $(PORT) -b $(BAUD) -U flash:w:$(MAINFILE).hex:i -D

lint: 
	$(LINT_TOOL) $(LINT_FLAGS) *

clean: 
	rm $(MAINFILE).hex $(MAINFILE).elf $(MAINFILE).o $(LIBFILE).o

