# AutomateInterfaceAddress-MultipleGroup with Validation
### The script automatically configures IP addresses on network devices
### The script is made for configuring networks with point-to-point connections, each interface receives a /30 address
### The script assumes device's hostname is made by one character and one number. The character identifies the group, the number the ID of the device. NB: ID has to be between 0 and 9. Example: hostname is R1
### The addresses are chosen using a convention. The four octets that made the IP address are selected in the following way
### The first two octets are assigned per Group. Groups info are read from file group_addresses. Example: device R6 is in group R, group R has first two octets 172.16. If two devices have a different group,
### a combinate group is made concatenating the two groups, in alphabetical order. Example: R5 is connected to S2, the group for the link is RS
### The third octet is selected comparing the devices' ID. For each connection, the two IDs are concatenated, with the smaller one in the first position. Example: connection R2 <-> R5, third octet is 25. If two devices belonging to the different groups are linked, the third octet is the ID repeated twice.
### The fourth octet is selected comparing the devices' ID as well. The device with smaller device ID has fourth octet equal to 1, the other device has the fourth octet equals to 2. If two devices belonging to different groups are linked, the fourth octet is ".1" for the device with the lower group (groups are compared in alphabetical order)
### Example: R2 interface Fa0/0 is connected to R3 interface Fa1/1. Group R has assigned address group "172.16". R2 Fa0/0 gets IP 172.16.23.1, R3 Fa1/1 gets ip 172.16.23.2. Example: R3 interface fa0/1 is connected to S3 interface fa1/3. The combined group is SR, SR has assigned address group "172.100". Re Fa0/1 gets ip 172.100.33.1, S3 Fa1/3 gets IP 172.100.33.2
### Each switch has to have at least one interface with a reachable address configured (usually, a management interface). Telnet connections have to be allowed and configured. CDP has to be enabled
### The hostname and the management IP address of each switch have to be specified in the file group_addresses.conf, in the form "hostname mgmt_ip", one device per line.
### NB: cdp has to be disabled on the interface used for the management of the switch (no cdp enable)
### The script is developed for Cisco 2961. FastEth 0/0 and 0/1 are L3 and shut down by default. FastEth 1/X are L2 and no shut by default
### The validation phase is done via ping. The IP address on the remote side of the link is pinged using as the source address the IP on the local side of the link. The result of the ping is obtained analyzing the "success rate" percentage: if it is greater than 0, at least one ping was successful and the connectivity is considered to be up
