Fork of the now archived https://bitbucket.org/thesheep/micropython-sdcard

SD Card driver for ESP8266 MicroPython
**************************************

Since MicroPython on the ESP8266 doesn't let you have multiple filesystems
at the moment, you can only replace the internal filesystem with the one on
the sd card. In order to do that, you need to compile your own firmware.

Edit your ``esp8266/modules/_boot.py`` and replace the mounting of the filesystem::

    import gc
    gc.threshold((gc.mem_free() + gc.mem_alloc()) // 4)
    import uos
    # from flashbdev import bdev
    #
    #try:
    #    if bdev:
    #        vfs = uos.VfsFat(bdev, "")
    #except OSError:
    #    import inisetup
    #    vfs = inisetup.setup()

    import sdcard
    sdcard.esp8266_mount()

    gc.collect()

Then add the ``sdcard.py`` file from this repository to ``esp8266/modules``.
Connect your SD Card socket to your ESP8266 board's SPI (``miso=Pin(12),
mosi=Pin(13), sck=Pin(14), cs=Pin(15)``), and enjoy!
