너무 맘에 든다. 

해커를 위한 책이라고 하지만, 해킹에 관한 기법보다는 network application을 작성하는데 유용한 scapy, dpkt 그리고 정규식에 대한 설명이 유용하다. 새 책을 사긴 그렇고 중고책을 하나 구할 까 했는데 알라딘에서 중고책 매입가가 4천원 대. 작년 초에 나온 책인데 너무 싸게 매입하는 게 아닌가

일단 책 보면서 유용하다 싶은 내용을 몇 가지 주제로 나눠 정리했다. 

### 소스 코드를 받을 수 있는 곳은

* [github](https://github.com/shadow-box/Violent-Python-Examples)  
* [COMPANION MATERIALS](http://booksite.elsevier.com/9781597499576/chapters.php)

### Semaphore  

	screenLock = Semaphore(value = 1)
    screenLock.acquire()
	screeLock.release(()


### pxssh library  

	s = pxssh.pxssh()
	s.login(host, user, password)
	s.sendline(xxx)
	s.prompt()  


## scapy

*'haslayer()' is powerful to check if packet has known protocol    

    from scapy.all import *   
           
	if pkt.haslayer(IP)  
	if pkt.haslayer(TCP)    
	if pkt.haslayer(ICMP)

* To see the file name use scapy  

		$ scapy
		Welcome to Scapy  
		>>> ls(IP)
		...

* CH4, 7-testFastFlux.py - pcap 파일을 읽어 특정 데이터를 가진 경우 확인

         def handlePkt(pkt):
             if pkt.haslayer(DNSRR):
                 rrname = pkt.getlayer(DNSRR).rrname
                 rdata = pkt.getlayer(DNSRR).rdata 
     
         pkts = rdpcap('fastFlux.pcap')
         for pkt in pkts:
             handlePkt(pkt)

* CH4, 6-spoofDetect.py - sniff해서 특정 데이터를 가진 패킷 확인  

		def testTTL(pkt):
		    try:
		        if pkt.haslayer(IP):
		            ipsrc = pkt.getlayer(IP).src
		            ttl = str(pkt.ttl)

		conf.iface = 'eth0'
		sniff(filter="tcp port 21", prn=testTTL, store=0)
		# filter is optional

* Make a packet easily  

		IPlayer = IP(src="1.1.1.1", dst="2.2.2.2")  
		TCPlayer = TCP(sport=123, dport=456)  
		pkt = IPlayer / TCPlayer  
		send(pkt)  

* or
		pkt = IP(src="1.1.1.1", dst="2.2.2.2") / TCP(dport=456) / Raw(load="^\xB0\x02\x89\x06\xFE\xC8\x89F\x04\xB0\x06\x89F")
		send(pkt, iface="eth0", count=10)  

		pkt = IP(src="1.1.1.1", dst="2.2.2.2") / TCP(dport=456) / Raw(load="Amanda")
		send(pkt, iface="eth0", count=10)  

* Too see the packet has string XXX=YYY and get YYY  

		raw = raw.sprintf("%Raw.load%")  
		name=re.findall("(?i)XXX=(.*)&", raw)
* Too see the string "Amanda" with tcpdump, use `-A` option which means ASCII  


* CH5, 7-uavSniff.py - good example of using Class  

   
## DPKT module

###  p144  
  
    
* CH4, 4-googleEarthPcap.py
* IP 주소를 이용해 IP의 물리적인 위치 정보를 찾고, 이를 이용해 google earth에 표시하기 위해 KML 파일 생성
* dpkt를 이용한 패킷 분석  
* dpkt는 pip로 설치 불가  

        for ts, buf in pcap:  
            ethernet = dpkt.ethernet.Ethernet(buf)  
            ip = eth.data
            src = socket.ntoa(ip.src)  
                
### p147  

* dpkt을 이용한 HTTP 분석
* 상위 protocol header를 가리킬 때 x.data 사용  
* 5-findDDoS.py  

        tcp = ip.data  
        http = dpkt.http.Request(tcp.data)  
        if http.method == 'GET':
        	uri = http.uri.lower()  
        	if '.zip' in uri:
        		print "bingo"  
        		
### p149  
        tcp = ip.data  
        sport = tcp.sport
        dport = tcp.dport  
        if '!lazor' in tcp.data.lower():
        	print "bingo"  



* except KeyboardInterrupt:  

        try:
        	foobar()
        except KeyboardInterrupt:
        	sys.exit(0)  


### Regular Expression  

        >>> print re.findall("(?i)XXX=(.*)", "!#ASFDASFASDFAS#$%$#XXX=YYY")
        ['YYY']
        >>> print re.findall("XXX=(.*)", "!#ASFDASFASDFAS#$%$#XXX=YYY")
		['YYY']

		왜 `(?i)`를 사용하는 걸까?

		>>> re.findall(r'[0-9a-zA-Z]{3}', "123 2314 13")
		['123', '231']

### Mechanize module

  0 #!/usr/bin/python                                                                                                                  
  1 # -*- coding: utf-8 -*-
  2 import mechanize
  3 
  4 
  5 def viewPage(url):
  6     browser = mechanize.Browser()
  7     page = browser.open(url)
  8     source_code = page.read()
  9     print source_code
 10 
 11 
 12 viewPage('http://www.syngress.com/')
 13 


 ### Twitter API

 * CH6, 9-twitterInterests.py  


 ## Template

		#!/usr/bin/python                                                                
		# -*- coding: utf-8 -*-

		def main(): 
		    parser = optparse.OptionParser('usage %prog -i <interface>') 
		    parser.add_option('-i', dest='interface', type='string', help='specify interface to listen on')

		    (options, args) = parser.parse_args() 
		 
		    if options.interface == None: 
		        print parser.usage 
		        exit(0) 
		    else: 
		        conf.iface = options.interface 

		    try:
		        print '[*] Starting Credit Card Sniffer.'
		        sniff(filter='tcp', prn=findCreditCard, store=0)
		    except KeyboardInterrupt:
		        exit(0)


		if __name__ == '__main__':
		    main()

## Template(Class)

		#!/usr/bin/python                                                                
		# -*- coding: utf-8 -*-

		class testThread(threading.Thread):

		    def __init__(self):
		        threading.Thread.__init__(self)

		        # add intialize variables or initialization functions

		    def run(self):
		    	# main function of this class

		def main():
		    testInstance = testThread()
		    testInstance.start()
