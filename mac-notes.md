# mac notes

# command line

## clear dns cache
`sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder`

## show resolver
`scutil --dns`
`cat /etc/resolv.conf # for info (file not used)`

## bounce mDNSResponder
```
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist
```

## show hardware ports
`networksetup -listallhardwareports`

## find dhcp lease server addr
```
% ipconfig getpacket en0 |grep server_id
server_identifier (ip): 192.168.86.3

## note: enable dhcp on en0
## sudo ipconfig set en0 DHCP
```

# daemon

## launchctl
```
## list all running (need sudo to see all)
% sudo launchctl list
## print disabled system
% sudo launchctl print-disabled system
## print disabled user
% launchctl print-disabled user/$(id -u)
## print lots state info (dumpstate has lot more info than print-cache)
% launchctl dumpstate
% launchctl print-cache
```
* location of per user override.plist
/var/db/com.apple.xpc.launchd/disabled.502.plist
* location of system override.plist
/var/db/launchd.db/com.apple.launchd/overrides.plist
