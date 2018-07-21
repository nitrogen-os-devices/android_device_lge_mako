# Follow these steps to build OREO for mako with Lineage OS mako trees (branch: lineage-15.1)
# Force Sync to latest Nitrogen OS ROM source (branch: o2)

# Changes needed for NFC to work
cd hardware/interfaces/  
git remote add nitin https://github.com/nitin1438/android_hardware_interfaces  
git pull nitin o2  

# Changes needed for GPS to work properly
cd ~/nitrogen/hardware/qcom/gps/  
git remote add gps https://github.com/nitins-taimen/android_hardware_qcom_gps  
git pull gps o2  

# To fix build error: https://pastebin.com/9Jmd0bhL
# external/strace/xlat/v4l2_memories.h:23:8: error: use of undeclared identifier 'V4L2_MEMORY_DMABUF'; did you mean 'V4L2_MEMORY_MMAP'?
cd ~/nitrogen/external/strace/  
git revert dc75b01004a0588c1eb3bc26d7248a6e473b2cdd  

# Clone device, kernel and vendor
cd ~/nitrogen/  
git clone https://github.com/nitrogen-os-devices/android_device_lge_mako -b o2-los-test device/lge/mako/  
git clone https://github.com/nitrogen-os-devices/android_kernel_lge_mako -b o2-los-test kernel/lge/mako/  
git clone https://github.com/TheMuppets/proprietary_vendor_lge -b lineage-15.1 vendor/lge/  

# Clone audio HAL (note the destination folder)  
rm -rf hardware/qcom/audio  
git clone https://github.com/LineageOS/android_hardware_qcom_audio -b lineage-15.1 hardware/qcom/audio/default  

# To fix build error:
# build/core/binary.mk:1481: error: device/lge/mako/camera/Android.mk: camera.mako: C_INCLUDES must be under the source or output directories: /mm-core/inc /libstagefrighthw /libgralloc /libgenlock.

Cherry-pick https://github.com/nitins-taimen/android_build_make/commit/05a9991f593a70bab87a995708f35455d0525aca  	
Cherry-pick https://github.com/nitins-taimen/android_vendor_nitrogen/commit/e9b46a41fd1d37853d8e91d49d828af4828a6045  

# Remove keymaster, media & display HALs & clone from Lineage OS  
rm -rf hardware/qcom/keymaster/ hardware/qcom/media hardware/qcom/display  
