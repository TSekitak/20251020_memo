# memo

1. imuノードを起動する

1-1. wslにimuを認識させる

1-2. wsl側で/dev/ttyUSB*の接続を確認

1-3. wsl側でsocatを使って/dev/ttyUSB*を中継

1-4. dockerコンテナ側でsocatを使って/dev/ttyUSB*を受信

1-5. dockerコンテナ側でimuノードのros2 runする
