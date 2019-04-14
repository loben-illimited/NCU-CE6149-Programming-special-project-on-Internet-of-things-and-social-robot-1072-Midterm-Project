# NCU-CE6149-Programming-special-project-on-Internet-of-things-and-social-robot-1072-Midterm-Project

## 你是誰？

我們是第五組 `智慧門鎖有限公司` 請多指教。

而我們的官網是 [IOT-Lab](https://iot-lab.surge.sh) 可以有空去看看喔！

而我們的口號是...

> 令生活變得更美好

## 有甚麼成員？

這個基於保密不便透露（笑

-----

## 作品 --- 智慧門鎖

### 創作靈感

我們作這個東西是因為：我們覺得現在得門鎖得功能不太方便，例如：只有很少的開鎖方法，門鎖管理員不能知道誰有去開門。
所以我們想改良一下那個鎖，令它有更多的功能，令我們的生活更方便。

現在加的新功能：
* 天氣節取
* 管理系統

### 怎樣去看看我們的Project的成果

去我們的官網 [IOT-Lab](https://iot-lab.surge.sh) 再點選期中報告的鏈結即可~

### 您們`Arduino`的code是怎樣寫的？

> 這是我們include的東東
>
> ![img](https://i.imgur.com/9QiSVhp.png)

> 這是我們用來儲存東東的`global variable`
>
> ![img](https://i.imgur.com/O7IzY9T.png)

> 定義一些裏面會用到的東西
>
> ![img](https://i.imgur.com/CLs7Kq0.png)

> 拆分的`function`
>
> ![img](https://i.imgur.com/GWW9GT7.png)

### 那麼您們的網頁的功能是怎樣實現的？

#### `HTML` 篇

> 其實相當簡單
>
> 只需要對`HTML`有基本認真就可以看得懂
>
> ![img](https://i.imgur.com/a5TbK2u.png)

#### `Javascript` 篇

```javascript
var last_time_uid_time_record = ""; //儲存最後一次讀入UID的時間

//出處: https://coderwall.com/p/flonoa/simple-string-format-in-javascript
String.prototype.format = function() {
    a = this;
    for (k in arguments) {
    a = a.replace("{" + k + "}", arguments[k])
    }
    return a
}

setInterval(function(){ getTempHumr();}, 1000); //每1秒執行一次getTempHumr這個function

function getTempHumr(){
    $.ajax({
        type: "GET",
        url: "https://api.mediatek.com/mcs/v2/devices/D06JpJ2r/",
        headers: {  },
        contentType: "application/json"
    })
    .done(function(data){
        var dataChannels = data["results"]["0"]["dataChannels"];
        var card_uid_recordAt = dataChannels["0"]["dataPoint"]["recordedAt"]; //取得現時MCS提供的card_uid 寫入時間(UNIX time)
        var card_uid = dataChannels["0"]["dataPoint"]["values"]["value"]; //取得最後讀入卡的UID
        var temp = dataChannels["1"]["dataPoint"]["values"]["value"]; //取得溫度
        var humr =  dataChannels["2"]["dataPoint"]["values"]["value"]; //取得濕度
        var t_str = "The current temperature is "
        document.getElementById("temp_str").innerHTML = t_str + temp + "°C"; //輸出溫度
        var h_str = "The current humidity is ";
        document.getElementById("humr_str").innerHTML = h_str + humr + "%"; //輸出濕度
        
        var myDate = new Date(card_uid_recordAt *1000);
        var real_time = /*myDate.toGMTString()+"<br>"+*/myDate.toLocaleString();
        
        if(last_time_uid_time_record != card_uid_recordAt){ //觀察是否有卡片讀入
            last_time_uid_time_record = card_uid_recordAt;
            var table_data = "<tr> <td>{0}</td> <td>{1}</td> <td>{2}</td> <td>{3}</td> </tr>".format(table_index, card_uid, card_uid_recordAt, real_time); //這個看得明HTML也一定看得懂這個
            $('#uidData tr:last').after(table_data); //加插<tr>到table的最後
            table_index += 1; //有寫個for loop也知道這個是甚麼吧！
        }
    })
}
```

