## mt8173-sdk
```
SDK for Mediatek MT8173 &amp; MT8176 SoC
```
# Building Manual

## Installing Java and Tools

#### Add Java repository if no one is installed:
```
sudo apt-add-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```
#### Then install all other needed tools:
```
sudo apt-get update && sudo apt-get install git-core gnupg flex bison gperf libsdl1.2-dev libesd0-dev libwxgtk2.8-dev squashfs-tools build-essential zip curl libncurses5-dev zlib1g-dev openjdk-8-jre openjdk-8-jdk pngcrush schedtool libxml2 libxml2-utils xsltproc lzop libc6-dev schedtool g++-multilib lib32z1-dev lib32ncurses5-dev lib32readline-gplv2-dev gcc-multilib maven tmux screen w3m ncftp && sudo apt install ccache && sudo apt install liblz4-tool
```
## Downloading Source

#### Installing repo tools:
```
mkdir ~/bin
PATH=~/bin:$PATH
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```
#### Create source directory 
```
mkdir ~/mt8173
cd ~/mt8173
```
#### Configure Git
```
git config --global user.email "email@example.com"
git config --global user.name "git_username"
```
#### Initialize Repo:
```
repo init -u git://github.com/mt8173/mt8173-manifest.git -b master
```
#### Sync sources (it may require some hours)
```
repo sync -c -f -q -j8 --force-sync --no-clone-bundle --no-tags --optimized-fetch --prune
``` 
## Building Source

#### Enable ccache for Improve speed of build
```
export CCACHE_DIR=./.ccache
ccache -C
export USE_CCACHE=1
export CCACHE_COMPRESS=1
prebuilts/misc/linux-x86/ccache/ccache -M 50G
```
#### Clean your build
```
make clean && make clobber
```
#### To avoid jack errors
```
export ANDROID_JACK_VM_ARGS="-Xmx4096m -Xms512m -Dfile.encoding=UTF-8 -XX:+TieredCompilation"
./prebuilts/sdk/tools/jack-admin kill-server
./prebuilts/sdk/tools/jack-admin start-server
```
#### Start building :) 
```
sh build/envsetup.sh 
lunch full_wisky8176_tb_n-user
make -j8 | tee build.log
```
#### System firmware directory:
```
out/target/product/wisky8176_tb_n/
```
#### Copy system firmware script:
```
wisky/tools/copyimages.sh
```
