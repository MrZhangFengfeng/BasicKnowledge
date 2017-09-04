    var a = document.createElement("a");
    
    a.href = 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&tn=baidu&wd=winter&oq=winter&rsv_pq=c0de202800044840&
        rsv_t=ecd0%2Fi1L07R9j7na4HCE6vRakBxNFEiibjK49q%2FmFHHXvQUCztErh79H%2FT0&rqlang=cn&rsv_enter=0&rsv_sug3=1&
        rsv_sug1=1&rsv_sug7=100&rsv_sug4=682&rsv_sug=1';
        
    console.log("protocol:","-->",a.protocol.replace(":", ""))
    console.log("a.hostname","-->",a.hostname)
    console.log("a.port","-->",a.port)
    console.log("a.search","-->",a.search)
    console.log("a.pathname","-->",a.pathname)
    console.log("a.hash","-->",a.hash)
    
- - -
    "protocol:" "-->" "https"
    "a.hostname" "-->" "www.baidu.com"
    "a.port" "-->" ""
    "a.search" "-->" "?ie=utf-8&f=8&rsv_bp=1&tn=baidu&wd=winter&oq=winter&rsv_pq=c0de202800044840&
        rsv_t=ecd0%2Fi1L07R9j7na4HCE6vRakBxNFEiibjK49q%2FmFHHXvQUCztErh79H%2FT0&rqlang=cn&rsv_enter=0&
        rsv_sug3=1&rsv_sug1=1&rsv_sug7=100&rsv_sug4=682&rsv_sug=1"
    "a.pathname" "-->" "/s"
    "a.hash" "-->" ""
