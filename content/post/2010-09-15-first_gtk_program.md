+++
categories = ["Linux", "programming"]
comments = true
excerpt = "直接显示一个空白窗口，代码hello.c如下"
date = "2010-09-15T17:37:37+08:00"
published = true
status = "publish"
tags = ["gtk", "GUI", "Linux"]
title = "第一个GTK+程序"
type = "post"
description = ""
+++

直接显示一个空白窗口，代码hello.c如下

```cpp
/* compile command:
   gcc -o simple simple.c `pkg-config --libs --cflags gtk+-2.0`
*/

#include <gtk/gtk.h>

int main( int argc, char *argv[])
{
	GtkWidget* window;
	GtkWidget* frame;
	GtkWidget* label;

	gtk_init(&argc, &argv);

	window = gtk_window_new(GTK_WINDOW_TOPLEVEL);

	gtk_window_set_title(GTK_WINDOW(window), "Hello World");
	gtk_window_set_position(GTK_WINDOW(window), GTK_WIN_POS_CENTER);

	frame = gtk_fixed_new();
	gtk_container_add(GTK_CONTAINER(window), frame);
	
	label = gtk_label_new("Hello World");
	gtk_fixed_put(GTK_FIXED(frame), label, 10, 10);
	
	g_signal_connect_swapped(G_OBJECT(window), "destroy",
							 G_CALLBACK(gtk_main_quit), NULL);

	gtk_widget_show_all(window);
	gtk_main();

	return 0;
}
```

<img src="http://commondatastorage.googleapis.com/czc_public/appspotimages/2010-09-15-231732_208x108_scrot.png" alt="" width="208" height="108">
<!--more-->