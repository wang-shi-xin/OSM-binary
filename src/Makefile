
# these default settings can be overridden by setting environment variables
PREFIX   ?= /usr/local
LIBDIR   ?= $(PREFIX)/lib
CXX      ?= g++
CXXFLAGS ?= -O3
AR       ?= ar
PROTOC   ?= protoc
SONAME   ?= libosmpbf.so.1
VERSION  ?= 1.3.3

CXXFLAGS += -Wall -Wextra -pedantic -Wredundant-decls -Wdisabled-optimization -Wctor-dtor-privacy -Wnon-virtual-dtor -Woverloaded-virtual -Wsign-promo -Wno-long-long

.PHONY: clean install

all: libosmpbf.a libosmpbf.so ../include/osmpbf/fileformat.pb.h ../include/osmpbf/osmformat.pb.h

libosmpbf.a: fileformat.pb.o osmformat.pb.o
	$(AR) -cr $@ fileformat.pb.o osmformat.pb.o

libosmpbf.so: fileformat.pb.o osmformat.pb.o
	$(CXX) -shared -Wl,-soname,$(SONAME) -o $@ fileformat.pb.o osmformat.pb.o

%.pb.o: %.pb.cc
	$(CXX) $(CXXFLAGS) -fPIC -c -o $@ $<

%.pb.cc ../include/osmpbf/%.pb.h: %.proto
	$(PROTOC) --proto_path=. --cpp_out=. $<
	cp -v $(subst .proto,.pb.h,$<) ../include/osmpbf/

install:
	install -m 755 -d $(DESTDIR)$(LIBDIR)
	install -m 644 libosmpbf.a $(DESTDIR)$(LIBDIR)
	install -m 755 libosmpbf.so $(DESTDIR)$(LIBDIR)/libosmpbf.so.$(VERSION)
	ln -s libosmpbf.so.$(VERSION) $(DESTDIR)$(LIBDIR)/$(SONAME)
	ln -s libosmpbf.so.$(VERSION) $(DESTDIR)$(LIBDIR)/libosmpbf.so
	install -m 755 -d $(DESTDIR)$(PREFIX)/include/osmpbf
	install -m 644 ../include/osmpbf/osmpbf.h $(DESTDIR)$(PREFIX)/include/osmpbf
	install -m 644 ../include/osmpbf/fileformat.pb.h $(DESTDIR)$(PREFIX)/include/osmpbf
	install -m 644 ../include/osmpbf/osmformat.pb.h $(DESTDIR)$(PREFIX)/include/osmpbf

clean:
	rm -f *.pb.h *.pb.cc *.pb.o libosmpbf.a

