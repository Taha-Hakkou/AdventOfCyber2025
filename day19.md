## day 19

nmap -sV -T4 -p- -vv MACHINE_IP


* reconnaissance

1. install pymodbus

```sh
pip3 install pymodbus==3.6.8
```

2. connect to PLC's Modbus interface

```py
>>> from pymodbus.client import ModbusTcpClient
>>> 
>>> # Connect to the PLC on port 502
>>> client = ModbusTcpClient('MACHINE_IP', port=502)
>>> 
>>> # Establish connection
>>> if client.connect():
...     print("Connected to PLC successfully")
... else:
...     print("Connection failed")
... 
Connected to PLC successfully
>>>
```

3. Reading Holding Registers

```py
# rading holding register 0
>>> result = client.read_holding_registers(address=0, count=1, slave=1)
>>> 
>>> if not result.isError():
...     package_type = result.registers[0]
```

4. Reading coils:

```py
result = client.read_coils(address=10, count=1, slave=1)
>>> 
>>> if not result.isError():
...     verification = result.bits[0]
```

5. Writing values to coils and registers:

```py
result = client.write_coil(11, False, slave=UNIT_ID)
###
result = client.write_register(0, 0, slave=UNIT_ID)
```
