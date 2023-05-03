Download Link: https://assignmentchef.com/product/solved-cs3516-project-3-distance-vector-routing-protocol
<br>
<span class="kksr-muted">Rate this product</span>




1. Overview

In this assignment you will be asked to implement a distributed asynchronous distance vector (DV) routing protocol. This project will be completed using C over a network emulator already provided. No knowledge of Unix system calls will be needed to complete this project.

In this programming assignment, you will be writing the sending and receiving IP-level messages for routing data over a network

Your code must execute in an emulated hardware/software environment. The programming interface provided to your routines, i.e., the code that would call your entities from above and from below is very close to what is done in an actual Linux environment.

Figure 1: Default Network Configuration

2. Routines

In this project, you will be writing a “distributed” set of procedures that implement a distributed asynchronous distance vector routing for a network. Figure 1 shows the default node configuration that you will use to develop and test your code.

IMPORTANT NOTE: You code should work for the default node configuration in Figure 1. In addition, we will test your code with a secret node configuration (with 4 nodes). This means the links between the nodes may change and so might the weights compared to Figure 1. Your code should be general enough to work on any configuration of 4 nodes.

The rest of the document is described primarily in terms of the default node configuration given in Figure 1.

For this assignment, you are to write the following routines, which will “execute” asynchronously within the emulated environment that we have written for this assignment.

These routines exist in node0.c, and similarly named routines live in each of the nodeN.c files.

rtinit0() This routine will be called once at the beginning of the emulation. rtinit0() has no arguments. It should initialize the distance table in node0 to reflect the direct costs to is neighbors by using GetNeighborCosts(). In Figure 1, representing the default configuration, all links are bi-directional and the costs in both directions are identical. After initializing the distance table, and any other data structures needed by your node0 routines, it should then send to its directly-connected neighbors (in the Figure, 1, 2 and 3) its minimum cost paths to all other network nodes. This minimum cost information is sent to neighboring nodes in a routing packet by calling the routine tolayer2(), as described below. The format of the routing packet is also described below.

rtupdate0(struct rtpkt *rcvdpkt). This routine will be called when node 0 receives a routing packet that was sent to it by one of its directly connected neighbors. The parameter *rcvdpkt is a pointer to the packet that was received rtupdate0() is the “heart” of the distance vector algorithm. The values it receives in a routing packet from some other node i contain i’s current shortest path costs to all other network nodes. rtupdate0() uses these received values to update its own distance table (as specified by the distance vector algorithm). If its own minimum cost to another node changes as a result of the update, node 0 informs its directly connected neighbors of this change in minimum cost by sending them a routing packet. Recall that in the distance vector algorithm, only directly connected nodes will exchange routing packets. Thus, for the example in Figure 1, nodes 1 and 2 will communicate with each other, but nodes 1 and 3 will not communicate with each other.

Figure 2 provides a conceptual view of the relationship of the procedures inside node 0.Similar routines are defined for nodes 1, 2 and 3. Thus, you will write, at a minimum, 8 procedures in all: rtinit0(), rtinit1(), rtinit2(), rtinit3(),rtupdate0(), rtupdate1(), rtupdate2(), rtupdate3().

Figure 2: Node Procedure Relationships

3. Software Interfaces

The procedures described above are the ones that you will write. We have written the following routines that can be called by your routines:toLayer2(struct rtpkt pkt2send); where struct rtpkt is defined below. The purpose of this routine is to provide communication between the various nodes in your network. The procedure tolayer2() is defined in the file project3.c

getNeighborCosts( int myNodeID ); This routine allows your node to discover what other nodes are around it and what are the costs to those nodes. This routine returns a pointer to a struct NeighborCosts that is defined on a later slide. getNeighborCosts() is implemented in project3.c

printdt0( int MyNodeNumber, struct NeighborCosts *neighbor, struct distance_table *dtptr );This is an optional print function you can use. It will pretty print the distance table for node0. Please look in your node0.c code for an explanation of how it works and what you need in order to call it. What should be of special interest to you is that this print code is the same in each of the nodes; in other words, I’m using the input parameters to tailor the routine to work in general for all nodes. There is no need for me to write a separate and unique routine for each node. The distance table of each node is the principal data structure used by the distance vector algorithm. You may find it convenient to declare the distance table as a N-by-N array of int’s, where entry [i,j] in the distance table in node 0 is node 0’s currently computed cost to node i via direct neighbor j. If 0 is not directly connected to j, you can ignore this entry. We will use the convention that the integer value 9999 is “infinity.” Note that this distance table on each node may be used to run distance vector (DV) algorithm for updating the distance vector.

4. Software Structures

These are the data structures that you will use to communicate with the routines in Layer 2. You will certainly need additional structures of your own crafting to maintain additional information. extern struct rtpkt {int sourceid; // id of node sending this packet, 0, 1, 2, or 3

int destid; // id of router to which pkt is sent int mincost[MAX_NODES]; // min cost to node 0 … N };

An instance of this structure is declared in your starter node0.c … node3.c code.

struct NeighborCosts {int NodesInNetwork; // The total number of nodes in the entire network int NodeCosts[MAX_NODES]; // An array of cost};

An instance of a pointer to this structure is declared in your starter node0.c … node3.c code. Note that the filled-in structure is allocated in the function getNeighborCosts() and a pointer to this structure is returned by this function. Here’s the information returned as seen by gdb in my node0.c when using the default configuration shown in Figure 1.

(gdb) p *neighbor0$2 = {NodesInNetwork = 4, NodeCosts = {0, 1, 3, 7, 9999, 9999, 9999, 9999, 9999, 9999}}

MAX_NODES is set to 10. The simulator is able to support a network containing up to 10 nodes. But you will not need to work with more than 4 nodes.

6. Environment

You should put your procedures for nodes 0 through 3 in files called node0.c, …. node3.c. You are NOT allowed to declare any global variables that are visible outside of a given C file (e.g., any global variables you define in node0.c. may only be accessed inside node0.c). This is to force you to abide by the coding conventions that you would have to adopt is you were really running the procedures in four distinct nodes.

Your project will be evaluated on WPI server linux.wpi.edu.

To compile your routines:

gcc project3.c node0.c node1.c node2.c node3.c –o project3

You can download the code from here

https://users.wpi.edu/~yli15/courses/CS3516Fall20A/projects/Proj3/project_3_A20.zip

7. Output Trace

For your sample output, your procedures should print out a message whenever your rtinit0(), rtinit1(), rtinit2(), rtinit3() or rtupdate0(), rtupdate1(), rtupdate2(), rtupdate3() procedures are called, giving the time (available via my global variable clocktime).

For rtupdate0(), rtupdate1(), rtupdate2(), rtupdate3() you should

<ul>

 <li>Print the identity of the sender of the routing packet that is being passed to your routine,</li>

 <li>Whether or not the distance vector is updated, print the contents of the distance vector,e.g. for node 0, its shortest distance vector is [0,1,2,4] representing the shortest distancesto node 0, node 1, node 2, and node 3, respectively.</li>

 <li>A description of any messages sent to neighboring nodes as a result of any distancevector update.We will be evaluating your output listing with a TraceLevel value = 1. Highlight the final distance vector produced in each node. Your program will run until there are no more routing packets in transit in the network, at which point our emulator will terminate. Bellow you can see a sample output, with the last (finalized) distance vector update highlighted with yellow color for each node. They are the final distance vectors after convergence. When evaluating your program, we will look for those LAST updates and check if they are least costs (i.e., shortest distances) or not.-bash-3.2$ ./project3 1At time t=0.000000, rtinit0() called.At time t=0.000000, node 0 initial distance vector: 0 1 3 7At time t=0.000000, node 0 sends packet to node 1 with: 0 1At time t=0.000000, node 0 sends packet to node 2 with: 0 1At time t=0.000000, node 0 sends packet to node 3 with: 0 1At time t=0.000000, rtinit1() called.At time t=0.000000, node 1 initial distance vector: 1 0 1 9999At time t=0.000000, node 1 sends packet to node 0 with: 1 0 1 9999</li>

</ul>

At time t=0.000000, node 1 sends packet to node 2 with: 1 0 At time t=0.000000, rtinit2() called.At time t=0.000000, node 2 initial distance vector: 3 1 0 2 At time t=0.000000, node 2 sends packet to node 0 with: 3 1 At time t=0.000000, node 2 sends packet to node 1 with: 3 1 At time t=0.000000, node 2 sends packet to node 3 with: 3 1 At time t=0.000000, rtinit3() called.

3 7 3 7 3 7

1 9999

0 2 0 2 0 2

At time t=0.000000, node 3 initial distance vector: 7 9999 2 0

At time t=0.000000, node 3 sends packet to node 0 with: 7 9999 2 0At time t=0.000000, node 3 sends packet to node 2 with: 7 9999 2 0At time t=0.124745, rtupdate2() called, by a pkt received from Sender id: 0. At time t=0.586824, rtupdate3() called, by a pkt received from Sender id: 0. At time t=0.586824, node 3 current distance vector: 7 8 2 0At time t=0.586824, node 3 sends packet to node 0 with: 7 8 2 0At time t=0.586824, node 3 sends packet to node 2 with: 7 8 2 0At time t=0.609690, rtupdate2() called, by a pkt received from Sender id: 1. At time t=0.609690, node 2 current distance vector: 2 1 0 2At time t=0.609690, node 2 sends packet to node 0 with: 2 1 0 2At time t=0.609690, node 2 sends packet to node 1 with: 2 1 0 2At time t=0.609690, node 2 sends packet to node 3 with: 2 1 0 2At time t=0.745790, rtupdate0() called, by a pkt received from Sender id: 1. At time t=0.745790, node 0 current distance vector: 0 1 2 7At time t=0.745790, node 0 sends packet to node 1 with: 0 1 2 7At time t=0.745790, node 0 sends packet to node 2 with: 0 1 2 7At time t=0.745790, node 0 sends packet to node 3 with: 0 1 2 7At time t=0.810576, rtupdate3() called, by a pkt received from Sender id: 2. At time t=0.810576, node 3 current distance vector: 5 3 2 0At time t=0.810576, node 3 sends packet to node 0 with: 5 3 2 0At time t=0.810576, node 3 sends packet to node 2 with: 5 3 2 0At time t=0.867797, rtupdate3() called, by a pkt received from Sender id: 2. At time t=0.867797, node 3 current distance vector: 4 3 2 0At time t=0.867797, node 3 sends packet to node 0 with: 4 3 2 0At time t=0.867797, node 3 sends packet to node 2 with: 4 3 2 0At time t=0.962070, rtupdate2() called, by a pkt received from Sender id: 3. At time t=1.080034, rtupdate1() called, by a pkt received from Sender id: 0. At time t=1.080034, node 1 current distance vector: 1 0 1 8At time t=1.080034, node 1 sends packet to node 0 with: 1 0 1 8At time t=1.080034, node 1 sends packet to node 2 with: 1 0 1 8At time t=1.224738, rtupdate0() called, by a pkt received from Sender id: 2. At time t=1.224738, node 0 current distance vector: 0 1 2 5At time t=1.224738, node 0 sends packet to node 1 with: 0 1 2 5At time t=1.224738, node 0 sends packet to node 2 with: 0 1 2 5At time t=1.224738, node 0 sends packet to node 3 with: 0 1 2 5At time t=1.380520, rtupdate2() called, by a pkt received from Sender id: 3. At time t=1.383899, rtupdate2() called, by a pkt received from Sender id: 0. At time t=1.565862, rtupdate2() called, by a pkt received from Sender id: 3. At time t=1.657278, rtupdate3() called, by a pkt received from Sender id: 0. At time t=1.826770, rtupdate0() called, by a pkt received from Sender id: 3. At time t=2.077403, rtupdate3() called, by a pkt received from Sender id: 0. At time t=2.275775, rtupdate0() called, by a pkt received from Sender id: 3. At time t=2.276489, rtupdate2() called, by a pkt received from Sender id: 3. At time t=2.768510, rtupdate1() called, by a pkt received from Sender id: 2. At time t=2.768510, node 1 current distance vector: 1 0 1 3At time t=2.768510, node 1 sends packet to node 0 with: 1 0 1 3At time t=2.768510, node 1 sends packet to node 2 with: 1 0 1 3At time t=3.166427, rtupdate0() called, by a pkt received from Sender id: 2. At time t=3.956352, rtupdate1() called, by a pkt received from Sender id: 2. At time t=3.981693, rtupdate0() called, by a pkt received from Sender id: 3. At time t=4.103415, rtupdate2() called, by a pkt received from Sender id: 1. At time t=4.226752, rtupdate0() called, by a pkt received from Sender id: 3. At time t=5.663148, rtupdate2() called, by a pkt received from Sender id: 0. At time t=5.667306, rtupdate1() called, by a pkt received from Sender id: 0. At time t=5.743729, rtupdate0() called, by a pkt received from Sender id: 1. At time t=6.198489, rtupdate2() called, by a pkt received from Sender id: 1. At time t=6.780353, rtupdate0() called, by a pkt received from Sender id: 1. At time t=6.780353, node 0 current distance vector: 0 1 2 4At time t=6.780353, node 0 sends packet to node 1 with: 0 1 2 4At time t=6.780353, node 0 sends packet to node 2 with: 0 1 2 4

At time t=6.780353, node 0 sends packet to node 3 with: 0 1 2 4At time t=6.817634, rtupdate1() called, by a pkt received from Sender id: 0. At time t=7.599638, rtupdate3() called, by a pkt received from Sender id: 0. At time t=7.960743, rtupdate2() called, by a pkt received from Sender id: 0. At time t=8.295065, rtupdate1() called, by a pkt received from Sender id: 0.

10. FINAL NOTE

Make sure that the shortest paths you discover between nodes in the network do not have any loops. Therefore, a path of the form n1-&gt;n0-&gt;n1-&gt;n2-&gt;n3 is not valid.

11. Submission

As a part of the submission you need to provide the following:

Create a zip file “your-wpi-login_project3.zip”

<ol>

 <li>All the code for the distance vector (DV) protocol (any .c and .h files), including any codethat was pre-provided as part of this project</li>

 <li>A ReadMe (txt) file that spells out exactly how to compile and run the code</li>

 <li>Makefile to make compilation easy.</li>

</ol>