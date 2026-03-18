# v8 strings

## How to build v8 on mac arm

### links

1. install xcode
   https://apps.apple.com/us/app/xcode/id497799835
2. find v8 repo
   https://github.com/v8/v8
3. get `depot_tools`
   https://www.chromium.org/developers/how-tos/install-depot-tools/
4. v8 debugger
   https://v8.dev/docs/d8

### full build sh

```sh
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git --depth 1

echo "export PATH=$(pwd)/depot_tools:\$PATH" >> ~/.zshrc
source ~/.zshrc

fetch --nohistory v8

cd v8
gn gen out/foo --args='target_cpu="arm64"'
ninja -C out/foo d8

echo "console.log('Hello world!');" > test.js
./out/foo/d8 test.js
```

### video

1. download from github: [./build_v8_d8.mp4](./build_v8_d8.mp4)
2. see on yandex disk: [https://disk.yandex.ru/i/pvOYS6Pa6MfYZA](https://disk.yandex.ru/i/pvOYS6Pa6MfYZA)

### DevTools Memory Snapshots

![](./ConsOneTwoByteString.jpg)

![](./InternalizedString_Code.jpg)

![](./InternalizedString_Object.jpg)

## Related links

- Юрий Карпов - Измеряем настоящую цену абстракций в JavaScript
  [https://youtu.be/UNHNgHWyCGc](https://youtu.be/UNHNgHWyCGc)