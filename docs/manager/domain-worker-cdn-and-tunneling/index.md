addEventListener(
   "fetch", event => {

       const ip = event.request.headers.get('cf-connecting-ip') || event.request.headers.get('x-forwarded-for') || (event.request.socket && event.request.socket.remoteAddress);
       let url = new URL(event.request.url);
       const worker_domain=url.hostname;
       url.hostname = "sub.domain.com";                        
       url.protocol = event.request.headers.get('x-forwarded-proto') || "https";
       let request = new Request(url, event.request);
       if (ip)
        request.headers.set('cf-connecting-ip', ip);
        request.headers.set('Host', worker_domain);
       event.respondWith(
           fetch(request)
       )
   }
)
