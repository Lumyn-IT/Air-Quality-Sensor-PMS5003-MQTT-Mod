# Air Quality Sensor PMS5003 MQTT mod ![ESP8266](https://img.shields.io/badge/ESP-8266-000000.svg?longCache=true&style=flat&colorA=CCCC33)

![photo_2024-12-18_22-14-15](https://github.com/user-attachments/assets/1eaa0297-5751-43c2-9e29-b137f5b52150)

I got hold of this sensor for monitoring air quality.
After days of testing, I found that it worked well for home use, well beyond expectations.
Thus was born the idea of being able to interface it with home automation.
After careful analysis, I deduce that the sensor is managed by a single integrated circuit that takes the information from the two sensors, makes it "readable" and writes it on the display.
Unfortunately, it seems to me that this integrated circuit does not have the possibility of being interfaced.
Furthermore, there is nothing on board that has to do with wifi.

I describe in detail what I deduced from the analysis of the components and the article in general.

The PMS5003 device performs a constant analysis of the surrounding air with a sampling rate of around 0.1L/min.
The sensor is extremely reactive and the values are updated approximately every 1sec.

The idea is to equip the device with an ESP8266 to be able to manage streaming and publish the sampled variables on MQTT brokers.

The hardware modifications will be extremely simple for anyone with a minimum of familiarity with electronics.
Obviously I decline any responsibility for damage caused by the interventions described in this article.
You will carry out each intervention at your own risk.

The software part will also be extremely simple.

But let's see in detail...

The streaming of the individual sensors, the PMS5003 for particulate matter and the AM2120 for temperature and humidity are serial (PMS5003) and onewire bus (AM2120), cyclically interrogated by the processor on board the device.

AM2120 Datashett: [AM2120](https://www.micros.com.pl/mediaserver/UPAM2120_0004.pdf)

PMS5003 Datasheet: [PMS5003](https://www.digikey.jp/htmldatasheets/production/2903006/0/0/1/PMS5003-Series-Manual.pdf)

I do not see any particular contraindications in sniffing the variables communicated by the individual sensors at the request of the processor, therefore, by intercepting the TX lines from the individual sensors and directing them towards the ESP8266 exactly as if they were sensors directly connected to it, it will be possible to receive the streaming and manage them with the ESP8266.
The interventions to do this are described in the following photos.

![photo_2024-12-18_22-18-26](https://github.com/user-attachments/assets/318ccfeb-47f1-4f40-ae8a-dedfa34fc978)

![photo_2024-12-18_22-18-30](https://github.com/user-attachments/assets/e0d68cd6-7c19-4d69-aa62-18fa03812c5b)

![photo_2024-12-18_22-18-35](https://github.com/user-attachments/assets/c8ae0463-5159-47d6-8739-51ece76cb108)

Pay particular attention to which digital inputs will be used by the ESP8266.
Nothing prevents you from changing them according to your needs, remembering however not to use compromising pins for the natural operation of the on-board peripherals of the ESP8266.

Below, for ease of use, I will report the individual lines of the sketch to pay attention to and correct for personal use of the project.

Line 2: Correct with the pins you chose (especially in case of a different card).

Line 5: Correct with the pin chosen for your am2120.

Lines 19 and 20: Correct with your network credentials.

Lines 34, 35, 36, 37 38 and 39: Correct with your MQTT Broker address, port, device, topic, username and password.

Line 52: Comment/uncomment to disable/enable debug mode. (Debug is via serial monitor on your IDE).

Line 67: Comment/uncomment to permanently disable/enable the blue LED on board the ESP8266.

Line 218: Comment/uncomment to disable/enable the flashing at the end of each data stream. Be careful: This only works if line 67 is enabled!

Once you have the streams on MQTT you will be free to do what you want with them.
My choice was that, now consolidated in my environment, to manage the variables with Node-Red, store them on InfuxDB and organize a Grafana dashboard for historical and real time visualization.

## License

Creative Commons License
This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. 

## Please check the Wiki

## Thanks

Thanks to ...

## Disclaimer

THIS SOFTWARE IS PROVIDED "AS IS" AND ANY EXPRESSED OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE REGENTS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
