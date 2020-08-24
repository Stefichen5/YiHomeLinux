# Yi Home Linux files

Here I share some of the files I got from a flash dump from my Yi Home 1080p.

I currently don't want to upload the whole dump since my Wi-Fi passwords are stored on there in plain text (well done, Yi...). But I might factory reset it and upload the whole dump in the future.

Here is what I did (you can do the same. Just make sure the offset is correct - might change between software versions: 

```bash
#extract device tree from firmware dump
dd if=dump.bin bs=1 skip=429056 count=65745 of=devicetree.dtb

#decompile it into dts
dtc -I dtb -O dts -o devicetree.dts devicetree.dtb

#extract linux kernel image
#size possible a little bit larger (trailing 0s?)
dd if=dump.bin bs=1 skip=524288 count=2080555 of=kernel.kimg
```

Hint: If you are not sure about where the dtb part of the dump starts/ends: 

- Run "binwalk dump.bin"
- Find the offset for the "device tree image (dtb)"
  - there might be multiple entries. Find sth in the neighbourhood of 0x68C00
- Open the dump in Hex Editor
- Go to the offset you figured out before
  - You will see a lot of padding "FF"s
  - The device tree file part starts with "0xD0 0x0D"
  - Great. You have found the beginning
- Scroll down until you see a lot of "00"s
  - The end of the device tree file is after the first "0x00" (so 1 "0x00" is still included)

If you are not sure: Just download my dtb and look at in in a hex editor of your choice.