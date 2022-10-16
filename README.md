# IOT


7 SEGMENT 4 DIGIT LED DISPLAY

import rp2
from rp2 import PIO
from machine import Pin
from time import sleep
# ------ #
# sevseg #
# ------ #
 
@rp2.asm_pio(out_init=[PIO.OUT_LOW]*8, sideset_init=[PIO.OUT_LOW]*4)
def sevseg():
    wrap_target()
    label("0")
    pull(noblock)           .side(0)      # 0
    mov(x, osr)             .side(0)      # 1
    out(pins, 8)            .side(1)      # 2
    out(pins, 8)            .side(2)      # 3
    out(pins, 8)            .side(4)      # 4
    out(pins, 8)            .side(8)      # 5
    jmp("0")                .side(0)      # 6
    wrap()
  
sm = rp2.StateMachine(0, sevseg, freq=2000, out_base=Pin(2), sideset_base=Pin(10))
sm.active(1)
 
digits = [
  0b11000000, # 0
  0b11111001, # 1
  0b10100100, # 2 
  0b10110000, # 3
  0b10011001, # 4
  0b10010010, # 5
  0b10000010, # 6
  0b11111000, # 7
  0b10000000, # 8
  0b10011000, # 9
]
 
def segmentize(num):
  return (
    digits[num % 10] | digits[num // 10 % 10] << 8
    | digits[num // 100 % 10] << 16 
    | digits[num // 1000 % 10] << 24 
  )
 
counter = 0
while True:
  sm.put(segmentize(counter));
  counter += 1
  sleep(0.1)





LED PRACTICAL 


import RPi.GPIO as GPIO    
import time
GPIO.setmode(GPIO.BOARD)      
GPIO.setup(3, GPIO.OUT)     
while True:
	GPIO.output(3,True)  
	time.sleep(2)         
	GPIO.output(3,False) 
	time.sleep(2)    
  
  
  
  
  TELEGRAM BOT
  
  commands 
  sudo apt-get install python-pip
sudo pip install telepot
git clone https://github.com/salmanfarisvp/TelegramBot.git


import sys
import time
import random
import datetime
import telepot
import RPi.GPIO as GPIO

#Set Gpio 
GPIO.setmode(GPIO.BCM)


#Button
GPIO.setup(12,GPIO.IN)


def handle(msg):
    chat_id = msg['chat']['id']
    command = msg['text']

    print 'Got command: %s' % command

    if(GPIO.input(12)):
        bot.sendMessage(chat_id,text="Button Pressed")
        time.sleep(0.2)
    

bot = telepot.Bot('Bot Token')
bot.message_loop(handle)
print 'I am listening...'

while 1:
     time.sleep(10)
