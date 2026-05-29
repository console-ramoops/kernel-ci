# Android Kernel Builder

Build Android kernels with GitHub Actions. Reads kernel sources from a configuration file, builds them with cross-compilation toolchains, and supports KernelSU patching.

## Workflow

| Step                 | Description                                           |
| -------------------- | ----------------------------------------------------- |
| Install prerequisites| Install build dependencies                            |
| Setup AnyKernel3     | Clone AnyKernel3 for packaging                        |
| Clone kernel source  | Clone the kernel source repository                    |
| Get toolchains       | Clone cross-compilation toolchains                    |
| Set args             | Configure build parameters                            |
| Setup KernelSU       | Patch kernel with KernelSU (if enabled)               |
| Make defconfig       | Generate kernel config                                |
| Build kernel         | Compile the kernel                                    |
| Upload artifacts     | Upload Image, Image.gz, dtb, dtbo.img                 |
| Pack AnyKernel3.zip  | Package kernel into flashable zip                     |
| Create GitHub Release| Publish release with kernel zip                       |

## Configuration

Kernel configs live in `.github/workflows/repos.json`. Each entry:

```json
{
  "kernelSource": {
    "name": "kernel-name",
    "repo": "https://github.com/user/repo",
    "branch": "branch",
    "device": "device",
    "defconfig": "device_defconfig"
  },
  "withKernelSU": true,
  "toolchains": [
    {
      "repo": "https://gitlab.com/user/toolchain",
      "branch": "main",
      "name": "toolchain-name",
      "binPath": ["bin"]
    }
  ],
  "params": {
    "ARCH": "arm64",
    "CC": "toolchain-name/bin/clang",
    "externalCommand": {
      "CROSS_COMPILE": "toolchain-name/bin/aarch64-linux-gnu-",
      "CROSS_COMPILE_ARM32": "toolchain-name/bin/arm-linux-gnueabi-"
    }
  },
  "AnyKernel3": {
    "use": true,
    "release": true,
    "repo": "https://github.com/user/AnyKernel3",
    "branch": "master"
  }
}
```

### Fields

| Field          | Description                                                |
| -------------- | ---------------------------------------------------------- |
| `kernelSource` | Kernel repo name, URL, branch, device, defconfig           |
| `withKernelSU` | Enable KernelSU patching (true/false)                      |
| `toolchains`   | Array of toolchain repos with branch, name, binPath        |
| `params`       | ARCH, CC, and externalCommand (CROSS_COMPILE, etc.)        |
| `AnyKernel3`   | Flashable zip config: use, release, repo, branch           |

## Usage

1. Fork this repo
2. Edit `.github/workflows/repos.json` with your kernel config
3. Go to Actions > Build Kernel > Run workflow
4. Download artifacts when complete

## Local Testing

Use [nektos/act](https://github.com/nektos/act):

```sh
act --artifact-server-path /tmp/artifacts
```

## License

Licensed under [CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)
