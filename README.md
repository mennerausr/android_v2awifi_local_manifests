# Build /e/OS 1.9 based on LineageOS 17.1 for v2awifi Samsung SM-T900

    #!/bin/bash
    
    export USE_CCACHE=1
    export CCACHE_DIR=$HOME/ccache
    export NINJA_ARGS=-j2
    export _JAVA_OPTIONS=-Xmx4g
    
    PATH=$PWD/../bin:$PATH
    
    rm -rf .repo/local_manifests/*
    mkdir -p .repo/local_manifests/
    #rm -rf  */samsung*/ .repo/project*/*/samsung* .repo/project-objects/exynos5420/android_*_samsung_* out/soong/.glob/*/samsung
    #rm -rf out
    
    cat > .repo/local_manifests/roomservice.xml <<\EOF
    <?xml version="1.0" encoding="UTF-8"?>
    <manifest>
      <project name="exynos5420/android_device_samsung_universal5420-common" path="device/samsung/universal5420-common" remote="github" />
      <project name="mennerausr/android_device_samsung_v2a-common" path="device/samsung/v2a-common" remote="github" />
      <project name="exynos5420/android_device_samsung_v2awifi" path="device/samsung/v2awifi" remote="github" />
      <project name="mennerausr/android_kernel_samsung_exynos5420" path="kernel/samsung/exynos5420" remote="github" />
      <project name="exynos5420/android_vendor_samsung_universal5420-common" path="vendor/samsung/universal5420-common" remote="github"/>
      <project name="exynos5420/android_vendor_samsung_v2a-common" path="vendor/samsung/v2a-common" remote="github"/>
      <project name="exynos5420/android_vendor_samsung_v2awifi" path="vendor/samsung/v2awifi" remote="github" />
      <project name="lineageos/android_hardware_samsung" path="hardware/samsung" remote="github" />
      <project name="lineageos/android_hardware_samsung_slsi_exynos" path="hardware/samsung_slsi/exynos" remote="github" />
      <project name="lineageos/android_hardware_samsung_slsi_exynos5" path="hardware/samsung_slsi/exynos5" remote="github" />
      <project name="lineageos/android_hardware_samsung_slsi_openmax" path="hardware/samsung_slsi/openmax" remote="github" />
      <project name="lineageos/android_device_samsung_slsi_sepolicy" path="device/samsung_slsi/sepolicy" remote="github" />
      <project name="exynos5420/android_hardware_samsung_slsi_exynos5420" path="hardware/samsung_slsi/exynos5420" remote="github"/>
    </manifest>
    EOF
    
    test -x ../bin/repo ||\
    curl https://storage.googleapis.com/git-repo-downloads/repo > ../bin/repo
    chmod a+x ../bin/repo
    PATH=$PWD/../bin:$PATH

    repo init -u https://gitlab.e.foundation/e/os/releases.git -g "default,-darwin" -b refs/tags/v1.9-q
    
    repo sync --force-sync
    repo forall -c 'git lfs pull'
    #or: ( cd prebuilts/prebuiltapks && git clone https://gitlab.e.foundation/e/os/android_prebuilts_prebuiltapks_lfs )
    
    echo help: && hmm
    
    test -f build/envsetup.sh || exit
    
    . build/envsetup.sh
    
    #lunch lineage_v2awifi-userdebug
    brunch v2awifi
    
