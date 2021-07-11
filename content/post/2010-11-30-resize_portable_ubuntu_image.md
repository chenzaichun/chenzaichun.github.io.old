+++
categories = ["Linux", "tools"]
comments = true
date = "2010-11-30T17:32:11+08:00"
excerpt = "When you first download and install Portable Ubuntu, you're going to be left with only about 500 MB of space on your Linux file system. This isn't enough to do much of anything with, so the first order of business is going to be increasing the size of the image.\n<br /><br /><br />There are instructions for doing so at the Portable Ubuntu website, but for convenience sake I am repeating them here."
published = true
status = "publish"
tags = ["Linux", "ubuntu", "Windows"]
title = "Resize a Portable Ubuntu Image"
type = "post"
description = ""
+++

<a href="http://blog.ruski.co.za/page/Resize-a-Portable-Ubuntu-Image.aspx">http://blog.ruski.co.za/page/Resize-a-Portable-Ubuntu-Image.aspx</a>
 When you first download and install <a style="text-decoration: none; color: #336699; border: 0px initial initial;" href="http://portableubuntu.sourceforge.net/" target="_blank">Portable Ubuntu</a>, you're going to be left with only about 500 MB of space on your Linux file system. This isn't enough to do much of anything with, so the first order of business is going to be increasing the size of the image.
There are instructions for doing so at the <a style="text-decoration: none; color: #336699; border: 0px initial initial;" href="http://portableubuntu.sourceforge.net/index.php?section=documentation" target="_blank">Portable Ubuntu website</a>, but for convenience sake I am repeating them here.
The process is fairly straight forward and basically consists of creating a new larger image file and then copying the existing image over to it. Once that is done we make the new image our primary partition.
To start, make sure that you're not currently running Portable Ubuntu. Exit it if you are.
Open a Windows Command Prompt window by clicking Start -> Run and entering "cmd".
Change to the directory in which you've installed Portable Ubuntu. For the purposes of this article we'll assume that is C<strong>:Portable_Ubuntu</strong>.
Change to the images folder:
<div class="code" style="font-size: 12px; color: black; font-family: Consolas, 'Courier New', Courier, monospace; background-color: #f1f1f1; line-height: normal; padding: 5px;">C:Portable_Ubuntu<span class="kwrd" style="color: #0000ff;">></span> cd images</div>
Then we create a new image using the <strong>fsutil</strong> utility. The number at the end is the size of the new image you want to create in bytes. I've opted to create a 4 GB image in the example below.
<div class="code" style="font-size: 12px; color: black; font-family: Consolas, 'Courier New', Courier, monospace; background-color: #f1f1f1; line-height: normal; padding: 5px;">C:Portable_Ubuntuimages<span class="kwrd" style="color: #0000ff;">></span> fsutil file createnew rootfs_.img 4294967296</div>
Next we must mount the new image in our Ubuntu instance. Edit the <strong>portable_ubuntu.conf</strong>file in the C<strong>:Portable_Ubuntuconfig</strong> directory using Notepad. You can use explorer for this or the commands below.
<div class="code" style="font-size: 12px; color: black; font-family: Consolas, 'Courier New', Courier, monospace; background-color: #f1f1f1; line-height: normal; padding: 5px;">C:Portable_Ubuntuimages<span class="kwrd" style="color: #0000ff;">></span> cd .. C:Portable_Ubuntu<span class="kwrd" style="color: #0000ff;">></span> cd config C:Portable_Ubuntuconfig<span class="kwrd" style="color: #0000ff;">></span> notepad portable_ubuntu.conf</div>
Replace the line in the config file that starts with "<strong>cobd3=</strong>" with the following in notepad:<strong>cobd3=imagesrootfs_.img</strong>
Save the file and close Notepad.
Now we need to run Portable Ubuntu again so that we can copy the contents of the original image over to the new image.
Run Portable Ubuntu and open a Terminal window when it is ready.
Type the following to make sure you have root access:
<div class="code" style="font-size: 12px; color: black; font-family: Consolas, 'Courier New', Courier, monospace; background-color: #f1f1f1; line-height: normal; padding: 5px;">$ sudo su</div>
You will be asked for a password. The default password for Portable Ubuntu is <strong>123456</strong>
Next we'll run the Data Definition (<strong>dd</strong>) tool on linux to create a raw copy of the original image. Don't screw this up as the dd tool has earned the nickname "Data Destroyer" for a good reason.
<div class="code" style="font-size: 12px; color: black; font-family: Consolas, 'Courier New', Courier, monospace; background-color: #f1f1f1; line-height: normal; padding: 5px;">$ dd if=/dev/cobd0 of=/dev/cobd3</div>
Once that is completed we'll have an exact copy of our data from the original image to the new larger image.
Next we'll do a file system check on the new image to make sure it's good.
<div class="code" style="font-size: 12px; color: black; font-family: Consolas, 'Courier New', Courier, monospace; background-color: #f1f1f1; line-height: normal; padding: 5px;">fsck.ext3 -f /dev/cobd3</div>
Then we resize the partition on the new image to take up all the space.
<div class="code" style="font-size: 12px; color: black; font-family: Consolas, 'Courier New', Courier, monospace; background-color: #f1f1f1; line-height: normal; padding: 5px;">resize2fs -f /dev/cobd3</div>
Now that that is done, we can make the new image our main image. To do this, exit the running Portable Ubuntu image.
Go back to the images folder and rename <strong>rootfs.img</strong> to <strong>rootfs.img.old</strong> and <strong>rootfs_img</strong> to<strong>rootfs.img</strong>.
Remove the line that you had modified in the <strong>portable_ubuntu.conf</strong> file (<strong>cobd3=imagesrootfs_.img</strong>) using Notepad.
Now you can run Portable Ubuntu again and you'll have a useful amount of free space left. You can delete the <strong>rootfs.img.old</strong> file if everything is working properly.
<!--more-->