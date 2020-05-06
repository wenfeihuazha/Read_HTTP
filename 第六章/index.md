# HTTP首部

## 6.1 HTTP报文首部

### HTTP请求报文

由:方法,HTTP版本,HTTP首部字段等部分构成.

### HTTP响应报文

由:方法,HTTP版本,状态码,HTTP首部字段构成

## 6.2 HTTP首部字段
他能起到传递额外重要信息的作用
> 首部字段给浏览器和服务器提供报文主体大小,所使用的语言,认证信息等内容

### 6.2.4 HTTP/1.1 首部字段一览
表 6-1: 通用首部字段
> P80
表 6-2: 请求首部字段
> P81
表 6-3: 响应首部字段
>P81
表 6-4: 实体首部字段
>P82

### 6.3.1 Cache-Control
通过职得首部字段Cache-Control的指令,就能操作缓存的工作机制.
>可以理解为通过该字段操作缓存信息

|指令类型|功能|
|:----:|:---:|
|public|明确表明其他用户也可利用缓存|
|private|与public行为相反(缓存服务器会对该特定用户提供资源缓存的服务,对其他用户发送过来的请求,代理服务器则不会返回缓存|
|no-cache|客户端角度: 不要缓存数据要从源服务器取数据  服务器角度:可以缓存,但是使用前要再确认|
|no-store|暗指请求或响应中包含机密信息,因此不能缓存|
|s-maxage|功能与max-age相同,但是s-maxage指令只适用于多位用户使用的公共缓存服务器.也就是说对于同一用户重复返回响应的服务器来说,没有任何作用|
|max-age|判断缓存时间比指定时间的数值更小,那么就接收缓存的资源.(如果设置为0,那么缓存服务器通常需要将请求转发给源服务器). 服务器返回响应中包含max-age时,缓存服务器将不对资源的有效性再做确认,max-age数值代表资源保存为缓存的最长时间|
|min-fresh|要求服务器返回至少还未过指定时间的缓存资源(假设60 就是60秒|
|max-stale|在指定时间内,过期也照常接收,为空的话,无论过期多久都正常接收|
|only-if-cached|只请求本地缓存数据,如果无响应,返回状态码504|
|must-revalidate|再次想服务器验证缓存是否有效,若无响应,应返回504(与max-stale冲突|
|proxy-revalidate|所有缓存服务器在接收到客户端带有该指令的请求返回响应之前,必须再次验证缓存的有效性|
|no-transform|规定无论是在请求还是响应中,缓存都不能改变实体主体的媒体类型(防止缓存或代理压缩图片等操作|
|cache-control|可以拓展指令(类似自定义指令,在服务端可以设置理解时有意义|

### 6.3.2 Connection
#### 控制代理不再转发的首部字段
#### 管理持久连接
>HTTP/1.1版本默认连接都是持久连接,当首部字段的值设置为close

### 6.3.4 Pragma
是HTTP/1.1版本以前历史遗留字段.
>pragma: no-cache  
在中间服务器如果不能以HTTP/1.1位基准时的处理方案

### 6.4.1 Accept
可以通知服务器,用户代理能够处理的媒体类型
>Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8  

文件类型|值
-------|---------
|文本文件|text/html, text/plain, text/css,application/xhtml+xml, application/xml|
|图片文件|image/jpeg, image/gif, image/png ...|
|视频文件|video/mpeg, video/quicktime ...|
|应用程序使用的二进制文件|application/octet-stream, application/zip ...|

>若想要给显示的媒体类型增加优先级，则使用 q= 来额外表示权重值 1，用分号（;）进行分隔。权重值 q 的范围是 0~1（可精确到小数点 后 3 位），且 1 为最大值。不指定权重 q 值时，默认权重为 q=1.0。

### 6.4.2 Accept-Charset
> Accept-Charset 首部字段可用来通知服务器用户代理支持的字符集及 字符集的相对优先顺序。另外，可一次性指定多种字符集。与首部字 段 Accept 相同的是可用权重 q 值来表示相对优先级。
 
### 6.4.3 Accept-Encoding
Accept-Encoding 首部字段用来告知服务器用户代理支持的内容编码及 内容编码的优先级顺序。可一次性指定多种内容编码。

类型|值
----|---
gzip | 由文件压缩程序 gzip（GNU zip）生成的编码格式 （RFC1952），采用 Lempel-Ziv 算法（LZ77）及 32 位循环冗余 校验（Cyclic Redundancy Check，通称 CRC）。
compress|由 UNIX 文件压缩程序 compress 生成的编码格式，采用 Lempel- Ziv-Welch 算法（LZW）。
deflate | 组合使用 zlib 格式（RFC1950）及由 deflate 压缩算法 （RFC1951）生成的编码格式。
identity | 不执行压缩或不会变化的默认编码格式

> 采用权重 q 值来表示相对优先级，这点与首部字段 Accept 相同。另 外，也可使用星号（*）作为通配符，指定任意的编码格式。

### 6.4.4 Accept-Language

> Accept-Language: zh-cn,zh;q=0.7,en-us,en;q=0.3

首部字段 Accept-Language 用来告知服务器用户代理能够处理的自然 语言集（指中文或英文等），以及自然语言集的相对优先级。可一次 指定多种自然语言集。 和 Accept 首部字段一样，按权重值 q 来表示相对优先级。

### 6.4.5 Authorization

> Authorization: Basic dWVub3NlbjpwYXNzd29yZA==

首部字段 Authorization 是用来告知服务器，用户代理的认证信息（证 书值）。通常，想要通过服务器认证的用户代理会在接收到返回的 401 状态码响应后，把首部字段 Authorization 加入请求中。共用缓存 在接收到含有 Authorization 首部字段的请求时的操作处理会略有差 异

### 6.4.6 Expect
> Expect: 100-continu

客户端使用首部字段 Expect 来告知服务器，期望出现的某种特定行 为。因服务器无法理解客户端的期望作出回应而发生错误时，会返回状态码 417 Expectation Failed。   

客户端可以利用该首部字段，写明所期望的扩展。虽然 HTTP/1.1 规范只定义了 100-continue（状态码 100 Continue 之意）。

### 6.4.7 From

首部字段 From 用来告知服务器使用用户代理的用户的电子邮件地址。通常，其使用目的就是为了显示搜索引擎等用户代理的负责人的 电子邮件联系方式。使用代理时，应尽可能包含 From 首部字段（但可能会因代理不同，将电子邮件地址记录在 User-Agent 首部字段内）。

### 6.4.8 Host
> Host: www.hackr.jp

首部字段 Host 会告知服务器，请求的资源所处的互联网主机名和端口号。Host 首部字段在 HTTP/1.1 规范内是唯一一个必须被包含在请求内的首部字段。  

首部字段 Host 和以单台服务器分配多个域名的虚拟主机的工作机制有很密切的关联，这是首部字段 Host 必须存在的意义。   

请求被发送至服务器时，请求中的主机名会用 IP 地址直接替换解决。但如果这时，相同的 IP 地址下部署运行着多个域名，那么服务器就会无法理解究竟是哪个域名对应的请求。因此，就需要使用首部字段 Host 来明确指出请求的主机名。若服务器未设定主机名，那直接发送一个空值即可。

### 6.4.9 If-Match
形如 If-xxx 这种样式的请求首部字段，都可称为条件请求。服务器接收到附带条件的请求后，只有判断指定条件为真时，才会执行请求。

### 6.4.10 If-Modified-Since
> If-Modified-Since: Thu, 15 Apr 2004 00:00:00 GMT  

首部字段 If-Modified-Since，属附带条件之一，它会告知服务器若If- Modified-Since 字段值早于资源的更新时间，则希望能处理该请求。而在指定 If-Modified-Since字段值的日期时间之后，如果请求的资源都没有过更新，则返回状态码 304 Not Modified 的响应。  

If-Modified-Since 用于确认代理或客户端拥有的本地资源的有效性。获取资源的更新日期时间，可通过确认首部字段 Last-Modified 来确定。

### 6.4.11 If-None-Match

首部字段 If-None-Match 属于附带条件之一。它和首部字段 If-Match 作用相反。用于指定 If-None-Match 字段值的实体标记（ETag）值与请求资源的 ETag 不一致时，它就告知服务器处理该请求。  

在 GET 或 HEAD 方法中使用首部字段 If-None-Match 可获取最新的资源。因此，这与使用首部字段 If-Modified-Since 时有些类似。

### 6.4.12 If-Range

首部字段 If-Range 属于附带条件之一。它告知服务器若指定的 If- Range 字段值（ETag 值或者时间）和请求资源的 ETag 值或时间相一致时，则作为范围请求处理。反之，则返回全体资源。

>下面我们思考一下不使用首部字段 If-Range 发送请求的情况。服务器端的资源如果更新，那客户端持有资源中的一部分也会随之无效，当然，范围请求作为前提是无效的。这时，服务器会暂且以状态码 412 Precondition Failed 作为响应返回，其目的是催促客户端再次发送请求。这样一来，与使用首部字段 If-Range 比起来，就需要花费两倍的功夫。

### 6.4.13 If-Unmodified-Since

> If-Unmodified-Since: Thu, 03 Jul 2012 00:00:00 GMT

首部字段 If-Unmodified-Since 和首部字段 If-Modified-Since 的作用相反。它的作用的是告知服务器，指定的请求资源只有在字段值内指定的日期时间之后，未发生更新的情况下，才能处理请求。如果在指定日期时间后发生了更新，则以状态码 412 Precondition Failed 作为响应返回。

### 6.4.14 Max-Forwards

> Max-Forwards: 10

通过 TRACE 方法或 OPTIONS 方法，发送包含首部字段 Max- Forwards 的请求时，该字段以十进制整数形式指定可经过的服务器最大数目。服务器在往下一个服务器转发请求之前，Max-Forwards 的值减 1 后重新赋值。当服务器接收到 Max-Forwards 值为 0 的请求时，则不再进行转发，而是直接返回响应。

使用 HTTP 协议通信时，请求可能会经过代理等多台服务器。途中，如果代理服务器由于某些原因导致请求转发失败，客户端也就等不到服务器返回的响应了。对此，我们无从可知。

可以灵活使用首部字段 Max-Forwards，针对以上问题产生的原因展开调查。由于当 Max-Forwards 字段值为 0 时，服务器就会立即返回响应，由此我们至少可以对以那台服务器为终点的传输路径的通信状况有所把握。

### 6.4.15 Proxy-Authorization
> Proxy-Authorization: Basic dGlwOjkpNLAGfFY5

接收到从代理服务器发来的认证质询时，客户端会发送包含首部字段 Proxy-Authorization 的请求，以告知服务器认证所需要的信息。

这个行为是与客户端和服务器之间的 HTTP 访问认证相类似的，不同之处在于，认证行为发生在客户端与代理之间。客户端与服务器之间的认证，使用首部字段 Authorization 可起到相同作用。

### 6.4.16 Range
> Range: bytes=5001-10000


对于只需获取部分资源的范围请求，包含首部字段 Range 即可告知服务器资源的指定范围。上面的示例表示请求获取从第 5001 字节至第 10000 字节的资源。

接收到附带 Range 首部字段请求的服务器，会在处理请求之后返回状态码为 206 Partial Content 的响应。无法处理该范围请求时，则会返回状态码 200 OK 的响应及全部资源。

### 6.4.17 Referer
> Referer: http://www.hackr.jp/index.htm

首部字段 Referer 会告知服务器请求的原始资源的 URI。

客户端一般都会发送 Referer 首部字段给服务器。但当直接在浏览器的地址栏输入 URI，或出于安全性的考虑时，也可以不发送该首部字段。

因为原始资源的 URI 中的查询字符串可能含有 ID 和密码等保密信息，要是写进 Referer 转发给其他服务器，则有可能导致保密信息的泄露。

### 6.4.18 TE
> TE: gzip, deflate;q=0.5

首部字段 TE 会告知服务器客户端能够处理响应的传输编码方式及相对优先级。它和首部字段 Accept-Encoding 的功能很相像，但是用于传输编码。

### 6.4.19 User-Agent
> User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:13.0) Gecko/20100101 Firefox/13.0.1

首部字段 User-Agent 会将创建请求的浏览器和用户代理名称等信息传达给服务器。

由网络爬虫发起请求时，有可能会在字段内添加爬虫作者的电子邮件地址。此外，如果请求经过代理，那么中间也很可能被添加上代理服务器的名称。

## 6.5 响应首部字段
响应首部字段是由服务器端向客户端返回响应报文中所使用的字段， 用于补充响应的附加信息、服务器信息，以及对客户端的附加要求等 信息。

### 6.5.1 Accept-Ranges
> Accept-Ranges: bytes

首部字段 Accept-Ranges 是用来告知客户端服务器是否能处理范围请 求，以指定获取服务器端某个部分的资源。

可指定的字段值有两种，可处理范围请求时指定其为 bytes，反之则 指定其为 none。

### 6.5.2 Age
> Age: 600

首部字段 Age 能告知客户端，源服务器在多久前创建了响应。字段值的单位为秒。

若创建该响应的服务器是缓存服务器，Age 值是指缓存后的响应再次 发起认证到认证完成的时间值。代理创建响应时必须加上首部字段 Age。 

### 6.5.3 ETag
>ETag: "82e22293907ce725faf67773957acd12"  

首部字段 ETag 能告知客户端实体标识。它是一种可将资源以字符串 形式做唯一性标识的方式。服务器会为每份资源分配对应的 ETag 值。  

另外，当资源更新时，ETag 值也需要更新。生成 ETag 值时，并没有 统一的算法规则，而仅仅是由服务器来分配。

### 6.5.4 Location

> Location: http://www.usagidesign.jp/sample.html

使用首部字段 Location 可以将响应接收方引导至某个与请求 URI 位置不同的资源。  

基本上，该字段会配合 3xx ：Redirection 的响应，提供重定向的 URI。 

几乎所有的浏览器在接收到包含首部字段 Location 的响应后，都会强 制性地尝试对已提示的重定向资源的访问。

### 6.5.5 Proxy-Authenticate
> Proxy-Authenticate: Basic realm="Usagidesign Auth"

首部字段 Proxy-Authenticate 会把由代理服务器所要求的认证信息发送 给客户端。  

它与客户端和服务器之间的 HTTP 访问认证的行为相似，不同之处在于其认证行为是在客户端与代理之间进行的。而客户端与服务器之间进行认证时，首部字段 WWW-Authorization 有着相同的作用。有关 HTTP 访问认证，后面的章节会再进行详尽阐述。

### 6.5.6 Retry-Afte

> Retry-After: 120  

首部字段 Retry-After 告知客户端应该在多久之后再次发送请求。主要配合状态码 503 Service Unavailable 响应，或 3xx Redirect 响应一起使用。  

字段值可以指定为具体的日期时间（Wed, 04 Jul 2012 06：34：24 GMT 等格式），也可以是创建响应后的秒数。

### 6.5.7 Server
> Server: Apache/2.2.17 (Unix)

首部字段 Server 告知客户端当前服务器上安装的 HTTP 服务器应用程序的信息。不单单会标出服务器上的软件应用名称，还有可能包括版本号和安装时启用的可选项。

> Server: Apache/2.2.6 (Unix) PHP/5.2.5

### 6.5.8 Vary

> Vary: Accept-Language

首部字段 Vary 可对缓存进行控制。源服务器会向代理服务器传达关于本地缓存使用方法的命令。  
从代理服务器接收到源服务器返回包含 Vary 指定项的响应之后，若再要进行缓存，仅对请求中含有相同 Vary 指定首部字段的请求返回缓存。即使对相同资源发起请求，但由于 Vary 指定的首部字段不相同，因此必须要从源服务器重新获取资源。

### 6.5.9 WWW-Authenticate

> WWW-Authenticate: Basic realm="Usagidesign Auth"

首部字段 WWW-Authenticate 用于 HTTP 访问认证。它会告知客户端适用于访问请求 URI 所指定资源的认证方案（Basic 或是 Digest）和带参数提示的质询（challenge）。状态码 401 Unauthorized 响应中，肯定带有首部字段 WWW-Authenticate。

## 6.6 实体首部字段

实体首部字段是包含在请求报文和响应报文中的实体部分所使用的首部，用于补充内容的更新时间等与实体相关的信息。

### 6.6.1 Allow
> Allow: GET, HEAD  

首部字段 Allow 用于通知客户端能够支持 Request-URI 指定资源的所有 HTTP 方法。当服务器接收到不支持的 HTTP 方法时，会以状态码 405 Method Not Allowed 作为响应返回。与此同时，还会把所有能支持的 HTTP 方法写入首部字段 Allow 后返回。

### 6.6.2 Content-Encoding
> Content-Encoding: gzip

首部字段 Content-Encoding 会告知客户端服务器对实体的主体部分选用的内容编码方式。内容编码是指在不丢失实体信息的前提下所进行的压缩。

主要采用以下 4 种内容编码的方式。（各方式的说明请参考 6.4.3 节 Accept-Encoding 首部字段）。
> *gzip
> *compress
> *deflate
> *identity

### 6.6.3 Content-Language
> Content-Language: zh-CN

首部字段 Content-Language 会告知客户端，实体主体使用的自然语言 （指中文或英文等语言）。

### 6.6.4 Content-Length

> Content-Length: 15000

首部字段 Content-Length 表明了实体主体部分的大小（单位是字节）。对实体主体进行内容编码传输时，不能再使用 Content-Length 首部字段。由于实体主体大小的计算方法略微复杂，所以在此不再展开。读者若想一探究竟，可参考 RFC2616 的 4.4。

### 6.6.5 Content-Location
> Content-Location: http://www.hackr.jp/index-ja.html

首部字段 Content-Location 给出与报文主体部分相对应的 URI。和首部字段 Location 不同，Content-Location 表示的是报文主体返回资源对应的 URI。

### 6.6.6 Content-MD5

> Content-MD5: OGFkZDUwNGVhNGY3N2MxMDIwZmQ4NTBmY2IyTY==

首部字段 Content-MD5 是一串由 MD5 算法生成的值，其目的在于检 查报文主体在传输过程中是否保持完整，以及确认传输到达。 

对报文主体执行 MD5 算法获得的 128 位二进制数，再通过 Base64 编码后将结果写入 Content-MD5 字段值。由于 HTTP 首部无法记录二进制值，所以要通过 Base64 编码处理。为确保报文的有效性，作为接收方的客户端会对报文主体再执行一次相同的 MD5 算法。计算出的值与字段值作比较后，即可判断出报文主体的准确性。 

采用这种方法，对内容上的偶发性改变是无从查证的，也无法检测出恶意篡改。其中一个原因在于，内容如果能够被篡改，那么同时意味着 Content-MD5 也可重新计算然后被篡改。所以处在接收阶段的客户端是无法意识到报文主体以及首部字段 Content-MD5 是已经被篡改过的。

### 6.6.7 Content-Range

> Content-Range: bytes 5001-10000/10000

针对范围请求，返回响应时使用的首部字段 Content-Range，能告知客户端作为响应返回的实体的哪个部分符合范围请求。字段值以字节为单位，表示当前发送部分及整个实体大小。

### 6.6.8 Content-Type

> Content-Type: text/html; charset=UTF-8

首部字段 Content-Type 说明了实体主体内对象的媒体类型。和首部字段 Accept 一样，字段值用 type/subtype 形式赋值。

### 6.6.9 Expires

> Expires: Wed, 04 Jul 2012 08:26:05 GM

首部字段 Expires 会将资源失效的日期告知客户端。缓存服务器在接收到含有首部字段 Expires 的响应后，会以缓存来应答请求，在 Expires 字段值指定的时间之前，响应的副本会一直被保存。当超过指定的时间后，缓存服务器在请求发送过来时，会转向源服务器请求资源。

源服务器不希望缓存服务器对资源缓存时，最好在 Expires 字段内写入与首部字段 Date 相同的时间值。

但是，当首部字段 Cache-Control 有指定 max-age 指令时，比起首部字段 Expires，会优先处理 max-age 指令。

### 6.6.10 Last-Modified
> Last-Modified: Wed, 23 May 2012 09:59:55 GMT

首部字段 Last-Modified 指明资源最终修改的时间。一般来说，这个值就是 Request-URI 指定资源被修改的时间。但类似使用 CGI 脚本进行动态数据处理时，该值有可能会变成数据最终修改时的时间。

## 6.7 为 Cookie 服务的首部字段

Cookie 的工作机制是用户识别及状态管理。Web 网站为了管理用户的状态会通过 Web 浏览器，把一些数据临时写入用户的计算机内。接着当用户访问该Web网站时，可通过通信方式取回之前发放的 Cookie。

调用 Cookie 时，由于可校验 Cookie 的有效期，以及发送方的域、路径、协议等信息，所以正规发布的 Cookie 内的数据不会因来自其他 Web 站点和攻击者的攻击而泄露。

### 至 2013 年 5 月，Cookie 的规格标准文档有以下 4 种。

由网景公司颁布的规格标准
> 网景通信公司设计并开发了 Cookie，并制定相关的规格标准。1994 年前后，Cookie 正式应用在网景浏览器中。目前最为普及的 Cookie 方式也是以此为基准的。

RFC2109

> 某企业尝试以独立技术对 Cookie 规格进行标准化统筹。原本的意图是想和网景公司制定的标准交互应用，可惜发生了微妙的差异。现在该标准已淡出了人们的视线。

RFC2965
> 为终结 Internet Explorer 浏览器与 Netscape Navigator 的标准差异而导致的浏览器战争，RFC2965 内定义了新的 HTTP 首部 Set-Cookie2 和 Cookie2。可事实上，它们几乎没怎么投入使用。

RFC6265
> 将网景公司制定的标准作为业界事实标准（De facto standard），重新定义 Cookie 标准后的产物。

目前使用最广泛的 Cookie 标准却不是 RFC 中定义的任何一个。而是在网景公司制定的标准上进行扩展后的产物。

### 6.7.1 Set-Cookie

> Set-Cookie: status=enable; expires=Tue, 05 Jul 2011 07:26:31 GMT; path=/; domain=.hackr.jp;


|属性|说明|
|:---:|----:|
|NAME=VALUE|赋予 Cookie 的名称和其值（必需项）|
|expires=DATE|Cookie 的有效期（若不明确指定则默认为浏览器关闭前为止）|
|path=PATH|将服务器上的文件目录作为Cookie的适用对象（若不指定则默认为文档所在的文件目录）|
|domain=域名|作为 Cookie 适用对象的域名（若不指定则默认为创建 Cookie 的服务器的域名）|
|Secure|仅在 HTTPS 安全通信时才会发送 Cookie|
|HttpOnly|加以限制，使 Cookie 不能被 JavaScript 脚本访问|

### 6.7.2 Cookie
> Cookie: status=enable

首部字段 Cookie 会告知服务器，当客户端想获得 HTTP 状态管理支 持时，就会在请求中包含从服务器接收到的 Cookie。接收到多个 Cookie 时，同样可以以多个 Cookie 形式发送。

## 6.8 其他首部字段

HTTP 首部字段是可以自行扩展的。所以在 Web 服务器和浏览器的应用上，会出现各种非标准的首部字段。

### 6.8.1 X-Frame-Options

> X-Frame-Options: DENY

首部字段 X-Frame-Options 属于 HTTP 响应首部，用于控制网站内容在其他 Web 网站的 Frame 标签内的显示问题。其主要目的是为了防止点击劫持（clickjacking）攻击。

### 6.8.2 X-XSS-Protection

> X-XSS-Protection: 1

首部字段 X-XSS-Protection 属于 HTTP 响应首部，它是针对跨站脚本攻击（XSS）的一种对策，用于控制浏览器 XSS 防护机制的开关。

### 6.8.3 DNT
> DNT: 1

首部字段 DNT 属于 HTTP 请求首部，其中 DNT 是 Do Not Track 的简称，意为拒绝个人信息被收集，是表示拒绝被精准广告追踪的一种方法。

首部字段 DNT 可指定的字段值如下。  
* 0 ：同意被追踪
* 1 ：拒绝被追踪

### 6.8.4 P3P
> P3P: CP="CAO DSP LAW CURa ADMa DEVa TAIa PSAa PSDa IVAa IVDa OUR BUS IND UNI COM NAV INT"

首部字段 P3P 属于 HTTP 相应首部，通过利用 P3P（The Platform for Privacy Preferences，在线隐私偏好平台）技术，可以让 Web 网站上 的个人隐私变成一种仅供程序可理解的形式，以达到保护用户隐私的目的。