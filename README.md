# rsyslog
## rsyslog config
```
module(load="imfile" PollingInterval="10")
template(name="json-template" type="list") {
    constant(value="{")
      constant(value="\"@timestamp\":\"")     property(name="timereported" dateFormat="rfc3339")
      constant(value="\",\"message\":\"")     property(name="msg" format="json")
      constant(value="\",\"logsource\":\"")   property(name="hostname")
      constant(value="\",\"severity\":\"")    property(name="syslogseverity-text")
      constant(value="\",\"facility\":\"")    property(name="syslogfacility-text")
      constant(value="\",\"program\":\"") property(name="programname")
      constant(value="\",\"pid\":\"")      property(name="procid")
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
