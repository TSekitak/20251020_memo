# memo

1. imuノードを起動する

1-1. wslにimuを認識させる

```terminal
usbipd list
usbipd bind --busid [BUSID]
usbipd attach --wsl --busid [BUSID]
usbipd list
```

1-2. wsl側で/dev/ttyACM*の接続を確認

```bash
ls /dev/ttyACM*
```

1-3. wsl側でsocatを使って/dev/ttyACM*を中継

```bash
sudo socat -d -d TCP4-LISTEN:2001,reuseaddr,fork FILE:/dev/ttyACM0,raw,echo=0,b115200

```

1-4. dockerコンテナ側でsocatを使って/dev/ttyACM*を受信

```bash
docker ps -a
docker start [CONTAINER ID or NAME]
docker ps -a
docker exec -it [CONTAINER ID or NAME] bash
socat -d -d PTY,link=/dev/ttyACM0,raw,echo=0 TCP:host.docker.internal:2001
```
1-5. 他のタブでdockerコンテナを開き/dev/ttyACM*の受信確認

1-6. dockerコンテナ側でimuノードのros2 runする

```bash
cd ~/[ROS2_WORKSPACE]
colcon build
source install/setup.bash
ros2 run
```
#### /dev/ttyUSB*と/dev/ttyACM*の違い

- ドライバ層と通信方式の違い

要約比較表

| 項目     | `/dev/ttyUSB*`      | `/dev/ttyACM*`        |
| ------ | ------------------- | --------------------- |
| 主な用途   | USB–UART変換          | USB CDC(仮想COMポート)     |
| ドライバ   | usbserial系          | cdc_acm               |
| 主なデバイス | FTDI, CP210x, CH340 | Arduino, STM32, Modem |
| 通信層    | UARTレベル             | USB CDCクラス            |
| 認識方法   | ベンダ固有ドライバ           | 標準クラスドライバ             |
| 例      | `/dev/ttyUSB0`      | `/dev/ttyACM0`        |

