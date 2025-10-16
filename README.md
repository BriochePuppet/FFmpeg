# Build FFmpeg Shared Library  

[FFmpeg 3.4](https://github.com/FFmpeg/FFmpeg/commit/9983d098ff0ee54bc3b77676dd885883bfbe4ffb)  

```diff
From 95245fa59461e388e1571650a9cfc5193b193256 Mon Sep 17 00:00:00 2001
From: HanetakaChou <HanetakaChou@outlook.com>
Date: Thu, 16 Oct 2025 19:26:39 +0200
Subject: [PATCH] soname

---
 configure | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

diff --git a/configure b/configure
index 9a49bcc569..d670db4109 100755
--- a/configure
+++ b/configure
@@ -3412,8 +3412,8 @@ LIBNAME='$(LIBPREF)$(FULLNAME)$(LIBSUF)'
 SLIBPREF="lib"
 SLIBSUF=".so"
 SLIBNAME='$(SLIBPREF)$(FULLNAME)$(SLIBSUF)'
-SLIBNAME_WITH_VERSION='$(SLIBNAME).$(LIBVERSION)'
-SLIBNAME_WITH_MAJOR='$(SLIBNAME).$(LIBMAJOR)'
+SLIBNAME_WITH_VERSION='$(SLIBPREF)$(FULLNAME).$(LIBVERSION)$(SLIBSUF)'
+SLIBNAME_WITH_MAJOR='$(SLIBPREF)$(FULLNAME).$(LIBMAJOR)$(SLIBSUF)'
 LIB_INSTALL_EXTRA_CMD='$$(RANLIB) "$(LIBDIR)/$(LIBNAME)"'
 SLIB_INSTALL_NAME='$(SLIBNAME_WITH_VERSION)'
 SLIB_INSTALL_LINKS='$(SLIBNAME_WITH_MAJOR) $(SLIBNAME)'
-- 
2.47.3
```

```diff
From af61b06b075c3bdb03d5a46038f178c4e5721bc8 Mon Sep 17 00:00:00 2001
From: HanetakaChou <HanetakaChou@outlook.com>
Date: Thu, 16 Oct 2025 19:54:56 +0200
Subject: [PATCH] incompatible function pointer types

---
 libavformat/tty.c | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/libavformat/tty.c b/libavformat/tty.c
index 954aafe33b..d146cc38a5 100644
--- a/libavformat/tty.c
+++ b/libavformat/tty.c
@@ -49,7 +49,7 @@ typedef struct TtyDemuxContext {
     AVRational framerate; /**< Set by a private option. */
 } TtyDemuxContext;
 
-static int read_probe(const AVProbeData *p)
+static int read_probe(AVProbeData *p)
 {
     int cnt = 0;
 
-- 
2.47.3
```

```bash
mkdir ./build

cd ./build

../configure --prefix=install --enable-shared --disable-static --enable-gpl --enable-version3 --enable-bsfs --disable-avdevice --disable-postproc --disable-avfilter --cc=clang --cxx=clang++ --extra-ldflags='-Wl,--enable-new-dtags' --extra-ldflags='-Wl,-rpath,XORIGIN'

make -j$(nproc) install

for so in ./install/lib/*.so; do chrpath -r '$ORIGIN' ${so}; done
```
