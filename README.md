#第一步，绑定你的域名到cloudflare。

#第二步，创建一个worker，并写入如下代码：

export default {
  async fetch(request, env) {
    const _url = new URL(request.url);
    const hostname = _url.hostname
    _url.hostname = "github.com"
    const req = new Request(_url, request);
    req.headers.set('origin', 'https://github.com');
    
    const res = await fetch(req);
    let newres = new Response(res.body, res);

    let location = newres.headers.get('location');
    if (location !== null && location !== "") {
      location = location.replace('://github.com', '://'+hostname);
      newres.headers.set('location', location);
    }
    return newres 
  },
};
#第三步，自定义域名，方便访问。
