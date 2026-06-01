Objectives
----------

The objective of this development was to provide a tiny windowing system
in the spirit of X, but greatly scaled down and appropriate for most
resource-limited embedded environments. The current NX implementation
supports the general following, high-level features:

* **Virtual Vertical Graphics Space**. Windows that reside in a
  virtual, *vertical* space so that it makes sense to talk about one
  window being on top of another and obscuring the window below it.

* **Client/Server Model**. A standard client server/model was adopted.
  NX may be considered a server and other logic that presents the
  windows are NX clients.

* **Multi-User Support**. NX includes *front-end* logic to support a
  separate NX server thread that can serve multiple NX client threads.
  The NX is a server thread/daemon the serializes graphics operations
  from multiple clients.

* **Minimal Graphics Toolset**. The actual implementation of the
  graphics operations is performed by common, *back-end* logic. This
  back-end supports only a primitive set of graphic and rendering
  operations.

* **Device Interface**. NX supports any graphics device either of two
  device interfaces:

  * Any device with random access video memory using the NuttX
    framebuffer driver interface (see ``include/nuttx/video/fb.h``).

  * Any LCD-like device than can accept raster line *runs* through a
    parallel or serial interface (see ``include/nuttx/lcd/lcd.h``). By
    default, NX is configured to use the frame buffer driver unless
    ``CONFIG_NX_LCDDRIVER`` is defined =y in your NuttX configuration
    file.

* **Transparent to NX Client**. The window client on "sees" the
  sub-window that is operates in and does not need to be concerned with
  the virtual, vertical space (other that to respond to *redraw*
  requests from NX when needed).

* **Framed Windows and Toolbars**. NX also adds the capability to
  support windows with frames and toolbars on top of the basic
  windowing support. These are windows such as those shown in the
  :ref:`fig_nx_screenshot` above. These framed windows sub-divide
  one one window into three relatively independent subwindows: A frame,
  the contained window and an (optional) toolbar window.

* **Mouse Support**. NX provides support for a mouse or other X/Y
  pointing devices. APIs are provided to allow external devices to give
  X/Y position information and mouse button presses to NX. NX will then
  provide the mouse input to the relevant window clients via callbacks.
  Client windows only receive the mouse input callback if the mouse is
  positioned over a visible portion of the client window; X/Y position
  is provided to the client in the relative coordinate system of the
  client window.

* **Keyboard input**. NX also supports keyboard/keypad devices. APIs
  are provided to allow external devices to give keypad information to
  NX. NX will then provide the mouse input to the top window on the
  display (the window that has the *focus*) via a callback function.
