pam_sessiondir = $(sysconfdir)/pam.d/
pam_session_DATA = \
	lightdm-remote-freerdp2

lightdm_sessiondir = $(datadir)/lightdm/remote-sessions
lightdm_session_DATA = \
	freerdp2.desktop

%.desktop: %.desktop.in
	@sed -e "s|\@libexecdir\@|$(libexecdir)|" $< > $@

session_startdir = $(pkgdatadir)
session_start_SCRIPTS = \
	freerdp2-session

freerdp2-session: freerdp2-session.in
	@sed -e "s|\@libexecdir\@|$(libexecdir)|" $< > $@
	@chmod +x $@

apparmordir = $(sysconfdir)/apparmor.d/
apparmor_DATA = \
	lightdm-remote-session-freerdp2

lightdm-remote-session-freerdp2: lightdm-remote-session-freerdp2.in
	@sed -e "s|\@libexecdir\@|$(libexecdir)|" $< > $@

libexec_PROGRAMS = \
	freerdp2-session-wrapper

freerdp2_session_wrapper_SOURCES = \
	freerdp2-session-wrapper.c
freerdp2_session_wrapper_CFLAGS = \
	-DPKGDATADIR="\"$(pkgdatadir)\"" \
	-Wall -Werror

freerdp2_known_hosts2dir = $(sysconfdir)/arctica-greeter/guest-session/skel/.config/freerdp/
freerdp2_known_hosts2_DATA = \
	known_hosts2

EXTRA_DIST = \
	$(pam_session_DATA) \
	freerdp2.desktop.in \
	freerdp2-session.in \
	lightdm-remote-session-freerdp2.in

CLEANFILES = \
	freerdp2.desktop \
	freerdp2-session \
	lightdm-remote-session-freerdp2

DISTCHECK_CONFIGURE_FLAGS = --enable-localinstall

dist-hook:
	@if test -d "$(top_srcdir)/.bzr"; \
		then \
			echo Creating ChangeLog && \
				( cd "$(top_srcdir)" && \
				echo '# Generated by Makefile. Do not edit.'; echo; \
				$(top_srcdir)/missing --run bzr log --gnu-changelog ) > ChangeLog.tmp \
				&& mv -f ChangeLog.tmp $(top_distdir)/ChangeLog \
				|| (rm -f ChangeLog.tmp; \
					echo Failed to generate ChangeLog >&2 ); \
		else \
			echo Failed to generate ChangeLog: not a branch >&2; \
		fi
		@if test -d "$(top_srcdir)/.bzr"; \
			then \
				echo Creating AUTHORS && \
					( cd "$(top_srcdir)" && \
					echo '# Generated by Makefile. Do not edit.'; echo; \
					$(top_srcdir)/missing --run  bzr log --long --levels=0 | grep -e "^\s*author:" -e "^\s*committer:" | cut -d ":" -f 2 | cut -d "<" -f 1 | sort -u) > AUTHORS.tmp \
					&& mv -f AUTHORS.tmp $(top_distdir)/AUTHORS \
					|| (rm -f AUTHORS.tmp; \
						echo Failed to generate AUTHORS >&2 ); \
			else \
				echo Failed to generate AUTHORS: not a branch >&2; \
	fi
