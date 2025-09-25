![[Pasted image 20241206212811.png]]
- line connect among router together with technique ? 
	- EX: 
		- Lease line how to work and have ip same with Fireber FTTH ?
		- vpn ... ?

------------------------------------------------------------------------

- [Router Edge want connect to internet need configure default route to gateway](https://www.google.com/search?q=how+to+setup+default+gateway+on+interface+cisco+router&oq=how+to+setup+defaul+gateway+on+interface+&gs_lcrp=EgZjaHJvbWUqCQgCECEYChigATIGCAAQRRg5MgkIARAhGAoYoAEyCQgCECEYChigATIJCAMQIRgKGKAB0gEKMjg3OTNqMGoxNagCCLACAQ&sourceid=chrome&ie=UTF-8#fpstate=ive&vld=cid:006ccbf8,vid:XyKxoDPr5_c,st:0)
```bash
ip route 0.0.0.0 0.0.0.0 172.16.18.68
```

![[Pasted image 20241206225416.png]]


- Router Edge not receive dhcp form internet cloud interface ( because eve-ng vm on promox server vm only config <mark style="background: #FFF3A3A6;">vmbr0</mark> same with LAN LOCAL HOME LAB)

![[Pasted image 20241206225751.png]]