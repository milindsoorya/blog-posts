## Run Ubuntu on M1 Macbook Air using UTM


## Introduction

Let's face it M1 mac's are the best laptop out there. With a perfect balance of power and batter life. For me the only thing holding it back was its OS. Mac OS is not as developer friendly as I expected. As i found out after using my M1 mac for a few months, some things are better suited for the OS it was designed to be run on.

This was not a problem in the intel mac era, as we could easily load a virtualisation software and run which ever OS we want, but with the new ARM chips its a different story. The only software that I could find which was free and had all the features was [UTM](https://mac.getutm.app/).

UTM is an amazing piece of software and it does its job perfectly (mostly), I did face some challenges installing Ubuntu, but to be fair, it was more related to the Ubuntu kernal than UTM.

UTM also supports installing [windows](https://mac.getutm.app/gallery/windows-11-arm) and many more OS. Check out all the available options [here](https://mac.getutm.app/gallery/).

## 1. Installing UTM

Installing UTM is pretty straight forward, you can find the software [here](https://mac.getutm.app/).

## 2. Downloading the OS Image

You have to download the Ubuntu server version, don't worry we can easily install GUI after installing the barebone OS. [Download Ubuntu](https://mac.getutm.app/gallery/ubuntu-20-04).

> Please verify you are downloading the ARM version of the software

The steps to install are mentioned on the bottom of the UTM ubuntu webpage. They usually autofill the storage allocation as 64gigs, I would recommend making it to 20gigs as it is more than enough, but feel free to change it according to your use case.

## 3. Possible Install issues

**Problems with Booting into Ubuntu**
Due to a recent update in the Linux version, some might find it hard to boot into Ubuntu. You can bypass this problem by doing the following steps in the UTM options menu, located at the top right corner of the application.

1. Disabling the QMEU > use hypervisor option
2. Changing the display type to Console only
3. Boot into Linux, but it will be VERY slow because it is using emulation instead of virtualization.
4. Once inside, install the last version of Linux `sudo apt install linux-image-5.4.0-100-generic`
5. Shut down the VM, open VM settings and under QEMU -> Teaks, check "Use Hypervisor" to re-enable virtualization.
6. Start the VM and when the UTM logo shows up, hold "Shift" to enter the GRUB menu
7. Select "Advanced Options for Ubuntu"
8. Select "Ubuntu, with Linux 5.4.0-100-generic" (or anything lower than 5.4.0-104)
9. You can now boot into Ubuntu again. You may choose to uninstall 5.4.0-104 and temporarily mask it from being updated in the future: `sudo apt-mark hold linux-image-$(uname -r)`

**Problems with internet connectivity**
There is a solution in the bottom of the UTM page for Ubuntu, for this issue. If its not working try out the solution proposed in this [discussion](https://github.com/utmapp/UTM/discussions/2435#discussioncomment-582912).

## Conclusion

That's it.. Its really easy to run Linux or even Windows on your shiny new M1 macs. The possibilities are endless and the experiance is really seamless. During my usage I was able to get very good performance out of the system.

> 💡 Cool thing to note here is that, now Mac's are really good at running windows and that too for longer than a regular windows system.

please note that you might sometimes face issues while running some of the x86 softwares on an ARM chip but for me its been pretty smooth.

👉🏼 for more update, questions ping me on [twitter](https://twitter.com/milindsoorya).

👉🏼 checkout other articles on my [blog](https://www.milindsoorya.com/blog).

## Referneces

Many thanks to **[taitleaney](https://github.com/taitleaney)** for finding the workaround about changing the Linux version. You can follow the thread on [github](https://github.com/utmapp/UTM/issues/2682).
