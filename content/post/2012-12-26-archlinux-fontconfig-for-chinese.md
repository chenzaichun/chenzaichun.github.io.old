+++
title = "archlinux下修改默认中文字体"
date = "2012-12-26T11:17:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["emacs", "linux", "archlinux"]
description = ""
+++


archlinux中的字体看起来真的很搓，试过ubuntu patch过的版本，还是不尽如人意。后来发现了`fontconfig-infinality`, 整个世界突然美好了。跟windows下的`MacType`差不多。

当我将ibus换成fcitx之后，问题又出现了，3.6版本之后的fcitx竟然没有字体设置了，对于字体的控制必须使用fontconfig。这必须的修改默认的中文字体。

<!--more-->

直接贴fontconfig的配置吧（我这里使用的是`DejaVu Sans YuanTi`）

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>

    <!-- fonts dirs -->
    <dir>~/.fonts</dir>

    <match>
        <test name="family">
            <string>sans-serif</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
	    <string>DejaVu Sans YuanTi</string>
            <string>DejaVu Sans</string>
            <string>Liberation Sans</string>
            <string>Droid Sans</string>
            <string>WenQuanYi Zen Hei</string>
            <string>WenQuanYi Micro Hei</string>
            <string>WenQuanYi Bitmap Song</string>
            <string>AR PL KaitiM Big5</string>
            <string>AR PL KaitiM GB</string>
            <string>AR PL Mingti2L Big5</string>
            <string>AR PL New Sung</string>
            <string>AR PL SungtiL GB</string>
            <string>AR PL UKai CN</string>
            <string>AR PL UKai HK</string>
            <string>AR PL UKai TW</string>
            <string>AR PL UKai TW MBE</string>
            <string>AR PL UMing CN</string>
            <string>AR PL UMing HK</string>
            <string>AR PL UMing TW</string>
            <string>AR PL UMing TW MBE</string>
        </edit>
    </match>
    <match>
        <test name="family">
            <string>serif</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
  	    <string>DejaVu Sans YuanTi</string>
            <string>DejaVu Serif</string>
            <string>Liberation Serif</string>
            <string>Bitstream Charter</string>
            <string>Droid Serif</string>
            <string>WenQuanYi Zen Hei Sharp</string>
            <string>WenQuanYi Bitmap Song</string>
            <string>AR PL New Sung</string>
            <string>AR PL SungtiL GB</string>
            <string>AR PL UMing CN</string>
            <string>AR PL UMing TW</string>
            <string>AR PL KaitiM Big5</string>
            <string>AR PL KaitiM GB</string>
            <string>AR PL Mingti2L Big5</string>
            <string>AR PL UKai CN</string>
            <string>AR PL UKai HK</string>
            <string>AR PL UKai TW</string>
            <string>AR PL UKai TW MBE</string>
            <string>AR PL UMing CN</string>
            <string>AR PL UMing HK</string>
            <string>AR PL UMing TW</string>
            <string>AR PL UMing TW MBE</string>
        </edit>
    </match>
    <match>
        <test name="family">
            <string>monospace</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
  	    <string>DejaVu Sans YuanTi Mono</string>
            <string>DejaVu Sans Mono</string>
            <string>Liberation Sans Mono</string>
            <string>Droid Sans Mono</string>
            <string>WenQuanYi Zen Hei Mono</string>
            <string>WenQuanYi Micro Hei Mono</string>
            <string>WenQuanYi Zen Hei Sharp</string>
            <string>AR PL UMing TW</string>
            <string>AR PL KaitiM Big5</string>
            <string>AR PL KaitiM GB</string>
            <string>AR PL Mingti2L Big5</string>
            <string>AR PL New Sung</string>
            <string>AR PL SungtiL GB</string>
            <string>AR PL UKai CN</string>
            <string>AR PL UKai HK</string>
            <string>AR PL UKai TW</string>
            <string>AR PL UKai TW MBE</string>
            <string>AR PL UMing CN</string>
            <string>AR PL UMing HK</string>
            <string>AR PL UMing TW</string>
            <string>AR PL UMing TW MBE</string>
        </edit>
    </match>

    <!-- 把serif ,sans,monospace的family(字体族)重新排序 -->
    <alias>
        <family>serif</family>
        <prefer>
  	    <family>DejaVu Sans YuanTi</family>
            <family>DejaVu Serif</family>
            <family>Liberation Serif</family>
            <family>Bitstream Charter</family>
            <family>Droid Serif</family>
            <family>WenQuanYi Zen Hei Sharp</family>
            <family>WenQuanYi Bitmap Song</family>
            <family>AR PL New Sung</family>
            <family>AR PL SungtiL GB</family>
            <family>AR PL UMing CN</family>
            <family>AR PL UMing TW</family>
            <family>AR PL KaitiM Big5</family>
            <family>AR PL KaitiM GB</family>
            <family>AR PL Mingti2L Big5</family>
            <family>AR PL SungtiL GB</family>
            <family>AR PL UKai CN</family>
            <family>AR PL UKai HK</family>
            <family>AR PL UKai TW</family>
            <family>AR PL UKai TW MBE</family>
            <family>AR PL UMing CN</family>
            <family>AR PL UMing HK</family>
            <family>AR PL UMing TW</family>
            <family>AR PL UMing TW MBE</family>
        </prefer>
    </alias>
    <alias>
        <family>sans-serif</family>
        <prefer>
  	    <family>DejaVu Sans YuanTi</family>
            <family>DejaVu Sans</family>
            <family>Liberation Sans</family>
            <family>Droid Sans</family>
            <family>WenQuanYi Zen Hei</family>
            <family>WenQuanYi Micro Hei</family>
            <family>WenQuanYi Bitmap Song</family>
            <family>AR PL New Sung</family>
            <family>AR PL KaitiM Big5</family>
            <family>AR PL KaitiM GB</family>
            <family>AR PL Mingti2L Big5</family>
            <family>AR PL SungtiL GB</family>
            <family>AR PL UKai CN</family>
            <family>AR PL UKai HK</family>
            <family>AR PL UKai TW</family>
            <family>AR PL UKai TW MBE</family>
            <family>AR PL UMing CN</family>
            <family>AR PL UMing HK</family>
            <family>AR PL UMing TW</family>
            <family>AR PL UMing TW MBE</family>
        </prefer>
    </alias>
    <alias>
        <family>monospace</family>
        <prefer>
  	    <family>DejaVu Sans YuanTi Mono</family>
            <family>DejaVu Sans Mono</family>
            <family>Liberation Sans Mono</family>
            <family>Droid Sans Mono</family>
            <family>WenQuanYi Zen Hei Mono</family>
            <family>WenQuanYi Micro Hei Mono</family>
            <family>WenQuanYi Zen Hei Sharp</family>
            <family>AR PL New Sung</family>
            <family>AR PL UMing TW</family>
            <family>AR PL KaitiM Big5</family>
            <family>AR PL KaitiM GB</family>
            <family>AR PL Mingti2L Big5</family>
            <family>AR PL SungtiL GB</family>
            <family>AR PL UKai CN</family>
            <family>AR PL UKai HK</family>
            <family>AR PL UKai TW</family>
            <family>AR PL UKai TW MBE</family>
            <family>AR PL UMing CN</family>
            <family>AR PL UMing HK</family>
            <family>AR PL UMing TW</family>
            <family>AR PL UMing TW MBE</family>
        </prefer>
    </alias>

    <!-- DPI -->
    <match target="pattern">
        <edit name="dpi" mode="assign">
            <double>96</double>
        </edit>
    </match>

    <!-- default : smoothed and hinted -->
    <match target="font">
        <edit name="antialias" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="autohint" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="hinting" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="hintstyle" mode="assign">
            <const>hintfull</const>
        </edit>
        <edit name="rgba" mode="assign">
            <const>rgb</const>
        </edit>
        <edit name="lcdfilter" mode="assign">
            <const>lcddefault</const>
        </edit>
    </match>

    <!-- 优先使用内嵌的点阵字 -->
    <match target="font">
        <edit name="embeddedbitmap" mode="assign">
            <bool>true</bool>
        </edit>
    </match>

    <!-- For point size less equal than 6 : only smoothed -->
    <match target="font" >
        <test name="size" compare="less_eq">
            <double>6</double>
        </test>
        <edit name="antialias" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="autohint" mode="assign">
            <bool>false</bool>
        </edit>
        <edit name="hinting" mode="assign">
            <bool>false</bool>
        </edit>
    </match>

    <!-- 注意没有指定hintstyle，不希望它作为全局设定 -->
    <match target="font">
        <test name="lang" compare="contains">
            <string>zh</string>
            <string>ja</string>
            <string>ko</string>
        </test>
        <edit name="hinting" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="autohint" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="antialias" mode="assign">
            <bool>true</bool>
        </edit>
    </match>

    <!-- Synthetic emboldening for fonts do not have bold face -->
    <match target="font">
        <test name="weight" compare="less_eq">
            <const>medium</const>
        </test>
        <test target="pattern" compare="more" name="weight">
            <const>medium</const>
        </test>
        <edit name="embolden" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="weight" mode="assign">
            <const>bold</const>
        </edit>
    </match>

    <!-- The dual-width Asian fonts (spacing=dual) are not rendered
         correctly, apparently FreeType forces all widths to match.
         Trying to disable the width forcing code by setting
         globaladvance=false alone doesn't  help. As a brute force
         workaround, also set spacing=proportional, i.e. handle
         them as proportional fonts:  CJK 双宽度纠正 -->
    <match target="font">
        <test name="lang" target="pattern" compare="contains">
            <string>zh</string>
            <string>ja</string>
            <string>ko</string>
        </test>
        <test name="spacing" compare="eq">
            <const>dual</const>
        </test>
        <edit name="spacing" mode="assign">
            <const>proportional</const>
        </edit>
        <edit name="globaladvance" mode="assign">
            <bool>false</bool>
        </edit>
    </match>

    <!-- There is a similar problem with dual width bitmap fonts.
         They don't have spacing=dual, therefore they are not
         handled by the above rule and still display as charcell
         fonts. For example "Efont Biwidth" has spacing=mono and
         "Misc Fixed Wide" has spacing=charcell. Force handling of
         these fonts as proportional fonts as well: -->
    <match target="font">
        <test name="lang" compare="contains">
            <string>zh</string>
            <string>ja</string>
            <string>ko</string>
        </test>
        <test name="outline" compare="eq">
            <bool>false</bool>
        </test>
        <test name="spacing" compare="eq">
            <const>mono</const>
            <const>charcell</const>
        </test>
        <edit name="spacing">
            <const>proportional</const>
        </edit>
        <edit name="globaladvance" binding="strong">
            <bool>false</bool>
        </edit>
    </match>

    <!-- 设定中文字体的最小字号 -->
    <match target="font">
        <test name="family" equal="any">
            <string>Unibit</string>
            <string>WenQuanYi Zen Hei</string>
            <string>WenQuanYi Micro Hei</string>
            <string>WenQuanYi Zen Hei Mono</string>
            <string>WenQuanYi Micro Hei Mono</string>
            <string>WenQuanYi Zen Hei Sharp</string>
            <string>WenQuanYi Bitmap Song</string>
            <string>AR PL UMing TW</string>
            <string>AR PL KaitiM Big5</string>
            <string>AR PL KaitiM GB</string>
            <string>AR PL Mingti2L Big5</string>
            <string>AR PL New Sung</string>
            <string>AR PL SungtiL GB</string>
            <string>AR PL UKai CN</string>
            <string>AR PL UKai HK</string>
            <string>AR PL UKai TW</string>
            <string>AR PL UKai TW MBE</string>
            <string>AR PL UMing CN</string>
            <string>AR PL UMing HK</string>
            <string>AR PL UMing TW</string>
            <string>AR PL UMing TW MBE</string>
            <string>AR PL UMing CN</string>
            <string>AR PL UMing TW</string>
        </test>
        <test name="pixelsize" compare="more_eq">
            <int>8</int>
        </test>
        <test name="pixelsize" compare="less_eq">
            <int>12</int>
        </test>
        <edit name="pixelsize" mode="assign">
            <int>12</int>
        </edit>
    </match>

    <!-- MingLiU/PMingLiU一定要全力hint，否则字会错乱 -->
    <match target="font">
        <test name="family" compare="eq">
            <string>MingLiU</string>
            <string>PMingLiU</string>
        </test>
        <edit name="hintstyle" mode="assign">
            <const>hintfull</const>
        </edit>
    </match>

    <!-- WenQuanYi Fonts -->
    <match target="font">
        <test name="family" equal="any">
            <string>WenQuanYi Zen Hei</string>
            <string>WenQuanYi Zen Hei Mono</string>
            <string>WenQuanYi Zen Hei Sharp</string>
            <string>WenQuanYi Micro Hei</string>
            <string>WenQuanYi Micro Hei Mono</string>
            <string>WenQuanYi Micro Hei Light</string>
        </test>
        <edit name="antialias" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="hinting" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="autohint" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="hintstyle" mode="assign">
            <const>hintfull</const>
        </edit>
        <edit name="embeddedbitmap" mode="assign">
            <bool>true</bool>
        </edit>
    </match>
    <match target="pattern">
        <test name="family" equal="any" compare="eq">
            <string>serif</string>
            <string>sans-serif</string>
            <string>monospace</string>
        </test>
        <test name="lang" equal="any" compare="eq">
            <string>zh-cn</string>
            <string>zh-tw</string>
            <string>zh-hk</string>
            <string>zh-sg</string>
        </test>
        <test name="pixelsize" compare="more_eq">
            <double>12</double>
        </test>
        <test name="pixelsize" compare="less_eq">
            <double>16</double>
        </test>
        <edit name="family" mode="prepend_first">
            <string>WenQuanYi Bitmap Song</string>
        </edit>
    </match>
    <!-- End WenQuanYi Fonts -->

    <!-- Firefly Truetype Font -->
    <match target="font">
        <test name="family" equal="any">
            <string>AR PL New Sung</string>
        </test>
        <edit name="antialias">
            <bool>true</bool>
        </edit>
        <edit name="hinting" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="autohint" mode="assign">
            <bool>false</bool>
        </edit>
    </match>
    <match target="font">
        <test name="family" equal="any">
            <string>AR PL New Sung</string>
        </test>
        <test name="size" compare="less_eq">
            <int>16</int>
        </test>
        <edit name="antialias" mode="assign">
            <bool>false</bool>
        </edit>
        <edit name="hinting" mode="assign">
            <bool>true</bool>
        </edit>
    </match>
    <match target="font">
        <test name="family" equal="any">
            <string>AR PL New Sung</string>
        </test>
        <edit name="globaladvance" mode="assign">
            <bool>false</bool>
        </edit>
    </match>
    <!-- end Firefly Truetype Font -->
</fontconfig>
```

