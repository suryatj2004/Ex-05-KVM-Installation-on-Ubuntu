# Ex-05-KVM-Installation-on-Ubuntu
## Aim:
To implement CPU Virtualization KVM is installed in ubuntu 12.04 and higher Version.

## Procedures:
By default, Linux Operating system provides within the kernel virtualization capabilities i.e. Kernel Virtual Machine (kvm). Before enabling the kvm feature, you will first need to ensure that you meet the hardware and software requirements.

### Step 1: Verifying that CPU support virtualization

To check that your computer support virtualization, you can issuse one of the following commands :

egrep -c ‘(vmx |svm’) /proc/cpuinfo

If this command returns the value 0, the cpu does not support hardware virtualization. If the command returns value 1 or greater, your cpu is capable of running virtualization software. The following screenshot shows the output of the

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/41b4b255-6f2c-4b50-a22a-0b7ca28f6231)

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/08c6603f-9019-441c-9111-7637edbf271c)

 
Another way to check would be to use the command kvm-ok.
I issue this (kvm -ok)command on my system as well and discovered that I was missing some packages (cpu checker). I had to install this package first in order to be able to run the kvm-ok command (see the screenshot below).

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/87a2d447-6bb8-4cbb-ae7a-cf6a94fbb7d7)

 

### Note :
If you receive a message similar to “INFO: your cpu does not support KVM extensions, KVM acceleration can not be used”, you might still be able to run virtual machines but the performance will not be really good since you will not be using KVM extensions.
If you receive a message similar to KVM Acceleration cannot be used might means that hardwared- assisted virtualization capabilities is present on the system but not activitated in the BIOS

### Checking the CPU architecture (32-bit or 64-bit)

We would recommend to run a 64-bit version of Ubuntu 12.04 simply because you will be able to host 32-bit and 64-bit virtual machines. Knowing that the new Microsoft Operating system only support 64-bit, this would make sense. To check this, you can simply try to install ubuntu 64-bit on your system, if the 64-bit architecture is not supported, you will get an error message and the installation process will be stopped.

Another way (if you have already installed Ubuntu) would be to issue the following command

egrep -c ‘lm’ /proc/cpuinfo

If the output is 0, you are not using a 64-bit CPU. If the Output is 1 or greater, you are running
64-bit CPU and can proceed with the KVM installation

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/3d489264-34b2-434b-a9ee-29ab11c68491)



Note: For your information, you can have kvm installed on a 32-bit system but will be then able to run only 32-bit guests

## Verifying that Operating system version

Using the system monitor interface or system details in ubuntu 12.04 , you can easily check that the operating system you are running is 32-bit or 64-bit. Whatever the desktop interface you are running, type in the dash/activities, system and select system monitor. In the sytem tab, you can see the version of the operating system.
For the geek, you can also using the command line and digit the following command line (see screenshot)

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/98714f0d-e79b-4bd7-af9f-4a091e3a87d8)



If the output is something like x86_x64, you are running a 64-bit
 
## Installating KVM packages

If you reach this section, we assume that you meet the basic requirements in order to have KVM software running. It’s time to download and install the kvm packages. With Ubuntu, this is quite easy. You can use the Ubuntu software GUI based interface or you can use the command line
If you prefer to use the GUI,
•	Launch the Ubuntu Software Center, and in the search box type qemu-kvm. Click on the package.The package is highlighted and you will see two buttons : more and Install. Click
on more button.

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/b8673344-8d38-465f-aa30-45ec05f98810)



Scroll down and select the 2 additional Add-ons

You are ready to install the package. Press the Install button (scroll up to see it)

Check that the Bridge-utils package has been installed as well. From the ubuntu Software Center, type in the search box bridge-utils and you should see it already installed. If not, install it

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/956ef8c5-d02b-48d4-a4d6-03d592afadd1)
 
 
If you prefer to use the command line ( slightly faster), simply type the following command and wait for the installation to complete.

sudo apt-get install qemu-kvm libvirt-bin bridge-utils

## Installating Management Interface

There are different management tools available with KVM virtualization solution. For this post, we will simply install the ‘de facto’ standard virtual Machine Manager (VMM). To perform the installation, you can use the Ubuntu software Center. In the search box, type virt and you should see in the list the VMM package. click on it and press the install button
or
You can perform the same installation operation using the command line by issuing the following command
sudo apt-get install virt-manager
After the installation complete, you can try to connect to the management interface (by typing in the Dash/activities search box virtual. the application icons will be displayed. Click on it.
 
![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/1fc9249e-b464-4325-b3cd-b33df93725fa)


The application will start but you will get immediately an error message. (see screenshot)

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/76acace9-6468-47b1-a7d7-53a388ab0e00)



Actually, you need to create a new user on your system and to add this user to a specific group (called libvirtd). This will basically grant the right to use the Virt-manager interface. With Ubuntu 12.04, it simply easier to perform the group creation from the command line. By default, Ubuntu
12.04 does not come with a utility to manage groups.
To add your user account (for example griffon) into the group libvirtd, you would type
•	sudo adduser griffon libvirtd
 
Note : If you want, you can also install the gnome users and group interface back into Ubuntu by installing the package gnome-system-tools. When installed, you should have a Users and Groups interface that can be used from the GUI.
You will need to logoff and login again in order to have the changes applied. Try to launch the virt- manager application again, and you should be able to have it started. You are now ready to create your first virtual machine using KVM as Hypervisor.

## Creating your First virtual machine

It’s time to create you first virtual machine on Ubuntu when using KVM as your preferred Hypervisor. At this stage, you have launched the Virtual Machine Manager and you should see a dialog box similar to this one

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/26df0287-1d7a-492a-9453-86271c8e0f2c)


click on the highlighted computer icon and the New virtual machine wizard starts.

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/13d4807f-af86-44c3-bb64-b842fc57ff89)


Provide the information and Press Forward.
In the following screen, select the installation source and the type of virtual machine that you want to install. Press Forward

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/7b8043c7-f13e-4103-9703-9efb696ed978)

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/b8de195a-745f-493c-b856-0671207fb44f)


In the next screen, simply specify CPU and Memory information. Press Forward
 
In the next screen, provide the information about the virtual disk to created and Press Forward

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/e494eb60-8010-4303-b607-ad459af4dc22)


In the final screen, provide the information about the Virtual networking and Press Finish

![image](https://github.com/Kaviarasu510/Ex-05-KVM-Installation-on-Ubuntu/assets/119392695/073e48d0-004e-4a21-b5fa-06cc0068bc7a)



At this stage, you will need to perform the installation of your operating system and you should be ready to go for the rest of your journey

## Result:
Thus, the implementation to support CPU Virtualization KVM is installed in ubuntu
successfully.
