#### 1.4.2 确保可靠性的TCP协议

- TCP位于传输层，提供可靠的字节流服务，所谓的字节流服务是指，为了方便传输，将大块数据分割成以报文段为单位的数据包进行管理。

#### 2.5 告知服务器意图的HTTP方法

- GET：GET方法用来请求访问已被URI识别的资源。指定的资源经服务器端解析后返回响应内容。
- POST：传输实体主体，POST的主要目的并不是获取响应的主体内容。
- PUT：传输文件。
- HEAD：获取报文首部，HEAD方法和GET方法一样，只是不返回报文主体部分，用于确认URI的有效性及资源更新的日期时间等。
- DELETE：删除文件，和PUT方法相反，DELETE方法按请求URI删除指定的资源。
- OPTIONS：询问支持的方法，OPTIONS方法用来查询针对请求URI指定的资源支持的方法。
- TRACE：追踪路径，TRACE方法是让Web服务器将之前的请求通信环回给客户端的方法。
- CONNECT：要求用隧道协议连接代理，要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信。主要使用SSL和TSL协议把通信内容加密后经网络隧道传输。

#### 2.7.1 持久连接

- HTTP/1.1和一部分的HTTP/1.0想出了持久连接，也称为HTTP keep-alive的方法。持久连接的特点是，只要任意一段没有明确提出断开连接，则保持TCP连接状态。持久连接的好处是减少了TCP连接的重复建立和断开所造成的额外开销，减轻了服务器端的负载，另外减少开销的那部分时间，使HTTP请求和响应能够更早地结束，这样web页面的显示速度也就相应提高了。

#### 2.7.2 管线化

- 持久连接使得多数请求以管线化方式发送成为可能。从前发送请求后需等待并受到响应，才能发送下一个请求。管线化技术出现后，不用等待响应亦可直接发送下一个请求。

#### 3.2 请求报文及响应报文的结构

- 请求报文：报文首部、空行、报文主体。报文首部包括：请求行、请求首部字段、通用首部字段、实体首部字段、其他。请求行包含用于请求的方法，请求URI和HTTP版本。
- 响应报文：报文首部、空行、报文主体。报文首部包括：状态行、响应首部字段、通用首部字段、实体首部字段、其他。状态行包含表明响应结果的状态码，原因短语和HTTP版本。

#### 3.3.3 分割发送的分块传输编码

- 在HTTP通信过程中，请求的编码实体资源尚未全部传输完成之前，浏览器无法显示请求页面。在传输大容量数据时，通过把数据分割成多块，能够让浏览器逐步显示页面。

#### 3.4 发送多种数据的多部分对象集合

- 发送邮件时，我们可以在邮件里写入文字并添加多份附件。这是因为采用了MIME(多用途因特网邮件扩展)机制，它允许邮件处理文本、图片、视频等多个不同类型的数据。

#### 3.5 获取部分内容的范围请求

- 指定范围发送的请求叫做范围请求。主要应用在下载过程中遇到网络中断的情况，能从之前下载中断处回复下载。

#### 4 返回结果的HTTP状态码

- 200：ok
- 204：No Content
- 206：Partial Content
- 301：永久性重定向，该状态码表示请求的资源已被分配了新的URI，以后应使用资源现在所指的URI。
- 302：临时性重定向，该状态码表示请求的资源已被分配了新的URI，希望用户(本次)能使用新的URI访问。
- 303：该状态码表示由于请求对应的资源存在着另一个URI，应使用GET方法定向获取请求的资源。
- 304：该状态码表示客户端发送附近条件的请求时，服务器端允许请求访问资源，但未满足条件的情况。
- 307：临时重定向。
- 400：该状态码表示请求报文中存在语法错误。
- 401：该状态码表示发送的请求需要用通过HTTP认证的认证信息。
- 403：表示对请求资源的访问被服务器拒绝了。
- 404：表明服务器上无法找到请求的资源。除此之外，也可以在服务器端拒绝请求且不想说明理由时使用。
- 500：表明服务器端在执行请求时发生了错误。
- 503：表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。