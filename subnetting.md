Reference: https://www.reddit.com/r/ccna/comments/l9cr5v/subnetting_trick_all_in_your_head_unconventional/
Understand the boundaries of CIDR notation to know which octet to work on:

- HERE.x.x.x ---- 0 to 8
- x.HERE.x.x ---- 8 to 16
- x.x.HERE.x ---- 16 to 24
- x.x.xHERE ---- 24 -32

1. If it's betweeb /1 and /8, you're going to work in the first octet (HERE.x.x.x) ---- 0 to 8
1. If it's between /8 and /16, you're going to work in the second octet (x.HERE.x.x)
2. If it's between /16 and /24, you're going to work in the third octet (x.x.HERE.x)
3. If it's between /24 and /32, you're going to work in the last octet (x.x.x.HERE)

Next up remember the 2x results (only x = [1-7], that's all you need)
21= 2 ,, 22= 4 ,, 23= 8 ,, 24= 16 ,, 25= 32 ,, 26= 64,, 27= 128

Now depending on what the /(MASK) is.. Locate where that (MASK) is situated between boundaries 1,2 or 3 and use the formula for host ((2n)-2) and for subnets (2n).. questions on subnetting mainly ask about the number of hosts or similar, rarely on subnets. Here's a few example to clarify my thought process.

Question: Your router needs to be assigned the first valid host address of the 2nd subnet on network 192.168.4.0/28. What address would you assign? (from subnetting.org)

/28. That's between /24-/32 (boundaries 3) so I need to work on the last subnet. Ignore everything else (x.x.x.0) in our case. Take the higher boundary (32) minus the MASK (28). 32-28=4. 4 is the n power --> 24 = 16 (14 host + Network + Broadcast). That 16 tells us that each 16 hop is going to be a NID in the last octet (remember, we do not care about the other octets.).
192.168.4.0 is the NID for the first subnet, 192.168.4.16 is the NID for the second subnet, 192.168.4.32 is the NID for the third subnet.......
Back to the question, it ask us about 192.168.4.0. Which is the first subnet so 192.168.4.0 - 192.168.4.15 (how do I know it ends at 15? because the second subnet starts at .16). (NID = x.x.x.0, and BID = x.x.x.15) (First = NID + 1 = x.x.x.1) (Last = BID - 1 = x.x.x.14)
192.168.4.0 (NID), 192.168.4.15 (BID), 192.168.4.1 (First), 192.168.4.14 (Last).
Wait.. The question ask about the second subnet though... second subnet NID is 192.168.4.16 (remember each hop of 16) and the Broadcast (BID) is 192.168.4.31 (remember, the next one starts at 32 [16+16])
Using the same principle.. ignore the other octets.. NID (x.x.x.16) BID (x.x.x.31) , First (x.x.x.17), Last (x.x.x.30).
The answer to the question is 192.168.4.17

Question: What is the valid host range for subnet 172.16.32.0/20? (from subnetting.org)

/20 is between /16-/24 so we only work in the third subnet and ignore the rest. 24 (higher boundary) minus the MASK 20 = 4
24 = 16, so each hope of 16 IN THE THIRD OCTET is a NID
x.x.16.x, x.x.32.x, x.x.48.x, x.x.64.x ........ 172.16.16.0, 172.16.32.0, 172.16.48.0, 172.16.64.0 .......
NID (172.16.32.0), BID (172.16.47.255) How do I know that? well, the NID of the next subnet range is 172.16.48.0, so it can't be that.
First (172.16.32.1) and Last (172.16.47.254) which is the answer to the question
Let me know if you have any question. I hope it can help someone and that I wasn't too confusing.

Question: What valid host range is IP address 172.23.23.18/22 part of? (from subnetting.org)

/22 is between /16-/24.
24 - 22 = 2
22 = 4
each NID is hop of 4
x.x.0.x, x.x.4.x, x.x.8.x, x.x.12.x, x.x.16.x, x.x.20.x, x.x.24.x
Question, 172.23.23.18 is between NID 20-24
172.23.20.0 (NID), 172.23.23.255 (BID), 172.23.20.1 (First), 172.23.23.254 (Last)

Question: what is the NID, BID, First and Last host of 10.12.34.145/10

/10 is between /8-/16
16 - 10 = 6 ....... 16 because it's the higher boundary
26 = 64
each NID is a hop of 64 in the SECOND OCTET
x.0.x.x, x.64.x.x, x.128.x.x, x.192.x.x .............
Clearly 10.12.x.x is between the first and second subnet
10.0.0.0 (NID), 10.63.255.255 (BID), 10.0.0.1 (First), 10.63.255.254 (Last)

Question: You have the following subnetted network: 172.16.0.0/21. You need to assign your router the first usable host address on the third subnet. What address would you use?

/21 is between /16-/24, work on third octet, ignore the rest
24-21=3
23 = 8
hop of 8 for each NID
x.x.0.x, x.x.8.x, x.x.16.x, x.x.24.x, x.x.32.x
Third subnet is x.x.16.x
NID=172.16.16.0,, BID=172.16.23.255,, First=172.16.16.1,, Last=172.16.23.254

Question: Network 172.31.0.0 needs to be subnetted into 67 different networks. Each subnet needs a minimum of 300 host addresses. What subnet mask would you use?

Note: Ok here, it's simply understanding 2n for subnets and 2n -2 for hosts

Because it ask 300 MINIMUM host, just need to worry that each subnet holds at least 300. But it specifically ask for 67 networks. So let's work for the subnet part (left to right from the 3rd octet, because this is where it tells us to subnet with the .0.0)

what n --> in 2n gives us 67 different networks
26 = 64
27 = 128
so n = 7
7 bits from /16 (third octet)
16 + 7 = 23 --> /23
how many host can each subnet have in /23
23 ^ 9 = 512 (512 - 2 = 510 valid host)
so /23 is the answer, with a possibility to subnet 172.31.0.0 into 128 subnets holding 510 valid hosts each
You can also go from right to left, but since your question focus more on the number of subnets, I did from left to right.

300 hosts minimum
what n from 2n - 2 will give me that
28 - 2 = 254
29 - 2 = 510
so it's 9
32 - 9 = 23
172.31.0.0/23 is the answer.

Question: What if the question has the mask instead of the CIDR notation?

It's going to be harder for sure. but here's my trick.

for example 255.255.255.248
256-248 = 8
2n = 8
Solve for n
n = 3
/32 - 3 = /29
/29 = 255.255.255.248
Then use /29 for the rest of the problem.
Another example

255.255.255.224
256 - 224 = 32
2n = 32
n = 5
/32 - 5 = /27
Use /27 for the rest.
etc. etc., with the actual mask, you will eventually know it by heart that .252 is /30, .192 is /26....... etc.

Question: How would I do the math without having the CIDR? If I was given 255.255.255.254 instead ?

You just have to know it by heart. I know that 255.255.255.255 is /32

255.255.255.254 is /31
255.255.255.252 is /30
255.255.255.248 is /29
255.255.255.240 is /28

Question: Are you able to subnet a subnet with this method? For example, create 3 subnets of the following ip 125.23.200.64/26 list network ids, host id range, number of usable host IDs, broadcast ids

/26, means we are working in the last octet
32-26=6. 32 because it's in the last octet

2^6=64. Each hop of 64 in the last octet is a NID
125.23.200.64, 125.23.200.128, 125.23.200.192

from the info above, you can find the Broadcast ID
125.23.200.127, 125.23.200.191, 125.23.200.255

Usable host is simply each hop (64 minus the Broadcast ID and the Network ID). Thus 64-2 = 62 usable hosts in each subnets

Question: So I'm practicing on subnetipv4.com and the problem is: 2.59.245.170/29. I'm having a hard time with this one. 32 - 29 = 3 2 to the power of 3 = 8 So hop of 8 for each NID on the last octet. The page says that the NID: 2.59.245.168 Why? I think I'm missing something here and can't wrap my head around it.

Hey my friend, let me break it down for you.

/29 is between /24-/32
so we only work with the last octet
in our case the last octet is .170
what subnet is .170 part of?
we know that each subnet is a hop of 8 because (32-29 = 3) and 23 = 8
x.x.x.0,, x.x.x.8,, x.x.x.16,, x.x.x.24 ...... x.x.x.168,, x.x.x. 176
.170 IP Address is between .168 and .176
so the subnet of .170 is the .168 subnet
x.x.x.168 NID
x.x.x.175 BID
x.x.x.169 First
x.x.x.174 Last
Let me know if you have anymore question. So we basically find what octet to work on, but you still gotta care about what the actual number of the octet is. in your example it's .170

Maybe I should've included an example with your question, it's a very good question that may mislead some people. Thanks a lot!

Question: How many subnets and hosts per subnet can you get from network 172.28.0.0/22?

hey it has been a while, so I had to refresh my memory but for those types of question, you don't need to go as deep (like my method)

You just take the /22 and add/subtract the lower/upper bound of the whole CIDR (/32)

Hosts = 32 - 22 = 10 --> 2^10 = 1024 --> (minus 2 NID/BID) --> 1022 usable hosts

Subnets = 22 - 16 = 6 --> 2^6 = 64 subnets

Why minus 16, because /22 is in the x.x.HERE.x part of the IP and the x.x.HERE.x of the IP is between 16-24, so you minus 16 because you want to know the amount of subnets (going from left-right)

For the number of hosts is going from right-left --> you have to take into account not only x.x.HERE.x but also x.x.x.HERE because, the last octet is also variable in your example

Versus the [172.28] is NOT variable, it'll stay the same for the amount of subnets

Question:
172.28.0.0/22?
Thank you for taking the time to respond. I understand the hosts bit. I get 22 is in the third octet but not why you are taking away 16.
 
So there are "boundaries" on each octet

- HERE.x.x.x ---- 0 to 8

- x.HERE.x.x ---- 8 to 16

- x.x.HERE.x ---- 16 to 24

- x.x.xHERE ---- 24 -32

Because your question is /22 which means it's in the third octet which the boundaries are 16-24

You are supposed to take away left to right for the amount of subnets. When I say left to right I mean going from left of the IP to the right of the IP until it reach the boundary of the octet in question. So going from left to right until the 3rd octet is 16.

You go from right to left, Right of the IP (the end of the IP) to the left of the IP for the amount of HOSTS.

The tricky part is that you can't just use 24 - 22, because the number of hosts also takes into account the last octet VERSUS the amount of subnets, the first two octet IP doesn't change (172.28.x.x) all possible hosts will have these two numbers: 172.28)

Hope that makes more sense, if not let me know :)
