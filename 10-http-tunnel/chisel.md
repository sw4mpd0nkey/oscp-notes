# Chisel

## Basic Port Forward

 chisel server --socks5 --reverse
 ./chisel client <kali>:8080 R:8000:<ip>:<port>
 
 when you connect to localhost:8000 you'll hit <ip><port>

## Set up socks proxy

chisel server --socks5 --reverse
./chisel client <kali>:8080 R:socks