# Deliverables

## Explanation of why the missing route breaks both directions (forward path vs. return path)

- Forward path: The packet makes it to srl2 (10.1.4.1, host2's default gateway).  
srl2 looks up 10.1.5.0/24 in its routing table, there is no static route, srl2 has no idea where to send it.  
It actively replies with an ICMP "Destination Net Unreachable" message back to host2 - `From 10.1.4.1`.  
The router knows it doesn't know, and is telling us.
- Return path: The packet from host3 (10.1.5.2) reaches srl3, which has a valid route to 10.1.4.0/24 via srl1 (the hub).  
srl1 also has a valid route to 10.1.4.0/24 via srl2. The packet successfully reaches host2.  
Host2 then tries to reply to 10.1.5.2. It sends the reply to its gateway srl2 — which again has no route to 10.1.5.0/24.  
This time, srl2 is dropping the return traffic and does not send an ICMP error back.  
From host3's perspective, it simply never got a response.  
It has no visibility into what happened on the return path. It just times out.
