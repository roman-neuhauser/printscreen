# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=	/usr/local
BINDIR ?=	$(PREFIX)/bin
MANDIR ?=	$(PREFIX)/share/man
MAN1DIR ?=	$(MANDIR)/man1

GZIPCMD?=	gzip
INSTALL_DATA?=	install -m 644
INSTALL_DIR?=	install -d 755
INSTALL_SCRIPT?=	install -m 755
RST2HTML?=$(call first_in_path,rst2html.py rst2html)

NAME =		printscreen

installed =	$(NAME).1.gz $(NAME)
artifacts =	$(installed) README.html

all: $(artifacts)

most: $(installed)

clean:
	$(RM) $(artifacts)

$(NAME): $(NAME).in
	$(INSTALL_SCRIPT) $< $@

html: README.html

%.html: %.rest
	$(RST2HTML) $< $@

%.1.gz: %.1
	$(GZIPCMD) < $< > $@

install: $(installed)
	@$(INSTALL_DIR) $(DESTDIR)$(BINDIR)
	@$(INSTALL_DIR) $(DESTDIR)$(MAN1DIR)
	@$(INSTALL_SCRIPT) $(NAME) $(DESTDIR)$(BINDIR)/$(NAME)
	@$(INSTALL_DATA) $(NAME).1.gz $(DESTDIR)$(MAN1DIR)/$(NAME).1.gz

define first_in_path
  $(or \
    $(firstword $(wildcard \
      $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
    )) \
  , $(error Need one of: $(1)) \
  )
endef

.DEFAULT_GOAL := most

.PHONY: all
.PHONY: clean
.PHONY: html
.PHONY: install
.PHONY: most
