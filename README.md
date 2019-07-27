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



## Comments
This Hackintosh build guide is ~NOT GUARANTEE~ 100% fully working in your conditions.

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
If you need other detailed settings fot this ASUS PRIME Z390-A M/B, please check released file.(like as USBport
Thanks.



## Screenshots



