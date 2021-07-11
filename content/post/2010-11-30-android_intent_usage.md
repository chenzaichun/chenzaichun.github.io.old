+++
categories = ["programming"]
comments = true
date = "2010-11-30T12:32:33+08:00"
excerpt = "Intent应该算是Android中特有的东西。你可以在Intent中指定程序要执行的动作（比如：view,edit,dial），以及程序执行到该动作时所需要的资料。都指定好后，只要调用startActivity()，Android系统会自动寻找最符合你指定要求的应用程序，并执行该程序。\n<br />下面列出几种Intent的用法\n<br />显示网页:"
published = true
status = "publish"
tags = ["android", "Google"]
title = "Android Intent的几种用法全面总结"
type = "post"
description = ""
+++

@<a href="http://hi.baidu.com/devil%B6%C0%B0%AE/blog/item/5f5caf5e3ce43f48faf2c004.html" target="_blank">Devil独爱的空间 </a>

<span class="t_tag" style="line-height: normal;">Intent</span>应该算是Android中特有的东西。你可以在Intent中指定<span class="t_tag" style="line-height: normal;">程序</span>要执行的动作（比如：view,edit,dial），以及程序执行到该动作时所需要的<span class="t_tag" style="line-height: normal;">资料</span>。都指定好后，只要调用startActivity()，Android<span class="t_tag" style="line-height: normal;">系统</span>会自动寻找最符合你指定要求的<span class="t_tag" style="line-height: normal;">应用</span>程序，并执行该程序。<br style="line-height: normal;"><br style="line-height: normal;">下面列出几种Intent的用法<br style="line-height: normal;">显示网页:
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("http://www.google.com");</li>
<li style="line-height: normal;">Intent it   = new Intent(Intent.ACTION_VIEW,uri);</li>
<li style="line-height: normal;">startActivity(it);</li>
</ol></div>
</div>
显示地图:
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("geo:38.899533,-77.036476");</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.Action_VIEW,uri);</li>
<li style="line-height: normal;">startActivity(it);</li>
</ol></div>
</div>
路径规划:
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("http://maps.google.com/maps?f=d&saddr=startLat%20startLng&daddr=endLat%20endLng&hl=en");</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_VIEW,URI);</li>
<li style="line-height: normal;">startActivity(it);</li>
</ol></div>
</div>
拨打电话:<br style="line-height: normal;">调用拨号程序
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("tel:xxxxxx");</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_DIAL, uri);  </li>
<li style="line-height: normal;">startActivity(it);  </li>
</ol></div>
</div>
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("tel.xxxxxx");</li>
<li style="line-height: normal;">Intent it =new Intent(Intent.ACTION_CALL,uri);</li>
<li style="line-height: normal;">要使用这个必须在配置<span class="t_tag" style="line-height: normal;">文件</span>中加入<uses-permission id="<span class="t_tag" style="line-height: normal;">android</span>.permission.CALL_PHONE" /></li>
</ol></div>
</div>
发送SMS/MMS<br style="line-height: normal;">调用发送<span class="t_tag" style="line-height: normal;">短信</span>的程序
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_VIEW);</li>
<li style="line-height: normal;">it.putExtra("sms_body", "The SMS text");</li>
<li style="line-height: normal;">it.setType("vnd.android-dir/mms-sms");</li>
<li style="line-height: normal;">startActivity(it);  </li>
</ol></div>
</div>
发送短信
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("smsto:0800000123");</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_SENDTO, uri);</li>
<li style="line-height: normal;">it.putExtra("sms_body", "The SMS text");</li>
<li style="line-height: normal;">startActivity(it);  </li>
</ol></div>
</div>
发送彩信
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("content://media/external/images/media/23");</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_SEND);</li>
<li style="line-height: normal;">it.putExtra("sms_body", "some text");</li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_STREAM, uri);</li>
<li style="line-height: normal;">it.setType("image/png");</li>
<li style="line-height: normal;">startActivity(it);</li>
</ol></div>
</div>
发送Email
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.parse("mailto:xxx@abc.com");</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_SENDTO, uri);</li>
<li style="line-height: normal;">startActivity(it);</li>
</ol></div>
</div>
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_SEND);</li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_EMAIL, "me@abc.com");</li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_TEXT, "The email body text");</li>
<li style="line-height: normal;">it.setType("text/plain");</li>
<li style="line-height: normal;">startActivity(Intent.createChooser(it, "Choose Email Client"));  </li>
</ol></div>
</div>
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Intent it=new Intent(Intent.ACTION_SEND);    </li>
<li style="line-height: normal;">String[] tos={"me@abc.com"};    </li>
<li style="line-height: normal;">String[] ccs={"you@abc.com"};    </li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_EMAIL, tos);    </li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_CC, ccs);    </li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_TEXT, "The email body text");    </li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_SUBJECT, "The email subject text");    </li>
<li style="line-height: normal;">it.setType("message/rfc822");    </li>
<li style="line-height: normal;">startActivity(Intent.createChooser(it, "Choose Email Client"));</li>
</ol></div>
</div>
添加附件
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_SEND);</li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_SUBJECT, "The email subject text");</li>
<li style="line-height: normal;">it.putExtra(Intent.EXTRA_STREAM, "file:///sdcard/mysong.mp3");</li>
<li style="line-height: normal;">sendIntent.setType("audio/mp3");</li>
<li style="line-height: normal;">startActivity(Intent.createChooser(it, "Choose Email Client"));</li>
</ol></div>
</div>
<span class="t_tag" style="line-height: normal;">播放</span>多媒体
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">  </li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_VIEW);</li>
<li style="line-height: normal;">Uri uri = Uri.parse("file:///sdcard/song.mp3");</li>
<li style="line-height: normal;">it.setDataAndType(uri, "audio/mp3");</li>
<li style="line-height: normal;">startActivity(it);</li>
</ol></div>
</div>
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.withAppendedPath(MediaStore.Audio.Media.INTERNAL_CONTENT_URI, "1");</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_VIEW, uri);</li>
<li style="line-height: normal;">startActivity(it);  </li>
</ol></div>
</div>
Uninstall 程序
<div class="blockcode" style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;">
<div style="font-family: Arial; word-wrap: break-word; word-break: break-all; visibility: visible !important; zoom: 1 !important; filter: none; font-size: 12px; line-height: normal;"><ol style="line-height: normal;">
<li style="line-height: normal;">Uri uri = Uri.fromParts("package", strPackageName, null);</li>
<li style="line-height: normal;">Intent it = new Intent(Intent.ACTION_DELETE, uri);</li>
<li style="line-height: normal;">startActivity(it);</li>
</ol></div>
</div>
<!--more-->