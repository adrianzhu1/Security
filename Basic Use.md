Knowledge
file: /etc/snort
config files
/etc/snort--> snort.conf, snortv2.conf, 
/etc/snort/rules--> local.rules

check version
sudo snort -V

1.Sniffer Mode

Parameter	Description
-v	      Verbose. Display the TCP/IP output in the console.
-d	      Display the packet data (payload).
-e	      Display the link-layer (TCP/IP/UDP/ICMP) headers. 
-X	      Display the full packet details in HEX.
-i	      This parameter helps define a specific network interface to listen to or sniff. Once you have multiple interfaces, you can choose a specific interface to sniff. 

2.Packet Logger Mode

Parameter	Description
-l	      Logger mode, target log, and alert output directory. Default output folder is /var/log/snort
          The default action is to dump as tcpdump format in /var/log/snort
-K        ASCII	Log packets in ASCII format.
-r	      Reading option: Review the logged events in Snort.
-n	      Specify the number of packets to be processed or read. Snort will stop after reading the specified number of packets.

sudo snort -dev -l.
sudo snort -dev -K ASCII -l.

sudo snort -r logname.log -X
sudo snort -r logname.log icmp
sudo snort -r logname.log tcp
sudo snort -r logname.log 'udp and port 53'

snort -r snort.log.1640048004 -n 10

3.IDS/IPS

Parameter	Description
-c	      Defining the configuration file.
-T	      Testing the configuration file.
-N	      Disable logging.
-D	      Background mode.
-A	      Alert modes;
          Full: Full alert mode, providing all possible information about the alert. This mode is also the default; once you use -A and don't specify a mode, Snort defaults to this mode.
          Fast mode displays the alert message, timestamp, source and destination IP addresses, along with port numbers.
          Console: Provides fast style alerts on the console screen.
          cmg: CMG style, basic header details with payload in hex and text format.
          None: Disabling alerting.

Sudo snort -c /etc/snort/snort.conf -T
sudo snort -c /etc/snort/snort.conf -D  --stop it use-->  sudo kill
sudo snort -c /etc/snort/snort.conf -A cmg
sudo snort -c /etc/snort/snort.conf -A fast

Using rule file without configuration file
It is possible to run Snort with only rules, without a configuration file. Running Snort in this mode 
will help you test the user-created rules. However, this mode will provide less performance.

sudo snort -c /etc/snort/rules/local.rules -A console


4.PCAP investigation

arameter	              Description
-r / --pcap-single=	    Read a single pcap
--pcap-list=""	        Read pcaps provided in command (space separated).
--pcap-show	            Show pcap name on console during processing.

snort -r icmp-test.pcap
single PCAP
sudo snort -c /etc/snort/snort.conf -q -r icmp-test.pcap -A console -n 10
Multiple PCAPs
sudo snort -c /etc/snort/snort.conf -q --pcap-list="icmp-test.pcap http2.pcap" -A console -n 10
multiple PCAPs with parameter
sudo snort -c /etc/snort/snort.conf -q --pcap-list="icmp-test.pcap http2.pcap" -A console --pcap-show

Rule
create rules 
ex: alert icmp any any <> any (msg:"you want to show"; id:35369; sid:1000001; rev:1;)
then follow process rules and task pcap
snort -c local.rules -A full -l . -r task9.pcap"

finally
cat alert
