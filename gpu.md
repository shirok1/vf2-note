# 给 VisionFive 2 安装 GPU 驱动程序

## 内核空间驱动

Buildroot 产物（[官方构建](https://github.com/starfive-tech/VisionFive2/releases)）携带的内核已包含，刷入 `sdcard.img`，替换 rootfs 时保留内核模块即可。

## 用户空间驱动

Imagination 并未**正式**开源 BXE 系列 GPU 的用户空间驱动。

### 预编译

在 [starfive-tech/soft_3rdpart](https://github.com/starfive-tech/soft_3rdpart) 的 `IMG_GPU/out` 目录中有预编译的驱动，提供以下目录结构：

<!-- <details> -->

```console
$ tree img-gpu-powervr-bin-1.17.6210866/
img-gpu-powervr-bin-1.17.6210866/
├── staging
│   └── usr
│       ├── include
│       │   ├── CL
│       │   │   ├── cl_egl.h
│       │   │   ├── cl_ext.h
│       │   │   ├── cl_gl.h
│       │   │   ├── cl.h
│       │   │   ├── cl_half.h
│       │   │   ├── cl_icd.h
│       │   │   ├── cl_platform.h
│       │   │   ├── cl_version.h
│       │   │   └── opencl.h
│       │   ├── drv
│       │   │   ├── CL
│       │   │   │   ├── cl_ext.h
│       │   │   │   ├── cl_img_external_semaphore.h
│       │   │   │   ├── cl_img_external_semaphore_sync_fd.h
│       │   │   │   ├── cl_img_safety_mechanisms.h
│       │   │   │   └── cl_img_semaphore.h
│       │   │   ├── EGL
│       │   │   │   └── eglext.h
│       │   │   ├── GLES
│       │   │   │   └── glext.h
│       │   │   └── GLES3
│       │   │       └── glimgext.h
│       │   ├── EGL
│       │   │   ├── eglext.h
│       │   │   ├── egl.h
│       │   │   └── eglplatform.h
│       │   ├── GLES
│       │   │   ├── glext.h
│       │   │   ├── gl.h
│       │   │   └── glplatform.h
│       │   ├── GLES2
│       │   │   ├── gl2ext.h
│       │   │   ├── gl2.h
│       │   │   └── gl2platform.h
│       │   ├── GLES3
│       │   │   ├── gl32.h
│       │   │   ├── gl3ext.h
│       │   │   ├── gl3.h
│       │   │   └── gl3platform.h
│       │   ├── KHR
│       │   │   └── khrplatform.h
│       │   ├── spirv
│       │   │   ├── extinst.glsl.std.450.grammar.json
│       │   │   ├── extinst.opencl.std.100.grammar.json
│       │   │   ├── GLSL.std.450.h
│       │   │   ├── OpenCL.std.h
│       │   │   ├── spirv.core.grammar.json
│       │   │   └── spirv.hpp
│       │   └── vulkan
│       │       ├── vk_icd.h
│       │       ├── vk_layer.h
│       │       ├── vk_platform.h
│       │       ├── vulkan_core.h
│       │       ├── vulkan.h
│       │       ├── vulkan_wayland.h
│       │       ├── vulkan_xcb.h
│       │       └── vulkan_xlib.h
│       └── lib
│           ├── libapicommon.a
│           ├── libcompute.a
│           ├── libcomputehwr.a
│           ├── libdbm.a
│           ├── libffcommon.a
│           ├── libffpfo.a
│           ├── libfftb.a
│           ├── libfftnl.a
│           ├── libGLESv1_CM_PVR_MESA.so -> libGLESv1_CM_PVR_MESA.so.1.17.6210866
│           ├── libGLESv1_CM_PVR_MESA.so.1.17.6210866
│           ├── libGLESv1_CM.so -> libGLESv1_CM_PVR_MESA.so.1.17.6210866
│           ├── libGLESv1_CM.so.1 -> libGLESv1_CM_PVR_MESA.so.1.17.6210866
│           ├── libGLESv2_PVR_MESA.so -> libGLESv2_PVR_MESA.so.1.17.6210866
│           ├── libGLESv2_PVR_MESA.so.1.17.6210866
│           ├── libGLESv2.so -> libGLESv2_PVR_MESA.so.1.17.6210866
│           ├── libGLESv2.so.2 -> libGLESv2_PVR_MESA.so.1.17.6210866
│           ├── libglslcompiler.so -> libglslcompiler.so.1.17.6210866
│           ├── libglslcompiler.so.1.17.6210866
│           ├── libglsllink.a
│           ├── libIMGeglsup.a
│           ├── libimgelf.a
│           ├── libOpenCL.so -> libPVROCL.so.1
│           ├── libOpenCL.so.1 -> libPVROCL.so.1
│           ├── libpds.a
│           ├── libperf_sim.a
│           ├── libpixfmts.a
│           ├── libpvr_dri_support.so -> libpvr_dri_support.so.1.17.6210866
│           ├── libpvr_dri_support.so.1.17.6210866
│           ├── libPVROCL.so -> libPVROCL.so.1.17.6210866
│           ├── libPVROCL.so.1 -> libPVROCL.so.1.17.6210866
│           ├── libPVROCL.so.1.17.6210866
│           ├── libPVRScopeServices.so -> libPVRScopeServices.so.1.17.6210866
│           ├── libPVRScopeServices.so.1.17.6210866
│           ├── libpvrtld.a
│           ├── librenderpass.a
│           ├── librogue2d.a
│           ├── libslotpacking.a
│           ├── libspecobj.a
│           ├── libsrv_um.so -> libsrv_um.so.1.17.6210866
│           ├── libsrv_um.so.1.17.6210866
│           ├── libsrv_um_static.a
│           ├── libsrvut.a
│           ├── libsutu_display.so -> libsutu_display.so.1.17.6210866
│           ├── libsutu_display.so.1.17.6210866
│           ├── libsync_linux.a
│           ├── libufwriter.so -> libufwriter.so.1.17.6210866
│           ├── libufwriter.so.1.17.6210866
│           ├── libuscasm.a
│           ├── libuscdisasm.a
│           ├── libusclink.a
│           ├── libusc.so -> libusc.so.1.17.6210866
│           ├── libusc.so.1.17.6210866
│           ├── libutil_linux.a
│           ├── libvertexunpack.a
│           ├── libVK_IMG.so -> libVK_IMG.so.1.17.6210866
│           ├── libVK_IMG.so.1 -> libVK_IMG.so.1.17.6210866
│           ├── libVK_IMG.so.1.17.6210866
│           ├── libvulkan-1.so -> libvulkan.so.1
│           ├── libvulkan.so -> libvulkan.so.1.17.6210866
│           ├── libvulkan.so.0 -> libvulkan.so.1
│           ├── libvulkan.so.1 -> libvulkan.so.1.17.6210866
│           ├── libvulkan.so.1.17.6210866
│           └── pkgconfig
│               ├── glesv2.pc
│               ├── glesv3.pc
│               └── vulkan.pc
└── target
    ├── etc
    │   ├── init.d
    │   │   └── rc.pvr
    │   ├── OpenCL
    │   │   └── vendors
    │   │       └── IMG.icd
    │   └── vulkan
    │       └── icd.d
    │           └── icdconf.json
    ├── lib
    │   └── firmware
    │       ├── rgx.fw.36.50.54.182
    │       └── rgx.sh.36.50.54.182
    └── usr
        ├── lib
        │   ├── libGLESv1_CM_PVR_MESA.so -> libGLESv1_CM_PVR_MESA.so.1.17.6210866
        │   ├── libGLESv1_CM_PVR_MESA.so.1.17.6210866
        │   ├── libGLESv1_CM.so -> libGLESv1_CM_PVR_MESA.so.1.17.6210866
        │   ├── libGLESv1_CM.so.1 -> libGLESv1_CM_PVR_MESA.so.1.17.6210866
        │   ├── libGLESv2_PVR_MESA.so -> libGLESv2_PVR_MESA.so.1.17.6210866
        │   ├── libGLESv2_PVR_MESA.so.1.17.6210866
        │   ├── libGLESv2.so -> libGLESv2_PVR_MESA.so.1.17.6210866
        │   ├── libGLESv2.so.2 -> libGLESv2_PVR_MESA.so.1.17.6210866
        │   ├── libglslcompiler.so -> libglslcompiler.so.1.17.6210866
        │   ├── libglslcompiler.so.1.17.6210866
        │   ├── libOpenCL.so -> libPVROCL.so.1
        │   ├── libOpenCL.so.1 -> libPVROCL.so.1
        │   ├── libpvr_dri_support.so -> libpvr_dri_support.so.1.17.6210866
        │   ├── libpvr_dri_support.so.1.17.6210866
        │   ├── libPVROCL.so -> libPVROCL.so.1.17.6210866
        │   ├── libPVROCL.so.1 -> libPVROCL.so.1.17.6210866
        │   ├── libPVROCL.so.1.17.6210866
        │   ├── libPVRScopeServices.so -> libPVRScopeServices.so.1.17.6210866
        │   ├── libPVRScopeServices.so.1.17.6210866
        │   ├── libsrv_um.so -> libsrv_um.so.1.17.6210866
        │   ├── libsrv_um.so.1.17.6210866
        │   ├── libsutu_display.so -> libsutu_display.so.1.17.6210866
        │   ├── libsutu_display.so.1.17.6210866
        │   ├── libufwriter.so -> libufwriter.so.1.17.6210866
        │   ├── libufwriter.so.1.17.6210866
        │   ├── libusc.so -> libusc.so.1.17.6210866
        │   ├── libusc.so.1.17.6210866
        │   ├── libVK_IMG.so -> libVK_IMG.so.1.17.6210866
        │   ├── libVK_IMG.so.1 -> libVK_IMG.so.1.17.6210866
        │   ├── libVK_IMG.so.1.17.6210866
        │   ├── libvulkan-1.so -> libvulkan.so.1
        │   ├── libvulkan.so -> libvulkan.so.1.17.6210866
        │   ├── libvulkan.so.0 -> libvulkan.so.1
        │   ├── libvulkan.so.1 -> libvulkan.so.1.17.6210866
        │   └── libvulkan.so.1.17.6210866
        └── local
            └── bin
                ├── hwperfbin2jsont
                ├── hwperfjsonmerge.py
                ├── ocl_extended_test
                ├── ocl_unit_test
                ├── pdump
                ├── pdump_optimise.py
                ├── pvrdebug
                ├── pvrhtb2txt
                ├── pvrhtbd
                ├── pvrhwperf
                ├── pvrlogdump
                ├── pvrlogsplit
                ├── pvr_memory_test
                ├── pvr_mutex_perf_test_mx
                ├── pvrsrvctl
                ├── pvrtld
                ├── rgx_blit_test
                ├── rgx_compute_test
                ├── rgx_kicksync_test
                ├── rgx_triangle_test
                ├── rgx_twiddling_test
                ├── rogue2d_fbctest
                └── rogue2d_unittest

32 directories, 173 files
```

<!-- </details> -->

看起来像是 Buildroot 的编译产物。

其中，Imagination 的内部专有程序库有：

- `libsrv_um.so`: 提供 `PVRSRV.*` 与 `RGX.*` 系列函数
  - 没有动态链接到这些库
- `libusc.so`: 提供 `PVRUniFlex.*` 系列函数
  - 动态链接到了 `libsrv_um.so`
- `libpvr_dri_support.so`: 提供 `KEGL.*` 与 `TQ.*` 系列函数
  - 动态链接到了 `libsrv_um.so`
- `libufwriter.so`: 提供 `BIL.*` 系列函数
  - 动态链接到了 `libsrv_um.so` 和 `libusc.so`
  - 导出函数中有一些用内部结构修饰的名字，看样子是用 C++ 写的
- `libsutu_display.so`: 提供 `sutu_Display.*` 系列函数
  - 动态链接到了 `libsrv_um.so`
- `libPVRScopeServices.so`: 提供返回实际函数地址的 `pvrssGetProcAddress` 函数
  - 没有动态链接到这些库
  - 应该是性能调试用的
- `libglslcompiler.so`: GLSL 编译器？
  - 动态链接到了 `libsrv_um.so` 和 `libusc.so`
  - 导出了 `GLSLCompileToUniflex` 函数，将 GLSL 转译成 Imagination 自己的 IL？

实现了标准 API，应当被外部程序加载的专有程序库有：

- `libPVROCL.so`: 对 OpenCL 的专有实现
  - 动态链接到了 `libsrv_um.so` 和 `libusc.so`
  - `clGetDeviceInfo` 中显示其硬编码了以下信息：
    - PowerVR B-Series BXE-4-32
    - Imagination Technologies
    - EMBEDDED_PROFILE
    - OpenCL 3.0
    - OpenCL C 1.2
    - SPIR-V_1.2
  - 似乎既可以直接作为 `libOpenCL.so` 使用，也可以作为 `opencl-driver`（即被 `ocl-icd` 加载）？
- `libGLESv1_CM_PVR_MESA.so`: 对 OpenGL ES 1 的专有实现？
  - 动态链接到了 `libpvr_dri_support.so`、`libsrv_um.so` 和 `libusc.so`
  - `glGetString` 中显示其硬编码了以下信息：
    - OpenGL ES-CM 1.1
    - Imagination Technologies
    - PowerVR B-Series BXE-4-32
- `libGLESv2_PVR_MESA.so`: 对 OpenGL ES 2/3 的专有实现？
  - 动态链接到了 `libglslcompiler.so`、`libpvr_dri_support.so`、`libsrv_um.so` 和 `libusc.so`
  - `glGetString` 中显示其硬编码了以下信息：
    - OpenGL ES 3.2 build 1.17@6210866
    - Imagination Technologies
    - PowerVR B-Series BXE-4-32
    - OpenGL ES GLSL ES 3.20 build 1.17@6210866
- `libVK_IMG.so`: Vulkan ICD 驱动
  - 动态链接到了 `libsrv_um.so`、`libufwriter.so` 和 `libusc.so`
  - 通过 `etc/vulkan/icd.d/icdconf.json` 来配置加载
- `libvulkan.so`: ~~应该是没“加料”的 Khronos 官方 ICD loader~~ 大意了，仔细观察发现其依然硬编码了 `libVK_IMG.so` 的名称，硬编码的日志格式和 Khronos 的不同，甚至可能和 WSI 的加载有关！

### Mesa 分支

前面的闭源驱动提供了 OpenGL ES 和 Vulkan ICD，但仍然需要我们自行编译 Mesa 来提供 OpenGL 无印和 EGL、GLX 的支持。

Starfive 的开发人员直接往 Buildroot 的 Mesa 打上了 Imagination 给他们的 [68 个 patch](https://github.com/starfive-tech/buildroot/tree/JH7110_VisionFive2_devel/package/mesa3d)，通过动态加载 `libpvr_dri_support.so` 在 DRI 框架下实现了 OpenGL 驱动（见 `drivers/dri/pvr/pvrcompat.c`）。不过 Mesa 在 21.3 版本后只保留了 Gallium 框架的驱动，把 DRI 框架给删了，所以这些 patch 后续也只能合并下 Amber 分支的更新（约等于没更新）。尝试了下在本地手动合并，结果在 [这里](https://github.com/shirok1/mesa/tree/jh7110-pvr)。另外，这些 patch 还包含了 Vulkan WSI 的实现（和窗体系统的集成）。

题外话：Imagination 在 2022 年 3 月 22 日提交（准确的说是被 Mesa 方面合并）了 [一份 PowerVR Vulkan 驱动](https://gitlab.freedesktop.org/mesa/mesa/-/merge_requests/15243)，但是这个驱动**只**支持 GX6250、AXE-1-16M 和 BXS-4-64 三款 GPU，跟 JH7110 没有任何关系。

### 给 Distro 打包

为了更好的划分职能，将以上这些库分为以下包：

- `pvr-ddk-vf2`：Starfive 提供的闭源程序库。在一些提倡细粒度打包的发行版还可以考虑进一步拆分成：
  - `pvr-ddk-vf2-base`：基础程序库，包含 `libsrv_um.so`、`libusc.so`、`libpvr_dri_support.so`、`libufwriter.so`、`libsutu_display.so`、`libPVRScopeServices.so`、`libglslcompiler.so`
  - `pvr-ddk-vf2-cl`：`libPVROCL.so`
  - `pvr-ddk-vf2-gles`：`libGLESv1_CM_PVR_MESA.so`、`libGLESv2_PVR_MESA.so`
  - `pvr-ddk-vf2-vulkan`：`libVK_IMG.so`
  - `pvr-ddk-vf2-devel`：`staging` 目录下对用户没什么用的静态库和独占头文件
- `mesa-vf2`：Mesa 分支的构建

#### Arch Linux

分别给闭源程序库和 Mesa 分支写了一份 `PKGBUILD`，其中 Mesa 分支的构建参考了 AUR 上的 `mesa-git`，把源换成合并过 Imagination 的 patch 的 repo。

Arch Linux 通常情况下使用 `libglvnd` 来实现不同 OpenGL 提供者之间的切换，但是目前 `libglvnd` 加载 `libGLESv2_PVR_MESA.so` 等时有问题，无法正常工作，所以需要改变 Mesa 编译配置，使其直接构建系统级的 `libGL.so`（而不是 `libGL_mesa.so`），另外还需要更改包的 `provides` 属性。对于闭源程序库，把覆盖系统级库的软链接（如 `libGLESv1_CM.so` -> `libGLESv1_CM_PVR_MESA.so.1.17.6210866`）单独做成包，以 `pvr-ddk-vf2-syses` 等命名。

目前 `rgx_triangle_test` 等闭源测试可以正常运行，但 EGL 组件有问题，不能正常创建 Wayland 上下文：
- glmark2-es2-drm 提示“Failed to find suitable EGL config”
- eglinfo 提示“Wayland platform: eglinfo: eglInitialize failed”
- sway 日志显示其试图向某个系统设备（具有 EGL client extension）查询 DRM 文件地址然后出错，但能正常加载 Imagination 的 OpenGL ES 驱动，然后指出这个驱动不支持 `GL_EXT_unpack_subimage` 并因此“Failed to create GLES2 renderer”，回退到 pixman CPU 渲染
- weston 日志（完整命令行为 `WAYLAND_DEBUG=1 weston --continue-without-input --xwayland --debug --logger-scopes=log,drm-backend`）无明显问题
- glmark2 非 drm 版本因为 llvmpipe 的问题会爆 Segmentation fault

Vulkan 驱动可以正确加载，但某个测试显示缺少 WSI 扩展（用于在 Wayland 等中创建 Vulkan 上下文，目前由 Mesa 分支提供），可能是在 pull 上游 amber 时不慎导致合并出错。因为上游 Mesa [更换了编写 WSI 扩展的框架](https://gitlab.freedesktop.org/mesa/mesa/-/merge_requests/13234>)，所以要靠自己（依靠臆想）理解进行 [修正](https://github.com/shirok1/mesa/commit/5e48a6ba47dbcd89d0fb5594e8c6b7c7345b9b6c)，有待用回 Imagination 提供的原始版本测试。

如果可以，希望能够用回 `libglvnd`。

#### Gentoo Linux

(TODO)
