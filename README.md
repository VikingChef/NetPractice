*This project has been created as part of the 42 curriculum by rrasmuss.*

# NetPractice

## Description

NetPractice is a networking exercise focused on configuring small IPv4 networks so that hosts can communicate correctly. The project develops practical understanding of TCP/IP addressing, subnet masks, CIDR (Classless Inter-Domain Routing) notation, default gateways, routers, switches, routing tables, and the relationship between local networks and routed networks.

Across 10 levels, the simulator presents incomplete or incorrect network configurations. The task is to adjust the available IP addresses, masks, gateways, and routes until every required communication path works in both directions.

## Instructions

1. Download and extract the NetPractice files from the project page.
2. From the extracted directory, run:

   ```sh
   ./run.sh
   ```

   This starts a local web server and opens the training interface in a browser.

3. If `run.sh` does not work, start a local server manually:

   ```sh
   python3 -m http.server 49242
   ```

   Then open `http://localhost:49242` in a browser.

4. Enter the 42 login in the interface to load the personal training configuration.
5. Solve each level by adjusting only the editable network fields.
6. Use **Check again** to test the current configuration.
7. After successfully completing each level, use **Get my config** to export the configuration before moving on.

### Submission

The repository root must contain:

- `README.md`
- 10 exported configuration files, one for each NetPractice level

The login must be entered in the training interface before exporting the files.

During the peer evaluation, three random levels must be completed within a limited time. External tools are not allowed during the evaluation; a simple calculator such as `bc` is tolerated.

## Lessons Learned by Level

### Level 1 - Hosts on the same subnet

I learned that two directly connected hosts need valid IPv4 addresses that belong to the same subnet. An IP address identifies a host, while the subnet mask determines which part of the address describes the network.

### Level 2 - Subnet masks, CIDR, and reserved addresses

I learned to convert between dotted-decimal masks and CIDR prefixes, and to calculate subnet blocks. CIDR is the `/number` shorthand for a subnet mask; at first it looked suspiciously like someone had added fractions to IP addresses for entertainment, but it eventually became useful. A `/30` contains four addresses: network, two usable hosts, and broadcast. I also learned that `127.0.0.0/8` is reserved for loopback and cannot be used for ordinary external interfaces.

### Level 3 - Switches and local networks

I learned that devices connected through a switch must still be in the same IP subnet to communicate directly. A switch forwards traffic within the local network, but it does not replace IP routing.

### Level 4 - Adding a router interface to a LAN

I learned that a router interface connected to a switched LAN must belong to the same subnet as the hosts on that LAN. The mask is just as important as the IP address because it defines which destinations are considered local.

### Level 5 - Gateways and routing-table direction

I learned to read a route as **WHERE -> NEXT**: the left side is the destination network, and the right side is the next-hop gateway. This finally stopped routing tables from looking like malicious spreadsheet cells. A host sends traffic for another subnet to a gateway that must itself be directly reachable on the host's local subnet.

### Level 6 - Default routes and return paths

I learned that a working forward path is not enough. The destination also needs a valid return path. `0.0.0.0/0` is the default route, used when no more specific route matches. Internet-facing exercises made the need for explicit return routes especially clear.

### Level 7 - Multiple routers and point-to-point links

I learned to treat each router-to-router connection as its own network. Routers only need to know the next hop, not the entire path. Small `/30` subnets are useful for point-to-point links because they provide exactly two usable host addresses.

### Level 8 - Private/public addressing and route aggregation

I learned that private address ranges are not routed by the simulated Internet, so Internet-reachable hosts must use suitable routable addresses. I also learned that one broader route can sometimes cover several smaller subnets when they all lie behind the same next hop.

### Level 9 - Overlapping subnets and specific routes

I learned how easily a wrong mask can make a router believe that a remote network is directly connected. Specific routes must point to the correct destination network and to a next-hop interface that is locally reachable. This level also reinforced public addressing, `/30` router links, and forward/return-path troubleshooting.

### Level 10 - Planning the address space as a whole

I learned to plan several subnets inside one larger address range without overlap. Router-interface masks determine the networks a router considers directly connected, so host masks alone are not enough when checking whether an address range is free. In other words, “that block looks empty” is not a networking strategy. I also learned to use a broader return route to summarize several internal subnets while keeping the internal links separated with smaller prefixes.

## Resources

### Networking concepts studied

The project required practical understanding of:

- TCP/IP and IPv4 addressing
- subnet masks and CIDR notation
- network, host, and broadcast addresses
- default gateways
- routing tables and next-hop routing
- routers and switches
- local vs routed traffic
- private and public IPv4 address ranges
- route aggregation / summarization
- forward and return paths
- basic OSI-model context for switches, routers, and IP networking

### References

- RFC 791 - Internet Protocol
- RFC 950 - Internet Standard Subnetting Procedure
- RFC 1918 - Address Allocation for Private Internets
- IETF RFC Index and networking documentation
- 42 NetPractice subject and training interface

### Use of AI

AI was used occasionally as a study aid for explanations and to review my reasoning, particularly around subnetting, routing, and simulator error messages. It was also used to help organise this README. All network configurations were changed, tested, and verified manually in the NetPractice interface, and I only kept changes I could understand and explain.
