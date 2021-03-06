Rajiko
====================

Permission Details:
-------------------
1. activeTab : to get tab's url to decide whether auto refresh or alert or which radio to record.
2. cookies : for force setting radiko.jp's current location.
3. storage : for storing your location configuration.
4. webRequest : modify request to pass the authentication.
5. webRequestBlocking : modify request to pass the authentication.
6. \*://\*.radiko.jp/\* : the only site we aimed at.
7. declartiveContent : [TODO] for showing icon only on radiko pages.But firefox does not support this api.When firefox supports this api,tab permission will not be required.
8. downloads : for downloading recored audio.
9. \*://\*.smartstream.ne.jp/\* : the site where audio stored.
10. unlimitedStorage : for recording radio.
11. \*://\*.d2-apps.net/\*: disable radiko's statistics requests. [TODO]



What's new:
-----------
+ version 0.2.0
    
    Now, you can use areafree(エリアフリー) and timeshift(タイムシフト) as premium(プレミアム会員)  freely without any operation.

    For using other area in timeshift(タイムシフト) page, please select area in areafree(エリアフリー) page first then switch back to timeshift(タイムシフト) page !

    The "3 hours a day" limitation of timeshift(タイムシフト) has been unblocked.You can listen no matter how long now. 

    "Choose Area" is only needed in displaying area in live(ライブ)

    If there's any bug or problem ,please try to disable and then enable it.If this does not help , please tell me via review page or github issue.


+ version 0.1.4.1
    
    bug fix: fix cookie error caused by different storage.local key name.

    continously improve mobile ui

    improve extension icon ui when recording


+ version 0.1.4

    change to responsive ui in firefox android !

    fix gps info mistake

    adjust to correct japan timezone via moment-timezone

+ version 0.1.3

    experimentally support recording radio. [Caution: this would cause slowing down popup page and increasing cpu usage if recording too long. No more than 30 minutes is recommended.]

+ version 0.1.2
    
    fix bug in firefox

+ version 0.1.1

    support firefox for android

+ version 0.1
    
    initial version
    
Suppport List:
------------------
+ firefox latest

+ chrome latest

+ chromium latest

+ yandex browser latest

+ firefox for android latest



Technical Details:
------------------
1. How it works?

    The authentication of pc(html5) version radkio validates user's location via ip address.
    
    However the android version of radkio validates user via geolocation provided by GPS(if possible),not via user's ip.
    
    So why don't we use the authentication method of android version in pc to bypass ip check?

    The authentication includes two step:
    1.  auth1

        request : platform_info , user_id

        response : a token to be valid, full_key_offset ,partial_key_length

    2.  auth2

        request: token ,platform_info ,user_id, a parital key generated by full key and offset ,  connection type (in android), gps location(in android)

        response: Your location (and your token is valid for only this location) / OUT
    
    In the pc version,the full key is simplely placed in the javascript code in `apps/js/playerCommon.js` :

    ```javascript
    player = new RadikoJSPlayer($audio[0], 'pc_html5', 'bcd151073c03b352e1ef2fd66c32209da9ca0afa' /*full key*/ ...
    ```
    However the android version's full key is protected by native dynamic librarys.Obviously the key is much longer than that in pc version.

2. But how do you generate the partialkey/how do you get fullkey?
    
    TODO

TODO
------------
1. Fake request headers more similarly (such as remove cookies and set accept,user agent,and etc) to avoid detection (partially done)
    due to the limitation of extension , cannot captialize some header's key 
2. Automatic switch location , no need for manually choice. (consider not supporting)
3. Add recording function? (find solution on firefox -> webRequest.filterResponseData() and localstorge/chrome.storage ->  downloads.download  URL.createObjectURL(BlobObject), chrome may use xhr to save data , double trafic?) 
    the right way to download data uri 
    https://stackoverflow.com/questions/40269862/save-data-uri-as-file-using-downloads-download-api/40279050

    how to merge? (src site use hls.js to play m3u8 and aac) 
        seems that directly concat is enough

4. Force Firefox android load web page,not app download page.(done)
5. consider generate different extension in different browser 

    https://stackoverflow.com/questions/45911251/what-is-the-best-way-to-create-a-cross-browser-gmail-extension
    https://www.smashingmagazine.com/2017/04/browser-extension-edge-chrome-firefox-opera-brave-vivaldi/
6. modify firefox android page to responsive page. (partially done)
7. break the time limitation of timeshift and be able to download timeshift content