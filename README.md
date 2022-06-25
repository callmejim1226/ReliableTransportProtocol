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
