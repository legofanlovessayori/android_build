# Android Build

Yeah, that's about it...  
Just whatever I use to build Android for my phones with.

## Setup

1. Ensure you are running a semi-recent Windows 11 version (using IoT LTSC 2024 in this case)
2. Enable WSL, Hyper-V, Windows Hypervisor Platform, and Virtual Machine Platform (will require a restart)
3. Install Ubuntu 22.04.3 from the [Microsoft Store](https://www.microsoft.com/store/productId/9PN20MSR04DW)
4. Run the Ubuntu-22.04.3 App from the Start Menu and configure it
5. Create a `.wslconfig` folder in your User folder and add this code (specific to my CPU, adjust based on your specs):

   ```ini
   [wsl2]
   memory=48GB
   processors=12
   swap=32GB
   ```

6. Terminate WSL and open it again:

   ```bash
   wsl --terminate Ubuntu-22.04.3
   ```

7. Update the Ubuntu WSL instance:

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

8. Install the dependencies:

   ```bash
   sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 \
   x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig \
   python-is-python3 repo git-lfs libssl-dev
   ```

9. Download and replace the `repo` applet in `/usr/bin` from [here](https://storage.googleapis.com/git-repo-downloads/repo)  
   (needed to avoid git-lfs errors because the repo binary version that the deb ships is horribly outdated)

## Building

1. Create your working directory on the VHDX (I would suggest using your WSL user's `/home` directory)
2. Set up repo / git:

   ```bash
   git config --global user.name [your user]
   git config --global user.email [your email]
   repo init -u https://github.com/PixelOS-AOSP/manifest.git -b fourteen --git-lfs
   ```

3. Create the `local_manifests` folder in the `.repo` directory and place the `roomservice.xml` in it:

   ```bash
   cd .repo
   git clone https://github.com/legofanlovessayori/android_build.git local_manifests
   ```

4. Initiate the source download:

   ```bash
   repo sync --force-sync --optimized-fetch -j `nproc`
   ```

5. Run the product configuration:

   ```bash
   . build/envsetup.sh
   lunch aosp_a71-ap2a-[buildtype]
   ```

6. Start the build:

   ```bash
   mka bacon
   ```
