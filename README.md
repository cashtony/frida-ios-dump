# frida-ios-dump
pull decrypted ipa from jailbreak device

### Usage

## 1. install [frida](http://www.frida.re/) on device and mac

- [device](https://zhangkn.github.io/2017/12/frida/#gsc.tab=0)
```
Start Cydia and add Frida’s repository by navigating to Manage -> Sources -> Edit -> Add and entering https://build.frida.re

```

## 2. iproxy 2222 22

## 3. ./dump.py 微信

```
➜  frida-ios-dump ./dump.py 微信
open target app......
start dump target app......
start dump /var/containers/Bundle/Application/6665AA28-68CC-4845-8610-7010E96061C6/WeChat.app/WeChat
WeChat                                        100%   68MB  11.4MB/s   00:05
start dump /private/var/containers/Bundle/Application/6665AA28-68CC-4845-8610-7010E96061C6/WeChat.app/Frameworks/WCDB.framework/WCDB
WCDB                                          100% 2555KB  11.0MB/s   00:00
start dump /private/var/containers/Bundle/Application/6665AA28-68CC-4845-8610-7010E96061C6/WeChat.app/Frameworks/MMCommon.framework/MMCommon
MMCommon                                      100%  979KB  10.6MB/s   00:00
start dump /private/var/containers/Bundle/Application/6665AA28-68CC-4845-8610-7010E96061C6/WeChat.app/Frameworks/MultiMedia.framework/MultiMedia
MultiMedia                                    100% 6801KB  11.1MB/s   00:00
start dump /private/var/containers/Bundle/Application/6665AA28-68CC-4845-8610-7010E96061C6/WeChat.app/Frameworks/mars.framework/mars
mars                                          100% 7462KB  11.1MB/s   00:00
AppIcon60x60@2x.png                           100% 2253   230.9KB/s   00:00
AppIcon60x60@3x.png                           100% 4334   834.8KB/s   00:00
AppIcon76x76@2x~ipad.png                      100% 2659   620.6KB/s   00:00
AppIcon76x76~ipad.png                         100% 1523   358.0KB/s   00:00
AppIcon83.5x83.5@2x~ipad.png                  100% 2725   568.9KB/s   00:00
Assets.car                                    100%   10MB  11.1MB/s   00:00
.......
AppIntentVocabulary.plist                     100%  197    52.9KB/s   00:00
AppIntentVocabulary.plist                     100%  167    43.9KB/s   00:00
AppIntentVocabulary.plist                     100%  187    50.2KB/s   00:00
InfoPlist.strings                             100% 1720   416.4KB/s   00:00
TipsPressTalk@2x.png                          100%   14KB   2.2MB/s   00:00
mm.strings                                    100%  404KB  10.2MB/s   00:00
network_setting.html                          100% 1695   450.4KB/s   00:00
InfoPlist.strings                             100% 1822   454.1KB/s   00:00
mm.strings                                    100%  409KB  10.2MB/s   00:00
network_setting.html                          100% 1819   477.5KB/s   00:00
InfoPlist.strings                             100% 1814   466.8KB/s   00:00
mm.strings                                    100%  409KB  10.3MB/s   00:00
network_setting.html                          100% 1819   404.9KB/s   00:00
```

congratulations!!! You've got a decrypted ipa file.

Drag to [MonkeyDev](https://github.com/AloneMonkey/MonkeyDev), Happy hacking!



# Q&A

- [exceptions.UnicodeDecodeError](http://iosre.com/t/failed-to-spawn-unable-to-launch-ios-app-timeout/10422/11)
```
114  		print "start dump target app......"
115  		session = device.attach(name);
116  		script = loadJsFile(session, DUMP_JS);
117  		script.post("dump");
118  		finished.wait();
(Pdb) s
--Return--
> /Users/devzkn/Downloads/kevin－software/ios-Reverse_Engineering/frida-ios-dump-master/dump.py(113)main()->None
-> createDir(os.getcwd()+"/"+OUTPUT)
(Pdb) l
108  		script = loadJsFile(session, APP_JS);
109  		name = target.decode('utf8');
110  		script.post(name);
111  		opened.wait();
112  		session.detach();
113  ->		createDir(os.getcwd()+"/"+OUTPUT)
114  		print "start dump target app......"
115  		session = device.attach(name);
116  		script = loadJsFile(session, DUMP_JS);
117  		script.post("dump");
118  		finished.wait();
(Pdb) s
UnicodeDecodeError: UnicodeD...ge(128)')
> /Users/devzkn/Downloads/kevin－software/ios-Reverse_Engineering/frida-ios-dump-master/dump.py(127)<module>()
-> main(sys.argv[1])
(Pdb) l
122  		if len(sys.argv) < 2:
123  			print "usage: ./dump.py 微信"
124  			sys.exit(0)
125  		else:
126  			try:
127  ->				main(sys.argv[1])
128  			except KeyboardInterrupt:
129  				if session:
130  					session.detach()
131  				sys.exit()
132  			except:
(Pdb) s
> /Users/devzkn/Downloads/kevin－software/ios-Reverse_Engineering/frida-ios-dump-master/dump.py(128)<module>()
-> except KeyboardInterrupt:
(Pdb) pp UnicodeDecodeError
<type 'exceptions.UnicodeDecodeError'>
```

解决方案是，确保frida-ios-dump-master 放在能UnicodeDecode成功的路径。最好是英文半角。

ps:因为py 加载的js 采用是相对路径，因此必须保证dump.py 能找到它们。
```
DUMP_JS = './dump.js'
APP_JS = './app.js'
```


