## rsyslog config
```
module(load="imfile" PollingInterval="10")
template(name="json-template" type="list") {
    constant(value="{")
    constant(value="\"message\":\"")     property(name="msg" format="json")
    constant(value="\",\"tag\":\"")      property(name="syslogtag" format="json")
    constant(value="\"}\n")
}
input(type="imfile"
    File="/home/kambiz/test.log"
    readTimeout="2"
    Tag="kambiz-test"
    Ruleset="sendToaiops")
:msg,contains,"kambiz kazemi" @192.168.43.37:2514;json-template
}
```
This rule forwards all messages that contain the word “kambiz kazemi” in the msg part to the server
```
systemctl restart rsyslog
```
```
echo "kambiz kazemi" >> /home/kambiz/test.log
```
Results:
```
{"message":"kambiz kazemi","tag":"kambiz-test"}
```
