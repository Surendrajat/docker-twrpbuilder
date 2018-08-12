## TWRP-Builder Docker  [![Docker Build Status](https://img.shields.io/docker/build/surendrajat/twrp-builder.svg)](https://hub.docker.com/r/surendrajat/twrp-builder)
> A minimal docker image based on `debian:stretch-slim` & used by `TWRPBuilder Project`  for CI/CD of unofficial twrp builds

### Usage:
>- **`GitLab CI`**
>> `.gitlab-ci.yml`
>> ```yml
>>image: surendrajat/twrp-builder:latest
>>build:
>>  script:
>>  - apt-get -yy update && apt-get -y install aria2
>>  - aria2c -x16 -s16 -q https://github.com/TwrpBuilder/twrp-sources/releases/download/omni_twrp-5.1.1-cleaned/omni_twrp-5.1.1_cleaned.tar.xz
>>    -d $HOME/ -o twrp.tar.xz
>>  - tar -xJf twrp.tar.xz --directory $HOME/twrp/ && rm twrp.tar.xz
>>  - cd $HOME/twrp/ && mkdir -p device/samsung/o7prolte && mv /builds/Surendrajat/andorid_device_samsung_o7prolte/* device/samsung/o7prolte/
>>  - git clone https://github.com/TwrpBuilder/device_generic_twrpbuilder.git device/generic/twrpbuilder
>>  - git clone https://github.com/omnirom/android_bootable_recovery.git bootable/recovery --depth=1
>>  - source build/envsetup.sh && lunch omni_o7prolte-eng && make -j16 recoveryimage
>>```
>- **`Travis CI`**
>> `.travis.yml`
>>```yml
>>sudo: required
>>services:
>>  - docker
>>before_install:
>>  - docker pull surendrajat/twrp-builder:latest
>>before_script:
>>  - cd $HOME && mkdir twrp
>>  - wget -q https://github.com/TwrpBuilder/twrp-sources/releases/download/omni_twrp-5.1.1-cleaned/omni_twrp-5.1.1_cleaned.tar.xz -O $HOME/twrp.tar.xz
>>  - tar -xJf twrp.tar.xz --directory $HOME/twrp/ && rm twrp.tar.xz
>>script:
>>  - cd $HOME/twrp/ && git clone https://github.com/surendrajat/android_device_samsung_o7prolte.git device/samsung/o7prolte
>>  - git clone https://github.com/TwrpBuilder/device_generic_twrpbuilder.git device/generic/twrpbuilder
>>  - git clone https://github.com/omnirom/android_bootable_recovery.git bootable/recovery --depth=1
>>  - |
>>    docker run --rm -i -v "$(pwd):/root/twrp/:rw,z" surendrajat/twrp-builder bash << EOF
>>    cd /root/twrp
>>    source build/envsetup.sh && lunch omni_o7prolte-eng && make -j16 recoveryimage
>>    exit
>>    EOF
>>```
