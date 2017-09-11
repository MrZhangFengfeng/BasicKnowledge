## jQuery的不熟带来的坑

- - -

    <p class='winter'>
    <p class='winter'>

    $('.winter').addClass()     //OK
    $('.winter')[0].addClass()     //error
    $($('.winter')[0]).addClass()   //OK    需要再包一层$()
    
- - -
    $('.winter').forEach(...)  //error
    $('.winter').each(...)   //jquery里是each不是foreach
