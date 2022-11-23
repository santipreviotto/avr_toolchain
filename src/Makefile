# Copyright (C) 2022  Santiago Previotto
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.
#
# \file Makefile
MAIN_FILE = main
LIB_FILE = blink

DEVICE = atmega328p
FRECUENCY = 16000000UL

PROGRAMMER = arduino
PROGRAMMER_BAUD = 57600
PROGRAMMER_PORT = /dev/ttyUSB0

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

