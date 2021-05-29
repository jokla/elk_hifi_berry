# elk_hifi_berry

# List of Hardware

I bought them from https://thepihut.com/ (UK store)

- Raspberry Pi 4 Model B 4GB 	1 	£54.00
- 4-Piece Raspberry Pi 4 Heatsink Set Orange 	1 	£2.00
- USB 2.0 MicroSD Card Reader (MicroSD to USB) 	1 	£2.00
- SanDisk MicroSD Card (Class 10 A1) 32GB 	1 	£8.00
- HDMI to Micro-HDMI Cable 2m (Gold Plated) for the Raspberry Pi 4 	1 	£6.00
- Official UK Raspberry Pi 4 Power Supply (5.1V 3A) Black 	1 	£7.50
- HiFiBerry DAC+ ADC Pro 	1 	£65.00

To connect the heaphones:
- RCA to 3.5mm Female Cable £4.90 from [Amazon.co.uk](https://www.amazon.co.uk/gp/product/B00KTHGDCS/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1)

# Installation:
- Follow the official ELK documentation [Getting started](https://elk-audio.github.io/elk-docs/html/documents/getting_started_with_development_kit_elk_pi_hardware.html)
- After the SD flashing, I connected the Raspberry PI to a monitor and set up the WiFi connection (see [here](https://elk-audio.github.io/elk-docs/html/documents/working_with_elk_board.html?highlight=wifi#over-wifi))
- You can connect to your device with ssh: `ssh mind@IP` (you can configure your router to give always the same IP to your device) or `ssh mind@elk-pi` 

# Run Sushi and a MIDI controller
- If you don't have a MIDI controller, you can use your smartphone and a USB cable. 
- If you have an Android device, you can use [MIDI Keyboard](https://play.google.com/store/apps/details?id=com.dreamhoundstudios.keyboard&hl=en_GB).
- Plug your phone and select MIDI in the USB setting. 
- Open the app, press the button `Output` and select `Android USB Peripheral Port`.
- Now connect to the elk-pi and follow [this ELK guide]( https://elk-audio.github.io/elk-docs/html/documents/run_elk_on_boards.html).
- Run sushi:  
`sushi -r --multicore-processing=2 -c ~/config_files/mda-vst3-configs/config_mda_synth.json &`
- Connect the smarphone Midi output (port 16) to Sushi (port 128) with the following command:   
 `aconnect 16 128`. 
- Check the result with `aconnect -l`
```bash
mind@elk-pi:~$ aconnect -l
client 0: 'System' [type=kernel]
    0 'Timer           '
    1 'Announce        '
client 14: 'Midi Through' [type=kernel]
    0 'Midi Through Port-0'
client 16: 'OnePlus' [type=kernel,card=0]
    0 'OnePlus MIDI 1  '
	Connecting To: 128:0
client 128: 'Sushi' [type=user,pid=513]
    0 'listen:in       '
	Connected From: 16:0
    1 'read:out        '
```

- Press the piano keys on your smartphone app and check if you can get MIDI events with `aseqdump -p 16:0`

```
mind@elk-pi:~$ aseqdump -p 16:0
Waiting for data. Press Ctrl+C to end.
Source  Event                  Ch  Data
 16:0   Note on                 0, note 57, velocity 100
 16:0   Note on                 0, note 62, velocity 100
 16:0   Note off                0, note 57, velocity 100
 16:0   Note on                 0, note 57, velocity 100
 16:0   Note off                0, note 62, velocity 100
 16:0   Note off                0, note 57, velocity 100
 16:0   Note on                 0, note 62, velocity 100

```
- Connect your the RCA to 3.5mm Female Cable to the HiFiBerry and the headphone to the 3.5mm connector.
- Play and check if you can hear any sound!


https://mclarenlabs.com/blog/2018/07/03/linux-midi-cheatsheet/


# Test audio output
`sushi -r --multicore-processing=2 -c ~/config_files/mda-vst3-configs/config_play_arp_mda_link.json &`   
 `speaker-test` or `speaker-test -t wav -c 6 ` (not working)   
