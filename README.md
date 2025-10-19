# memo

1. imuノードを起動する

1-1. wslにimuを認識させる

```terminal
usbipd list
usbipd bind --busid [BUSID]
usbipd attach --wsl --busid [BUSID]
usbipd list
```

1-2. wsl側で/dev/ttyUSB*の接続を確認

```bash
ls /dev/ttyUSB*
```

1-3. wsl側でsocatを使って/dev/ttyUSB*を中継

```bash
sudo socat -d -d TCP4-LISTEN:2001,reuseaddr,fork FILE:/dev/ttyUSB0,raw,echo=0,b115200

```

1-4. dockerコンテナ側でsocatを使って/dev/ttyUSB*を受信

```bash
docker ps -a
docker start [CONTAINER ID or NAME]
docker ps -a
docker exec -it [CONTAINER ID or NAME] bash
socat -d -d PTY,link=/dev/ttyUSB0,raw,echo=0 TCP:host.docker.internal:2001
```
1-5. 他のタブでdockerコンテナを開き/dev/ttyUSB*の受信確認

1-6. dockerコンテナ側でimuノードのros2 runする

```bash
cd ~/[ROS2_WORKSPACE]
colcon build
source install/setup.bash
ros2 run
```
