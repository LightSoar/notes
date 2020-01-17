## List dynamic libraries
* `ldd -v /usr/bin/mp3blaster`
* `objdump -p /path/program`

## Virtual serial port
* `socat -d -d pty,raw,echo=0 pty,raw,echo=0`

## .inputrc
```sh
$include /etc/inputrc

## Case insensitive tab completion
set completion-ignore-case On

## arrow up
"\e[A":history-search-backward
## arrow down
"\e[B":history-search-forward
```

## .bashrc
```sh
f() {
    grep -Irnw "$1" .
}
```
