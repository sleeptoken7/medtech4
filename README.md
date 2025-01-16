
//FRAMING#include <stdio.h>
#include <string.h>
void receiver();
char frames[1024];
int main(){
int n,len,i;
char buffer[256],length[10];
printf("How many frames you want to send:");
bzero(buffer,256);
scanf("%d",&n);
for(i=0;i<n;i++){
printf("Enter frame\n");
scanf("%s",buffer);
printf("String length of buffer is %d\n",strlen(buffer));
len=strlen(buffer);
len=len+1;
printf(length,"%d",len);
strcat(frames,length);
strcat(frames,buffer);}
for (i=0;frames[i]!='\0';i++)
printf("%c",frames[i]);
receiver();
return 0;}
void receiver(){
int i=0,framelen,lpvar;
char leninchar;
printf("\n\nThis is the receiver\n");
printf("\nData received is %s",frames);
while(frames[i]!='\0'){
leninchar=frames[i];
framelen=(int)leninchar-(int)'\0';
printf("\nLength of this frame is %d\n",framelen);
printf("\nFrame--->");
lpvar=i+framelen;
while(i<lpvar){
printf("%c",frames[i++]);}
printf("\n");}}

//BITSTUFF
#include <stdio.h>
#include <string.h>
int main() {
char data[100], stuffed[200];
int i, j = 0, count = 0;
printf("Enter the data: ");
scanf("%s", data);
for (i = 0; data[i]; i++) {
stuffed[j++] = data[i];
count = (data[i] == '1') ? count + 1 : 0;
if (count == 5) stuffed[j++] = '0', count = 0;}
stuffed[j] = ‘\0’;
printf("Bit-stuffed data: %s\n", stuffed);
return 0;}

//DVR
#include<stdio.h>
struct node {
    unsigned dist[20], from[20];
} rt[10];
int main() {int dmat[20][20], n, i, j, k, updated;
printf("Enter number of nodes: ");
scanf("%d", &n);
printf("Enter cost matrix (use 999 for infinity):\n");
for (i = 0; i < n; i++) {
for (j = 0; j < n; j++) {
scanf("%d", &dmat[i][j]);
dmat[i][i] = 0; // Distance to self is 0
rt[i].dist[j] = dmat[i][j];
rt[i].from[j] = j;}}
do {
updated = 0; // Track changes
for (i = 0; i < n; i++) {
for (j = 0; j < n; j++) {
for (k = 0; k < n; k++) {
if (rt[i].dist[j] > dmat[i][k] + rt[k].dist[j]) {
rt[i].dist[j] = dmat[i][k] + rt[k].dist[j];
rt[i].from[j] = k;
updated++;}}}}
} while (updated != 0); // Repeat until no changes
 for (i = 0; i < n; i++) {
printf("\nRouter %d table:\n", i + 1);
printf("Node\tVia\tDist\n");
for (j = 0; j < n; j++) {
printf("%d\t%d\t%d\n", j + 1, rt[i].from[j] + 1, rt[i].dist[j]);}}return 0;}

//LeakyBUCKET#include <stdio.h>
#define MIN(x, y) ((x) > (y) ? (y) : (x))
int main() {
int cap, orate, drop = 0, remain = 0, inp, nsec, i;
printf("Enter bucket size: ");
scanf("%d", &cap);
printf("Enter output rate: ");
scanf("%d", &orate);
printf("Enter number of seconds: ");
scanf("%d", &nsec);
printf("\nSec\tReceived\tSent\tDropped\tRemaining\n");
for (i = 1; i <= nsec; i++) {
printf("Enter packets at second %d: ", i);
scanf("%d", &inp);
if (inp > cap) {
printf("Bucket overflow! Packet discarded.\n");continue;}
int sent = MIN(remain + inp, orate);
drop = (remain + inp > orate) ? (remain + inp - orate) : 0;
remain = (remain + inp > orate) ? (remain + inp - orate) : 0;
printf("%d\t%d\t\t%d\t%d\t%d\n", i, inp, sent, drop, remain);}
return 0;}

//NS3-1
#include "ns3/netanim-module.h" #include "ns3/core-module.h" #include "ns3/network-module.h" #include "ns3/internet-module.h" #include "ns3/point-to-point-module.h" #include "ns3/applications-module.h" 
using namespace ns3; 
int main (int argc, char *argv[]) { 
Time::SetResolution(Time::NS);
 NodeContainer nodes; 
nodes.Create(2); 
PointToPointHelper pointToPoint;
 pointToPoint.SetDeviceAttribute ("DataRate", StringValue ("5Mbps"));
 pointToPoint.SetChannelAttribute ("Delay", StringValue ("2ms")); NetDeviceContainer devices; devices = pointToPoint.Install (nodes); 
InternetStackHelper stack; stack.Install (nodes); Ipv4AddressHelper address; address.SetBase ("10.1.1.0", "255.255.255.0");
 Ipv4InterfaceContainer interfaces = address.Assign (devices); 
UdpEchoServerHelper echoServer (9); ApplicationContainer serverApps = echoServer.Install (nodes.Get (1)); 
serverApps.Start (Seconds (1.0));
 serverApps.Stop (Seconds (10.0)); 
UdpEchoClientHelper echoClient (interfaces.GetAddress (1), 9); 
echoClient.SetAttribute ("MaxPackets", UintegerValue (1)); 
echoClient.SetAttribute ("Interval", TimeValue (Seconds (1.0))); 
echoClient.SetAttribute ("PacketSize", UintegerValue (1024));
 ApplicationContainer clientApps = echoClient.Install (nodes.Get (0)); 
clientApps.Start (Seconds (2.0)); 
clientApps.Stop (Seconds (10.0)); AnimationInterface anim ("first.xml"); Simulator::Run (); 
Simulator::Destroy (); 
return 0; }





