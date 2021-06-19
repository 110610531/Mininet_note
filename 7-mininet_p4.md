# mininet p4

## ex1

![](https://github.com/110610531/Mininet_note/blob/main/pic/p4-test1.jpg)

* `cd 2-test`

* `p4app.json`

```sh
{

  "program": "basic.p4",

  "switch": "simple_switch",

  "compiler": "p4c",

  "options": "--target bmv2 --arch v1model --std p4-16",

  "switch_cli": "simple_switch_CLI",

  "cli": true,

  "pcap_dump": true,

  "enable_log": true,

  "topo_module": {

    "file_path": "",

    "module_name": "p4utils.mininetlib.apptopo",

    "object_name": "AppTopoStrategies"

  },

  "controller_module": null,

  "topodb_module": {

    "file_path": "",

    "module_name": "p4utils.utils.topology",

    "object_name": "Topology"

  },

  "mininet_module": {

    "file_path": "",

    "module_name": "p4utils.mininetlib.p4net",

    "object_name": "P4Mininet"

  },

  "topology": {
    "assignment_strategy": "l2",

    "links": [["h1", "s1"],["h2", "s2"],["h3", "s3"],["s1", "s2"],["s2", "s3"] ],

    "hosts": {

      "h1": {

      },

      "h2": {

      },
      "h3": {
      }

    },

    "switches": {

      "s1": {

        "cli_input": "cmd.txt",

        "program": "basic.p4"

      },
      "s2": {

        "cli_input": "cmd2.txt",

        "program": "basic.p4"

      },
      "s3": {

        "cli_input": "cmd3.txt",

        "program": "basic.p4"

      }

    }

  }

}
```

* `cmd.txt`

```sh
table_add mac_forward forward 00:00:0a:00:00:01 => 1
table_add mac_forward forward 00:00:0a:00:00:02 => 2
table_add mac_forward forward 00:00:0a:00:00:03 => 2
```

* `cmd2.txt`

```sh
table_add mac_forward forward 00:00:0a:00:00:01 => 2
table_add mac_forward forward 00:00:0a:00:00:02 => 1
table_add mac_forward forward 00:00:0a:00:00:03 => 3
```

* `cmd3.txt`

```sh
table_add mac_forward forward 00:00:0a:00:00:01 => 2
table_add mac_forward forward 00:00:0a:00:00:02 => 2
table_add mac_forward forward 00:00:0a:00:00:03 => 1
```

![](https://github.com/110610531/Mininet_note/blob/main/pic/15.jpg)

## ex2

![](https://github.com/110610531/Mininet_note/blob/main/pic/p4-test2.jpg)

* `cd 3-test`

* `p4app.json`

```sh
{

  "program": "ip_forward.p4",

  "switch": "simple_switch",

  "compiler": "p4c",

  "options": "--target bmv2 --arch v1model --std p4-16",

  "switch_cli": "simple_switch_CLI",

  "cli": true,

  "pcap_dump": true,

  "enable_log": true,

  "topo_module": {

    "file_path": "",

    "module_name": "p4utils.mininetlib.apptopo",

    "object_name": "AppTopoStrategies"

  },

  "controller_module": null,

  "topodb_module": {

    "file_path": "",

    "module_name": "p4utils.utils.topology",

    "object_name": "Topology"

  },

  "mininet_module": {

    "file_path": "",

    "module_name": "p4utils.mininetlib.p4net",

    "object_name": "P4Mininet"

  },

  "topology": {

    "assignment_strategy": "manual",

    "auto_arp_tables": "true",

    "auto_gw_arp": "true",

    "links": [["h1", "s1"], ["h2", "s2"], ["h3", "s3"], ["s1", "s2"], ["s2", "s3"]],

    "hosts": {

      "h1": {

        "ip": "10.1.1.2/24",

        "gw": "10.1.1.254"

      },

      "h2": {

        "ip": "10.2.2.2/24",

       "gw": "10.2.2.254"

      },
      "h3": {

        "ip": "10.3.3.2/24",

       "gw": "10.3.3.254"

      }

    },

    "switches": {

      "s1": {

        "cli_input": "cmd.txt",

        "program": "ip_forward.p4"

      },
      "s2": {

        "cli_input": "cmd2.txt",

        "program": "ip_forward.p4"

      },
      "s3": {

        "cli_input": "cmd3.txt",

        "program": "ip_forward.p4"

      }
    }
  }
}
```

* `cmd.txt`

```sh
table_add ipv4_lpm set_nhop 10.1.1.2/24 => 00:00:0a:01:01:02 1
table_add ipv4_lpm set_nhop 10.2.2.2/24 => 00:00:0a:02:02:02 2
table_add ipv4_lpm set_nhop 10.3.3.2/24 => 00:00:0a:03:03:02 2
```

* `cmd2.txt`

```sh
table_add ipv4_lpm set_nhop 10.1.1.2/24 => 00:00:0a:01:01:02 2
table_add ipv4_lpm set_nhop 10.2.2.2/24 => 00:00:0a:02:02:02 1
table_add ipv4_lpm set_nhop 10.3.3.2/24 => 00:00:0a:03:03:02 3
```

* `cmd3.txt`

```sh
table_add ipv4_lpm set_nhop 10.1.1.2/24 => 00:00:0a:01:01:02 2
table_add ipv4_lpm set_nhop 10.2.2.2/24 => 00:00:0a:02:02:02 2
table_add ipv4_lpm set_nhop 10.3.3.2/24 => 00:00:0a:03:03:02 1
```

![](https://github.com/110610531/Mininet_note/blob/main/pic/16.jpg)

## ex3

![](https://github.com/110610531/Mininet_note/blob/main/pic/p4-test4.jpg)



## ex4

![](https://github.com/110610531/Mininet_note/blob/main/pic/p4-test3.jpg)




## copy-to-cpu

![](Pic/ctc.jpg)

* TCP

server

```sh
import socket
import sys

# Create a TCP/IP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind the socket to the port
server_address = ('10.0.0.2', 10000)
print >>sys.stderr, 'starting up on %s port %s' % server_address
sock.bind(server_address)

# Listen for incoming connections
sock.listen(1)

connection, client_address = sock.accept()
print >>sys.stderr, 'connection from', client_address
data = connection.recv(1024)
print >>sys.stderr, 'received "%s"' % data
connection.close()
```

client

```sh
import socket
import sys

# Create a TCP/IP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect the socket to the port where the server is listening
server_address = ('10.0.0.2', 10000)
print >>sys.stderr, 'connecting to %s port %s' % server_address
sock.connect(server_address)

# Send data
message = 'hello world'
print >>sys.stderr, 'sending "%s"' % message
sock.sendall(message)
sock.close()
```

![](Pic/17.jpg)

## send-to-cpu

![](Pic/stc.jpg)

![](https://github.com/110610531/Mininet_note/blob/main/pic/18.jpg)
