# 三次握手
client : SYN = 1 seq = x
server : SYN = 1 ACK = 1   ack = x+1 seq = y
client : ACK = 1 seq = x+1 ack = y+1

两次握手会死锁，A向B发送，B收到后给A回复，但是A丢失，此时B认为连接已建立给A发送信息，A因为没收到确认帧所以一直忽略B的分组，B认为发送信息丢失一直发送

# 四次挥手
client : FIN = 1 seq = u
server : ACK = 1 seq = v ack = u+1
==server向client数据传送==
server : FIN = 1 ACK = 1 seq = w ack = u+1
client : ACK = 1 seq = u+1 ack = w+1
