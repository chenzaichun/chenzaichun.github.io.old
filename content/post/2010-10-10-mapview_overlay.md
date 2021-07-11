+++
categories = ["programming"]
comments = true
published = true
status = "publish"
date = "2010-10-10T23:16:17+08:00"
tags = ["android", "Google"]
title = "MapView中画图"
type = "post"
description = ""
+++


通过Overlay的方式画图，不废话了，直接代码吧。需要完整代码的这里下载：<a href="http://commondatastorage.googleapis.com/czc_public/code/android/maptest2.tar.gz" target="_blank">猛击这里</a>

参考链接： <a href="http://westyi.blogbus.com/logs/69324000.html">http://westyi.blogbus.com/logs/69324000.html</a>

```java
package com.appspot.zaichunchen;

import android.graphics.Bitmap;
import android.graphics.Canvas;
import android.graphics.Point;

import com.google.android.maps.GeoPoint;
import com.google.android.maps.MapView;
import com.google.android.maps.Overlay;
import com.google.android.maps.Projection;

/**
 * Map Overlay
 * 
 * @author ChenZaichun
 * 
 */
public class Marker extends Overlay {
	private GeoPoint point = null;
	private Bitmap bmp = null; 
	private Point deviation = null; 

	/**
	 * 
	 * @param point
	 *            GeoPoint of icon
	 * @param bmp
	 *            Icon Bitmap
	 * @param deviation
	 *            icon offset, use window coordinates (left --> right, up --> down)
	 */
	public Marker(GeoPoint point, Bitmap bmp, Point deviation) {
		this.point = point;
		this.bmp = bmp;
		this.deviation = deviation;
	}

	/**
	 * Draw the icon
	 */
	@Override
	public void draw(Canvas canvas, MapView mapView, boolean shadow) {
		if (!shadow) {// not the shadow layer
			Projection projection = mapView.getProjection();
			if (point != null && bmp != null) {
				Point pos = projection.toPixels(point, null);
				//add offset
				canvas.drawBitmap(bmp, pos.x + deviation.x, pos.y + deviation.y, null);
			}	
		}
	}
}

```

使用的方法：
<!--more-->

```java
Double lat = 31.23717 * 1E6;
Double lng = 121.50811 * 1E6;

GeoPoint point = new GeoPoint(lat.intValue(), lng.intValue());

Bitmap bmp = BitmapFactory.decodeResource(getResources(),
				R.drawable.marker);
Point deviation = new Point(-15, -36);
Marker marker = new Marker(point, bmp, deviation);
mapView.getOverlays().add(marker);
```
