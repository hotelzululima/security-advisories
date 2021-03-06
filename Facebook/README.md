
# Vulnerabilities in Facebook and Facebook Messenger for Android applications


## 1. Advisory Information

**Title:** Vulnerabilities in Facebook and Facebook Messenger for Android applications

**Advisory ID:** STIC-2014-0529

**Advisory URL:** [http://www.fundacionsadosky.org.ar/publicaciones/#seguridad-en-tic](http://www.fundacionsadosky.org.ar/publicaciones/#seguridad-en-tic)

**Date published:** 2014-07-28

**Date of last update:** 2014-07-28

**Vendors contacted:** Facebook Inc. (NASDAQ:FB)

**Release mode:** Coordinated release



## 2. Vulnerability Information

**Class:** Information Exposure Through Sent Data [[http://cwe.mitre.org/data/definitions/201.html](http://cwe.mitre.org/data/definitions/201.html)], Information Exposure Through Sent Data [[http://cwe.mitre.org/data/definitions/201.html](http://cwe.mitre.org/data/definitions/201.html)], Unintended Proxy or Intermediary [[http://cwe.mitre.org/data/definitions/441.html](http://cwe.mitre.org/data/definitions/441.html)]

**Impact:** Denial of service, Data loss

**Remotely Exploitable:** Yes

**Locally Exploitable:** Yes

**CVE Identifier:** [http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNY](http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNY), [http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNX](http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNX), [http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNZ](http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNZ)



## 3. Open proxy in Facebook application for Android
[[http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNZ](http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNZ)]
According to Facebook's published financial results for the second quarter of 2014, as of June 30th the company had 1.07 billion mobile active users and an average of 654 million mobile daily active users[1]. The Facebook application for Android is among the top 10 most installed Android applications worldwide with 500 to 1,000 million installations as of June 24th, 2014[3].

The application embeds a generic HTTP server component that is used as a caching proxy for playing video recordings. This server is misconfigured and accepts requests from any client, local or remote, allowing attackers to connect to it and use a victim's device as an open proxy. As a results, among other things, an attacker could carry out various forms of denial of service attacks such as filling up the device's storage or running up the subscriber's data transfer limit over 3G or LTE networks. 


## 4. Disclosure of private video content in Facebook application for Android
[[http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNX](http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNX)] 
The application allows users to upload video to Facebook and configure who should be able to play it back (publicly accessible, friends only, oneself, custom list). The application also allows users to playback video on the Android device. Viewing video content marked by the user as private is prevented by Facebook in accordance to the company's privacy policy [2] if the connecting client is a web browser. However, if the user connects to Facebook using the Android application the confidentiality of private video and audio content is not enforced.

 The application retrieves video content for playback in an insecure manner, allowing anyone with access to the same network where the Android device is connected or to any network in the path between the device and Facebook's Content Delivery Network to capture or retrieve video content disregarding the user's configured access policy and bypassing Facebook's privacy policy.


## 5. Disclosure of audio recordings in chat messages in Facebook and Facebook Messenger for Android
[[http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNY](http://cve.mitre.org/cgi-bin/cvename.cgi?name=2014-NNNNY)]
The Facebook Messenger application is also among the top 10 most installed Android applications worldwide with 500 to 1000 million installs [4] . Both Facebook and Facebook Messenger applications allow users to send and playback audio recordings as messages within a chat session. Transmission of the audio content is done using an insecure network protocol, allowing anyone with access to the same network where the Android device is connected or to any network in the path between the device and Facebook's Content Delivery Network to capture or retrieve chat audio recordings bypassing Facebook's privacy policy.  


## 6. Video Cache Server vulnerability: Vulnerable packages

* Facebook Android application older than version 13.0.0.13.14

## 7. Video vulnerability: Vulnerable packages

* Facebook Android application older than version 10.0.0.28.27 up until June 11th, 2014.

## 8. Audio vulnerability: Vulnerable packages

* Facebook Android application older than version 10.0.0.28.27
* Facebook Messenger Android application older than version 5.0.0.25.1

## 9. Vendor Information, Solutions and Workarounds
 
Facebook acknowledged and corrected all three vulnerabilities. According to the company, the audio recording issue was already known and a fix was being beta tested at the time the bug was originally reported. The company released new application updates that fix both audio and video vulnerabilities.
The fix to the disclosure of audio recordings required a new application update. The fix to the video disclosure vulnerability works with current and prior versions of the application that support retrieval of video from the CDN using HTTPS.
 
Facebook's new update to version 13.0.0.13.14 fixed the open proxy issue by configuring the video cache server to listen only to local requests.

To determine which version of the applications you have installed on your Android device, go to "Settings|application settings|manage application" then tap on the Facebook or Facebook Messenger app.


## 10. Credits

This vulnerability was discovered and researched by Joaquín Manuel Rinaudo. 
The publication of this advisory was coordinated by Programa Seguridad en TIC.


## 11. Technical Description

Facebook uses an HTTP server as caching proxy for media content. The server is hosted in the mobile application's process space and listens on a local non-fixed ephemeral TCP port. The constructor of the class _com.facebook.video.server.VideoServerBase_ embedded in the Facebook application instantiates this GenericHttpServer object. The created instance listens to requests from any client, local or remote, enabling an attacker to perform requests to third party servers through it.
 
The server accepts three types of GET requests: _/proxy_, _/cache-window_ and _/cache-thru_. The parameters for these requests are _'remote-uri'_ whose value is an URL and a _'vid'_ identifier. Upon receiving a request, the server performs a HEAD request to the _'remote-uri' _ URL to obtain the _content-length_ of the resource, it then obtains the requested resource with a series of GET requests until the previously declared content-length is reached. Any redirect response to the HEAD request is followed by a GET request to the redirected location.
 
While the 'proxy' request will simply forward the content to the server's client, the 'cache-thru' and 'cache-window' requests indicate the server to not only forward the content to the client but also to store a copy on the phone internal memory under _data/data/com.facebook.katana/files/video-cache_. 

An attacker could use a victim's mobile with the Facebook app installed as an open proxy by querying the embedded HTTP server for _/proxy_ and passing as a parameter a shortened URL that points to any arbitrarily selected target site. Since all redirects are followed, an attacker could use a shortened URL, obtained from a site like _goo.gl_, as the target site parameter so the proxy works on all sites. She can also cause the phone to run out of internal storage by simply querying _/cache-thru_ with a _'remote-uri'_ set to a site containing a large file. The same can be done for running up the subscriber's data transfer limit over 3G, LTE networks.  

To reproduce the vulnerability follow these steps:

1) Connect with adb shell to a device running Facebook and run netstat to find out the listening port

2) From a device in the same network run _telnet [Phone IP] [listening port]_ and enter the following request:

_GET /cache-thru?remote-uri=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3Dz9Uz1icjwrM&vid=a HTTP/1.1_
3) The phone queries the link with a HEAD request which youtube servers will respond with a 302 redirect to _m.youtube.com_. The victim then queries m.youtube.com and downloads the video content to the phone's internal memory cache and forwards it to the client that requested it over the telnet connection.
  
Videos hosted on the Facebook CDN network are obtained via HTTP. When a user requests playback of a video hosted on Facebook an instance of the VideoServerBase class performs a request to an instance of the GenericHTTPServer class with a parameter of /caching-thru with its value set to the URI of the video to retrieve from the CDN. Since the URI scheme is HTTP, the caching proxy requests to download the content are performed over an insecure transport.    

Anyone with access to the local network of the Android device or to any network in the path between the device and Facebook's CDN can obtain the URL and video content by capturing network packets or can retrieve the video content directly from Facebook's CDN once the URL is known.

Steps to reproduce the vulnerability:

1) Download and install Facebook application.

2) Login to Facebook using any account (we will call it "account A").

3) Using a web browser login to Facebook using a separate account ("account B"), post a video and allow access to it just to accounts A and B.   

4) Using the Facebook application for Android logged in using account A let the video status load but do not yet play the video.

5) Set a proxy for the Android phone. This will make all HTTPS requests stop working but they are not needed to reproduce the vulnerability.

6) Click on the video and let it play.

7) Copy the URL in the GET request obtained from the proxy (this emulates an attacker sniffing the network) and paste it in a web browser to watch the video without any authentication.

The third vulnerability involves audio recordings sent from one Facebook user to another user through chat on both Facebook and Facebook Messenger applications. The sender's application uploads the audio recording using an HTTPS POST request to _graph.facebook.com_ and then a HTPPS GET request to _api.facebook.com/method/messaging.getAttachment_ that is responded with a redirect to the actual content at _attachment.fbsbx.com_. Although the initial POST and GET requests are sent over HTTPS and authenticated using the user's OAuth access token, the redirect response to retrieve the audio content is obtained over HTTP. Likewise, the receiver's application downloads the audio recording using an HTTPS GET request to _api.facebook.com/method/getAttachment_ that is responded with a redirect with a URL to the actual audio content on _attachment.fbsbx.com_ over HTTP. The uid parameter in both requests indicate the Facebook IDs of sender and receiver, respectively.

This vulnerability was found in the _com.facebook.ui.media.fetch.MediaRedirectHandler_ class in method _getLocationURI_ in packages previous to version 10.0.28.27 . This method calls a private method that translates the URI scheme from HTTPS to HTTP for any request redirected to domain _attachment.fbsbx.com_.

As a result of the above, an attacker access to the same network where the Android device is connected or to any network in the path between the device and the _attachment.fbsbx.com_ network can capture or retrieve chat audio recordings bypassing Facebook's privacy policy.   

Steps to reproduce the disclosure of audio recordings vulnerability

1) Login to Facebook using the Facebook application for Android.

2) Capture network packets using any network sniffing tool (e.g. wireshark).

3) Within the Facebook app open a chat window and send a recording.

4) Find the GET request to _attachment.fbsbx.com_ in the captured traffic. Use any web browser to open the specified URL to obtain the recording.


## 12. Report Timeline

* **2014-05-13:** 
          The researcher sent a technical description of the vulnerabilities to the vendor.
        
* **2014-05-13:** 
          The vendor acknowledged the audio recording vulnerability and said it was fixed a month ago,that the fixed app is only available to beta users and that an app update will be released in the near future.
        
* **2014-05-19:** 
          The vendor acknowledged the video vulnerability and requested information about the device used for the tests.
        
* **2014-05-19:** 
          The researcher sent the requested information to the vendor.
        
* **2014-05-29:** 
          The researcher requested an status update and informed the vendor that the Programa Seguridad en TIC plan to release a security advisory to notify affected users about the issues and provide guidance to apply fixes. He asked the vendor to continue the communication over email rather than using Facebook's vulnerability reporting system since the latter requires researcher and coordinator to have a Facebook account.
        
* **2014-06-04:** 
          Facebook communicated with Programa Seguridad en TIC  via email. Programa Seguridad en TIC set June 18th as publication date for the security advisory and indicated that there is evidence of very similar vulnerabilities being actively exploited by third parties to collect media content deemed private by Facebook users.
        
* **2014-06-05:** 
          Facebook asked for evidence of active exploitation and assured that a fix for the recording vulnerability was already being rolled out before the report was sent.
        
* **2014-06-06:** Programa Seguridad en TIC send a reference [5] to NSA presentation slides 82-87 leaked by Glenn Greenwald about network traffic capture activities to obtain Facebook images hosted in Akamai servers.
        
* **2014-06-12:** 
          A new update for the Facebook application was released. The researcher analyzed the new application and found that vulnerabilities 2 and 3 were fixed.
        
* **2014-06-14:** 
          Facebook informed the researcher that the video and audio vulnerabilities had been patched and asked for the researcher's confirmation.
        
* **2014-06-16:** 
          The researcher acknowledged that both vulnerabilities were fixed. Programa Seguridad en TIC Asked about the open proxy pending issue. 
        
* **2014-06-16:** 
          Facebook requests proof-of-concept code or steps to reproduce the open proxy issue. 
        
* **2014-06-16:** 
          Steps to reproduce sent to Facebook by Joaquín Manuel Rinaudo. 
        
* **2014-06-16:** 
           Draft of the advisory sent to The MITRE Corporation requesting assignment of CVE identifiers for the audio and video vulnerabilities. 
        
* **2014-06-20:** 
           Response from Mitre saying none of the two vulnerabilities meet the CVE inclusion criteria and therefore CVE identifiers will not be assigned.           
        
* **2014-06-22:** Programa Seguridad en TIC asks for an update about the open proxy issue. 
        
* **2014-06-23:** 
          Reply from Facebook security saying its working with the team and will be in touch when there is information to share.
        
* **2014-06-24:** 
           Email to Mitre requesting a CVE identifier to be assigned for the third bug (open proxy) and providing additional details and opinion about how the other two meet the CVE inclusion criteria.
        
* **2014-07-01:** 
          Facebook sent a new confirmation of working on the fix for open proxy issue.
        
* **2014-07-02:** Programa Seguridad en TIC expressed to Facebook its concern about elapsed time since the original report and asked why a simple fix is taking so long.
        
* **2014-07-02:** 
          Facebook replied that it is actively working on addressing the issue.
        
* **2014-07-03:** 
           Email to Mitre asking if they have a decision on use of CVE identifiers in light of the additional details about the vulnerabilities provided in the previous email.
        
* **2014-07-08:** 
          The researcher reminded Facebook that the advisory was originally scheduled for publication on June 18 and that no estimated date for the fix to open proxy issue was provided, so a new publication date was set for July 16, 2014 and should be considered final. Programa Seguridad en TIC remains willing to move the date on basis of receiving concrete and detailed information about plans to fix the open proxy issue.
        
* **2014-07-08:** 
           Email to Mitre asking again for a response to the request for CVE identifiers.
        
* **2014-07-09:** 
          Facebook asked to hold off the publication a week further, assuring that the fix would be up by then. 
        
* **2014-07-11:** Programa Seguridad en TIC moved the release date to July 23, 2014 as the final date. 
        
* **2014-07-11:** 
         Facebook informed that the patch was already released in the beta version of Android. 
        
* **2014-07-11:** 
          The researcher acknowledged that the open proxy vulnerability is fixed in the beta version.
        
* **2014-07-16:** 
           Email to Mitre indicating no response was received to the prior email asking for a decision regarding the assignment of CVE identifiers and asking for a response within 48 hours  since all the bugs have been fixed and the update is available to vulnerable users. Publication of the security advisory is imminent. 
        
* **2014-07-17:** 
           Email from Mitre stating that CVE assignment group does not have a final conclusion about whether the reported vulnerabilities fit the inclusion criteria and recommending public disclosure without CVE IDs. The original bug descriptions and additional explanations were reviewed by the staff several times but a final conclusion was not reached. CVE assignment group feels that the "research observations occupy a boundary between site-specific and customer-controlled software that can be the subject of extensive debate".
        
* **2014-07-21:** 
          Facebook informed that an application update was available to 5 % of the users.
        
* **2014-07-21:** 
          The vendor informed that the update was available to 50% of the users.
        
* **2014-07-21:** 
          The vendor informed that the update was available to 100 % of the users.
        
* **2014-07-22:** Programa Seguridad en TIC asked if the figures provided by Facebook correspond to availability or actual installations of the update.
        
* **2014-07-22:** 
          The vendor replied that percentage figures correspond to availability not actual installations.
        
* **2014-07-28:** 
          The advisory was released.
        

## 13. References

[1] [http://investor.fb.com/releasedetail.cfm?ReleaseID=861599](http://investor.fb.com/releasedetail.cfm?ReleaseID=861599)

[2] [https://www.facebook.com/note.php?note_id=%20322194465300](https://www.facebook.com/note.php?note_id=%20322194465300)

[3] [https://play.google.com/store/apps/details?id=com.facebook.orca](https://play.google.com/store/apps/details?id=com.facebook.orca)

[4] [https://play.google.com/store/apps/details?id=com.facebook.katana](https://play.google.com/store/apps/details?id=com.facebook.katana)

[5] [http://hbpub.vo.llnwd.net/o16/video/olmk/holt/greenwald/NoPlaceToHide-Documents-Compressed.pdf](http://hbpub.vo.llnwd.net/o16/video/olmk/holt/greenwald/NoPlaceToHide-Documents-Compressed.pdf)

## 14. About Fundación Dr. Manuel Sadosky

The Dr. Manuel Sadosky Foundation is a mixed (public / private) institution whose goal is to promote stronger and closer interaction between industry and the scientific-technological system in all aspects related to Information and Communications Technology (ICT). The Foundation was formally created by a Presidential Decree in 2009. Its Chairman is the Minister of Science, Technology, and Productive Innovation of Argentina; and the Vice-chairmen are the chairmen of the country’s most important ICT chambers: The Software and Computer Services Chamber (CESSI) and the Argentine Computing and Telecommunications Chamber (CICOMRA).
For more information visit: https://www.fundacionsadosky.org.ar


## 15. Copyright Notice

The contents of this advisory are copyright  (c) 2014 Fundación Sadosky and are licensed under a Creative Commons Attribution Non-Commercial Share-Alike 4.0 License:
[http://creativecommons.org/licenses/by-nc-sa/4.0/](http://creativecommons.org/licenses/by-nc-sa/4.0/)
