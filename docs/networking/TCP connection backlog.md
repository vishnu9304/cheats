### Linux TCP Backlogs

Ref: https://bunn.cc/2017/syn-backlog/

#### Key Points

1 - The incoming connections get queued up, waiting for the application to accept(2) them.

2 - The queue that holds established connections is (of course) of finite length. 

3 - The current version of the linux uses 2 distinct queues.
    SYN Queue with a size specified by the system wide setting **/proc/sys/net/ipv4/tcp_max_syn_backlog**
    Accept Queue with a size specified by the application, if not specified it will defaults to **/proc/sys/net/core/somaxconn**

#### What will happen if **Accept Queue** is full?

If the TCP implementation in Linux receives the ACK packet of the 3-way handshake and the accept queue is full, it will basically ignore that packet.

**Note:**  There is a timer associated with the SYN RECEIVED state. If the **ACK** packet is not received (or if it is ignored, as in the case considered here), then the TCP implementation will resend the **SYN/ACK** packet (with a certain number of retries specified by **/proc/sys/net/ipv4/tcp_synack_retries** and using an exponential backoff algorithm). After retries it will sent **RST** packet.

#### What is Half Open Connections?

If the client first waits for data from the server and the server never reduces the backlog, then the end result is that on the client side, the connection is in state ESTABLISHED, while on the server side, the connection is considered CLOSED.
This means that we end up with a half-open connection!

**Note:**  If the accept queue is full, then the kernel will impose a limit on the rate at which **SYN** packets are accepted.
If too many SYN packets are received, some of them will be dropped.
In this case, it is up to the client to retry sending the **SYN** packet and we end up with the same behavior as in BSD derived implementations.