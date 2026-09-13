# Como compilar este kernel

Ambiente: chroot Alpine 3.18 real (não proot-distro), Clang do próprio Alpine.

## Preparação obrigatória a cada sessão nova
export TMPDIR=/tmp
mkdir -p /tmp
chmod 1777 /tmp

## Comando de build completo
make CC=clang CROSS_COMPILE= O=out ARCH=arm64 \
  KCFLAGS="-w -Wno-error=int-conversion -Wno-error=implicit-function-declaration -Wno-error=incompatible-pointer-types" \
  -j2

## Por que essas flags
- KCFLAGS=-w: silencia avisos comuns (kernel escrito pra Clang 11, compilamos com Clang 16+)
- Wno-error=int-conversion / incompatible-pointer-types: necessário pro drivers/gpu/mediatek/gpu_rgx/.../mtk_mfgsys.c compilar (código antigo que o Clang novo trata como erro fatal por padrão)
- CONFIG_IKHEADERS já desabilitado no a12_defconfig (o cpio prebuilt da AOSP é x86, não roda em ARM64)
