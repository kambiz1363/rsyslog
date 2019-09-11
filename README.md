# rsyslog
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
ruleset(name="sendToaiops") {
    action(type="omfwd" Target="192.168.43.37" Port="2514" Protocol="udp" template="json-template")
}

```
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
