SHELL = /bin/bash
VERSION ?= $(shell rpmspec --srpm -q --qf '%{VERSION}' mock/mock.spec)

all:
	@echo nothing to do

install:
	cd mock; \
	pysite=$$(python3 -c 'import site; print(site.getsitepackages()[0])'); \
	for i in py/mock.py; do \
		sed -i 's|^\(__VERSION__\).*|\1 = "'$(VERSION)'"|' $$i; \
		sed -i 's|^\(SYSCONFDIR\).*|\1 = "'/etc'"|' $$i; \
		sed -i 's|^\(PYTHONDIR\).*|\1 = "'$$pysite'"|' $$i; \
		sed -i 's|^\(PKGPYTHONDIR\).*|\1 = "'$$pysite/mockbuild'"|' $$i; \
	done; \
	for i in docs/mock.1 docs/mock-parse-buildlog.1; do \
		sed -i 's|@VERSION@|'$(VERSION)'|' $$i; \
	done; \
	install -d $(DESTDIR)/usr/bin; \
	install -d $(DESTDIR)/usr/libexec/mock; \
	install mockchain $(DESTDIR)/usr/bin/mockchain; \
	install py/mock-parse-buildlog.py $(DESTDIR)/usr/bin/mock-parse-buildlog; \
	install py/mock.py $(DESTDIR)/usr/bin/mock; \
	install create_default_route_in_container.sh $(DESTDIR)/usr/libexec/mock/; \
	install -d $(DESTDIR)/usr/share/pam.d; \
	cp -a etc/pam/* $(DESTDIR)/usr/share/pam.d/; \
	install -d $(DESTDIR)/usr/share/defaults/mock; \
	cp -a etc/mock/* $(DESTDIR)/usr/share/defaults/mock/; \
	cp -a docs/site-defaults.cfg $(DESTDIR)/usr/share/defaults/mock/; \
	install -d $(DESTDIR)/usr/share/bash-completion/completions/; \
	cp -a etc/bash_completion.d/* $(DESTDIR)/usr/share/bash-completion/completions/; \
	install -d $(DESTDIR)/$$pysite; \
	cp -a py/mockbuild $(DESTDIR)/$$pysite/; \
	install -d $(DESTDIR)/usr/share/man/man1; \
	install -d $(DESTDIR)/usr/share/defaults/sudo/sudoers.d/; \
	install -m 440 ../mock.sudoers $(DESTDIR)/usr/share/defaults/sudo/sudoers.d/mock; \
	cp -a docs/mock.1 docs/mock-parse-buildlog.1 $(DESTDIR)/usr/share/man/man1/;
