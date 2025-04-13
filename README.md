# Pcileech-FirmwareCustomizationGuide
### This guide teaches you how to stand on the shoulders of giants and use open source projects to make your own private firmware.

**🤬I hate scammers who make crap firmware and sell it at inflated prices, and people who like to be vague in their guidelines.**

**😊That's why this guide was created, to make your own proprietary firmware with zero experience!**

### Emphasis
* This guide teaches you to make your own firmware using open source projects instead of starting from scratch!

* this guide step by step and you will definitely be able to create your own firmware!

* This guide the firmware made by  can meet your usage needs, please test it by yourself!

* This guide uses descriptions that even primary school children can understand!

### How to get help

Official website：https://beater.solutions

discord：https://discord.gg/beater

# Let's start making firmware！

### Capturing real configuration space

Beginners naturally take the easiest network card solid as a demonstration (demonstration of the use of ekknod open source WIFI source code, let us thank ekknod), to buy a PCIe network card, the price does not matter, inserted into a computer that can run normally, pay attention to the PCIe network card, do not buy the wrong other interfaces! Acquisition software recommended TeleScan more convenient, but TeleScan default display bar value is wrong, you need to manually repair (the following will be demonstrated), skilled hands can use Arbor. production process is not only applicable to the network card, you have other firmware source code can also be done.

Capture the real configuration space open TeleScan to find the network card you bought (class more bad to find you can choose other arrangement), see the picture box out of the card's Base Address Register column that Bar? Left-click to select Bar0 after the box will become blue, in the right side of the long bar in the digital box, long press 1 to replace all the numbers in the box for 1, at this time, the blue selected box value and the long box number 1 are green, right-click to select the Write ¡ú DOWORD to save (you can use Alt + Shift + D shortcut keys to save), save the changes in the blue selected box value and the long bar box value will become black! After saving the changes, the values in the blue checked box and the values in the long bar box will turn black, and so on, Bar0~Bar5 will be repaired to the correct Bar value (only Bar0/Bar2/Bar4 in the figure have values to be repaired). After fixing the Bar values, you can save the tlscan file (the save button is the second one in the second row).

![image](https://github.com/user-attachments/assets/921485fb-7e5d-4231-9652-e082a11bf311)
![image.png](https://tc-cdn.flowus.cn/oss/f0a6f1e8-6806-47df-9a01-5b35798d01f0/image.png?time=1744568100&token=47663b7369807265c5897a51c1c2b169ebe79219a54024279e2ba1623fe54ade&role=free)

### pcileech_cfgspace.coe

Nowadays there are many one-click tlscan to COE tool, Just find a tlscan to COE tool to convert , the generated file renamed pcileech_cfgspace.coe replaced into the IP folder you use the source code, overwrite the original pcileech_cfgspace.coe file.

![image](https://github.com/user-attachments/assets/57e92ce6-b12a-4434-9e02-25d244c87d2b)

### pcileech_fifo.sv pcileech_pcie_cfg_a7.sv

pcileech_fifo.sv file in your source code decompression src folder, you can use Vivado/Notepad++/Notepad to open the modification (depending on personal habits, the final firmware are to use Vivado to generate, Vivado open the path to srcs/sources 1/imports/pcileech- xxx/src/pcileech-fifo.sv), open the pcileech_fifo.sv file see the picture boxed out of the rw[203] and rw[206] two places? rw[203] of the b after the change of 0, rw[206] after the change of b after the change of 1. change remember to save!

![image](https://github.com/user-attachments/assets/d0804606-12e5-4803-953b-6ebf04d53dd4)

If you want to change the value after rw[143:128]~rw[199:192], you can fill in the corresponding value read by TeleScan backwards or randomly (the effect is the same).

![image.png](https://tc-cdn.flowus.cn/oss/24536238-e847-4856-8420-fa2f8ae9a639/image.png?time=1744568100&token=1f4b710074faf5e29cec9bdc21fce55cf8c3a05dc3f4b59b77d94228890525f2&role=free)
![image](https://github.com/user-attachments/assets/c00f67d5-7a41-4b68-b204-5a68235cac4f)


After changing the pcileech_fifo.sv file now change the pcileech_pcie_cfg_a7.sv file, also in the src folder after you unzipped the source code, what software to use to change the same depends on personal habits (Vivado open the path to srcs/sources 1/imports/pcileech-xxx/ src/pcileech fifo.sv). src/pcileech fifo.sv), see the rw[127:64] boxed out in the picture, the 16-bit value after h is the firmware DSN, here fill in the DSN (abbreviation of Device Serial Number Capability) read by TeleScan to your card, the 1st DW+2nd DW value added together is not exactly 16 bits? The value in 1st DW+2nd DW should be 16 bits, the value in 2nd DW should be filled in the front, the value in 1st DW should be filled in the back, don't fill in the opposite. Then change rw[21] to 0 and rw[20] to 1 (here it is recommended to keep the default unchanged). change remember to save!

![image.png](https://tc-cdn.flowus.cn/oss/f07c904f-6530-429f-8cdc-0095cc799c13/image.png?time=1744568100&token=831889eb3728d1370b18467e289a8d0fd455084b479f92206aead875abe43af6&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/d6258fc3-2f05-437f-81ee-83e3e43cd747/image.png?time=1744568100&token=b6ef1a58fbf48c8cd821837234e843c0fd43e08124e7c95a90c496d245b68f2d&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/8464c44c-c4c6-4ace-84be-3ea8a66f2bbc/image.png?time=1744568100&token=b8cb7bf62d55c36644beccf22b8f737a00fdd72185c6c2a7ac45a4ab9c9ea061&role=free)

If you want to change the value after rw[143:128] and rw[199:192], you can fill in the corresponding value read by TeleScan upside down or just fill in the corresponding value (the same as the above rw[143:128]~rw[199:192]-fill in the random value needs to be consistent).

![image.png](https://tc-cdn.flowus.cn/oss/09efe919-5b67-4d66-a58a-98a63fde7a6d/image.png?time=1744568100&token=ece007e0a412b8de3bc8c6e9bdcfd737e99f10b67252a1b61feb245539322de0&role=free)

### pcie_7x_0_core_top.v

This document is recommended to use other people to change the good version of the province of a pointer to check, after all, the public source code and then how to sew patch is not too bad, is not it? The pursuit of hand-operated words in accordance with the following chart in the order of control can be modified, the first ID board is very simple to modify the box h after the value changed to TeleScan to read out the corresponding parameters of the card can be, the other do not make any changes.

![image](https://github.com/user-attachments/assets/979c7a2c-c5ff-41ea-9ed7-04b27287c2db)

The second BAES board fills in the offset offset address of the NIC read out by Telesan, i.e., the value before h after offset in the Advanced Error Reporting Capability and Virtual Channel Capability brackets, which only needs to be changed in two places.

![image](https://github.com/user-attachments/assets/7298fccb-4af9-4488-9071-735dc53aebbb)

The third BAR board fill Telesan read out the value of the network card's Base Address Register0~5, 04 and 04 end of the BAR after the BAR full F, for example, BAR1/BAR3/BAR5. because bar0 can not be 1 at the end of the 1 is on behalf of the 10, 0 is on behalf of the memory space to be changed to 0 bar2 and bar4 followed by the 4 or C is the 64-bit address, then bar3 and bar5 should be changed to all F.

![image](https://github.com/user-attachments/assets/e9dfa690-15d8-4527-b2d4-57656306a84f)
![image.png](https://tc-cdn.flowus.cn/oss/d652db63-bd90-4d97-85dd-6e004aa17d7b/image.png?time=1744568100&token=4f6af34915f5c9c3395e206e75f071f185705e16e220ee42b77dd70d2f6b4ede&role=free)

The following one by one to check and find the changes, as shown in the figure against the modification of each place, Telesan read out the network card has to change to the corresponding value and open (FALSE to TRUE), there is no skip.
![image.png](https://tc-cdn.flowus.cn/oss/1d096c22-c7cc-4866-98d4-6a247588be00/image.png?time=1744568100&token=536f32a77d641dde409e84d9b5104da40e96ee7d05ac577e2f5ccaeb484e6ba1&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/760ce7ab-eb4b-4a5f-a384-9695192804ef/image.png?time=1744568100&token=21b5a938e2bd8296001f892e5c2f8ac1a303fdf9450ba23b8cfc5a4c255c0132&role=free)
![image](https://github.com/user-attachments/assets/824970e5-ea8c-4790-989b-022ce77c2179)
![image.png](https://tc-cdn.flowus.cn/oss/eb46849d-6c0d-484f-8a8e-8e3f1b05e59f/image.png?time=1744568100&token=4bb83d064ce6c9edee85b0790f8a8c66d1107a4029041c1fc935565b7fcea4ef&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/d813db8a-3ffd-4810-9d1b-8b86ad3d1014/image.png?time=1744568100&token=437851742fa1e4bf65f99b2f36562dc60c290bbc75b99a335316c7893887666d&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/7fa7f676-82bf-4725-a1cc-ebfb987cc74e/image.png?time=1744568100&token=2fa37ab10b4071a17f50c2b7709b8725716e44a338045309574c260f6045828e&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/13041461-6ff5-49e9-b5ee-0ea451f2fe33/image.png?time=1744568100&token=c63ab0da5e7ece72a2fc9007ad851185a55f7706304a2b64fe71b039db760a91&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/0a28bb0e-3124-43b8-999c-75a87197d1cc/image.png?time=1744568100&token=28f8b05f16101fa45027938d7f8c7514ac4afcf52c75e4be915b69e68ee181f3&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/4144d1ae-2688-4520-aed1-a683381d93e4/image.png?time=1744568100&token=24fac9b38aab4f1a471efe34c8d9c6303b7bb210663de41b548e21de3dd1132a&role=free)
![image](https://github.com/user-attachments/assets/fef12c92-f1d0-4688-86de-e858e7b93203)
![image.png](https://tc-cdn.flowus.cn/oss/ad146e45-a85f-4a99-a315-e0ad2fe20883/image.png?time=1744568100&token=c7104c0702470d640cc5f6f6dda4b627856b4857938cdfba02eb1e2cf1c17f33&role=free)
![image.png](https://tc-cdn.flowus.cn/oss/f21816e3-6b42-468d-8ffa-992e4e460f9d/image.png?time=1744568100&token=525b41d124a9563fdc0e6cfda9f8ee211db25d337c19beebd683564d8e860eb3&role=free)

### Great job, now you can generate your handmade proprietary firmware!

Vivado left navigation bar click Generate Bitstream, pop-up window box out of the option to generate the firmware when the number of CPU cores used, the recommended default core number, if your computer configuration is higher can be appropriate to increase the output core. Generate the firmware pcileech _enigma_x1 _top.bin in the source code file under the impl_1 folder.

![image](https://github.com/user-attachments/assets/af09824b-143f-4639-9a0b-344104ce69f0)








