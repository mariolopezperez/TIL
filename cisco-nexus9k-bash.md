#### Nexus 9K bash oneliners

`config t`
`feature bash-shell`
`run bash`


Once bash has been activated you can combine shell commands with Cisco CLI commands

`vsh -c "sh int desc | i DMZ" | cut -d" " -f1 | xargs -i vsh -c "sh int {} description " | grep Eth|tr -s " " " " |cut -d" " -f1,4`
`vsh -c "sh int desc | i DMZ" | cut -d" " -f1 | xargs -i vsh -c "sh int {} switchport " |grep "Name:"`
`vsh -c "sh int desc | i DMZ" | cut -d" " -f1 | xargs -i vsh -c "sh int {} switchport " |grep "Trunking VLANs Allowed"`
