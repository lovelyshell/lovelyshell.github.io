``` python
amily@luna ~/l/shuiguolao> shuiguolao -i
python3 -i /home/amily/lab/shuiguolao/py/lib/preload.py
>>> # Create a new user
... 
>>> uxx = User('xx')
>>> uxx.save()
>>> uxx
User: xx 1005 /home/xx /bin/sh x
>>> 
>>> # Or like this:
... os.users.add('yy')
User: yy 1006 /home/yy /bin/sh x
>>> 
>>> # Find a user
... os.users.get('xx')
User: xx 1005 /home/xx /bin/sh x
>>> # Or like this:
... uyy = User('yy')
>>> uyy
User: yy 1006 /home/yy /bin/sh x
>>> 
>>> # The way for creating and finding UserGroup is similar:
... usbfs = UserGroup('usbfs')
>>> usbfs.save()
>>> UserGroup('usbfs')
UserGroup: usbfs 1008 [] x
>>> # Or like this:
... vbox = os.usergroups.add('vboxusers')
>>> os.usergroups.get('vboxusers')
UserGroup: vboxusers 1009 [] x
>>> 
>>> # Let current user join in usbfs
... import getpass
>>> myname = getpass.getuser()
>>> me = User(myname)
>>> me.join(usbfs)
Adding user amily to group usbfs
True
>>> usbfs.members
['amily']
>>> # Then quit
... me.quit(usbfs)
Removing user amily from group usbfs
True
>>> usbfs.members
[]
>>> 
>>> # Let all users join in vboxusers
... for u in os.users.list:
...     u.join(vbox)
... 
>>> vbox.members
['root', 'daemon', 'bin', 'sys', 'sync', 'games', 'man', 'lp', 'mail', 'news', 'uucp', 'proxy', 'www-data', 'backup', 'list', 'irc', 'gnats', 'nobody', '_apt', 'systemd-timesync', 'systemd-network', 'systemd-resolve', 'messagebus', 'uuidd', 'tss', 'dnsmasq', 'avahi-autoipd', 'usbmux', 'rtkit', 'pulse', 'speech-dispatcher', 'avahi', 'saned', 'colord', 'geoclue', 'Debian-gdm', 'amily', 'systemd-coredump', 'libvirt-qemu', 'Debian-exim', 'nm-openvpn', 'mysql', 'ua', 'ub', 'jazz', 'xx', 'yy']
>>> # Then quit
... for u in os.users.list:
...     u.quit(vbox)
... 
>>> vbox.members
[]
>>> 
>>> # Clear our footprint 
... os.usergroups.remove(usbfs)
True
>>> os.usergroups.remove(vbox)
True
>>> os.users.remove(uxx)
True
>>> os.users.remove(uyy)
True
```

