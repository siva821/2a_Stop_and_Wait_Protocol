# 2a_Stop_and_Wait_Protocol
## AIM 
To write a python program to perform stop and wait protocol
## ALGORITHM
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
Server.py :
```
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind(('localhost', 12345))
print("Server running...")

expected_seq = 0

while True:
    data, addr = sock.recvfrom(1024)
    seq = int(data[:1])
    msg = data[1:].decode()

    if seq == expected_seq:
        print(f"Received frame {seq}: {msg}")
        sock.sendto(f"ACK{seq}".encode(), addr)
        expected_seq = 1 - expected_seq
    else:
        # Resend ACK for last received frame
        sock.sendto(f"ACK{1 - expected_seq}".encode(), addr)
```
client.py :
```
import socket
import time

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.settimeout(2)
server = ('localhost', 12345)

frames = ['Hello', 'World', 'Stop', 'Wait']
seq = 0

for f in frames:
    msg = f"{seq}{f}".encode()
    while True:
        try:
            print(f"Sending frame {seq}: {f}")
            sock.sendto(msg, server)
            ack, _ = sock.recvfrom(1024)
            if ack.decode() == f"ACK{seq}":
                print(f"Received {ack.decode()}")
                seq = 1 - seq
                break
        except socket.timeout:
            print("Timeout, resending frame...")

sock.close()
```

## OUTPUT :
<img width="587" height="176" alt="image" src="https://github.com/user-attachments/assets/ed2f276a-c3e9-4474-bf7f-abed1f30137c" />

## RESULT :
Thus, python program to perform stop and wait protocol was successfully executed.
