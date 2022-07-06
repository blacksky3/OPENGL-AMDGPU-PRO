# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <blacksky3@tuta.io> <https://github.com/blacksky3>
#Credits: Janusz Lewandowski <lew21@xtreeme.org>
#Credits: David McFarland <corngood@gmail.com>
#Credits: Andrew Shark <ashark @at@ linuxcomp.ru>


#Upstream pages
#https://aur.archlinux.org/pkgbase/amdgpu-pro-installer
#https://github.com/Ashark/archlinux-amdgpu-pro
#https://repo.radeon.com/amdgpu

major=22.20
minor=1438747
ubuntu_ver=22.04

pkgbase=opengl-amdgpu-pro
pkgname=(opengl-amdgpu-pro lib32-opengl-amdgpu-pro)
pkgver=${major}_${minor}
pkgrel=1
arch=(x86_64)
url='https://repo.radeon.com/amdgpu'
license=(custom: multiple)
source=(progl
        progl.bash-completion
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/a/appprofiles-amdgpu-pro/libgl1-amdgpu-pro-appprofiles_${major}-${minor}~${ubuntu_ver}_all.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb)

# extracts a debian package
# $1: deb file to extract
extract_deb(){
  local tmpdir="$(basename "${1%.deb}")"
  rm -Rf "$tmpdir"
  mkdir "$tmpdir"
  cd "$tmpdir"
  ar x "$1"
  tar -C "${pkgdir}" -xf data.tar.xz
}

# move ubuntu specific /usr/lib/x86_64-linux-gnu to /usr/lib
# $1: debian package library dir (goes from opt/amdgpu or opt/amdgpu-pro and from x86_64 or i386)
# $2: arch package library dir (goes to usr/lib or usr/lib32)
move_libdir(){
  local deb_libdir="$1"
  local arch_libdir="$2"

  if [ -d "${pkgdir}/${deb_libdir}" ]; then
    if [ ! -d "${pkgdir}/${arch_libdir}" ]; then
      mkdir -p "${pkgdir}/${arch_libdir}"
    fi
    mv -t "${pkgdir}/${arch_libdir}/" "${pkgdir}/${deb_libdir}"/*
    find ${pkgdir} -type d -empty -delete
  fi
}

# move copyright file to proper place and remove debian changelog
move_copyright(){
  find ${pkgdir}/usr/share/doc -name "changelog.Debian.gz" -delete
  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  find ${pkgdir}/usr/share/doc -name "copyright" -exec mv {} ${pkgdir}/usr/share/licenses/${pkgname} \;
  find ${pkgdir}/usr/share/doc -type d -empty -delete
}

package_opengl-amdgpu-pro(){
  pkgdesc='AMDGPU Pro OpenGL driver'
  license=(custom: AMDGPU-PRO EULA)
  provides=(libgl)
  depends=(libdrm libx11 libxcb libxdamage libxext libxfixes libxxf86vm)
  backup=(etc/amd/amdapfxx.blb)

  extract_deb "${srcdir}"/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libgl1-amdgpu-pro-appprofiles_${major}-${minor}~${ubuntu_ver}_all.deb
  extract_deb "${srcdir}"/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
  move_copyright

  # extra_commands:
  move_libdir "usr/lib/x86_64-linux-gnu" "usr/lib"
  move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib/amdgpu-pro"
  move_libdir "opt/amdgpu-pro/lib/xorg" "usr/lib/amdgpu-pro/xorg"
  move_libdir "opt/amdgpu/share/drirc.d" "usr/share/drirc.d"
  sed -i "s|/opt/amdgpu-pro/lib/x86_64-linux-gnu|#/usr/lib/amdgpu-pro  # commented to prevent problems of booting with amdgpu-pro, use progl script|" "${pkgdir}"/etc/ld.so.conf.d/10-amdgpu-pro-x86_64.conf
  install -Dm755 "${srcdir}"/progl "${pkgdir}"/usr/bin/progl
  install -Dm755 "${srcdir}"/progl.bash-completion "${pkgdir}"/usr/share/bash-completion/completions/progl
  # For some reason, applications started with normal OpenGL (i.e. without ag pro) crashes at launch if this conf file is presented, so hide it for now, until I find out the reason of that.
  mv "${pkgdir}"/usr/share/drirc.d/10-amdgpu-pro.conf "${pkgdir}"/usr/share/drirc.d/10-amdgpu-pro.conf.hide
}

package_lib32-opengl-amdgpu-pro(){
  pkgdesc='AMDGPU Pro OpenGL driver (32-bit)'
  license=(custom: AMDGPU-PRO EULA)
  provides=(lib32-libgl)
  depends=(amdgpu-pro-libgl=${major}_${minor}-${pkgrel} lib32-libdrm lib32-libx11 lib32-libxcb lib32-libxdamage lib32-libxext lib32-libxfixes lib32-libxxf86vm)
  backup=(etc/amd/amdrc etc/ld.so.conf.d/10-amdgpu-pro-i386.conf)

  extract_deb "${srcdir}"/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
  move_copyright

  # extra_commands:
  rm "${pkgdir}"/etc/amd/amdrc "${pkgdir}"/opt/amdgpu-pro/lib/xorg/modules/extensions/libglx.so "${pkgdir}"/opt/amdgpu/share/drirc.d/10-amdgpu-pro.conf
  move_libdir "usr/lib/i386-linux-gnu" "usr/lib32"
  move_libdir "opt/amdgpu-pro/lib/i386-linux-gnu" "usr/lib32/amdgpu-pro"
  sed -i "s|/opt/amdgpu-pro/lib/i386-linux-gnu|#/usr/lib32/amdgpu-pro  # commented to prevent problems of booting with amdgpu-pro, use progl32 script|" "${pkgdir}"/etc/ld.so.conf.d/10-amdgpu-pro-i386.conf
}
