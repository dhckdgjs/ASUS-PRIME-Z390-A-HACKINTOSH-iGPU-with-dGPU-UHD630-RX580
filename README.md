# ASUS-PRIME-Z390-A-HACKINTOSH-Clover-iGPU-with-dGPU-UHD630-RX580


## Components
- M/B: ASUS PRIME Z390-A(BIOS Ver. 1005)
- CPU: Intel® Core™ i7-8700 Processor
- iGPU: Intel® UHD Graphics 630
- dGPU: SAPPHIRE NITRO AMD Radeon RX 580 4GB
- Lan: Intel® I219V, 1 x Gigabit LAN Controller
- WiFi/Bluetooth: BCM94360CS2
- Audio: Realtek® ALC S1220A 8-Channel High Definition Audio
- Case: Fractal Design R5 Blackout



## Caution - 19.12.01
If you update your M/B Bios to 1105 version (or 1201, 1302), after that you’ll show stuck/freeze at booting progress bar.


Error messages like these(ASUS RTC bug related)

- apfs module start 1393
- SMCSuperIO: ssio @ detected device Nuvoton NCT679BD(after SMCRTC: start)
- SMMSensors: [Fatal] ~~~



So, DO NOT UPDATE Bios 1105 or new one.
(ASUS doesn’t allow downgrade to 1005 version or below)

But if you had already update your bios, you can use this ACPI DSDT patch as below.

```
Comment: ACPI Patch
Find: A00A9353 54415301
Replace: A00A910A FF0BFFFF
```

This patch also works in H370, B360 chipset M/B.


* References

https://www.tonymacx86.com/threads/a-solution-of-asus-new-bios-ver-2012-not-downgrading.273151/

https://www.tonymacx86.com/threads/the-everything-works-asus-z390-i-gaming-i7-8700k-sapphire-rx580-pulse-build.272572/page-133#post-1941938

https://www.tonymacx86.com/threads/fix-for-boot-hangs-after-bios-update-acpi-patch.275091/








## Comments
This Hackintosh build guide is NOT GUARANTEE 100% fully working in your conditions.

This guide has been tested on MacOS Mojave 10.14.4 and 10.14.5 and prefers the use of an AMD dGPU for ease of installation. This settings are considered using dGPU(RX580) and iGPU(UHD 630 with Quicksync) simultaneously in specifically the FCPX(10.14.6) and these dual GPUs can nearly full hardware accelerated.(similar to the Headless setting but not the exact same)
And this guide can be used on the Gigabyte M/B also. (some settings different)

Special Thanks to CaseySJ(at tonymacx86.com) and Newlife(at x86.co.kr)



## Procedure
1. Install MacOS 10.14.5 or newer(with Lilu.kext and Whatevergreen.kext )
2. Set the ACPI settings as below
```
				<dict>
					<key>Comment</key>
					<string>change HECI to IMEI</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					SEVDSQ==
					</data>
					<key>Replace</key>
					<data>
					SU1FSQ==
					</data>
				</dict>
				<dict>
					<key>Comment</key>
					<string>change MEI to IMEI</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					TUVJXw==
					</data>
					<key>Replace</key>
					<data>
					SU1FSQ==
					</data>
				</dict>
				<dict>
					<key>Comment</key>
					<string>change GFX0 to IGPU</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					R0ZYMA==
					</data>
					<key>Replace</key>
					<data>
					SUdQVQ==
					</data>
				</dict>
				<dict>
					<key>Comment</key>
					<string>change PEGP to GFX0</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					UEVHUA==
					</data>
					<key>Replace</key>
					<data>
					R0ZYMA==
					</data>
				</dict>
```


3. Add Kext patch to the Kernel and Kext Patches
```
			<dict>
				<key>Comment</key>
				<string>Black Screen Patch Vega 56/64, RX580 etc. (c)Pike R. Alpha</string>
				<key>Disabled</key>
				<false/>
				<key>Find</key>
				<data>
				Ym9hcmQtaWQ=
				</data>
				<key>InfoPlistPatch</key>
				<false/>
				<key>Name</key>
				<string>AppleGraphicsDevicePolicy</string>
				<key>Replace</key>
				<data>
				Ym9hcmQtaXg=
				</data>
			</dict>
```


4. Set the SMBIOS to iMac 19,1
5. Change M/B Bios Primary display(ASUS) or Initial Display Output(Gigabyte) to PEG or PCIE(iGPU MUST be turn on)
6. Apply the Framebuffer patch(Headless) on your Devices setting  in config.plist. (check settings as below and recommend using the Hackintool) 
```
platform-id: 0x3E910003(maybe works well 0x3E920003)

device-id: 0x3E91 Intel UHD 630(maybe works well 0x3E92)

Check some options:
	- General: DeviceProperties, Connectors, VRAM, Graphic Device
	- Advanced: Hotplug Reboot Fix(I’m not sure), Spoof Video Device ID, VRAM 2048 MB, GfxYTile Fix
```

Ref. [GUIDE General Framebuffer Patching Guide (HDMI Black Screen Problem) | tonymacx86.com](https://www.tonymacx86.com/threads/guide-general-framebuffer-patching-guide-hdmi-black-screen-problem.269149/)

7. Remove Whatevergreen.kext on your EFI/CLOVER/kexts/Other
8. Done


## Summary
As far as I have been able to test, everything works well except Thunderbolt devices.(I don’t have ThunderboltEX 3 or Titan/Alpine ridge add-on card)
If you need other detailed settings fot this ASUS PRIME Z390-A M/B, please check released file.(like as USBport)
Thanks.



## Screenshots


![Sys info_PCI](https://user-images.githubusercontent.com/35429874/61994177-59df8980-b0b2-11e9-857f-47d757fa7a0f.png)
![Sys info_USB](https://user-images.githubusercontent.com/35429874/61994187-6c59c300-b0b2-11e9-896a-8a3ac4609117.png)
![Sys info_Audio](https://user-images.githubusercontent.com/35429874/61994188-711e7700-b0b2-11e9-908f-1ffd44d945a8.png)
![Sys info_Bluetooth](https://user-images.githubusercontent.com/35429874/61994190-71b70d80-b0b2-11e9-8f2a-18757d28cd83.png)
![Sys info_Network](https://user-images.githubusercontent.com/35429874/61994191-71b70d80-b0b2-11e9-888d-b25cac1842a8.png)
![PRIME Z390-A - USB port map final](https://user-images.githubusercontent.com/35429874/61994465-821cb780-b0b5-11e9-9d00-b12ed9046afc.jpg)
