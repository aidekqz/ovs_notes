# Ovs_notes
## Base
```sh
# Print database content
ovs-vsctl show
```

### stp and rstp

```sh
# enable stp
ovs-vsctl set Bridge br0 stp_enable=true
# set stp priority. 0x8000 - default value
ovs-vsctl set Bridge br0 other_config:stp-priority=0x8000
# set port stp path cost
ovs-vsctl set Port eth0 other_config:stp-path-cost=10
# set MaxAge and ForwardDelay to default value
ovs-vsctl set Bridge br0 other_config:stp-max-age=20
ovs-vsctl set Bridge br0 other_config:stp-forward-delay=15

#rstp simmilar
ovs-vsctl set Bridge br0 rstp_enable=true
# 0x800 -> 32768 dec
ovs-vsctl set Bridge br0 other_config:rstp-priority=32768
ovs-vsctl set Bridge br0 other_config:rstp-max-age=20
ovs-vsctl set Bridge br0 other_config:rstp-forward-delay=15
ovs-vsctl set Port eth0 other_config:rstp-path-cost=150
```

### ovsdb

```
# Example select from ovsdb database. [--columns is optional]
ovs-vsctl --format=table --columns=name,mac_in_use,link_speed find Interface name=eth0
```

```sh
# List databases ovsdb server
ovsdb-client list-dbs --db tcp:192.168.1.2:6642,tcp:192.168.1.3:6642
# Get database schema
ovsdb-client get-schema --db tcp:192.168.1.2:6642,tcp:192.168.1.3:6642 OVN_Southbound
# Watch real-time changes in Port_Binding table
ovsdb-client monitor "unix:/var/run/ovn/ovnsb_db.sock" OVN_Southbound Port_Binding
```

```sh
# create internal port
ovs-vsctl add-port br0 test-port -- set interface test-port type=internal
# set tag for port
ovs-vsctl set port test-port tag=110
# set trunks for port
ovs-vscctl set port test-port trunks=120-130,140
```
