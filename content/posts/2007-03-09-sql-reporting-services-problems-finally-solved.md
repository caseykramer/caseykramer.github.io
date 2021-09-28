---
title: SQL Reporting Services Problems Finally Solved!
author: drrandom

date: 2007-03-09T16:04:35+00:00
url: /2007/03/09/sql-reporting-services-problems-finally-solved/
dsq_thread_id:
  - 4000086748
tags:
  - Uncategorized

---
A little background:  I've been working with a SQL 2000 Reporting Server for about 4 months now, trying to do some integration into an intranet app, and some reporting conversion.  For the last 3 months or so, the web services API, and the ability to publish from within Visual Studio have been gone.  Needless to say this has put a serious damper on my ability to integrate RS into the client's intranet.  I did some looking out on the web, and the newsgroups, and could never find anything that quite worked.  The real interesting thing was that if IIS was refreshed, I could use the Web Service API once, and only once.  If I attempted to connect again, I would loose my connection, and not be able to connect again until IIS was bounced.  What was really strange was that it looked like some data was being sent, but eventually it would just give up.  Also strange was that this did not seem to effect the Report Manager app that MS uses as the front-end for the report server.

Well, as you may guess, I wouldn't be writing this if I didn't have a solution (at lest not with &#8220;Solved!&#8221; in the title), so here it is:  HTTP Keep-Alives.  I disabled HTTP Keep-Alives on the &#8220;Default Web Site&#8221; and life was groovy again.  I still need to check publishing from Visual Studio, but I can connect to the web service API, which is a good first start.

Its the little things in life that bring such joy.....

**UPDATE:  
** So after moving right along for a little while with my HTTP Keep-Alive disabled, I attempted to go to the ReportManager page to change the data source of a report.  When I arrived, I was greeted by a 401 error.  WTF?  So for some unknown reason the Keep-Alive is required by the Report Manager.  Since you can't enable or disable the Keep-Alive at the Virutal Directory level, I was SOL. After trying some stupid things that did not work (set the Connection: Keep-Alive header on the ReportManager virtual dir), I finally found [this little gem on Google ](1). It seems that there is a bug in ASP.Net that will cause it to suddenly drop the connection, even though there is still data.  The fix is to grab your HttpWebRequest object, and set the Keep-Alive to false, and the ProtocolVersion to HttpVersion10.  After doing this, life seems to be okay again.

 [1]: http://p2p.wrox.com/topic.asp?TOPIC_ID=4858 "ASP.Net Bug"