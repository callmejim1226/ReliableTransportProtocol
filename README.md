## Description
For this project, me and my partner designed a simple transport protocol that provides reliable datagram service. Our protocol will be responsible for ensuring data is delivered in order, without duplicates, missing data, or errors. Since the local area networks at Northeastern are far too reliable to be interesting, we were provided with access to a virtual machine that will emulate an unreliable network.

For the assignment, we wrote code that will transfer a file reliably between two nodes (a sender and a receiver). We assumed that the receiver is run first and will wait indefinitely, and the sender can just send the data to the receiver.

## Our Programs
For this project, we have a sending program 3700send that accepts data and sends it across the network, and a receiving program 3700recv that receives data and prints it out in-order.  Limiting ourselves to not use any other transport protocol libraries in our project (such as TCP), we used UDP as a transport protocol. We had to construct the packets and acknowledgements ourself, and interpret the incoming packets ourself.

## Simulator
As noted above, rather than testing your router on a real network, we will test it in a simulator. The simulator takes care of reading in the configuration file, launching your sender and receiver, forwarding messages between them, simulating the network, feeding in data to your sender, and verifying that the your receiver prints out the right data. The simulator comes with a suite of configuration files in the directory configs/ that define situations your code must be able to handle. These are the same configuration files that we will use when evaluating everyone’s code (in addition to a few additional configurations we do not reveal). Note that you do not need to parse the configuration files; the simulator is responsible for this functionality.

The simulator is implemented in a script named run that is available as part of the starter code.

At no point during this project should you modify run. Furthermore, when we grade your code, we will use the original versions of run and the configurations in configs/, not your (possibly modified) versions.

The simulator accepts the following command line spec:

$ ./run <config-file>
  
The simuulator will read in the config file, start up your sender and receiver with the appropriate arguments, and then feed your sender data. The simulator will record the output your receiver prints out, and will compare that output to what is expected. The simulator will tell you whether your router passed the test case, printing out a summary of the error if your code did not do what was expected.

## Program Specification
The command line syntax for your sending program is given below. The sender program takes command line arguments of the remote IP address and UDP port number. The syntax for launching your sending program is therefore:

$ ./3700send <recv_host> <recv_port>
  
recv_host (Required) The domain name (e.g., “foo.com”) or IP address (e.g., “1.2.3.4”) of the remote host
recv_port (Required) The UDP port of the remote host.
The sender must open a UDP socket and send packets to the given IP address on the given port.

The data that the sender must transmit to the receiver must be supplied in STDIN. The sender must read in the data from STDIN and transmit it to the receiver via the UDP socket. Your program may assume that the 3700recv program is running on the remote host/port (i.e., you do not need to worry about connection setup).

Your sender may print out any debug information that you wish, but it must do so to STDERR.

The command line syntax for your receiving program is given below. The receiving program takes a single command line argument of the UDP port number to bind to. The syntax for launching your receiving program is therefore:

$ ./3700recv
  
On startup, the receiver must bind to a UDP port, and the first thing that it prints out to STDERR must be the message:

Bound to port <port>
where <port> is the local UDP port number it chose. It will then wait for datagrams from the sender. Similar to 3700send, you may add your own output messages to your receiver but they must be printed to STDERR.

The receiver program must print out the data that it receives, and only the data it receives, from the sender to STDOUT. The data that it prints must be identical to the data that was supplied to the sender via STDIN. In other words, data cannot be missing, reordered, or contain any bit-level errors.

NOTE: Your 3700send program must exit once it has determined the data has been successfully received by the sender. This is the signal that the simulator uses to tell that the test is over. Your 3700recv program should not exit of its own volition, even after it believes it has received all of the data; the simulator will kill it automatically once the 3700send program exits.


## Approach 
- We used UDP as our transfer layer protocol (socket) in order to let our sender and receiver work together and transmit data reliably.
- In order to transfer data between our sender and receiver, we implemented a three-way handshake, and to increase the speed of transferring the data, we had to increase the size of congestion window starting from slow start. 
- In order to transfer a file with any number of packets dropped, duplicated, and delayed, and under a variety of different available bandwidths and link latencies, in our code, we made sure our receiver send proper ack, where it contains the right sequence number to our sender. We also made sure our sender has a proper size of congestion window to send packets and receive acks from the receiver. 


## Challenges
- We first faced the challenge when we had to transfer a file with any number of packets duplicated. It was challenging to fix this issue since our receiver receives a segment that has a bigger number of packets than expected. 
- Similarly, we also faced the challenge when we had to transfer a file with any number of packets dropped. It was challenging since we had to write a code in our receiver that will handle the issue when our receiver didn't get the expected packet because this packet has been dropped while our sender sent it to our receiver. 
- Moreover, when our receiver receives the segment that has reordered packets, we had to make sure that our code in the receiver sends the proper ack to our sender every time the receiver receives the data from our sender. 


## Tests
-  We included print statements in our STDERR and run nettest in live to see what sequence number does our receiver receives from our sender, and what ack number does our sender receives from our receiver. 
- We set our netsim with a certain number of bandwidth, latency, delay, drop, reorder, and duplicate and ran nettest with every size inputs to confirm our code is running successfully. 
