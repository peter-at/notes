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