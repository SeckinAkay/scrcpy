> [!UYARI]
> **Bu GitHub deposu (scrcpy>), güvenilir deposundan çatallanmıştırr.
kaynağıdır. İsimlerinde `scrcpy` geçse bile, rastgele web sitelerinden
sürümleri indirmeyin.***

# scrcpy (v4.1)

<img src="app/data/scrcpy.svg" width="128" height="128" alt="scrcpy" align="right" />

Bu uygulama, USB veya [TCP/IP](doc/connection.md#tcpip-wireless) üzerinden bağlanan Android cihazları yansıtır ve aşağıdaki yöntemlerle kontrol edilmesini sağlar.
bilgisayarın klavyesi ve faresi. _Root_ erişimi veya cihaza yüklenecek bir uygulama gerektirmez. _Linux_, _Windows_ ve _macOS_ üzerinde çalışır.

[![Linux](https://img.shields.io/badge/Linux-download-orange?style=for-the-badge&logo=linux)](doc/linux.md)&nbsp;
[![Windows](https://img.shields.io/badge/Windows-download-blue?style=for-the-badge&logo=windows)](doc/windows.md)&nbsp;
[![macOS](https://img.shields.io/badge/macOS-download-brightgreen?style=for-the-badge&logo=apple)](doc/macos.md)&nbsp;

![screenshot](assets/screenshot-debian-600.jpg)

Şunlara odaklanır:

 - **hafiflik**: yerel (native) yapı, yalnızca cihaz ekranını görüntüler
 - **performans**: cihaza bağlı olarak 30~120 fps
 - **kalite**: 1920×1080 veya üzeri
 - **düşük gecikme süresi**: [35~70 ms][lowlatency]
 - **hızlı başlatma**: ilk görüntünün belirmesi ~1 saniye sürer
 - **müdahaleci olmama**: Android cihazda kalıcı hiçbir şey bırakmaz
 - **kullanıcı avantajları**: hesap gerektirmez, reklam içermez, internet bağlantısı gerektirmez
 - **özgürlük**: ücretsiz ve açık kaynaklı yazılım



Özellikleri şunları içerir:
 - [ses aktarımı](doc/audio.md) (Android 11+)
 - [kayıt](doc/recording.md)
 - [sanal ekran](doc/virtual-display.md)
 - [Android cihaz ekranı kapalıyken](doc/device.md#turn-screen-off) ekran yansıtma
 - çift yönlü [kopyala-yapıştır](doc/control.md#copy-paste)
 - [yapılandırılabilir görüntü kalitesi](doc/video.md)
 - [kamera yansıtma](doc/camera.md) (Android 12+)
 - [web kamerası olarak yansıtma (V4L2)](doc/v4l2.md) (yalnızca Linux)
 - fiziksel [klavye][hid-keyboard] ve [fare][hid-mouse] simülasyonu (HID)
 - [gamepad](doc/gamepad.md) desteği
 - [OTG modu](doc/otg.md)
 - ve daha fazlası…

[hid-keyboard]: doc/keyboard.md#physical-keyboard-simulation
[hid-mouse]: doc/mouse.md#physical-mouse-simulation

## Ön Koşullar

Android cihazın en az API 21 (Android 5.0) sürümüne sahip olması gerekir.

[Ses aktarımı](doc/audio.md), API >= 30 (Android 11+) sürümlerinde desteklenir.

Cihazınızda/cihazlarınızda [USB hata ayıklamayı etkinleştirdiğinizden][enable-adb] emin olun.

[enable-adb]: https://developer.android.com/studio/debug/dev-options#enable

Bazı cihazlarda (özellikle Xiaomi), aşağıdaki hatayı alabilirsiniz:

```
Injecting input events requires the caller (or the source of the instrumentation, if any) to have the INJECT_EVENTS permission.
```

Bu durumda, klavye ve fare kullanarak kontrol edebilmek için `USB hata ayıklama (Güvenlik Ayarları)` adlı [ek bir seçeneği][control] etkinleştirmeniz gerekir (bu, `USB hata ayıklama` seçeneğinden farklı bir öğedir). Bu seçenek ayarlandıktan sonra cihazı yeniden başlatmak gereklidir.

[control]: https://github.com/Genymobile/scrcpy/issues/70#issuecomment-373286323

scrcpy'yi [OTG modunda](doc/otg.md) çalıştırmak için USB hata ayıklamanın gerekli olmadığını unutmayın.
## Uygulamayı edinin

 - [Linux](doc/linux.md)
 - [Windows](doc/windows.md) ([nasıl çalıştırılacağını](doc/windows.md#run) okuyun)
 - [macOS](doc/macos.md)


## Bilinmesi gereken ipuçları

 - [Çözünürlüğü düşürmek](doc/video.md#size) performansı büyük ölçüde artırabilir
   (`scrcpy -m1024`)
 - [_Sağ tıklama_](doc/mouse.md#mouse-bindings) `GERİ` (BACK) işlevini tetikler
 - [_Orta tıklama_](doc/mouse.md#mouse-bindings) `ANA EKRAN` (HOME) işlevini tetikler
 - <kbd>Alt</kbd>+<kbd>f</kbd> [tam ekran](doc/window.md#fullscreen) modunu açıp kapatır
 - Daha pek çok [kısayol](doc/shortcuts.md) mevcuttur


## Kullanım örnekleri

Ayrı sayfalarda [belgelenmiş](#user-documentation) pek çok seçenek bulunmaktadır.
İşte bunlardan bazı yaygın örnekler:

 - Ekranı H.265 formatında (daha iyi kalite) yakalayın, boyutu 1920 ile sınırlayın,
   kare hızını 60 fps ile sınırlayın, sesi devre dışı bırakın ve fiziksel bir
   klavye simülasyonu ile cihazı kontrol edin:
    ```bash
    scrcpy --video-codec=h265 --max-size=1920 --max-fps=60 --no-audio --keyboard=uhid
    scrcpy --video-codec=h265 -m1920 --max-fps=60 --no-audio -K  # short version
    ```

 - Start VLC in a new virtual display (separate from the device display):

    ```bash
    scrcpy --new-display=1920x1080 --start-app=org.videolan.vlc
    ```

 - Start VLC in a new _flex_ display using H.265 with a bitrate of 16 Mbps,
   while keeping the display active so it does not turn off:

    ```bash
    scrcpy --new-display -x --keep-active --start-app=org.videolan.vlc --video-codec=h265 -b16M
    ```

 - Record the device camera in H.265 at 1920x1080 (and microphone) to an MP4
   file:

    ```bash
    scrcpy --video-source=camera --video-codec=h265 --camera-size=1920x1080 --record=file.mp4
    ```

 - Capture the device front camera and expose it as a webcam on the computer (on
   Linux):

    ```bash
    scrcpy --video-source=camera --camera-size=1920x1080 --camera-facing=front --v4l2-sink=/dev/video2 --no-playback
    ```

 - Control the device without mirroring by simulating a physical keyboard and
   mouse (USB debugging not required):

    ```bash
    scrcpy --otg
    ```

 - Control the device using gamepads plugged into the computer:

    ```bash
    scrcpy --gamepad=uhid
    scrcpy -G  # short version
    ```

## Kullanıcı belgeleri

Uygulama, pek çok özellik ve yapılandırma seçeneği sunmaktadır. Bunlar aşağıdaki sayfalarda belgelenmiştir:

 - [Bağlantı](doc/connection.md)
 - [Video](doc/video.md)
 - [Ses](doc/audio.md)
 - [Kontrol](doc/control.md)
 - [Klavye](doc/keyboard.md)
 - [Fare](doc/mouse.md)
 - [Oyun kolu](doc/gamepad.md)
 - [Cihaz](doc/device.md)
 - [Pencere](doc/window.md)
 - [Kayıt](doc/recording.md)
 - [Sanal ekran](doc/virtual-display.md)
 - [Tüneller](doc/tunnels.md)
 - [OTG](doc/otg.md)
 - [Kamera](doc/camera.md)
 - [Video4Linux](doc/v4l2.md)
 - [Kısayollar](doc/shortcuts.md)


## Kaynaklar

 - [SSS](FAQ.md)
 - [Çeviriler][wiki] (her zaman güncel olmayabilir)
 - [Derleme talimatları](doc/build.md)
 - [Geliştiriciler](doc/develop.md)
 - [Sürüm imzalarını doğrulama](doc/verify-release.md)

[wiki]: https://github.com/Genymobile/scrcpy/wiki


## Makaleler

- [scrcpy ile tanışın][article-intro]
- [Scrcpy artık kablosuz çalışıyor][article-tcpip]
- [Ses desteğiyle Scrcpy 2.0][article-scrcpy2]

[article-intro]: https://blog.rom1v.com/2018/03/introducing-scrcpy/
[article-tcpip]: https://www.genymotion.com/blog/open-source-project-scrcpy-now-works-wirelessly/
[article-scrcpy2]: https://blog.rom1v.com/2023/03/scrcpy-2-0-with-audio/

## İndirme Linkleri
https://github.com/Genymobile/scrcpy/releases/download/v4.1/scrcpy-linux-x86_64-v4.1.tar.gz
sha256:ad56ae8bfeedf41e824945c11dbf55fcb092b3e615b9b486f48a50e30d389635
16.9 MB
Jul 12
https://github.com/Genymobile/scrcpy/releases/download/v4.1/scrcpy-macos-aarch64-v4.1.tar.gz
sha256:20fd47c9014dd5e0fa77091f3cb7adbda8445a360c4584aeaa0150b5b3988ff3
12.4 MB
Jul 12
https://github.com/Genymobile/scrcpy/releases/download/v4.1/scrcpy-macos-x86_64-v4.1.tar.gz
sha256:ee2a7223bc8dbdc4f482db1134bcf441178dafb833492b71ca4c22090c58ce72
13.3 MB
https://github.com/Genymobile/scrcpy/releases/download/v4.1/scrcpy-server-v4.1
sha256:deacb991ed2509715160ffdc7907e47b4160eb30d1566217e9047fd5b8850cae
717 KB
Jul 12
https://github.com/Genymobile/scrcpy/releases/download/v4.1/scrcpy-win32-v4.1.zip
sha256:fa57b36622a53b6aec74c5e5b5c08236165efa445c4f186d48f176ebf9c24eec
9.72 MB
Jul 12
https://github.com/Genymobile/scrcpy/releases/download/v4.1/scrcpy-win64-v4.1.zip
sha256:5b12172b3264b2889f4583ee64752ce832e29bc8b1089dca81093459697165db
10.8 MB
Jul 12

## License

    Copyright (C) 2018 Genymobile
    Copyright (C) 2018-2026 Romain Vimont

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
