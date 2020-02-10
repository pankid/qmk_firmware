# Nick notes on flashing kbd75_v2

These are my notes on programing my kbd75_v2 keybaord. I edited the keymap to have numpad numbers on the following:

```
i=7 o=8 p=9 [=0
k=4 l=5 ;=6 ENTER=ENTER
,=1 .=2 /=3 SHIFT=.
```

To access these, hold down the key diretly to the right of the spacebar while hitting these keys. This is to access the second layer. 


This keyboard uses the atmega32u4 as its controller with dfu as the firmware. 

To access the bootloader mode, plug the keyboard in while holding the space and b key. 


the file to edit is in `./keyboards/kbd75/keymaps/pankid/keymap.c`

I copied the default keymap directory to pankid and edited this file to create a third layer with the numpad keys. 

keycodes can be found here: [https://docs.qmk.fm/#/keycodes](https://docs.qmk.fm/#/keycodes)

instructions for programing and flashing can be found here: [https://docs.qmk.fm/#/newbs](https://docs.qmk.fm/#/newb)s

I did the following steps:

make sure homebrew installed and updated if on osx. in linux, just install git. 

```
git clone https://github.com/qmk/qmk_firmware.git
cd qmk_firmware
util/qmk_install.sh
```

after installation, make a copy of the default keymap and change the name to something meaningful. 

```
cd keyboards/kbd75/keymaps
cp -r default pankid
```

Now edit the the copied keymap.c file. Each row of ------, is a row on the keyboard. Use the keycodes to add what key you want it to be. MO(1) is for [1] = LAYOUT(. MO(2) is for accessing [2] = LAYOUT(

once you are finished go back to the main qmk_firmware directory and compile. 

```
make kbdfans/kbd75/rev2:pankid
```
once you have the firmware built without errors, you can now flash the keyboard. first run the make command, then restart the keyboard holding space and the b key to enter bootloader mode. Once the keyboard boots into bootloader mode the make command will show that it was found and fllash it. 

```
sudo make kbdfans/kbd75/rev2:pankid:dfu
```

once flashing is finished, reboot your keyboard  and test it out. 

## git notes on managing a fork

to setup the fork I am following these instructions: [https://help.github.com/articles/fork-a-repo/](https://help.github.com/articles/fork-a-repo/)

then following these instructions to sync the fork: [https://help.github.com/articles/syncing-a-fork/}(https://help.github.com/articles/syncing-a-fork/)

my step by step commands are: 

```
cd /home/pan/git/qmk_firmware
git remote -v #This will show you what is followed in the currect repository
git remote add upstream https://github.com/qmk/qmk_firmware.git #this will add the qmk_firmware project as the upstream
git remote -ve # we should now see the upstream repository set
git fetch upstream
git checkout master
git merge upstream/master
```

we should now be setup with a merged repo from upstream. May need to seup the `git config --global user.email` and `git config --global user.name`

now to commit any changes

```
git add --all 
git commit -m "some note"
git push
```

