# Modbus cheatsheet
## Terminology
<table>
  <tr class="header">
    <th></th>
    <th>Coils (co)</th>
    <th>Discrete input (di)</th>
    <th>Input register (ir)</th>
    <th>Holding register (hr)</th>
  </tr>
  <tr>
    <td>Access</td>
    <td>RW</td>
    <td>RO</td>
    <td>RO</td>
    <td>RW</td>
  </tr>
  <tr>
    <td>Size</td>
    <td>1 bit</td>
    <td>1 bit</td>
    <td>16 bits</td>
    <td>16 bits</td>
  </tr>
  <tr>
    <td>Read code</td>
    <td>1 Multiple</td>
    <td>2 Multiple</td>
    <td>4 Multiple</td>
    <td>3 Multiple</td>
  </tr>
  <tr>
    <td>Write code</td>
    <td>5 Single<br/>15 Multiple</td>
    <td>N/A (RO)</td>
    <td>N/A (RO)</td>
    <td>6 Single<br/>16 Multiple</td>
  </tr>
  <tr>
    <td>Number range</td>
    <td>000001<br/>065536</td>
    <td>100001<br/>165536</td>
    <td>300001<br/>365536</td>
    <td>400001<br/>465536</td>
  </tr>
  <tr>
    <td markdown="span">pymodbus</td>
    <td>read_coils<br/>write_coil<br/>write_coils</td>
    <td>read_discrete_inputs</td>
    <td>read_input_registers</td>
    <td>read_holding_registers<br/>write_register<br/>write_registers</td>
  </tr>
  <tr>
    <td markdown="span">libmodbus</td>
    <td>read_bits<br/>write_bits</td>
    <td>read_input_bits</td>
    <td>read_input_registers</td>
    <td>read_registers<br/>write_registers</td>
  </tr>
</table>

## Topology
<table>
  <tr class="header">
    <th>Protocol</th>
    <th>Data store</th>
    <th>Agent</th>
  </tr>
  <tr>
    <td>Modbus RTU/ASCII</td>
    <td>Slave</td>
    <td>Master</td>
  </tr>
  <tr>
    <td>Modbus TCP</td>
    <td>Server</td>
    <td>Client</td>
  </tr>
</table>

# References
1. [pymodbus](https://pymodbus.readthedocs.io/en/latest/)
1. [libmodbus](https://github.com/stephane/libmodbus)
