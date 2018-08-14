# D-Link DIR-846 Remote Code Execution #

**vender** ：D-Link

**Firmware version**:100.26

**Exploit Author**: bigbear@galaxylab.org

**Vendor Homepage**: http://www.dlink.com.cn/

**Hardware Link**:http://support.dlink.com.cn/ProductInfo.aspx?m=DIR-846

## Vul detail ##

Reproduction Steps:
1. Go to your wi-fi router gateway [i.e: http://192.168.0.1]
1. login with admin
1. Send http request with admin cookies
![](dlink1.png)
1. Gets the result of the command execution. The user is root.
![](dlink2.png)
