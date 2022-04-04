# 计算机网络

TCP/IP四、五、七层模型

## 应用层

### HTTPS

#### 对称加密与非对称加密

##### 对称加密

对称加密指加密和解密使用同一密钥，优点是运算速度快，缺点是如何安全将密钥传输给另一方。常见的对称加密算法有DES、AES等。

##### 非对称加密

非对称加密指的是加密和解密使用不同的密钥（公钥 私钥），公钥加密的信息只有私钥才能解密，私钥加密的信息只有公钥才能解密。优点是解决了对称加密中存在的问题。缺点是运算速度慢。常见的非对称加密算法有RSA、DSA、ECC等。

HTTPS实质上是非对称加密传输对称密钥，对称加密传输报文，从而保证通信效率。

**简化流程：**服务端生成一对非对称密钥，将公钥发给客户端。客户端生成对称密钥，用服务端发来的公钥进行加密，加密后发给服务端。服务端收到后用私钥进行解密，得到客户端的对称密钥。然后通信双方就可以使用对称密钥进行高效的通信了。

#### HTTPS加密过程

1. 客户端向服务器发起第一次握手请求，告诉服务器客户端所支持的SSL的版本、加密算法以及密钥长度等信息。
2. 服务端将自己的公钥发给数字认证机构，数字认证机构利用自己的私钥对服务器的公钥进行数字签名，并给服务器颁发公钥证书。
3. 服务端将证书发给客户端。
4. 客户端利用数字认证机构的公钥，向数字证书认证机构验证公钥证书上的数字签名，确认服务器公开密钥的真实性。
5. 客户端使用服务端公开密钥加密自己生成的对称密钥，发给服务端。
6. 服务端收到后利用私钥解密信息，获得客户端发来的对称密钥。
7. 通信双方可用对称密钥来加密解密信息。

<div align='center'>
    <img src='./imgs/https.png' width='800px'>
    <br/><br/>HTTPS加密过程
</div>


## 传输层

### 传输控制协议（Transmission Control Protocol，TCP）

<div align='center'>
    <img src='./imgs/TCP02.png' width='1000px'>
    <br/><br/>TCP包头
</div>


#### 三次握手

<div align='center'>
    <img src='./imgs/TCP01.png' width='1000px'>
    <br/><br/>TCP三次握手
</div>


##### 为什么三次握手

主要有两方面的原因：

1. TCP是双工通信，为了确保客户端->服务器、服务器->客户端之间的链路都是通的，需要三次握手。服务端能收到客户端的SYN包说明客户端->服务器之间的链路是通的，但不能保证服务器发给客户端的SYN包能顺利到达，假若是两次（一次）握手并且服务器发给客户端的这个SYN包丢失了，会导致出现问题，客户端认为服务端没有和它建立链接，而服务端认为它已经和客户端建立了链接，再如果这时候服务端已经分配了相关资源还会造成资源的浪费。
2. 防止已失效的请求到达服务端（这点最重要），如果是已失效的报文，如果是两次（一次）握手的话，服务端会直接建立链接会造成资源浪费。

##### 建立链接是初始序列号怎么得来的

根据双方地址、端口计算得到的

```c
//net/ipv4/tcp_ipv4.c
	/*
	 * 如果write_seq字段值为零，则说明该传输控制块还
	 * 未设置初始序号，因此需调用secure_tcp_sequence_number()，
	 * 根据双方的地址、端口、系统时间计算初始序列号，同时根据
	 * 发送需要和当前时间得到用于设置IP首部ID域的值。
	 */
	if (!tp->write_seq)
		tp->write_seq = secure_tcp_sequence_number(inet->inet_saddr,
							   inet->inet_daddr,
							   inet->inet_sport,
							   usin->sin_port);

	inet->inet_id = tp->write_seq ^ jiffies;
```

```c
#ifdef CONFIG_INET
static u32 seq_scale(u32 seq)
{
	/*
	 *	As close as possible to RFC 793, which
	 *	suggests using a 250 kHz clock.
	 *	Further reading shows this assumes 2 Mb/s networks.
	 *	For 10 Mb/s Ethernet, a 1 MHz clock is appropriate.
	 *	For 10 Gb/s Ethernet, a 1 GHz clock should be ok, but
	 *	we also need to limit the resolution so that the u32 seq
	 *	overlaps less than one time per MSL (2 minutes).
	 *	Choosing a clock of 64 ns period is OK. (period of 274 s)
	 */
	return seq + (ktime_get_real_ns() >> 6);
}
#endif

u64 siphash_3u32(const u32 first, const u32 second, const u32 third,
		 const siphash_key_t *key)
{
	u64 combined = (u64)second << 32 | first;
	PREAMBLE(12)
	v3 ^= combined;
	SIPROUND;
	SIPROUND;
	v0 ^= combined;
	b |= third;
	POSTAMBLE
        
    /*
    /lib/siphash.c
    #define PREAMBLE(len) \
	u64 v0 = 0x736f6d6570736575ULL; \
	u64 v1 = 0x646f72616e646f6dULL; \
	u64 v2 = 0x6c7967656e657261ULL; \
	u64 v3 = 0x7465646279746573ULL; \
	u64 b = ((u64)(len)) << 56; \
	v3 ^= key->key[1]; \
	v2 ^= key->key[0]; \
	v1 ^= key->key[1]; \
	v0 ^= key->key[0];
	
	#define SIPROUND \
	do { \
	v0 += v1; v1 = rol64(v1, 13); v1 ^= v0; v0 = rol64(v0, 32); \
	v2 += v3; v3 = rol64(v3, 16); v3 ^= v2; \
	v0 += v3; v3 = rol64(v3, 21); v3 ^= v0; \
	v2 += v1; v1 = rol64(v1, 17); v1 ^= v2; v2 = rol64(v2, 32); \
	} while (0)
	
	#define POSTAMBLE \
	v3 ^= b; \
	SIPROUND; \
	SIPROUND; \
	v0 ^= b; \
	v2 ^= 0xff; \
	SIPROUND; \
	SIPROUND; \
	SIPROUND; \
	SIPROUND; \
	return (v0 ^ v1) ^ (v2 ^ v3);
    */
}

u32 secure_tcp_seq(__be32 saddr, __be32 daddr,
		   __be16 sport, __be16 dport)
{
	u32 hash;

	net_secret_init();
	hash = siphash_3u32((__force u32)saddr, (__force u32)daddr,
			    (__force u32)sport << 16 | (__force u32)dport,
			    &net_secret);
	return seq_scale(hash);
}
```




#### 四次挥手

<div align='center'>
    <img src='./imgs/TCP03.png' width='1000px'>
    <br/><br/>TCP四次挥手
</div>

##### 为什么四次挥手

双工通信，确保两个链接都正常关闭。

##### TIME-WAIT和CLOSE-WAIT的区别

TIME-WAIT是主动关闭形成的，当第四次挥手报文发出后，主动关闭链接的一方进入TIME-WAIT状态。

CLOSE-WAIT是被动关闭形成的，当收到主动关闭链接方的FIN报文后，返回ACK报文并进入CLOSE-WAIT状态


#### 拥塞控制

#### 流量控制

#### 滑动窗口

#### TCP如何保证可靠重传

- 校验和

	TCP校验和覆盖TCP首部及TCP数据，而IP首部中的校验和只覆盖IP的首部，不覆盖IP数据报中的任何数据。

	计算校验和与IP类似，每16位字取反后相加，但TCP长度可以为奇数字节，在计算校验和时不足16位的填充0，但填充的字节可能不会被传送。

	计算校验和时TCP段包含一个12B的伪首部，目的是让TCP double check 检查数据是否已经正确到达目的地（IP有没有错接，接受非本IP的，接受非本应用的）。

	如果校验和为0，则最终校验结果为16位全1（65535，二进制反码），如果传送的校验和为0，则说明发送端没有计算校验和。

	<div align='center'>
	    <img src='./imgs/20210921191508.png' width='1000px'>
	    <br/><br/>计算校验和的伪首部
	</div>


	<div align='center'>
	    <img src='./imgs/20210921200313.png' width='1000px'>
	    <img src='./imgs/20210921200329.png' width='1000px'>
	    <br/><br/>输入TCP段的校验和检测
	    <img src='./imgs/20210921200811.png' width='1000px'>
	    <br/><br/>输出TCP段的校验和检测
	</div>


​	

- 序列号

- 超时重传

- 流量控制

- 拥塞避免

#### TCP粘包

### 用户数据报协议（User Datagram Protocol， UDP）

### TCP与UDP区别

|      | 是否面向连接 | 可靠性 | 传输形式   | 传输效率 | 消耗资源 | 应用场景      | 首部字节 |
| ---- | ------------ | ------ | ---------- | -------- | -------- | ------------- | -------- |
| TCP  | 面向连接     | 可靠   | 字节流     | 慢       | 多       | 文件/邮件传输 | 20-60    |
| UDP  | 无连接       | 不可靠 | 数据报文段 | 块       | 少       | 视频/语音传输 | 8        |



## 网络层	

### IP与MAC的区别

IP为逻辑地址，MAC为硬件地址。IP有可能发生变化，不固定，屏蔽了硬件差异。MAC地址是固定的，每个硬件唯一。

IP地址是地域相关的，对于同一个子网上的设备，IP前缀是一样的，便于路由，而MAC分布不规律。

IP地址可以理解为快递的收件地址，MAC地址可以理解为收件人，在一次网络通信中二者是缺一不可的。

## 物理链路层