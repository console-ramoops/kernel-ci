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

## Usage

1. Fork this repo
2. Edit `.github/workflows/repos.json` with toolchain, KernelSU, and AnyKernel3 settings
3. Go to Actions > Build Kernel > Run workflow
4. Fill in the kernel source fields in the form (name, repo, branch, device, defconfig)
5. Download artifacts when complete

## Local Testing

Use [nektos/act](https://github.com/nektos/act):

```sh
act --artifact-server-path /tmp/artifacts
```

## License

Licensed under [CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)
