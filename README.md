# gcc14_mold2_deb12
GCC 14.2 and mold 2.35 packaged for Debian 12 bookworm

https://gcc.gnu.org/onlinedocs/libstdc++/manual/license.html

https://github.com/rui314/mold/blob/main/LICENSE

made with:
```
  sudo apt install gcc-12 g++-12 git libgmp-dev libmpfr-dev libmpc-dev 
  # mold linker
  pushd
  git clone --branch stable https://github.com/rui314/mold.git
  cd mold
  sudo ./install-build-deps.sh
  cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_COMPILER=g++ -B build
  cmake --build build -j$(nproc)
  sudo cmake --build build --target install
  popd
  # gcc 14.2 (with gcc-12)
  pushd
  wget https://ftp.fu-berlin.de/unix/languages/gcc/releases/gcc-14.2.0/gcc-14.2.0.tar.xz
  tar -xvf gcc-14.2.0.tar.xz
  mkdir -p gcc-14.2.0/build && cd "$_"
  CC=gcc-12 CXX=g++-12 LD=/usr/local/bin/mold ../configure \
      --program-suffix=-14 \
      --enable-shared \
      --with-ld=/usr/local/bin/mold \
      --disable-multilib \
      --enable-languages=c,c++,lto
  make -j 4  # more caused OOM
  popd
```

