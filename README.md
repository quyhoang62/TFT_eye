# ESP32 LCD Round 240x240 Eyes

Du an Arduino cho `ESP32` hien thi mat dong tren man hinh tron `240x240` su dung driver `GC9A01` va thu vien `TFT_eSPI`.

Sketch nay la ban chuyen doi y tuong `Uncanny Eyes` sang he sinh thai `ESP32 + TFT_eSPI`, cho phep:

- Hien thi 1 hoac 2 mat.
- Tu dong dao mat, chớp mat va thay doi kich thuoc dong tu.
- Chon nhieu kieu mat khac nhau tu cac file trong thu muc `data/`.
- Gan them ma nguoi dung trong `user.cpp` de mo rong chuc nang.

## Tong quan du an

Du an duoc to chuc theo kieu Arduino IDE:

- `ESP32LCDRound240x240Eyes.ino`: diem vao chinh, khoi tao man hinh va goi ham cap nhat.
- `eye_functions.ino`: toan bo logic ve mat, chớp mat, di chuyen dong tu, render tung pixel.
- `config.h`: noi cau hinh kieu mat, so man hinh, so mat, input va cac thong so hien thi.
- `user.cpp`: khu vuc de chen ma mo rong rieng.
- `user_bat.cpp`: vi du dieu khien servo + cam ung dien dung.
- `user_xmas.cpp`: vi du dieu khien NeoPixel.
- `data/*.h`: du lieu anh mat duoc nhung san trong ma nguon.

## Cau hinh hien tai

Theo `config.h`, project dang duoc dat mac dinh nhu sau:

- Su dung `1` man hinh.
- Hien thi `1` mat.
- Kieu mat dang bat la `data/doeEye.h`.
- `TRACKING` dang bat: mi mat se thay doi theo vi tri dong tu.
- `AUTOBLINK` dang bat: mat tu dong chớp.
- `TFT1_CS = -1`: chan CS/man hinh dang lay theo cau hinh trong `TFT_eSPI`.
- `DISPLAY_BACKLIGHT = -1`: chua cau hinh chan dieu khien den nen trong repo.

## Yeu cau

Phan cung toi thieu:

- `ESP32`
- Man hinh tron `240x240` dung driver `GC9A01`

Thu vien can thiet:

- `TFT_eSPI`
- `SPI` (di kem Arduino core)

Thu vien tuy chon, chi can khi bat cac file demo:

- `Adafruit_FreeTouch`
- `Servo`
- `Adafruit_NeoPixel`

## Cai dat

1. Cai `ESP32 board package` trong Arduino IDE.
2. Cai thu vien `TFT_eSPI`.
3. Cau hinh `TFT_eSPI` dung voi man hinh `GC9A01` va dung chan noi thuc te cua ban.
4. Mo file `ESP32LCDRound240x240Eyes.ino` trong Arduino IDE.
5. Chinh sua `config.h` neu can.
6. Chon board ESP32 dung voi phan cung cua ban.
7. Compile va upload.

## Cau hinh TFT_eSPI

Repo nay khong chua file cau hinh board rieng cho man hinh. Sketch dang su dung:

- `#include <TFT_eSPI.h>`
- `TFT1_CS = -1`

Dieu do co nghia la ban phai cau hinh dung driver va pin trong thu vien `TFT_eSPI` truoc khi nap code. Tuy theo ban thu vien, viec nay thuong nam trong mot file `User_Setup` hoac `User_Setup_Select`.

Ban can dam bao it nhat cac muc sau da dung:

- Driver `GC9A01`
- Kich thuoc man hinh `240x240`
- Chan `MOSI`, `SCLK`, `CS`, `DC`, `RST`, `BL` neu ban su dung
- Toc do SPI phu hop voi man hinh

## Tuy bien trong config.h

### 1. Chon kieu mat

Mo `config.h` va chi de lai `mot` dong `#include` trong nhom sau:

- `data/defaultEye.h`
- `data/dragonEye.h`
- `data/noScleraEye.h`
- `data/goatEye.h`
- `data/newtEye.h`
- `data/terminatorEye.h`
- `data/catEye.h`
- `data/owlEye.h`
- `data/naugaEye.h`
- `data/doeEye.h`

### 2. Chon so man hinh va so mat

- `TFT_COUNT`: so man hinh, ho tro `1` hoac `2`
- `NUM_EYES`: so mat, ho tro `1` hoac `2`

Neu ban dung 2 man hinh, can cau hinh them:

- `TFT1_CS`
- `TFT2_CS`
- `TFT_1_ROT`
- `TFT_2_ROT`
- `EYE_1_XPOSITION`
- `EYE_2_XPOSITION`

### 3. Dieu khien den nen

Neu man hinh co chan backlight va ban muon PWM:

- dat `DISPLAY_BACKLIGHT` thanh chan dieu khien
- dieu chinh `BACKLIGHT_MAX`

Neu khong dung, giu `-1`.

### 4. Input dieu khien mat

Sketch ho tro cac tuy chon sau:

- `JOYSTICK_X_PIN`, `JOYSTICK_Y_PIN`: dieu khien huong nhin bang joystick analog
- `LIGHT_PIN`: cam bien anh sang hoac bien tro de dieu khien kich thuoc dong tu
- `BLINK_PIN`: nut chớp mat chung
- `LH_WINK_PIN`, `RH_WINK_PIN`: nut nhay tung ben

Neu khong khai bao cac input tren, mat se tu dong di chuyen va thay doi dong tu.

### 5. Hanh vi dong

Co the bat/tat:

- `TRACKING`: mi mat di theo dong tu
- `AUTOBLINK`: chớp mat tu dong
- `IRIS_SMOOTH`: lam muot thay doi kich thuoc dong tu

## File user.cpp de mo rong chuc nang

Neu ban muon them logic rieng ma khong sua code render chinh, hay dat code vao:

- `user_setup()`: chay mot lan trong `setup()`
- `user_loop()`: duoc goi dinh ky trong qua trinh render

Luu y:

- Chi nen bat `mot` file `user*.cpp` tai mot thoi diem.
- `user_loop()` la ham block, neu xu ly qua nang se lam giam FPS.

## Cac demo co san

### user.cpp

File mac dinh, khong lam gi, dung de ban viet logic rieng.

### user_bat.cpp

Demo dieu khien servo va cam bien cham dung:

- Can `Adafruit_FreeTouch`
- Can `Servo`
- Muon dung demo nay, doi `#if 0` thanh `#if 1`
- Dong thoi phai tat `user.cpp` hoac cac `user*.cpp` khac

### user_xmas.cpp

Demo dieu khien day `NeoPixel` voi mau Giang sinh:

- Can `Adafruit_NeoPixel`
- Muon dung demo nay, doi `#if 0` thanh `#if 1`
- Dong thoi phai tat `user.cpp` hoac cac `user*.cpp` khac

## Ghi chu ve hieu nang

Code da bat:

- `#define USE_DMA`

Neu cau hinh `TFT_eSPI` va board ESP32 cua ban ho tro DMA, toc do ve se tot hon. Toc do thuc te con phu thuoc vao:

- toc do SPI
- chat luong day noi
- man hinh co on dinh o xung cao hay khong

## Cach chay nhanh

1. Cau hinh `TFT_eSPI` cho `GC9A01 240x240`.
2. De nguyen `config.h` neu ban muon chay 1 mat mac dinh.
3. Nap sketch `ESP32LCDRound240x240Eyes.ino` vao ESP32.
4. Neu hinh xoay sai, chinh `TFT_1_ROT`.
5. Neu vi tri mat lech, chinh `EYE_1_XPOSITION`.

## Tai nguyen tham khao

- Video huong dan goc: <https://youtu.be/pmCc7z_Mi8I>

## Ban quyen

Du an su dung giay phep `MIT`. Xem file `LICENSE` de biet chi tiet.
